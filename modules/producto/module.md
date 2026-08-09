# modules/producto/module.md — Manifiesto del módulo `producto`

> Un **módulo** empaqueta roles + disciplinas para un tipo de trabajo. Este añade lo que hace falta
> cuando el proyecto tiene un **producto con estructura y decisiones por tema** — un codebase, un
> corpus, una tesis, un kit de plantillas. Se activa con `módulos: [producto]` en `stele.config.md`.
> El núcleo no depende de él; desactivarlo devuelve un marco agnóstico limpio.
>
> **Se llamaba `software` hasta la sesión 58, y `módulos = software` sigue siendo válido para
> siempre** (alias permanente; ver `core/templates/config.md`). El nombre viejo nombraba un **dominio**
> mientras el criterio de activación, escrito aquí abajo desde el principio, habla de un **producto**:
> *"no que haya un compilador"*. Un detector que buscaba `Cargo.toml`/`package.json`/`src/` dejó fuera
> a dos proyectos reales que sí cumplían el criterio — una tesis con su corpus y una carpeta recién
> creada. **El detector se retiró**: ahora BOOTSTRAP pregunta, y AUDITAR vigila los `pendiente`.

## Qué aporta

| Aporta | Detalle |
| --- | --- |
| **Roles** | `specs`, `specs_dir`, `architecture`, `effort` (ver `roles.md`). Ninguno es de arranque: todos son `on-demand` o contenedor. |
| **Templates** | `templates/{specs,architecture,effort}.md` |
| **Features** | enciende `effort_log` (opcional; el rol `effort` depende de este toggle) |
| **Convenciones** | git/test/deploy + reglas de código (ver `conventions.md`) |
| **Regla dura** | *HANDOVER en `EN_PROGRESO` antes de editar el PRIMER archivo de código* (ver abajo) |
| **Detectores** | Lo que el ritual AUDITAR puede comprobar gracias a estos roles (ver abajo) |

## Regla dura: checkpoint antes del primer archivo de código

Antes de tocar el primer archivo de **código** de la sesión, `handover` debe quedar en
`EN_PROGRESO` con objetivo + alcance + verificación prevista + sello. No depende del tamaño estimado del
cambio: una sesión puede cortarse en cualquier momento y el checkpoint (~20 líneas) siempre cuesta
menos que reconstruir el contexto desde el diff. **Exención:** cambios que SOLO tocan el **contenido**
de la documentación — no una **migración estructural** (mover o renombrar docs: rituales CONFIG y
ACTUALIZAR), que es interrumpible aunque no toque código.

Esta regla vive en el módulo (no en el núcleo) porque presupone la noción de "archivo de código".
El núcleo agnóstico usa un `checkpoint_trigger` genérico configurable (`stele.config.md` →
Wording, default *antes de un cambio interrumpible*); este módulo lo especializa a "antes del primer
archivo de código". Un proyecto de software sin código todavía —o cuyo producto no es código— debe
reescribirlo con el ritual `config`, o la exención de documentación deja la regla muerta.

## Qué aporta al ritual AUDITAR

Las clases de drift son del núcleo (`core/rituals/auditar.md`): son propiedades de cualquier
documentación, no de software. Lo que aporta este módulo son **detectores concretos**, porque una
detección necesita saber *qué doc contradice a cuál*, y eso depende de los roles activos:

| Clase | Detector que habilitan estos roles |
| --- | --- |
| 4 — índice desincronizado | Par `specs` ↔ `specs_dir`: cada tema de `specs_dir` tiene entrada en `specs`, y cada entrada apunta a un archivo que existe. Al revés también: una sección de `specs` que ya superó ~50 líneas debería estar extraída |
| 7 — hallazgo sin hogar | Los hogares de promoción que añade el módulo: decisión por feature → `specs`; patrón o mapa del producto → `architecture`. Se suman a los del núcleo (`gotchas`, `charter`), no los sustituyen: por cada sesión del rango, sus *Decisiones* deben tener eco en alguno |
| 8 — crecimiento sin revisión | Los topes del módulo: tema de `specs` ~600-800 líneas |
| 2 y 6 — estado obsoleto y bloqueo | `specs` es donde viven las fases y las preguntas abiertas por feature, y por eso es el doc que más rápido caduca cuando el producto avanza |
| 1 — afirmación caducada | Patrones comprobables propios de software, para el detector de entorno del núcleo: puertos, unidades de servicio (`\b[a-z0-9_-]+\.service\b`), nombres de contenedor e imágenes. **Solo si el proyecto opera servicios en red** (ver abajo). Valen las cuatro cautelas del núcleo, empezando por que un candidato extraído es un **recorte** de la afirmación |

### Números contra formas: por qué el filtro de puertos es distinto

**Una ruta tiene forma; un número solo tiene contexto.** Por eso el filtro de plausibilidad de rutas
—contrastar contra raíces reales— descarta el 80% sin equivocarse, y el equivalente para puertos **no
existe**: ningún rango numérico distingue un puerto de un año, de un recuento de líneas, de una norma
(`8601`) o de un identificador. La documentación técnica está llena de números en el rango de puertos.

Así que **el filtro de verdad es el ancla léxica del propio barrido**, no el rango:

```bash
# el ancla es lo que filtra: dos puntos PEGADOS a los digitos, o la palabra delante
grep -rhoE "\b(puerto |port |:)[0-9]{2,5}\b" {base} --include="*.md"
```

Con tres avisos, porque el ancla tampoco es gratis:

- **`:` pegado a dígitos casa con `archivo:línea`** (`SKILL.md:401`), con **marcas de tiempo**
  (`10:36`), con fragmentos IPv6 y con identificadores de dispositivo. Dos reglas baratas: si el token
  de la izquierda parece un archivo, no es un puerto; y **si a la izquierda hay uno o dos dígitos y a
  la derecha exactamente dos, es una hora**.
- **Cuál de esas poblaciones domina depende de TU corpus, no del patrón.** Medido en dos árboles: en
  uno las horas eran el 57% y `archivo:línea` el 5%; en el otro, al revés (93% referencias, horas
  anecdóticas). Cuenta la tuya antes de decidir qué excluyes.
- **El rango `<1024` sigue sirviendo, pero como segundo filtro, no como el principal.** Documentarlo
  al revés —que es lo que hacía este módulo— vende como filtro lo que apenas descarta nada.

### El ancla es ciega en tablas, y ahí es donde viven los puertos

**El ancla compra precisión y paga con recall, y paga donde más duele.** Una URL en prosa arrastra
`host:puerto`; una fila de inventario lleva `| 8080 |` y nada más. Así que el patrón anclado **funciona
peor cuanto mejor estructurado está el documento** — y este detector existe para auditar documentos
operativos, que tienden a ser tablas. Caso de campo: **tres puertos reales, documentados y
verificables, sin ni una sola mención con ancla** en 254 documentos — solo aparecían como número
desnudo en celdas de inventario. Se cita así, en absoluto y no como fracción, a propósito: el
denominador de aquel caso salía del propio barrido, así que servía para exhibir el fallo pero no para
cuantificarlo.

Por eso hay una segunda estrategia, para el mismo valor y otro tipo de documento:

```bash
# un numero solo en una celda: es una afirmacion tabular
grep -rhoE '\| *[0-9]{4,5} *\|' {base} --include="*.md"
```

Midió **0 falsos** en campo, y no por suerte: una celda que contiene **solo** un número no tiene sitio
donde esconder un año o una norma. Las dos son complementarias —el ancla coge la prosa, la celda coge
las tablas— y juntas dieron 8 de 10 con ruido casi nulo.

**Esto no vale para rutas.** El patrón de rutas es de **forma**, no de ancla, así que una tabla no le
quita nada. El problema es específico de los valores **sin forma**: necesitan contexto, y estructurar
un documento es precisamente quitarle contexto.

### Cuándo NO aplica este detector

Los patrones de puertos, servicios y contenedores son de **proyectos que operan servicios en red**, no
de "software" en general. Una librería, un compilador o un paquete de análisis no tienen nada que
comprobar aquí y solo arrastrarían falsos. Es más estrecho que el módulo, y por eso va condicionado en
vez de en un módulo aparte: **un módulo es un paquete de roles**, y esto no aporta ninguno.

El hogar de promoción es lo que hace detectable la clase 7 —la más valiosa y la más invisible— y es
justo lo que un proyecto sin este módulo tiene más estrecho: sin `specs` ni `architecture`, un
hallazgo sobre el **producto** solo puede promoverse a `entry` o a `charter`, y el detector se queda
en eso. Las trampas de trabajo sí tienen hogar sin el módulo —`gotchas` es del núcleo—, y ese fue
precisamente el arreglo de la sesión 58.

## Cuando el producto no es código

Los roles de este módulo (`specs`, `architecture`) describen el **objeto del trabajo**; los del
núcleo describen el **proceso de continuidad**. Por eso viven aquí. El criterio para activar el
módulo es que el proyecto tenga un **producto con estructura y decisiones por feature** — no que haya
un compilador. Un kit de documentación, un corpus curado, una tesis con su corpus o una colección de
plantillas encajan igual que un codebase: `architecture` mapea la estructura de ese producto y
`specs` sus decisiones.

**`gotchas` estuvo aquí y se fue al núcleo en la sesión 58**, por este mismo criterio aplicado bien:
las trampas que acababa guardando —del entorno, de publicar, de verificar, de dónde guardar— son
**proceso**, no objeto. La descripción del rol decía *"al leer el código"* y lo que contenía no lo era.
Un proyecto de campo perdió una trampa real por eso: vivía en el `handover`, que se poda por diseño.

Lo que sí hay que ajustar en ese caso es el `checkpoint_trigger` (ver arriba): si el producto **es**
documentación, la exención por "solo contenido de documentación" eximiría al proyecto entero y
dejaría la regla dura muerta. Especialízalo a lo que hace las veces de código.

**Escribir código no es lo que decide activar el módulo.** Un proyecto de documentación pura genera
scripts en cuanto hay volumen: inventariar, emparejar, comparar dos fuentes, mover veinte archivos.
Eso es **herramienta para manipular el proyecto**, no producto, y su hogar es `artifacts_dir` — no
convierte el proyecto en software ni pide estos roles. El criterio sigue siendo el de arriba: un
producto con estructura y decisiones por feature.

## Defaults que aporta al manifiesto

```text
módulos: [software]
features:  effort_log = on
nombres:   specs=requirements.md  specs_dir=temas/
           architecture=architecture.md  effort=esfuerzo.md
wording:   checkpoint.trigger = antes de la primera edición de código
```

## Al desactivarlo

`config` retira sus filas del mapa, pero **no borra** los docs `specs`/`architecture`/`effort`
(quedan huérfanos preservados + aviso). El usuario decide archivarlos. Ninguno era de arranque, así
que la lista de lectura no cambia — `gotchas`, que sí lo es, no pertenece a este módulo.
El `checkpoint_trigger` vuelve al default genérico del núcleo.
