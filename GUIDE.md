# GUIDE — fundamentos del marco (por qué, y las fronteras)

> Referencia. Se lee una vez. La operación diaria está en `SKILL.md`. Este archivo explica el
> *por qué* de cada regla, para que quien adapte el marco no rompa lo que lo hace funcionar.
> Es **agnóstico de dominio**: sirva el proyecto para software, para preparar materiales, planear
> actividades o investigar. Lo específico de software vive en `modules/software/`.

## El problema que resuelve

Los agentes no recuerdan entre sesiones. Sin disciplina, el contexto se recupera de dos maneras
malas: (a) leyendo montañas de historial cada vez (coste de tokens que crece sin límite), o (b)
reconstruyéndolo desde el diff (frágil, lento, se pierde el *por qué*). En un caso real, el archivo
de "estado" creció a ~53K tokens porque cada cierre *prependía* un resumen sin podar — y se leía
entero al arrancar cada sesión.

## Los seis pilares (agnósticos)

1. **Continuidad.** El objetivo es que cualquier agente retome sin reconstruir contexto.
2. **Estado ≠ historial.** El *estado* (dónde estamos) se **sobrescribe** y vive acotado; el
   *historial* (qué pasó) es **append-only** en archivos que **no se releen** salvo búsqueda puntual.
3. **Un hogar por dato.** Cada hecho vive en UN documento; los demás apuntan, no copian. Nada se
   duplica ni se desincroniza, y siempre sabes dónde buscar (y dónde escribir).
4. **Arranque barato.** El set de apertura es pequeño y fijo *a propósito*; todo lo demás es bajo
   demanda con `grep`.
5. **Decisiones a su hogar.** Lo que perdura se lleva a su documento hogar en el momento en que se
   decide, no solo al historial.
6. **Curación.** Los documentos vivos se **podan**: una entrada obsoleta se borra (su rastro queda
   en el historial). No se acumula.

## Los tres rituales base + dos de ciclo de vida

- **Abrir** (ponerse al día, barato) · **Checkpoint** (dejar el salto en curso a salvo antes de un
  cambio interrumpible) · **Cerrar** (dejar registro durable).
- **Bootstrap** (instanciar el marco en un proyecto) y **Config** (adaptar nombres/parámetros).

Detalle operativo de los cinco: `SKILL.md`.

## Arquitectura: núcleo · módulos · config

El marco es **modular y configurable**, en tres capas:

- **Núcleo** (`core/`) — roles + rituales + principios, agnósticos de dominio. Se define sobre
  **ids de rol estables** (`entry`, `charter`, `protocol`, `state`, `handover`, `index`, `session`),
  no sobre nombres de archivo. Fuente de los roles: `core/roles.md`.
- **Módulos** (`modules/<nombre>/`) — paquetes de roles + disciplinas para un tipo de trabajo. El
  primero es `software` (añade `specs`/`architecture`/`gotchas`/`effort` + convenciones + la regla
  del checkpoint antes del primer archivo de código). Un proyecto no-software simplemente no lo activa.
- **Config** (`stele.config.md`, en la raíz del proyecto) — **fuente única** que enlaza `rol →
  nombre`, activa módulos y fija toggles/presupuestos/wording/idioma y las **tres rutas**. Todo lo
  accionable en tablas. De aquí se **generan** dos derivados: el auto-arranque y el mapa de
  documentación.

**Separar rol de nombre** es lo que hace configurable el marco: los rituales y punteros se expresan
en roles; la config los resuelve a nombres. Renombrar es una operación del ritual `config`, no un
find/replace a ciegas — es segura porque el marco se auto-documenta.

## Las tres rutas: `kit` · `base` · `loader`

La misma separación aplica a las **ubicaciones**. Hay tres, independientes entre sí, todas en la
sección Rutas del manifiesto:

| Ruta | Default | Qué es | Ciclo de vida |
| --- | --- | --- | --- |
| `kit` | `.stele` | El marco vendorizado: `SKILL.md`, `GUIDE.md`, `core/`, `modules/`. | **Reemplazable**: se actualiza borrando y re-vendorizando. |
| `base` | `.` | Los docs instanciados (los roles) y el `history_dir`. | **Tuyo**: crece cada sesión, se versiona, no se regenera jamás. |
| `loader` | `CLAUDE.md` | El auto-arranque en la raíz. | **Derivado**: se regenera desde el manifiesto. |

Por qué son tres parámetros y no uno: **`kit` y `base` tienen ciclos de vida opuestos**. Uno se tira
y se reemplaza entero al actualizar; el otro es el trabajo acumulado del proyecto y no puede perderse
nunca. Confundirlos no es un problema estético de layout — es lo que hace que una actualización del
marco se lleve por delante el historial. De ahí el **invariante duro: `base` nunca dentro de `kit`**
(ni iguales). El caso inverso, `kit` dentro de `base`, es legal pero se avisa: contamina los `grep`
del ritual de apertura con plantillas del marco.

**Modo auto-hospedado** (`kit = .`): el repo del propio marco, donde el kit no se vendoriza sino que
se desarrolla en sitio. Ahí el invariante se relaja — nunca hay un borrado del kit que pueda llevarse
los docs — y `base` es un subdirectorio suyo. Es el único caso en que el kit **es el producto** del
proyecto: los docs de `base` hablan *sobre* el marco, igual que en un proyecto de software hablan
sobre el código, y no se confunden con él.

`loader` es tercero porque el nombre del archivo de auto-arranque depende del **agente**, no del
proyecto: `CLAUDE.md` para Claude Code, `AGENTS.md` para otros, `.github/copilot-instructions.md`
para Copilot. Es la única pieza acoplada a una herramienta concreta, y aislarla mantiene agnóstico
todo lo demás.

**Dos anclas fijas en la raíz** que no siguen a `base`: el `loader` (el agente lo carga solo, sin
indirección) y el manifiesto `stele.config.md` (es el punto desde el que se resuelve todo lo demás,
así que no puede depender de una ruta que él mismo declara).

Combinaciones típicas: `kit = .stele` + `base = .` (default: docs en la raíz, marco invisible) ·
`base = stele` (todo lo del marco junto y visible) · `base = docs` (proyecto con carpeta de docs ya
establecida) · `kit = .claude/skills/stele` (Claude Code: una sola copia del kit, que además queda
disponible como `/stele`).

## Alternativas descartadas (para no volver a proponerlas)

- **Referencia por rol en vivo** (escribir `[state]` en los docs y resolverlo al leer): descartada.
  Markdown no tiene indirección nativa, y el marco es agente-primero: los docs instanciados llevan
  **nombres concretos y legibles**. La indirección se resuelve **una vez**, en `bootstrap`, y
  renombrar es una operación del ritual `config` — el único renombrador sancionado.
- **Roles y módulos definidos por el usuario:** fuera de alcance a propósito. La config llega hasta
  nombres, toggles, presupuestos, wording, idioma y rutas; ampliar el vocabulario de roles rompería
  que el núcleo sea la fuente del mapa derivado.
- **`idioma` no es traducción en runtime.** Dirige qué variante de plantilla se instancia y el
  wording de los rituales, nada más; el contenido del proyecto queda como lo escribió su autor. Los
  ids de rol y los headers `##` del manifiesto son **tokens fijos que no se traducen**, para que el
  parseo nunca dependa del idioma.

## Roles y fronteras (núcleo)

El error más común al adoptar el marco es solapar documentos. Fronteras de los roles del núcleo
(nombres default entre paréntesis; la config puede cambiarlos):

- **`entry` (AGENTS.md) — cómo trabajar.** Proceso, estructura, convenciones operativas, checklists
  (como punteros). Entrada única. Incluye el *mapa de documentación* (generado). NO lleva el detalle
  de otros hogares: apunta.
- **`charter` (DESIGN.md) — por qué el proyecto es así (a gran escala).** Norte, principios,
  restricciones y no-negociables, decisiones estructurales grandes (ADR-lite, fechadas), glosario.
  Estable. Frontera: *principios transversales* aquí; *specs por feature* → módulo.
- **`protocol` (PROTOCOL.md) — formatos.** Cómo se documenta (formatos de cada archivo, topes,
  operaciones de bajo coste). Referencia de formato.
- **`state`/`handover`/`index`/`session` — el historial** (`history_dir`, HISTORY/): estado que se
  sobrescribe, checkpoint del salto en curso, índice y detalle por sesión (se leen con grep).

Los roles que añade un módulo se describen en su `modules/<nombre>/roles.md` (p. ej. software:
`gotchas`, `specs`, `architecture`, `effort`).

## Presupuestos de tamaño (lo que mantiene barato el arranque)

| Rol | Tope objetivo | Al superarlo |
| --- | --- | --- |
| `state` | ~100 líneas | Podar; es estado, no historia |
| `handover` | ~30 líneas | Solo lo del salto activo |
| `charter` | ~200 líneas | Extraer una decisión a un tema del módulo + link |
| `session` | sin tope | Es histórico; se lee con grep, no al arrancar |

Los presupuestos son parámetros de la config (sección Presupuestos); los módulos pueden añadir los
suyos (p. ej. software: sección de `gotchas` por subsistema ~150-200 líneas).

## Por qué la regla dura del checkpoint

Una sesión puede cortarse en cualquier momento (límite de uso, cierre de la herramienta,
intervención del usuario). Si eso pasa con trabajo a medias y `handover` diciendo "sin trabajo
activo", la siguiente sesión reconstruye el contexto desde el diff — caro y con pérdida del *por
qué*. Escribir el checkpoint (~20 líneas) **antes** del cambio interrumpible cuesta siempre menos.
Por eso es regla dura, no un juicio. El módulo software la especializa a "antes del primer archivo
de código"; el núcleo usa el `checkpoint_trigger` genérico configurable.

## Adaptar el marco a un proyecto

- **Bootstrap** instancia el marco (ver `SKILL.md` → ritual BOOTSTRAP). Detecta greenfield vs
  adopción (si ya hay docs, los mapea sin sobrescribir), y **hace eco del layout resuelto — las tres
  rutas — antes de escribir nada**: corregir la interpretación cuesta cero antes del scaffold y caro
  después.
- **Config** cambia nombres, módulos, toggles, presupuestos, wording, idioma y las tres rutas — sin
  romper referencias, porque regenera los derivados.
- **Escala la ceremonia.** `effort` y la numeración `sesion-NNN` son opcionales; un proyecto pequeño
  puede empezar solo con el núcleo e ir sumando.
- **Cura, no acumules.** Los documentos vivos se podan; lo superado ya vive en el historial.
