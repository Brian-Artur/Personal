# Alerta Alex

```bash
{
  "Alerta": "Compra/Venta",
  "Par": "JASMY3L-USDT",
  "Order": "sell",
  "Amount": "50$",
  "WALLET": "6",
  "BOT": "TraderPRO",
  "Analisis": "{{exchange}}:{{ticker}}",
  "Precio": "{{close}}",
  "Fecha_Hora": "{{timenow}}",
  "Temporalidad": "{{interval}}"
}
```

```json
{
	"secreto": "secreto_de_brian",
	"perfil": "brian",
	"tipo": "rsi",
	"symbol": "BTCUSDT",
	"valor": 75
}
```


Órdenes directas multiusuario: cuando Brian también use directas, un secreto por propietario (`_DIRECTO_ALEX`, `_DIRECTO_BRIAN`), cada uno mapea a su propietario en el backend. El propietario NUNCA va en el body — se deriva del secreto. Validar cartera contra cuentas.propietario.


```json
{ 
	"secreto": "...", 
	"orden": "buy|sell", 
	"par": "JASMYUSDT", 
	"amount": "50$|50%|50", 
	"cartera": "4" 
}
```


|amount|orden|consulta|a Bybit|
|---|---|---|---|
|`50$`|buy/sell|nada|qty:"50", quoteCoin|
|`50`|buy/sell|nada|qty:"50", baseCoin|
|`50%`|buy|saldo USDT|qty:"<50% USDT>", quoteCoin|
|`50%`|sell|saldo token|qty:"<50% token>", baseCoin|

Cuando metas **órdenes límite** (post-v1 de Alex), cambiar `walletBalance` → `availableToWithdraw` en `BybitConsultor`, porque las límite bloquean fondos y el total dejaría de reflejar lo vendible.

