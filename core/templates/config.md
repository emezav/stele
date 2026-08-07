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
| kit_origen | `https://github.com/emezav/stele` |
| remitente | — |
| remitente_publico | — |

> `persistencia` = cómo se vuelve durable el trabajo al cerrar: `git` · `ninguna` (los archivos en
> disco son el registro) · `comando` (ejecuta `persistencia_cmd`). **`persistencia_cmd` nunca lleva
> secretos:** este archivo es markdown legible y versionado; las credenciales viven en el entorno o
> en el gestor de la herramienta. Ver `{{kit}}/SKILL.md` → CERRAR, paso 7.
>
> `remitente` = con qué nombre firma este proyecto la **correspondencia privada**, y lo que permite
> reconocer lo que va dirigido a él. **Elegido, no derivado:** un valor calculado de la carpeta o la
> ruta rompe el rastro al renombrar **y no es anónimo** —el espacio de búsqueda de una ruta es
> diminuto—. Lo propone el agente y **lo aprueba el usuario**, que es el único que sabe qué le
> identifica en su contexto. **Identifica, no autentica**, y por tanto **nunca es un secreto**: si
> alguien lo convierte en prueba de identidad, pasa a ser una credencial y no puede vivir aquí. `—` =
> este proyecto no firma ni siquiera en privado.
>
> `remitente_publico` = con qué nombre puede aparecer en un **buzón público**, que es un canal
> distinto y una decisión distinta. **`—` (default) = anónimo: no se publica su nombre.** Son dos
> claves porque son **dos trabajos incompatibles** —identificar ante un corresponsal concreto, donde
> conviene ser reconocible; y protegerse en un canal que lee cualquiera, donde conviene no serlo— y una
> sola no puede hacer los dos. Caso real: una organización que firma en privado con su nombre real,
> a propósito, para que el historial se le acumule entre varios proyectos suyos, y que **no quiere ese
> nombre publicado**. Con un solo campo, ese proyecto tendría que elegir entre atribución y privacidad.
>
> El default es el seguro: **en la duda, anónimo**. Autorizar se puede después; despublicar, no.
>
> `kit_origen` = de dónde se vendorizó el kit. Lo escribe `bootstrap` y lo lee el ritual ACTUALIZAR
> para traer la versión nueva; sin él, actualizar se bloquea. **No es un número de versión** — eso se
> descartó por derivable (`{{kit}}/guide.md` → Alternativas descartadas): la procedencia es lo
> contrario, no hay nada en el árbol de donde deducirla, y el `README` del kit no sirve porque
> apuntaría al upstream aunque tú vendorizaras de un fork. `—` si el kit no se vendoriza (modo
> auto-hospedado). Es una URL o ruta pública: **nunca** con credenciales embebidas.

## Rutas

> Tres rutas independientes, todas relativas a la raíz del proyecto y sin `/` final. `kit` es
> maquinaria **reemplazable** (se sustituye entera con el ritual ACTUALIZAR); `base` son tus docs,
> versionados, y nunca se tocan al actualizar. **Invariante duro: `base` nunca puede quedar dentro de
> `kit`.** Ver `{{kit}}/guide.md` → "Las tres rutas".

| Ruta | Valor | Qué es |
| --- | --- | --- |
| kit | .stele | El marco vendorizado (`SKILL.md`, `guide.md`, `core/`, `modules/`). |
| base | . | Raíz de los docs instanciados. `.` = raíz del proyecto. |
| loader | CLAUDE.md | Loader de auto-arranque, siempre en la raíz. GENERADO. |

## Nombres (rol → archivo)

| Rol | Archivo | Origen |
| --- | --- | --- |
| entry | AGENTS.md | núcleo |
| charter | design.md | núcleo |
| manual | manual.md | núcleo |
| protocol | protocol.md | núcleo |
| state | latest.md | núcleo |
| handover | handover.md | núcleo |
| index | index.md | núcleo |
| history_dir | history/ | núcleo |
| session | sesion-{NNN}-{YYYY-MM-DD}.md | núcleo |
| audit | audit.md | núcleo |
| correspondence | cartas.md | núcleo |
| letter | carta-{NNN}-{YYYY-MM-DD}.md | núcleo |
| correspondence_dir | correspondencia/ | núcleo |
| artifacts_dir | artefactos/ | núcleo |
| gotchas | memory.md | software |
| specs | requirements.md | software |
| specs_dir | temas/ | software |
| architecture | architecture.md | software |
| effort | esfuerzo.md | software |

> `artifacts_dir` es el hogar de lo que una sesión produce y **no es documentación**: scripts de un
> solo uso, extracciones de binarios, volcados intermedios. Un subdirectorio por sesión. **No se
> instancia en bootstrap** —lo crea el primer artefacto, y su ausencia significa que ninguna sesión ha
> necesitado producir nada—; `—` lo desactiva del todo.
> **Con `persistencia = git` no se versiona por defecto:** añádelo a tu `.gitignore`. Si prefieres el
> rastro auditable dentro del repo, versiona al menos los artefactos que el `{{session}}` marca como
> sostén de un cambio irreversible, e ignora el resto. Ver `{{kit}}/SKILL.md` → CERRAR.

## Features (toggles)

| Feature | Valor |
| --- | --- |
| effort_log | on |
| effort_unit | horas-ingeniero |
| session_greeting | on |
| audit_log | on |
| audit_every_n_sessions | 10 |
| correspondence_log | on |
| manual_doc | on |

> `manual_doc` enciende el `manual`, el único doc dirigido a **la persona** y no al agente. Apágalo si
> quien usa el proyecto es quien lo configuró y conoce el marco; enciéndelo en cuanto lo use alguien
> más — o el propio autor dentro de un año. **Está en `on` por defecto a propósito:** un doc que hay
> que acordarse de activar no existe para quien más lo necesitaba, que es justo quien no sabe que el
> toggle existe.
>
> `audit_every_n_sessions` **no dispara nada**: es el umbral con el que el cierre decide si anota
> "auditoría vencida" en los pendientes de `{{state}}`. Auditar sigue siendo un acto explícito del
> usuario (ver `{{kit}}/SKILL.md` → AUDITAR). `—` = sin recordatorio. Con `audit_log = off` no hay
> desde cuándo contar y el recordatorio no aplica.
>
> **El número es del usuario, no del marco.** El default 10 es un punto de partida deliberadamente
> bajo: más vale que el aviso llegue pronto y el usuario lo aplace —cuesta una línea en `{{state}}`—
> a que el drift se acumule sin que nadie lo mire. Súbelo o bájalo con el ritual `config` (clase
> *feature*) según lo que cambie tu documentación por sesión.

## Presupuestos

| Doc (rol) | Máx. líneas |
| --- | --- |
| state | 100 |
| handover | 50 |

## Wording de rituales

| Ritual | Parámetro | Texto |
| --- | --- | --- |
| checkpoint | trigger | antes de un cambio interrumpible |
| abrir | saludo | 1-3 líneas: última sesión + estado handover + próximo paso |

> El `trigger` de arriba es el default **agnóstico** del núcleo. Si el módulo `software` está
> activo, bootstrap escribe aquí su especialización: *antes de la primera edición de código*.
> Un proyecto sin código lo adapta a lo que haga sus veces (ritual `config`, clase *wording*).
