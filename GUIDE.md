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
  nombre`, activa módulos y fija toggles/presupuestos/wording/idioma/`base`. Todo lo accionable en
  tablas. De aquí se **generan** dos derivados: el auto-arranque y el mapa de documentación.

**Separar rol de nombre** es lo que hace configurable el marco: los rituales y punteros se expresan
en roles; la config los resuelve a nombres. Renombrar es una operación del ritual `config`, no un
find/replace a ciegas — es segura porque el marco se auto-documenta.

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
|---|---|---|
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
  adopción (si ya hay docs, los mapea sin sobrescribir).
- **Config** cambia nombres, módulos, toggles, presupuestos, wording, idioma y `base` (raíz o
  subdirectorio, visible u oculto) — sin romper referencias, porque regenera los derivados.
- **Escala la ceremonia.** `effort` y la numeración `sesion-NNN` son opcionales; un proyecto pequeño
  puede empezar solo con el núcleo e ir sumando.
- **Cura, no acumules.** Los documentos vivos se podan; lo superado ya vive en el historial.
