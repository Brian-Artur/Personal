# Tramas — Alcance por versión

## v1 — probar la tesis sobre UNA trama, entera
 
 - Objetivo: demostrar "el mapa del dinero público malgastado, traducido a lo que habría pagado" con un caso completo
 - Modelo núcleo: `entidad` + `relacion` + `cita` [hecho]
 - Backend: /salud [hecho] → repository → ruta
 - Tablas de detalle por tipo (`persona`, `empresa`, `caso`…) colgando de `entidad`
 - **Motor coste + equivalencia** (`referencia_coste`: concepto, valor_euros, fuente) ← el diferenciador, al centro
 - Vista grafo relacional (leída de MariaDB)
 - Vista ficha (detalle + citas con fuente)
 - Regla de oro: una sola trama poblada de verdad > diez a medias
 
## v2 — el mapa (headliner), una vez el motor está probado
 
 - Vista mapa estética Google Maps, marcadores de obra/gasto geolocalizado
 - Requiere: coordenadas en entidades/obras + librería de mapa + estilo de tiles + datos de obra pública
 - Es el bloque grande siguiente, por eso va aquí y no en v1 (no por prioridad, por tamaño)

## v3+ — ampliación
 
 - Mapa 3D/aéreo (MapLibre terreno inclinado o 3D Tiles fotorrealistas)
 - Ampliar alcance histórico y nº de tramas (el trabajo de contenido sin fin)
 - Cruces con registros oficiales (tipo BORME/BDNS/PLACSP) si quiero llegar ahí