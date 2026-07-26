# Conectar back

*Terminal 1*
```bash
cd ~/Documents/bot_trading/backend && npm run dev
```

*Terminal 2*
```bash
cloudflared tunnel run bot-trading
```

![[../../_assets/Pasted image 20260725104807.png]]

Son necesarias las 2 aplicaciones y no 1 porque hace falta una primera más específica, que va a ser la que use TradingView a la hora de mandar alerta. Y la segunda, que es para acceder a datos y operar, la creamos después porque es más genérica y así no pisa a esta primera.