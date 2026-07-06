# Modelo de negocio
- ## Palabras clave
- 3 Sustantivos
	- Cliente *( id, nombre, telefono )*
	- Servicio *( id, nombre, duracion_min, precio)*
	- Cita *( id, cliente_id, servicio_id, inicio, estado )*
		- estado : ENUM('reservada','cancelada','completada')
- 3 Verbos
	- Crear
	- Mover
	- Cancelar
-
- # Agenda - Calendario
- ## Etapas
- **Esqueleto**: monorepo front/back (como `pokeapi-lab`, salvo que quieras separar) + un `/health` que confirme que Express habla con MariaDB. *Verificas con `curl /health`.*
  logseq.order-list-type:: number
- **Esquema + semilla**: las tres tablas y datos inventados (3-4 clientes, `corte`+`uñas`, 5-6 citas repartidas en una semana). *Verificas con un `SELECT`.*
  logseq.order-list-type:: number
- **Backend solo-lectura**: `GET servicios`, `GET clientes`, `GET citas` de una semana. *Verificas con Postman.*
  logseq.order-list-type:: number
- **Frontend solo-lectura**: calendario semanal que **pinta** las citas leídas. ← Aquí llega el momento "papel → pantalla". Es tu recompensa a mitad de camino, y es a propósito: es el antídoto contra saltar del barco.
  logseq.order-list-type:: number
- **Backend escritura**: `POST cita` (crear), `PATCH cita` (mover = cambiar el inicio), cancelar. *Verificas con Postman.*
  logseq.order-list-type:: number
- **Frontend interactivo**: crear (click en hueco → form), mover, cancelar, contra la API real.
  logseq.order-list-type:: number
- **Pulido del calendario**. Aquí la UI sí es producto.
  logseq.order-list-type:: number
-
-