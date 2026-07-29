# Contrato de cuentas

para cada cuenta en `cuentas`:
  si NO activa → ignorar
  si activa:
    buscar su credencial en el entorno → si falta, MORIR al arrancar
    construir su escucha (con su cuentaId y su key) → arrancar WebSocket
    si además operable:
      construir su ejecutor
  guardar en el mapa: cuentaId → { escucha, ejecutor? }