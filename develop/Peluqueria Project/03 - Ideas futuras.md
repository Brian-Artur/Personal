# v-futura: arrastrar citas (drag & drop)

- Reintroduce variante de estado `{ tipo: "moviendo"; id }` en `Seleccion` — descartada en v1 porque el `datetime-local` la hacía innecesaria; con drag sí se justifica.
- Auto-avance de semana: temporizador (`setInterval`) que dispara `setLunesVisible(±7)` mientras el puntero está en la franja lateral; se cancela al volver al centro. Requiere `useEffect` con limpieza del intervalo.
- Casos borde a resolver: soltar fuera de celda válida, sobre otra cita (colisión), fuera de horario, soltar durante el cambio de semana, eventos táctiles.
- Prerrequisito: v1 cerrada con mover funcional vía formulario.