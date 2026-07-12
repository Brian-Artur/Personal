# V1
	- TradingView
	- CloudFlare Tunnel (IP pública - webhook)
	- Hostinger (dominio)
	- Windows 24/7
		- React
		- Node + Express
		- MariaDB
			- `alertas` : lo que llega de TradingView (registro histórico)
			  logseq.order-list-type:: number
			  Es un log de solo-añadir. *Un registro de los que está pasando fuera*
				- `id` — clave, autoincremental
				- `tipo` — qué es (rsi, precio, cruce_linea, dominancia…)
				- `valor` — el número (62.5, 64000…)
				- `symbol` — la moneda (BTCUSDT)
				- `direccion` — opcional, para cruces de línea (arriba/abajo)
				- `payload` — el JSON crudo tal cual llegó (JSON), para auditoría y depuración
				- `recibida_en` — marca de tiempo
			- `reglas` — los parámetros de cada estrategia
			  logseq.order-list-type:: number
				- `id` — clave
				- `nombre` — identificador que liga esta fila con su archivo de código (p. ej. `rsi_reversion`)
				- `activa` — encendida/apagada (booleano)
				- `modo` — auto / semiautomatico
				- `symbol` — sobre qué opera
				- `qty` — tamaño de la operación
				- `ventana_validez_seg` — cuán recientes deben ser las condiciones para valer
				- `confirmacion_expira_seg` — cuánto espera una propuesta semiautomática antes de caducar
				- `estado_actual` — fuera / en_largo / en_corto
				- `actualizada_en`
			- `ordenes` — el intento de ejecución en Bybit (el corazón)
			  logseq.order-list-type:: number
				- `id` — clave
				- `regla_id` — qué regla la originó (FK a `reglas`)
				- `order_link_id` — tu identificador de cliente, **ÚNICO** (idempotencia: si reintentas tras un corte, Bybit deduplica)
				- `bybit_order_id` — el id que devuelve Bybit (nulo hasta que responde)
				- `symbol`, `side` (buy/sell), `tipo_orden` (market/limit)
				- `qty` — cantidad
				- `precio` — nulo en market, obligatorio en limit
				- `estado` — el estado de la máquina (ahora lo detallo)
				- `filled_qty` — cuánto se ha llenado (esto cubre los llenados parciales sin necesidad de un estado aparte)
				- `avg_price` — precio medio de ejecución
				- `motivo` — texto nulo, para guardar el porqué de un rechazo o error (oro para depurar)
				- `confirmacion_expira_en` — marca de tiempo, solo para semiautomáticas en espera
				- `creada_en`, `actualizada_en`
			- `posiciones` — espejo de Bybit (lo abierto ahora mismo)**
			  logseq.order-list-type:: number
				- `symbol` — único (en modo one-way, una posición por símbolo)
				  logseq.order-list-type:: number
				- `side` (long/short), `size`, `entry_price`, `unrealized_pnl`
				  logseq.order-list-type:: number
				- `actualizada_en`
				  logseq.order-list-type:: number
			- `saldo` — espejo de Bybit (por moneda)
			  logseq.order-list-type:: number
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
				- `clave` — clave primaria (p. ej. `trading_activo`)
				  logseq.order-list-type:: number
				- `valor` — el valor (`true`)
				  logseq.order-list-type:: number
				- `actualizado_en`
				  logseq.order-list-type:: number
	- Bybit (operar)
	-
-
- # V2
	- Análisis - Python
		- CCXT
			- Histórico : OHLCV , varias temporalidades
			- Exchanges : Bybit, KuCoin y Binance
		- Pasar datos a Pandas
			- Transformar datos para incormporar a tabla de MariaDB
-
- # V3
	- React para ver los históricos de Pandas
-
- # V4
	- ML : SciKit-Learn
		- Unas **características** (features): los números que describen la situación.
		- Una **etiqueta** (target): lo que quieres predecir.
			- Coluna de si/no
	- > Cuidado con los datos futuros
- # V5
	- Definir reglas-estrategias, desde la interfaz
- ![image.png](../assets/image_1783842207261_0.png)