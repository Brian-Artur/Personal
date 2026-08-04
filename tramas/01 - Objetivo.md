# Tramas — Giro de objetivo (2026-07)
  ## El giro
 - DE: herramienta de **consulta** (como entramado.org) — para quien ya quiere investigar y sabe leer un sumario
 - A: herramienta de **impacto** — para quien pasaba por ahí y hay que hacerle *sentir* la gravedad
 - Disparador: descubrí entramado.org, ya hace el "grafo de consulta" mejor de lo que yo llegaría en meses
 - Conclusión: no compito con su producto; construyo otro con tesis distinta
 
## Qué se mantiene (modelo ya validado contra entramado.org)
 - Convergí solo, razonando desde el problema, con casi todas sus decisiones → señal de que el modelo es el correcto
 - `entidad` tipada + `relacion` (arista con `estado`) + `cita` colgando de la arista
 - `estado` (confirmada/probable/rumor) ≠ `situacion_procesal` — dos ejes independientes
 - Todo enlazado y atribuido, nunca reproducir texto ajeno
 - MariaDB relacional (NO Neo4j) — su grafo no me hace falta hasta que "caminos de N saltos" sea el core; hoy no lo es
  
## Diferenciadores (el foso frente a entramado.org)

### Ingeniería (finita — esto SÍ es trabajo de v1)
- **Coste → equivalencia**: sobrecoste en € traducido a algo con cuerpo ("200 M€ = un hospital de 200 camas")
  - es la idea más fuerte del proyecto; es lo que evoca sentimiento donde el grafo no llega
  - encaja en el modelo sin romperlo: coste = atributo de relación/caso; equivalencias = tabla aparte
- **Mapa del dinero**: geolocalizar OBRA PÚBLICA y gasto, no personas

### Contenido/alcance (infinito — PELIGRO de front-loading, va diferido)
- Ampliar la historia más atrás (no solo las 2 legislaturas de Sánchez)
- Más entidades, más tramas
- ⚠️ Esto no es ingeniería, es curro de investigación sin fin. Es lo que me haría abandonar. v1 NO amplía alcance: elige UNA trama y la cierra entera.
  
## Regla actualizada (seguridad + legal)
 - El nuevo objetivo ("percibirlos cercanos a su casa") empuja hacia terreno peligroso si el mapa señala personas
 - Mantengo: ciudad/barrio, nunca la casa concreta
 - REFUERZO: el mapa va sobre **obra y dinero público** (vías, hospitales, contratos), NO sobre domicilios de personas
 - Mismo golpe emocional de cercanía, sin señalar el pueblo de nadie, sobre hechos documentados