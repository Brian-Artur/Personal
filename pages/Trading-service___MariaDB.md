- `alertas` : lo que llega de TradingView (registro histórico)
  logseq.order-list-type:: number
  collapsed:: true
  Es un log de solo-añadir. *Un registro de los que está pasando fuera*
	- `id` — clave, autoincremental
	- `tipo` — qué es (rsi, precio, cruce_linea, dominancia…)
	- `valor` — el número (62.5, 64000…)
	- `symbol` — la moneda (BTCUSDT)
	- `direccion` — opcional, para cruces de línea (arriba/abajo)
	- `payload` — el JSON crudo tal cual llegó (JSON), para auditoría y depuración
	- `recibida_en` — marca de tiempo
- `reglas` — los parámetros de cada estrategia. *Cuándo actuar*. Los ajustes que definen las necesidades de cada operación distinta.
  logseq.order-list-type:: number
	- `id` — clave
	- `nombre` — El mismo nombre que tenga el archivo con la lógica en .ts (p. ej. *rsi_reversion* -> *rsi_reversion.ts*)
	- `activa` — Un interruptor por cada regla
		- true
		- false (booleano)
	- `modo` — Ejecuta solo o pide permiso
		- auto : dispara sola
		- semiautomatico : espera tu visto bueno
	- `symbol` — sobre qué moneda opera
	- `qty` — tamaño de la operación (0.001, ...)
	- `ventana_validez_seg` — cuán recientes deben ser las condiciones para valer. *900 = 15min*
	- `confirmacion_expira_seg` — Si es semiautomática y te propone algo, ¿cuánto espera tu respuesta antes de rendirse? Si vale *300 (5 minutos)* y no confirmas en ese rato, *la propuesta se cae sola*.
	- `estado_actual` — Evita compras duplicadas
		- fuera (sin posición)
		- en_largo (compró, espera vender)
		- en_corto (vendió, espera comprar)
	- `actualizada_en`
- `ordenes` — el intento de ejecución en Bybit (el corazón)
  logseq.order-list-type:: number
	- `id` — clave
	- `regla_id` — qué regla la originó (FK a `reglas`)
		- Permite, al ejecutarse, actualizar el `estado_actual` de esa regla.
	- `order_link_id` — identificador **ÚNICO** que impide que se haga dobles operaciones en Bybit, tras un apagón, corte, etc.
	- `bybit_order_id` — el id que le pone Bybit. Es nulo hasta que responde. Lo guardo para poder consultarla o cancelarla después.
	- `symbol`, `side` (buy/sell), `tipo_orden` (market/limit)
	- `qty` — cantidad
	- `precio` — A qué precio? Nulo en market, obligatorio si es limit (solo compras a ese precio o mejor)
	- `estado` — el estado de la máquina
		- Un encargo automático nace *pendiente*
		- Uno semiautimático nace en *esperando_confirmacion* y  pasa a *pendiente* cuando confirmas.
		- Cuando el programa todavía no hizo la petición a Bybit *procesando*.
		- Cuando se está enviando es *enviada*.
		- Si Bybit lo aceptó *ejecutada*.
		- *caducada*
		- *rechazada*
		- *omitida*
		- *error*
	- `filled_qty` — cuánto se ha llenado (esto cubre los llenados parciales sin necesidad de un estado aparte)
	- `avg_price` — precio medio de ejecución
	- `motivo` — texto nulo, para guardar el porqué de un rechazo o error (oro para depurar)
	- `confirmacion_expira_en` — marca de tiempo, solo para semiautomáticas en espera
	- `creada_en` - Cuando nació
	- `actualizada_en` - Cuándo cambió por última vez
- `posiciones` — **espejo de Bybit** *lo abierto ahora mismo*
  logseq.order-list-type:: number
  collapsed:: true
	- `symbol` — único (en modo one-way, una posición por símbolo)
	  logseq.order-list-type:: number
	- `side` (long/short), `size`, `entry_price`, `unrealized_pnl`
	  logseq.order-list-type:: number
	- `actualizada_en`
	  logseq.order-list-type:: number
- `saldo` — **espejo de Bybit** . Cuánto dinero hay de cada moneda.
  logseq.order-list-type:: number
  collapsed:: true
	- `coin` — único (BTC, USDT…)
	  logseq.order-list-type:: number
	- `balance` — total
	  logseq.order-list-type:: number
	- `disponible` — libre (lo que no está bloqueado en órdenes)
	  logseq.order-list-type:: number
	- `equity`
	  logseq.order-list-type:: number
	- `actualizado_en`
	  logseq.order-list-type:: number
- **`configuracion` — ajustes globales (clave/valor)**
  logseq.order-list-type:: number
  collapsed:: true
	- `clave` — clave primaria (p. ej. `trading_activo`)
	  logseq.order-list-type:: number
	- `valor` — el valor (`true`)
	  logseq.order-list-type:: number
	- `actualizado_en`
	  logseq.order-list-type:: number