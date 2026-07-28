# 1. Alerta entrante → orden (el flujo principal)

```
POST /api/webhook                          http/routers/webhook.ts
  → esquemaAlerta.safeParse()              types/alerta.ts        (valida formato)
  → secretoValido()                        http/routers/webhook.ts (timingSafeEqual)
  → insertar(alerta)                       repositories/alertas.ts (guarda el hecho)
  → res 200  ← responde ANTES de evaluar (timeout de TradingView)
  → evaluarTodas(ejecutor)                 services/evaluador.ts   (async, sin await)
      para cada regla activa:
      → listarActivas()                    repositories/reglas.ts
      → registro.get(nombre)               domain/rules/indice.ts  (busca el código)
      → recientes(symbol, ventana, perfil) repositories/alertas.ts (filtra por ventana)
      → regla.evaluar(ctx)                 domain/rules/rsiReversion.ts (LÓGICA PURA)
      ── si decision.operar = false → para aquí
      → crearDesdeDecision()               services/ordenes.ts
          → estaActivo()                   services/trading.ts     (¿pausado?)
          → crear(orden)                   repositories/ordenes.ts (INSERT)
      ── si estado ≠ "pendiente" → para aquí (omitida / esperando_confirmacion)
      → despachar(id, ejecutor)            services/ejecucion.ts   → sigue en flujo 2b
```

La bifurcación por `estado` es la traducción de tus tres modos: `omitida` (pausa), `esperando_confirmacion` (semi), `pendiente` (auto → despacha ya).

# 2b. Despacho de una orden (el envío al exchange)

```
despachar(id, ejecutor)                    services/ejecucion.ts
  → reclamar(id)                           repositories/ordenes.ts (CLAIM ATÓMICO)
  ── si affectedRows ≠ 1 → "no_reclamada", para
  → datosParaEnvio(id)                     repositories/ordenes.ts (JOIN a reglas → cuenta_id)
  → obtenerEjecutor(cuenta_id)             [pendiente de cablear en index.ts]
  ── si no hay ejecutor → marcarError, para
  → ejecutor.enviarOrden()                 exchange/bybitReal.ts   (o bybitFake.ts)
  → marcarEnviada(id, bybit_order_id)      repositories/ordenes.ts
  ── si lanza → marcarError()              repositories/ordenes.ts
```

# 3. Fill entrante (Bybit empuja la ejecución)

```
[WebSocket privado]                        exchange/bybitRealEscucha.ts
  → procesarFill(fill)                      services/fills.ts
      → aplicarFill()                       repositories/ordenes.ts (JOIN → cuenta_id, regla_id, side)
      ── si no encontrada / ya tratada → para
      ── si fill.completa:
         buy  → cambiarEstado(regla,"en_largo")  repositories/reglas.ts
              → upsert(posicion)                 repositories/posiciones.ts
         sell → cambiarEstado(regla,"fuera")     repositories/reglas.ts
              → cerrar(posicion)                 repositories/posiciones.ts
```

Aquí está la clave de por qué `estado_actual` de la regla solo cambia con el fill, no al crear la orden: es este flujo, no el 1, quien lo mueve.

# 4. Confirmación de orden semiautomática

```
POST /api/ordenes/:id/confirmar            http/routers/ordenes.ts
  → confirmarOrden(id, ejecutor)           services/confirmacion.ts
      → confirmar(id)                      repositories/ordenes.ts (UPDATE valida caducidad en el propio WHERE)
      ── si affectedRows ≠ 1:
         → estadoDe(id)                    repositories/ordenes.ts (para explicar el porqué)
         → { ok:false, ... }
      → despachar(id, ejecutor)            services/ejecucion.ts   → flujo 2b
```

El semi entra por el flujo 1 y se **para** en `esperando_confirmacion`. Este flujo lo retoma: `confirmar` lo pasa a `pendiente` y despacha. La caducidad se valida dentro del `UPDATE`, no antes — misma filosofía de claim atómico.

# 5. Pausa / reanudar

```
POST /api/trading/pausar                   http/routers/trading.ts
  → cambiarEstado(false)                   services/trading.ts (primero BD, luego memoria)
POST /api/trading/reanudar → cambiarEstado(true)
GET  /api/trading/estado   → estaActivo()  (lee la copia en memoria, rápida)
```

El efecto se nota en el flujo 1: `crearDesdeDecision` llama a `estaActivo()`; si está pausado, la orden nace `omitida` y nunca llega a despacharse.

# 6. Consultas del panel (solo lectura)

```
GET /api/posiciones        → listarPosiciones()  repositories/posiciones.ts
GET /api/saldo             → listarSaldo()        repositories/saldo.ts
GET /api/reglas            → listarReglas()        repositories/reglas.ts
GET /api/alertas/recientes → listarAlertas(20)     repositories/alertas.ts
```

Directos a repo, sin lógica de servicio. Es lo que refresca `useFetch` en el frontend.