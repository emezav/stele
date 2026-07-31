# stele.config — Configuración del marco

> Fuente única de la configuración de ESTE proyecto. La editas a mano o con el ritual `config`
> (`.stele/SKILL.md`). El **auto-arranque** y el **mapa de documentación** se GENERAN de aquí —
> no los edites por separado. Todo lo accionable va en **tablas**; la prosa solo explica.
>
> **Contrato de parseo:** los headers `##` son secciones canónicas y fijas (se referencian por
> posición, no por su texto). En cada tabla, col1 = clave, col2 = valor; filas extra se ignoran.
> `—` en un nombre = rol desactivado. Fila ausente = default del rol/feature (ver
> `.stele/core/roles.md` y `.stele/modules/<mód>/`). Al aplicar un cambio, `config` reescribe
> la tabla afectada **completa** y regenera los derivados.

## Meta

| Parámetro | Valor |
|---|---|
| idioma  | es |
| base    | . |
| módulos | software |

## Nombres (rol → archivo)

| Rol | Archivo | Origen |
|---|---|---|
| entry        | AGENTS.md      | núcleo |
| charter      | DESIGN.md      | núcleo |
| protocol     | PROTOCOL.md    | núcleo |
| state        | LATEST.md      | núcleo |
| handover     | HANDOVER.md    | núcleo |
| index        | INDEX.md       | núcleo |
| history_dir  | HISTORY/       | núcleo |
| session      | sesion-{NNN}-{YYYY-MM-DD}.md | núcleo |
| gotchas      | MEMORY.md      | software |
| specs        | REQUIREMENTS.md | software |
| architecture | ARCHITECTURE.md | software |
| effort       | ESFUERZO.md    | software |

## Features (toggles)

| Feature | Valor |
|---|---|
| effort_log        | on |
| effort_unit       | horas-ingeniero |
| session_greeting  | on |
| session_numbering | NNN |

## Presupuestos

| Doc (rol) | Máx. líneas |
|---|---|
| state    | 100 |
| handover | 30 |

## Wording de rituales

| Ritual | Parámetro | Texto |
|---|---|---|
| checkpoint | trigger | antes de la primera edición de código |
| abrir      | saludo  | 1-3 líneas: última sesión + estado handover + próximo paso |
