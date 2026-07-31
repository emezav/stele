# {{specs}} — Qué hace el producto (specs, contratos, decisiones por feature)

> **Fuente de verdad del producto**: specs, contratos de API, modelo de datos, decisiones por
> feature, contexto de negocio detallado. Es lo que perdura y comparten todos los agentes y
> sesiones. Frontera: los *principios y apuestas grandes transversales* van en `{{charter}}`;
> los *patrones de código* en `{{architecture}}`; las *trampas de código* en `{{gotchas}}`.
>
> **Estructura:** el índice (`§`) lleva entradas ≤3 líneas + link. Cuando el detalle de una
> decisión supera ~50 líneas, se extrae a un archivo de tema (p. ej. `{{specs_dir}}/<TEMA>.md`) y
> aquí queda el resumen + link. Un tema que supera ~600-800 líneas se divide en sub-temas.

## Cómo usar este archivo
- **Antes de implementar**, `grep -n` la sección relevante y lee solo esa parte (no el archivo completo).
- **Toda decisión** de producto/arquitectura/integración/negocio que deba perdurar se documenta
  aquí *en el momento en que se toma* — si no está aquí, es invisible para otros agentes y sesiones.

## § Índice de decisiones
ADAPTAR: una entrada por decisión/feature, ≤3 líneas, con link al tema cuando aplique.

### §1 — <Área / feature>
- **<Decisión / contrato / regla>** — resumen ≤3 líneas. Detalle: `{{specs_dir}}/<TEMA>.md`.

## Modelo de datos
ADAPTAR: entidades clave y relaciones (o link al tema). Identificadores canónicos y qué NO confundir.

## Contratos de API / integración
ADAPTAR: endpoints/convenciones estables (o link al tema por recurso).
