# {{specs}} — Qué hace el producto (specs, contratos, decisiones por feature)

> **Fuente de verdad del producto**: specs, contratos de API, modelo de datos, decisiones por
> feature, contexto de negocio detallado. Es lo que perdura y comparten todos los agentes y
> sesiones. Frontera: los *principios y apuestas grandes transversales* van en `{{charter}}`;
> los *patrones de código* en `{{architecture}}`; las *trampas de código* en `{{gotchas}}`.
>
> **Estructura:** una sección `§` por decisión o feature, **autocontenida**. Este archivo se lee con
> `grep` y por rango, nunca entero: lo que importa es que cada sección sea corta por sí misma, no el
> tamaño del total. **Umbral de extracción: ~50 líneas** — por encima, la decisión se mueve a un
> archivo de tema (`{{specs_dir}}<TEMA>.md`) y aquí queda un resumen de 2-3 líneas + link. Un tema
> que supera ~600-800 líneas se divide en sub-temas.

## Cómo usar este archivo

- **Antes de implementar**, `grep -n` la sección relevante y lee solo esa parte (no el archivo completo).
- **Toda decisión** de producto/arquitectura/integración/negocio que deba perdurar se documenta
  aquí *en el momento en que se toma* — si no está aquí, es invisible para otros agentes y sesiones.

## § Índice de decisiones

ADAPTAR: una sección por decisión/feature, autocontenida y por debajo de ~50 líneas; con link al
tema cuando la haya superado.

### §1 — <Área / feature>

- **<Decisión / contrato / regla>** — el qué, el porqué y lo que se descartó. Si esta sección pasa
  de ~50 líneas, se extrae: `{{specs_dir}}<TEMA>.md`.

## Modelo de datos

ADAPTAR: entidades clave y relaciones (o link al tema). Identificadores canónicos y qué NO confundir.

## Contratos de API / integración

ADAPTAR: endpoints/convenciones estables (o link al tema por recurso).
