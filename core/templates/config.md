# stele.config — Configuración del marco

> Fuente única de la configuración de ESTE proyecto. La editas a mano o con el ritual `config`
> (`{{kit}}/SKILL.md`). El **auto-arranque** y el **mapa de documentación** se GENERAN de aquí —
> no los edites por separado. Todo lo accionable va en **tablas**; la prosa solo explica.
>
> **Contrato de parseo:** los headers `##` son secciones canónicas y fijas, en este orden:
> `Meta` · `Rutas` · `Nombres` · `Features` · `Presupuestos` · `Wording de rituales`. Se
> referencian por posición, no por su texto. En cada tabla, col1 = clave, col2 = valor; columnas
> y filas extra se ignoran. `—` en un nombre = rol desactivado. Fila ausente = default del
> rol/feature (ver `{{kit}}/core/roles.md` y `{{kit}}/modules/<mód>/`). Al aplicar un cambio,
> `config` reescribe la tabla afectada **completa** y regenera los derivados.

## Meta

| Parámetro | Valor |
| --- | --- |
| idioma | es |
| módulos | software |
| persistencia | git |
| persistencia_cmd | — |

> `persistencia` = cómo se vuelve durable el trabajo al cerrar: `git` · `ninguna` (los archivos en
> disco son el registro) · `comando` (ejecuta `persistencia_cmd`). **`persistencia_cmd` nunca lleva
> secretos:** este archivo es markdown legible y versionado; las credenciales viven en el entorno o
> en el gestor de la herramienta. Ver `{{kit}}/SKILL.md` → CERRAR, paso 7.

## Rutas

> Tres rutas independientes, todas relativas a la raíz del proyecto y sin `/` final. `kit` es
> maquinaria **reemplazable** (se sustituye entera con el ritual ACTUALIZAR); `base` son tus docs,
> versionados, y nunca se tocan al actualizar. **Invariante duro: `base` nunca puede quedar dentro de
> `kit`.** Ver `{{kit}}/GUIDE.md` → "Las tres rutas".

| Ruta | Valor | Qué es |
| --- | --- | --- |
| kit | .stele | El marco vendorizado (`SKILL.md`, `GUIDE.md`, `core/`, `modules/`). |
| base | . | Raíz de los docs instanciados. `.` = raíz del proyecto. |
| loader | CLAUDE.md | Loader de auto-arranque, siempre en la raíz. GENERADO. |

## Nombres (rol → archivo)

| Rol | Archivo | Origen |
| --- | --- | --- |
| entry | AGENTS.md | núcleo |
| charter | DESIGN.md | núcleo |
| protocol | PROTOCOL.md | núcleo |
| state | LATEST.md | núcleo |
| handover | HANDOVER.md | núcleo |
| index | INDEX.md | núcleo |
| history_dir | HISTORY/ | núcleo |
| session | sesion-{NNN}-{YYYY-MM-DD}.md | núcleo |
| gotchas | MEMORY.md | software |
| specs | REQUIREMENTS.md | software |
| specs_dir | temas/ | software |
| architecture | ARCHITECTURE.md | software |
| effort | ESFUERZO.md | software |

## Features (toggles)

| Feature | Valor |
| --- | --- |
| effort_log | on |
| effort_unit | horas-ingeniero |
| session_greeting | on |

## Presupuestos

| Doc (rol) | Máx. líneas |
| --- | --- |
| state | 100 |
| handover | 30 |

## Wording de rituales

| Ritual | Parámetro | Texto |
| --- | --- | --- |
| checkpoint | trigger | antes de un cambio interrumpible |
| abrir | saludo | 1-3 líneas: última sesión + estado handover + próximo paso |

> El `trigger` de arriba es el default **agnóstico** del núcleo. Si el módulo `software` está
> activo, bootstrap escribe aquí su especialización: *antes de la primera edición de código*.
> Un proyecto sin código lo adapta a lo que haga sus veces (ritual `config`, clase *wording*).
