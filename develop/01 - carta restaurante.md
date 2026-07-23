# **Proyecto futuro: carta digital de restaurante (alérgenos derivados)**

- Núcleo v1: carta filtrable donde alérgenos y etiquetas (vegano / vegetariano ovo-lácteo) se _derivan_ de los ingredientes del plato, no se mantienen a mano
    - Modelo: `ingrediente` → alérgenos; `plato` → ingredientes; alérgenos/dietas del plato = calculados
    - Plato derivado hereda ingredientes del original salvo cambios explícitos
- Diferido a v2 (ERP): coste por plato, consumo de almacén, sugerencia de pedido a proveedor
- Diferido a v3: LLM "maître" que sugiere platos (best-sellers, menú del día)
- Nota: estructuralmente rima con peluquería → mini-ERP (recurso/servicio ≈ ingrediente/plato). No es un desvío, es la misma trayectoria de skills