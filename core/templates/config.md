# stele.config — Configuración del marco

> Fuente única de la configuración de ESTE proyecto. La editas a mano o con el ritual `config`
> (`{{kit}}/SKILL.md`). El **auto-arranque** y el **mapa de documentación** se GENERAN de aquí —
> no los edites por separado. Todo lo accionable va en **tablas**; la prosa solo explica.
>
> **Contrato de parseo:** los headers `##` son secciones canónicas y fijas, en este orden:
> `Meta` · `Rutas` · `Nombres` · `Features` · `Presupuestos` · `Wording de rituales`. Se
> referencian por posición, no por su texto. En cada tabla, col1 = clave, col2 = valor; columnas
> y filas extra se ignoran.
>
> **Centinela de "vacío": un `—` (em-dash, U+2014) en un nombre = rol desactivado.** El nombre del
> carácter va escrito al lado a propósito, para que un barrido que lo sustituya deje una
> contradicción visible en vez de una frase coherente y falsa.
>
> **Y se aceptan también `---` y `-` como el mismo centinela, indefinidamente.** No es tolerancia a
> la chapuza: un agente que escribe el manifiesto **genera ASCII de entrada** y nunca llega a emitir
> el em-dash, así que la defensa de "nombrar el carácter" —que protege contra *sustituirlo*— no hace
> nada contra *no escribirlo nunca*. Observado en campo: un bootstrap emitió `---` en tres valores de
> *Meta*. Aceptar las tres formas **elimina la clase entera**: no se puede corromper convirtiendo a
> ASCII algo que ya vale en ASCII. Emite `—` si puedes; si no, `---` es igual de válido.
>
> **Fila ausente = default del rol/feature** (ver `{{kit}}/core/roles.md` y `{{kit}}/modules/<mód>/`).
> Por eso **un rol inactivo se escribe con el centinela y no se omite**: omitirlo dice *"déjalo en su
> default"*, que es lo contrario de *"está apagado"*, y desde fuera se ven igual.
>
> Al aplicar un cambio, `config` reescribe la tabla afectada **completa** y regenera los derivados.

## Meta

| Parámetro | Valor |
| --- | --- |
| idioma | es |
| módulos | producto |
| persistencia | git |
| persistencia_cmd | — |
| kit_origen | `https://github.com/emezav/stele` |
| remitente | — |
| remitente_publico | — |

> `módulos` admite **tres clases de valor**, y la tercera no es un adorno: una lista de módulos
> activos · `—` (ninguno, **decidido**) · `pendiente` (**falta decidirlo**, típico de un proyecto que
> nace vacío). `—` y `pendiente` producen la misma instancia hoy y significan cosas distintas mañana:
> si los dos se escribieran igual, nadie volvería a plantearse la pregunta. AUDITAR comprueba los
> `pendiente` contra el árbol real. El em-dash `—` aquí significa *ninguno*, igual que en la tabla
> *Nombres* significa *rol desactivado*.
>
> **Alias permanentes.** Un módulo que cambia de nombre **no invalida los manifiestos en campo**:
> `software` se sigue aceptando como el nombre anterior de `producto`, indefinidamente, y no hay que
> migrar nada. Si tu manifiesto lo lleva, déjalo. Mismo precedente que `RICO` en el bloque del loader:
> el kit no rompe un valor que ya está escrito en un archivo que no controla.
>
> `persistencia` = cómo se vuelve durable el trabajo al cerrar: `git` · `ninguna` (los archivos en
> disco son el registro) · `comando` (ejecuta `persistencia_cmd`). **`persistencia_cmd` nunca lleva
> secretos:** este archivo es markdown legible y versionado; las credenciales viven en el entorno o
> en el gestor de la herramienta. Ver `{{kit}}/core/rituals/cerrar.md`, paso 7.
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

> **`loader` es una LISTA, y un valor único sigue siendo válido para siempre** (mismo precedente que
> `RICO` y `módulos = software`). Cada entrada es una **puerta**: un archivo en la raíz con el nombre
> exacto que auto-carga un harness — `CLAUDE.md` en Claude Code, `AGENTS.md` en Codex y la mayoría.
> Todas llevan el mismo bloque y apuntan al mismo `entry`: **una puerta no contiene conocimiento, solo
> qué leer y en qué orden.**
>
> **Por qué varias y no una.** Con una sola, el agente que instala tiene que **adivinar quién abrirá
> el proyecto después**, y un proyecto usado por dos agentes no se puede expresar. Falló en campo: un
> agente instaló el marco perfecto con el nombre equivocado y **al reabrir la carpeta no arrancó**, sin
> error y sin aviso. Con varias, **equivocarse deja de ser fatal**: otra puerta sigue abierta.
>
> **El kit conoce dos; añade la tuya si tu harness lee otro nombre** — esa tabla es conocimiento del
> mundo exterior que el kit no puede verificar, y tú sí sabes cuál es el tuyo.
>
> Tres rutas independientes, todas relativas a la raíz del proyecto y sin `/` final. `kit` es
> maquinaria **reemplazable** (se sustituye entera con el ritual ACTUALIZAR); `base` son tus docs,
> versionados, y nunca se tocan al actualizar. **Invariante duro: `base` nunca puede quedar dentro de
> `kit`.** Ver `{{kit}}/core/reference/rutas-y-tokens.md` → "Las tres rutas", que es donde están los
> **invariantes numerados** y es lo que CONFIG tiene que validar. El *porqué* del modelo está en
> `{{kit}}/guide.md`, en una sección del mismo título: **son dos capas, no dos copias.**

| Ruta | Valor | Qué es |
| --- | --- | --- |
| kit | .stele | El marco vendorizado (`SKILL.md`, `guide.md`, `core/`, `modules/`). |
| base | . | Raíz de los docs instanciados. `.` = raíz del proyecto. |
| loader | CLAUDE.md, AGENTS.md | **Puertas** de auto-arranque, siempre en la raíz. GENERADO. Lista separada por comas. |

## Nombres (rol → archivo)

| Rol | Archivo | Origen |
| --- | --- | --- |
| entry | guia-agente.md | núcleo |
| gotchas | memory.md | núcleo |
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
| specs | requirements.md | producto |
| specs_dir | temas/ | producto |
| architecture | architecture.md | producto |
| effort | esfuerzo.md | producto |

> `artifacts_dir` es el hogar de lo que una sesión produce y **no es documentación**: scripts de un
> solo uso, extracciones de binarios, volcados intermedios. Un subdirectorio por sesión. **No se
> instancia en bootstrap** —lo crea el primer artefacto, y su ausencia significa que ninguna sesión ha
> necesitado producir nada—; `—` lo desactiva del todo.
> **Con `persistencia = git` no se versiona por defecto:** añádelo a tu `.gitignore`. Si prefieres el
> rastro auditable dentro del repo, versiona al menos los artefactos que el `{{session}}` marca como
> sostén de un cambio irreversible, e ignora el resto. Ver `{{kit}}/core/rituals/cerrar.md`.

## Features (toggles)

| Feature | Valor | Origen |
| --- | --- | --- |
| session_greeting | on | núcleo |
| audit_log | on | núcleo |
| audit_every_n_sessions | 10 | núcleo |
| correspondence_log | on | núcleo |
| manual_doc | on | núcleo |
| effort_log | on | producto |
| effort_unit | horas-ingeniero | producto |

> **La columna `Origen` importa: una feature de módulo no puede quedar encendida si su módulo no lo
> está.** `effort_log` enciende el rol `effort`, que lo aporta `producto`; con `módulos = pendiente`
> o `—`, dejarla en `on` promete un log que no existe. Ocurrió en campo: un bootstrap copió
> `effort_log = on` de esta plantilla con el módulo inactivo, y no había `esfuerzo.md` en ninguna
> parte. Al activar el módulo después, `config` las enciende con él.
> La columna es informativa para el parseo —col1 = clave, col2 = valor— y **normativa para quien
> escribe el manifiesto**.
>
> `manual_doc` enciende el `manual`, el único doc dirigido a **la persona** y no al agente. Apágalo si
> quien usa el proyecto es quien lo configuró y conoce el marco; enciéndelo en cuanto lo use alguien
> más — o el propio autor dentro de un año. **Está en `on` por defecto a propósito:** un doc que hay
> que acordarse de activar no existe para quien más lo necesitaba, que es justo quien no sabe que el
> toggle existe.
>
> `audit_every_n_sessions` **no dispara nada**: es el umbral con el que el cierre decide si anota
> "auditoría vencida" en los pendientes de `{{state}}`. Auditar sigue siendo un acto explícito del
> usuario (ver `{{kit}}/core/rituals/auditar.md`). `—` = sin recordatorio. Con `audit_log = off` no hay
> desde cuándo contar y el recordatorio no aplica.
>
> **El número es del usuario, no del marco.** El default 10 es un punto de partida deliberadamente
> bajo: más vale que el aviso llegue pronto y el usuario lo aplace —cuesta una línea en `{{state}}`—
> a que el drift se acumule sin que nadie lo mire. Súbelo o bájalo con el ritual `config` (clase
> *feature*) según lo que cambie tu documentación por sesión.

## Presupuestos

| Doc (rol) | Máx. líneas |
| --- | --- |
| entry | 200 |
| gotchas | 400 |
| state | 100 |
| handover | 50 |
| SKILL.md (enrutador del kit) | 400 |

> **Las cuatro primeras filas son el set de arranque, y en ese orden lo importa la puerta: su suma es
> el precio de cada sesión, y es el único sitio donde podar abarata algo.** De esas cuatro, `state` y
> `handover` llevaban tope desde el principio y `entry` y `gotchas` no llevaban ninguno — **y esas dos
> sin tope eran el 80%** de lo que se carga. Se había acotado lo que se **sobrescribe**, que por
> construcción no crece, y quedó libre lo que se **acumula**.
>
> **`gotchas` lleva tope TOTAL, y es nuevo.** El manual le ponía ~150-200 líneas **por sección** y
> nada al conjunto, así que un doc podía crecer **añadiendo secciones** sin violarlo jamás. Medido en
> el repo del propio marco **el 2026-08-12, antes de podarlo**: **487 líneas en 13 secciones, ninguna
> por encima de 122** —conforme en todas, en todo momento— y aun así **el 64% de todo lo que se cargaba
> al arrancar**.
>
> **El último no es de tu proyecto: es del kit, y está aquí porque faltaba.** El marco ponía tope a lo
> que el adoptante escribe y ninguno a lo que él mismo distribuye. Medido en `27a41bd`: `SKILL.md`
> llegó a **1845 líneas / 36 242 tokens** creciendo de forma monótona —de veinticinco cambios seguidos, ninguno lo
> redujo— mientras el ritual usado **cada** sesión ocupaba 12 líneas y el usado **cada diez** ocupaba
> 615. Al partirlo por ritual bajó a ~366. Si vuelve a pasar de 400, **el sitio correcto de lo que
> entró es un ritual**, no el enrutador.

## Wording de rituales

| Ritual | Parámetro | Texto |
| --- | --- | --- |
| checkpoint | trigger | antes de un cambio interrumpible |
| abrir | saludo | 1-3 líneas: última sesión + estado handover + próximo paso |

> El `trigger` de arriba es el default **agnóstico** del núcleo. Si el módulo `producto` está
> activo, bootstrap escribe aquí su especialización: *antes de la primera edición de código*.
> Un proyecto sin código lo adapta a lo que haga sus veces (ritual `config`, clase *wording*).
