# 2 estados de ordenes

- `orden 1 creada en estado "omitida"` → trading en pausa: registra pero no operaría.
- `orden 2 creada en estado "esperando_confirmacion"` → trading activo + regla semiautomática: propone y espera tu visto bueno. 

# 5 condiciones de alertas

- **Secreto válido** — si no, ni se registra (401).
- **Perfil correcto** — la alerta tiene que coincidir con el `perfil_origen` de la regla (o la regla tenerlo en NULL).
- **Dentro de la ventana de validez** — la alerta tiene que ser reciente (`ventana_validez_seg`). Una de hace horas ya no cuenta.
- **La condición de la regla se cumple** — el RSI < 30, el estado `fuera`, etc. (la lógica en código).
- **Trading activo** — decide si la orden nace ejecutable o `omitida`.