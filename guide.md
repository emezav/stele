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

## Tres rituales base + tres de ciclo de vida + uno de verificación + dos de correspondencia

- **Abrir** (ponerse al día, barato) · **Checkpoint** (dejar el salto en curso a salvo antes de un
  cambio interrumpible) · **Cerrar** (dejar registro durable).
- **Bootstrap** (instanciar el marco en un proyecto), **Actualizar** (traer una versión nueva del kit
  y reconciliar la instancia) y **Config** (adaptar nombres/parámetros).
- **Auditar** (verificar hacia dentro que lo escrito sigue siendo cierto).
- **Contrastar** (recibir de fuera un informe sobre tu trabajo y decidir qué entra) y **Remitir**
  (escribir hacia fuera lo que encontraste y no es tuyo).

Detalle operativo de los nueve: `SKILL.md`.

**Por qué el séptimo existe.** Los seis primeros se reparten en dos grupos —los que **escriben**
documentación y los que mantienen el **marco**— y ninguno de los dos re-verifica el **contenido** ya
escrito: `abrir` lee poco a propósito, `cerrar` escribe el estado nuevo sin releer el viejo, y
`config`/`actualizar` tocan el manifiesto y la maquinaria, no lo que dicen los docs. El hueco solo se
nota con el uso prolongado, y lo que crece dentro es **drift**: afirmaciones que fueron ciertas y
dejaron de serlo. **Un dato obsoleto se lee como hecho**, así que documentación derivada es peor que
documentación ausente — la ausencia se nota, el drift no.

Auditar es, en la práctica, el ritual de dos pilares que hasta ahora solo eran buenas intenciones: la
**curación** (pilar 6) y **un hogar por dato** (pilar 3). El fallo más caro que encuentra no es una
frase falsa sino una **ausencia**: conocimiento que se quedó en un registro de sesión y nunca se
promovió al doc que se lee al abrir. Eso no se ve leyendo ningún documento; solo aparece contrastando
dos. Por eso no puede ser un efecto secundario de otro ritual, y por eso **se invoca**: auditar es
caro por naturaleza, y meterlo en `abrir` rompería el pilar 4 (arranque barato).

**Por qué existen los dos de correspondencia.** Mismo test, otra respuesta. Los siete anteriores miran
**hacia dentro**: escriben la documentación del proyecto, mantienen su marco, o re-verifican lo ya
escrito. Ninguno maneja el trato con **el exterior**. Visto como flujo, la asimetría salta:
*actualizar* es cómo un cambio del kit **baja** hasta quien lo usa, y no existía el camino de
**vuelta** — cómo un hallazgo de quien lo usa **sube**. Y no es cosa de kits: cualquier proyecto recibe
informes sobre su trabajo (la revisión de un director, los resultados de un socio, la evaluación de un
curso) y encuentra cosas que pertenecen a otro.

**Contrastar** es la vía de entrada de **mayor consecuencia** que tiene un proyecto: lo que pasa por
ahí se incorpora al producto y viaja a todo el que lo use. Un mal audit cuesta una tarde; un informe
mal integrado se publica. Por eso su regla central no es *aceptar* sino **contrastar**: el diagnóstico
de quien reporta viaja —tiene el caso, que tú no ves—, pero el remedio hay que volver a derivarlo desde
tu diseño —que él no ve—.

**Remitir** es el espejo, y su parte difícil no es escribir sino **darse cuenta**. El disparador sale
de generalizar la clase 7 de auditar: una trampa que estás a punto de archivar en tus gotchas y que en
realidad habla de *la herramienta que usas* no tiene su hogar en tu repo — ahí no arregla nada.

**El canal se queda fuera del marco, y es una decisión, no un olvido.** El kit estandariza **la
carta**, nunca el transporte: así viaja por correo, por un issue, por un PR, o pegada en la sesión de
otro agente, y **copiar y pegar es el suelo que siempre funciona**, sin pedirle a nadie que aprenda
git. Hay una excepción que no costó nada: como *actualizar* ya se trae el árbol entero del kit, un
buzón dentro del kit **baja solo** con la actualización. La bajada era gratis; la subida necesita
cartero.

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

**La frontera núcleo/módulo** es una sola pregunta: el núcleo modela el **proceso de continuidad**
(dónde estamos, qué pasó, qué quedó a medias — agnóstico); un módulo modela el **objeto del trabajo**
(el codebase, el corpus, el plan). Por eso `specs`, `architecture` y `gotchas` son roles de módulo
aunque suenen universales: un proyecto de planeación los dejaría vacíos, y un documento vacío es
peso muerto en cada adopción. El corolario práctico es que activar `software` no exige un
compilador, sino un producto con estructura y decisiones por feature — un kit de documentación
también lo es (ver `modules/software/module.md`).

**Separar rol de nombre** es lo que hace configurable el marco: los rituales y punteros se expresan
en roles; la config los resuelve a nombres. Renombrar es una operación del ritual `config`, no un
find/replace a ciegas — es segura porque el marco se auto-documenta.

La misma separación, aplicada una tercera vez, resuelve un problema distinto: **el vocabulario del
marco no es el vocabulario de la conversación**. Los ids, las claves del manifiesto y los estados se
*parsean*, así que son contrato y no se traducen; pero el saludo, los ecos y los informes no los
consume ninguna máquina. Ahí el agente habla **en llano y nombra el archivo** (`SKILL.md` → "Cómo se
le habla al usuario"). No hace falta un parámetro: como el nombre del archivo va siempre entre
paréntesis, un usuario técnico no pierde precisión y uno que no lo es no tiene que aprenderse
`handover` para saber que quedó trabajo a medias. Lo que la regla **no** permite es suavizar el
hecho: si algo quedó a medias o dos docs se contradicen, se dice.

## Las tres rutas: `kit` · `base` · `loader`

La misma separación aplica a las **ubicaciones**. Hay tres, independientes entre sí, todas en la
sección Rutas del manifiesto:

| Ruta | Default | Qué es | Ciclo de vida |
| --- | --- | --- | --- |
| `kit` | `.stele` | El marco vendorizado: `SKILL.md`, `guide.md`, `core/`, `modules/`. | **Reemplazable**: se sustituye entero con el ritual ACTUALIZAR. |
| `base` | `.` | Los docs instanciados (los roles) y el `history_dir`. | **Tuyo**: crece cada sesión, se versiona, no se regenera jamás. |
| `loader` | `CLAUDE.md` | El auto-arranque en la raíz. | **Derivado por bloque**: se regenera el bloque del marco, no el archivo. |

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
para Copilot. Aislarlo mantiene agnóstico casi todo lo demás — con una excepción que conviene saber:
si el `entry` conserva su nombre por defecto `AGENTS.md`, **también** hay agentes que lo auto-cargan
desde la raíz. Entonces mover `base` fuera de la raíz les quita el `entry` sin romper nada visible,
porque el loader sigue funcionando. Es un fallo silencioso; ver el aviso de `SKILL.md` → "Layouts con
nombre".

**El loader es derivado en parte, no desechable.** Lo que genera el marco es el **bloque** entre
`STELE:INICIO` y `STELE:FIN`; el **archivo** puede ser compartido, porque muchos equipos ya tenían un
`CLAUDE.md` o un `AGENTS.md` escrito a mano antes de adoptar stele. Por eso, si existe, se modifica y
no se crea de cero: es la misma regla de adopción que rige para cualquier doc de rol. La distinción
no es teórica — tratarlo como puro derivado destruyó el `CLAUDE.md` de un proyecto real.

**Dos anclas fijas en la raíz** que no siguen a `base`: el `loader` y el manifiesto
`stele.config.md`.

El **loader** porque el agente lo carga por nombre al abrir la sesión: si se moviera, no habría quién
le dijera dónde está. El **manifiesto** porque no es un doc del proyecto, es **el resolvedor**:
`base` declara dónde viven los docs de los *roles*, y el manifiesto no es un rol — es lo que traduce
roles a nombres y rutas. Guardarlo dentro de lo que él mismo resuelve es un error de categoría, la
misma razón por la que `package.json` no vive en `src/`. Y es donde lo busca un humano, que también
lee la configuración.

La pregunta aparece sola al usar `agrupado`: *si todo lo del marco se junta, ¿por qué el manifiesto
se queda fuera?* Porque agrupar del todo es imposible — el loader nunca se mueve, así que la raíz
pasaría de dos archivos del marco a uno. Ver "Alternativas descartadas".

Las combinaciones habituales tienen **nombre** (`default`, `agrupado`, `docs`, `skill`) para poder
pedirlas y confirmarlas de un tirón; la tabla vive en `SKILL.md` → "Layouts con nombre". Son
vocabulario, no un cuarto parámetro: **no se guardan en el manifiesto**, porque el layout ya es
derivable de las tres rutas y un dato con dos hogares se desincroniza. Por eso el eco del bootstrap
lo *nombra* pero lo que se escribe son siempre las rutas.

## Persistencia y la red de recuperación

Cerrar una sesión **escribe** el registro; persistirlo es otro paso, y el marco no asume cuál. El
parámetro `persistencia` lo declara: `git` · `ninguna` (los archivos en disco son el registro) ·
`comando` (una orden que el usuario configuró: `rclone`, un empaquetado fechado, otro VCS). El
núcleo es agnóstico a propósito — un proyecto de planeación o de investigación no tiene por qué usar
git — y el ritual de cierre se resuelve contra ese parámetro.

Lo importante no es el mecanismo sino lo que se pierde sin él. **Un VCS no es solo respaldo: es la
red que permite reconstruir el *qué* cuando la documentación falla.** Sin esa red, la disciplina
documental **sube**, no baja:

- El registro de sesión pasa a ser el **único** rastro de qué cambió. "Archivos tocados" ya no puede
  listar rutas: tiene que decir qué cambió dentro de cada una, porque no habrá diff que consultar.
- La regla "revertir solo los hunks propios" **presupone que puedes identificar hunks**. Sin VCS
  degrada a "no toques lo que no escribiste en esta sesión, y ante la duda pregunta".
- El checkpoint se vuelve **más** crítico, no menos: sin `git status` ni diff, el `handover` es la
  única forma de saber qué quedó a medias tras una interrupción.

**El marco nunca implementa la persistencia**: eso sería runtime, y rompería "markdown puro". El
paso de cierre es declarativo — le dice al usuario qué hacer, o ejecuta el comando que él configuró.

**Carpeta sincronizada** (Drive, OneDrive, Dropbox) es el caso más común de `ninguna`: guardar en
disco ya *es* el respaldo, sin credenciales ni configuración. Dos trampas que hacen que no sustituya
a un VCS:

- **La sincronización no es atómica.** Versiona archivo por archivo; un cierre toca cinco a la vez y
  no se recupera el conjunto como unidad. Las tres reglas de arriba siguen aplicando igual.
- **Los conflictos rompen justo los archivos append-only.** Cerrar sesión desde dos máquinas genera
  copias en conflicto, y `index`/`effort` son precisamente los que se duplican o se pisan.

**Credenciales, nunca.** Ningún doc del marco lleva tokens, claves ni cadenas de conexión: son
markdown legible, normalmente versionado, y en un kit vendorizado hasta se copian a otros proyectos.
Un comando de persistencia nombra la herramienta; las credenciales viven en el entorno o en el
gestor de esa herramienta.

## Alternativas descartadas (para no volver a proponerlas)

- **Referencia por rol en vivo** (escribir `[state]` en los docs y resolverlo al leer): descartada.
  Markdown no tiene indirección nativa, y el marco es agente-primero: los docs instanciados llevan
  **nombres concretos y legibles**. La indirección se resuelve **una vez**, en `bootstrap`, y
  renombrar es una operación del ritual `config` — el único renombrador sancionado.
- **Roles y módulos definidos por el usuario:** fuera de alcance a propósito. La config llega hasta
  nombres, toggles, presupuestos, wording, idioma y rutas; ampliar el vocabulario de roles rompería
  que el núcleo sea la fuente del mapa derivado.
- **Mover el manifiesto bajo `base`** (para que `agrupado` deje la raíz limpia): descartado, y no
  por la circularidad aparente — esa se rompería con un puntero desde el loader. Se descarta porque
  no consigue su objetivo (el loader no puede moverse, así que la raíz queda igual de "sucia", con un
  archivo en vez de dos) y porque el manifiesto es el resolvedor, no un doc de rol. A cambio de eso
  habría que añadir una ruta más, reescribir el invariante 5 y dejar no conformes las instancias ya
  existentes. Además, alcanzarlo solo a través del loader lo haría depender de un **derivado**:
  borrarlo o renombrarlo dejaría el manifiesto localizable únicamente por búsqueda.
- **Marcador de versión del kit** (`VERSION`, changelog de migración): descartado. El **diff** dice
  *qué* cambió y dónde, que es lo único accionable; un número no dice qué migrar, hay que acordarse
  de subirlo en cada cambio del kit, y miente en cuanto alguien lo olvida — un dato derivable con un
  segundo hogar. El ritual ACTUALIZAR garantiza que el diff exista siempre, que es lo que hace
  innecesario el marcador. **La procedencia es el caso contrario y sí se guarda** (`kit_origen`, en
  Meta): de qué remoto salió tu copia no se deduce de nada — ni del árbol, ni de un diff, ni del
  `README` vendorizado, que apuntaría al upstream aunque hubieras clonado un fork. Lo derivable no se
  guarda; lo no derivable, sí. Sin `kit_origen`, ACTUALIZAR se bloquea en el primer paso.
- **Un toggle de vocabulario** (`tecnico` | `llano`, para decidir cómo le habla el agente al
  usuario): descartado. La regla de hablar en llano **y nombrar el archivo** no le quita nada a un
  usuario técnico —el nombre va igualmente entre paréntesis—, así que no hay dos públicos que
  atender, solo uno. Un toggle habría añadido un parámetro a cada adopción, una rama más en cada
  ritual y una forma nueva de quedar incoherente con el resto de la config, a cambio de una salida
  algo más corta. El vocabulario **de datos** sigue intacto: eso es lo que hace innecesario el
  parámetro.
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
- **`charter` (design.md) — por qué el proyecto es así (a gran escala).** Norte, principios,
  restricciones y no-negociables, decisiones estructurales grandes (ADR-lite, fechadas), glosario.
  Estable. Frontera: *principios transversales* aquí; *specs por feature* → módulo.
- **`protocol` (protocol.md) — formatos.** Cómo se documenta (formatos de cada archivo, topes,
  operaciones de bajo coste). Referencia de formato.
- **`state`/`handover`/`index`/`session` — el historial** (`history_dir`, history/): estado que se
  sobrescribe, checkpoint del salto en curso, índice y detalle por sesión (se leen con grep).
- **`audit` (audit.md) — la serie de auditorías**, también en `history_dir` y append-only. Opcional
  (feature `audit_log`) y **no se instancia en bootstrap**: lo crea la primera auditoría, y su
  ausencia significa que el proyecto nunca se auditó. Guarda *cuándo y con qué alcance*, no los
  hallazgos: lo que perdura de una auditoría vive en el hogar que corrigió.
- **`correspondence`/`letter`/`correspondence_dir` — el intercambio con el exterior**, con la **misma
  forma que el historial**: un índice, un archivo por carta (inmutable, numerado) y su carpeta.
  Opcional (feature `correspondence_log`) y **no se instancia en bootstrap**: lo crea la primera
  carta. La numeración **no distingue dirección**, así que leer 1..N es leer la conversación y una
  carta sin respuesta detrás es un hilo abierto. El índice guarda lo que ningún otro doc guarda: **qué
  se rechazó y por qué** — un descarte razonado evita volver a discutirlo y le dice a quien escribió
  cómo calibrar la próxima.
- **`artifacts_dir` (artefactos/) — lo que la sesión produce y no es documentación:** scripts de un
  solo uso, extracciones, volcados. Un subdirectorio por sesión, y **tampoco se instancia en
  bootstrap**: lo crea el primer artefacto. Existe porque el historial guarda el *qué* y no el *cómo*,
  y un script de un solo uso escrito en el temporal privado del agente se lleva el *cómo* consigo —
  con `persistencia = ninguna`, la única reconstrucción posible. Ver `SKILL.md` → Precedencia.

Los roles que añade un módulo se describen en su `modules/<nombre>/roles.md` (p. ej. software:
`gotchas`, `specs`, `architecture`, `effort`).

## Presupuestos de tamaño (lo que mantiene barato el arranque)

| Rol | Tope objetivo | Al superarlo |
| --- | --- | --- |
| `state` | ~100 líneas | Podar; es estado, no historia |
| `handover` | ~50 líneas | Solo lo del salto activo |
| `charter` | ~200 líneas | Extraer una decisión a un tema del módulo + link |
| `session` | sin tope | Es histórico; se lee con grep, no al arrancar |

Los presupuestos son parámetros de la config (sección Presupuestos); los módulos pueden añadir los
suyos (p. ej. software: sección de `gotchas` por subsistema ~150-200 líneas).

**Un presupuesto excedido se decide, no se recorta.** El tope es un **umbral de olor**, no un límite:
lo que pide al cruzarse es una decisión, y hay dos salidas legítimas. Podar, si al mirarlo sobra algo
—que es el caso corriente—. O **subirlo con `config`** para ese proyecto, si el doc de verdad necesita
ese tamaño: un proyecto grande con varios frentes en paralelo puede necesitar un `handover` que no cabe
en el default, y forzarlo a caber destruye justo lo que el doc existe para conservar. Lo que no vale es
recortar hasta cuadrar el número: eso deja el tope intacto y el contenido mutilado, y además borra la
señal — un presupuesto que siempre se cumple porque se recorta ya no mide nada.

Y antes de subirlo, mira **por qué** no cabe: un `handover` que se desborda porque lleva tres saltos en
paralelo no está diciendo que el tope sea pequeño, sino que se está usando para algo que no es un
checkpoint. Los dos diagnósticos piden arreglos distintos.

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
- **Actualizar** trae una versión nueva del kit y reconcilia la instancia contra ella. Lo importante
  no es copiar archivos: es **leer el diff** entre el kit viejo y el nuevo y decidir qué implica
  (¿roles nuevos? ¿cambió la forma del manifiesto?). Por eso la versión nueva se trae **al lado**, no
  encima: el diff necesita las dos versiones, y traerlas en ese orden convierte la seguridad en una
  propiedad de la secuencia en vez de en una disciplina que hay que recordar. Nunca toca `base`: una
  plantilla nueva no reinstancia tus docs.
- **Config** cambia nombres, módulos, toggles, presupuestos, wording, idioma y las tres rutas — sin
  romper referencias, porque regenera los derivados.
- **Escala la ceremonia.** `effort` y la numeración `sesion-NNN` son opcionales; un proyecto pequeño
  puede empezar solo con el núcleo e ir sumando.
- **Cura, no acumules.** Los documentos vivos se podan; lo superado ya vive en el historial.
