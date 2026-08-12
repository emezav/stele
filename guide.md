# GUIDE — fundamentos del marco (por qué, y las fronteras)

> Referencia. Se lee una vez. La operación diaria está en `SKILL.md`. Este archivo explica el
> *por qué* de cada regla, para que quien adapte el marco no rompa lo que lo hace funcionar.
> Es **agnóstico de dominio**: sirva el proyecto para software, para preparar materiales, planear
> actividades o investigar. Lo específico del **producto** vive en `modules/producto/`.

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

## Lo que este marco NO puede garantizar, y por qué conviene saberlo

Un marco hecho de prosa **exige**; no impide. La diferencia importa porque **lo que se exige se
olvida**, y este proyecto lo lleva documentado con casos propios: reglas incumplidas en el mismo commit
que las escribía, un barrido ordenado por un ritual y no corrido en nueve sesiones, controles positivos
que validaban el filtro y no la fuente.

Contado sobre este kit: **52 pasos numerados y reglas duras que se exigen**, contra un puñado de cosas
que no dependen de que nadie se acuerde. Todas son **prestadas** — ninguna la impone el marco:

| Garantía | Quién la impone | Qué consigue |
| --- | --- | --- |
| Las **puertas** y sus `@`-imports | el harness, al abrir | Que el agente **no pueda** empezar sin el set de arranque en contexto |
| **`.gitignore`** | `git`, en cada `add` | Que lo que no debe viajar no entre por descuido (`add -f` lo salta: es contra el olvido, no contra la decisión) |
| El **linter** de markdown | el editor | Que un error de forma se vea **al escribirlo**. Caza forma, nunca veracidad |
| El **sello** contra el servidor | el servidor de `git` | Que exista un objeto **contra el que el otro pueda contradecirte** |

> **La lista es un suelo, no un techo, y la primera versión de esta sección lo confundió.** Decía
> *"tres"* y dejaba fuera el linter por ser *"del entorno"* — cuando el harness también lo es y sí se
> contaba. Corregido con dato: en este proyecto el linter ha cazado errores reales **en once sesiones**,
> y un acta lo dice con todas las letras (*"lo vio el linter, no yo"*). Lo destapó un corresponsal, y
> su formulación es la que hay que retener: **el medio pone el techo; casi todo proyecto vive por
> debajo del suyo**, por decisiones que nadie vuelve a mirar. La pregunta útil no es *"¿cuántas
> garantías tengo?"* sino **"¿cuántos lectores automáticos estoy dispuesto a adoptar?"**.

**Y la del sello dice menos de lo que parece, que es la corrección más fina de las dos.** `git
ls-remote` confirma que el identificador **resuelve**, no que el commit haga lo que la carta afirma. La
existencia la garantiza el servidor; **el contenido lo garantiza que alguien vaya a mirar**. Así que
esa fila no describe una garantía de herramienta sino **una condición para que exista otra**: la del
corresponsal que comprueba. Es la única de la tabla que no se puede prestar de ningún programa, y por
eso las dos veces que se comprobó de verdad, fue la otra parte quien lo hizo.

De ahí la regla que conviene tener delante al diseñar cualquier salvaguarda:

> **En prosa, una garantía necesita un lector que no sea el agente.** Mientras el único que lee la
> regla es quien debe cumplirla, la regla es una exhortación por buena que sea su redacción.

Tiene tres consecuencias prácticas, y las tres ya están en el marco:

- **Por eso el saludo de arranque es el único observable que distingue un marco activo de uno
  apagado.** Descansa sobre la garantía **más fuerte** de la tabla —el harness carga la puerta quiera
  el agente o no—, y por eso puede fallar **ruidosamente**: si no saluda, algo de esa cadena se rompió.
- **Por eso un dato que quieras comprobable se escribe donde algo pueda leerlo**: el estado de una
  carta vive en una **columna** de una tabla, no en un párrafo, para que un comando pueda contradecirlo.
- **Y por eso el control positivo se imprime AL LADO del resultado**, no en otra parte: mover el dato
  junto a la decisión es lo más cerca de una garantía que puede estar la prosa.

Lo que **no** se sigue de esto es que dé igual escribir las reglas. Se sigue que **una regla nueva sin
un lector que no seas tú es una deuda**, y que la pregunta al escribirla es siempre la misma: *¿qué
tendría que mirar esto para que saltárselo fuera imposible en vez de solo incorrecto?*

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
  primero es `producto` (añade `specs`/`architecture`/`effort` + convenciones + la regla del
  checkpoint antes del primer archivo de código). Un proyecto sin producto con estructura no lo activa.
- **Config** (`stele.config.md`, en la raíz del proyecto) — **fuente única** que enlaza `rol →
  nombre`, activa módulos y fija toggles/presupuestos/wording/idioma y las **tres rutas**. Todo lo
  accionable en tablas. De aquí se **generan** dos derivados: el auto-arranque y el mapa de
  documentación.

**La frontera núcleo/módulo** es una sola pregunta: el núcleo modela el **proceso de continuidad**
(dónde estamos, qué pasó, qué quedó a medias — agnóstico); un módulo modela el **objeto del trabajo**
(el codebase, el corpus, el plan). Por eso `specs` y `architecture` son roles de módulo aunque suenen
universales: un proyecto de notas sueltas los dejaría vacíos, y un documento vacío es peso muerto en
cada adopción. El corolario práctico es que activar `producto` no exige un compilador, sino un
producto con estructura y decisiones por feature — una tesis con su corpus o un kit de documentación
también lo son (ver `modules/producto/module.md`).

**`gotchas` fue el contraejemplo que corrigió la frontera** (sesión 58). Estaba en el módulo por sonar
a código, y lo que acababa guardando —trampas del entorno, de publicar, de verificar, de dónde
guardar— es **proceso**. Es el complemento de `history`: uno episódico que crece, otro destilado que
se cura. Por eso bajó al núcleo, y por eso se carga en cada arranque mientras el historial no.

**Y el módulo se llamaba `software`**, que era el otro síntoma del mismo error: el nombre nombraba un
dominio mientras el criterio hablaba de un producto. `módulos = software` sigue siendo válido para
siempre — alias permanente, mismo precedente que `RICO`.

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
| `loader` | `CLAUDE.md, AGENTS.md` | Las **puertas** de auto-arranque en la raíz. Es una **lista**. | **Derivado por bloque**: se regenera el bloque del marco, no el archivo. |

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
para Copilot. Aislarlo mantiene agnóstico todo lo demás.

**Y es una lista, no un valor, porque un proyecto puede abrirse con más de un agente.** Con un solo
nombre, quien instala tiene que **adivinar quién abrirá el proyecto después** — y un proyecto usado
por dos agentes no se puede ni expresar. Cada puerta es un **adaptador**: dice *qué leer y en qué
orden*, nunca *cómo trabajar*, que vive una sola vez en el `entry`.

**El acoplamiento que había que evitar no era el del loader: era que un doc del proyecto compitiera
por un nombre que el harness reclama.** El `entry` se llamaba `AGENTS.md`, así que con `base != .`
esos agentes dejaban de leerlo, en silencio. Se disolvió separando las dos cosas: el `entry` es
`guia-agente.md` y `AGENTS.md` es una puerta, que vive en la raíz pase lo que pase con `base`.

**El loader es derivado en parte, no desechable.** Lo que genera el marco es el **bloque** entre
`STELE:INICIO` y `STELE:FIN`; el **archivo** puede ser compartido, porque muchos equipos ya tenían un
`CLAUDE.md` o un `AGENTS.md` escrito a mano antes de adoptar stele. Por eso, si existe, se modifica y
no se crea de cero: es la misma regla de adopción que rige para cualquier doc de rol. La distinción
no es teórica — tratarlo como puro derivado destruyó el `CLAUDE.md` de un proyecto real.

**Y el bloque tampoco es puro derivado, así que por default también se conserva.** ACTUALIZAR y CONFIG
portan a él el delta del kit en vez de reescribirlo, porque **la divergencia respecto de la plantilla
nace en el bootstrap**: el manifiesto, el idioma y las convenciones del proyecto ya lo instancian
distinto el primer día. Regenerar entero es la excepción y hay que declararla (`LIMPIO`), tras
comprobarla con un diff. El default va del lado de lo irreversible: lo del kit se recupera del kit; lo
que el proyecto escribió en su bloque no está en ningún otro sitio.

**Anclas fijas en la raíz** que no siguen a `base`: **las puertas** y el manifiesto
`stele.config.md`. El manifiesto es uno; las puertas, tantas como harness haya que atender.

Las **puertas** porque el agente las carga **por nombre** al abrir la sesión: si se movieran, no
habría quién le dijera dónde están. El **manifiesto** porque no es un doc del proyecto, es **el resolvedor**:
`base` declara dónde viven los docs de los *roles*, y el manifiesto no es un rol — es lo que traduce
roles a nombres y rutas. Guardarlo dentro de lo que él mismo resuelve es un error de categoría, la
misma razón por la que `package.json` no vive en `src/`. Y es donde lo busca un humano, que también
lee la configuración.

La pregunta aparece sola al usar `agrupado`: *si todo lo del marco se junta, ¿por qué el manifiesto
se queda fuera?* Porque agrupar del todo es imposible — el loader nunca se mueve, así que la raíz
pasaría de dos archivos del marco a uno. Ver "Alternativas descartadas".

Las combinaciones habituales tienen **nombre** (`default`, `agrupado`, `docs`, `skill`) para poder
pedirlas y confirmarlas de un tirón; la tabla vive en `core/reference/rutas-y-tokens.md`. Son
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
  única forma de saber qué quedó a medias tras una interrupción. **Y por eso ahí no lleva sello:** el
  sello es la instrucción de contrastarlo contra el árbol, y sin VCS no hay árbol que consultar. Lo
  que lo sustituye es que el documento diga **qué se observa en disco** para saber por dónde iba.
  Con VCS pasa lo contrario, y es el error que se comete: **el `handover` deja de ser la única fuente
  y sigue leyéndose como si lo fuera.** Un checkpoint escrito antes del trabajo describe un pasado en
  presente, así que tiene que decir cuál de las dos manda cuando discrepan — y manda el árbol.

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

- **`entry` (guia-agente.md) — cómo trabajar.** Proceso, estructura, convenciones operativas, checklists
  (como punteros). Entrada única. Incluye el *mapa de documentación* (generado). NO lleva el detalle
  de otros hogares: apunta.
- **`charter` (design.md) — por qué el proyecto es así (a gran escala).** Norte, principios,
  restricciones y no-negociables, decisiones estructurales grandes (ADR-lite, fechadas), glosario.
  Estable. Frontera: *principios transversales* aquí; *specs por feature* → módulo.
- **`manual` (manual.md) — para la persona, no para el agente.** Es el único rol cuyo lector es
  humano, y por eso se **redacta** con el proyecto delante en vez de rellenarse: contesta *qué se
  espera de mí*, *qué avisos voy a ver y qué me piden*, y sobre todo *qué NO tengo que hacer*. Opcional
  (feature `manual_doc`), **en `on` por defecto** porque un doc que hay que acordarse de activar no
  existe para quien más lo necesita. Frontera con este archivo que estás leyendo: **`guide` explica el
  marco a quien lo adopta; `manual` explica *ese* proyecto a quien lo vive** — sin nombrar roles,
  tokens ni rituales. Y no nace completo: **crece con las preguntas que la persona hace de verdad**,
  que es lo que impide que sea genérico.
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

Los roles que añade un módulo se describen en su `modules/<nombre>/roles.md` (p. ej. `producto`:
`specs`, `specs_dir`, `architecture`, `effort`).

## Presupuestos de tamaño (lo que mantiene barato el arranque)

| Rol | Tope objetivo | ¿Lo importa la puerta? | Al superarlo |
| --- | --- | --- | --- |
| `entry` | ~200 líneas | **sí** | Es el *cómo se trabaja*; lo demás va a su hogar y se apunta |
| `gotchas` | ~150-200 líneas por sección **y ~400 en total** | **sí** | **Curar**: una entrada resuelta u obsoleta se borra |
| `state` | ~100 líneas | **sí** | Podar; es estado, no historia |
| `handover` | ~50 líneas | **sí** | Solo lo del salto activo |
| `charter` | ~200 líneas | no, bajo demanda | Extraer una decisión a un tema del módulo + link |
| `session` | sin tope | no | Es histórico; se lee con grep, no al arrancar |

Los presupuestos son parámetros de la config (sección Presupuestos); los módulos pueden añadir los
suyos (p. ej. `producto`: un tema de `specs` ~600-800 líneas).

**La tercera columna es la que hace útil a esta tabla, y faltaba.** Los cuatro primeros roles son
*exactamente* lo que la puerta importa: son el arranque, y su suma es lo que todo agente paga en toda
sesión antes de hacer nada. Los otros dos no se cargan nunca solos. Un tope sin esa distinción se lee
como higiene; con ella se lee como lo que es, **el presupuesto del pilar 4**.

**Dos de esas filas son nuevas y conviene decirlo:** `entry` no tenía tope —el documento que se lee
**primero** en cada sesión era el único del set de apertura sin presupuesto— y el total de `gotchas`
tampoco existía. Las dos ausencias son del mismo tipo: **el set que el pilar 4 promete acotar tenía la
mitad de sus miembros sin acotar**, y ninguna de las dos daba error.

**Y `gotchas` lleva ahora un tope TOTAL además del de sección, porque el de sección no puede verlo
crecer.** Medido el **2026-08-12** sobre el `gotchas` de este propio repo: **487 líneas en 13 secciones,
y ninguna pasaba de 122** — cumplía el presupuesto en todas, en todo momento, mientras se convertía en
**el 64% del arranque**. Un doc que crece **añadiendo secciones** nunca viola un tope por sección; el tope por
sección acota el desorden dentro de una entrada, y el crecimiento real de un `gotchas` es por número
de entradas. **Un presupuesto que solo mira las partes certifica un total que nadie acotó.**

### Y el kit también tiene presupuesto, porque también se carga

Los topes de arriba acotan lo que **tú** escribes. Faltaba el del kit, y su ausencia costó cara: los
nueve rituales vivían en `SKILL.md`, que creció de forma monótona —de veinticinco cambios seguidos,
**ninguno** lo redujo— hasta **1845 líneas / 36 242 tokens** (medido en `27a41bd`), mientras el ritual usado *cada* sesión
ocupaba 12 líneas y el usado *cada diez* ocupaba 615. **La masa era inversamente proporcional a la
frecuencia de uso**, que es la forma exacta de romper *coste de tokens acotado* sin que ningún cambio
individual parezca culpable.

El corte fue por ritual: `SKILL.md` enruta, y cada ritual se lee **solo cuando se invoca**.

> **Corpus fijado: `7ce11a9`.** Tokenizador `tiktoken`, codificación `o200k_base`, medido sobre
> `git cat-file` y no sobre el árbol de trabajo. **Sin esas tres cosas la tabla no es una medición
> sino un contador**, y un contador caduca en silencio: dos versiones atrás esta tabla no las llevaba,
> y siete de sus diez cifras —correctas el día que se escribieron— envejecieron entre un 2% y un 170%
> sin que nada avisara. Si vas a rehacerla, deja el identificador dentro.
>
> **Control del método, calculado y no elegido:** entre el corpus anterior (`72347d8`) y este,
> **nueve de los once ficheros no cambiaron** —`git diff --name-only`— y los nueve reproducen su cifra
> **exacta**. Por eso los dos deltas de abajo son crecimiento real y no deriva del tokenizador. El
> control no es *"no cambió desde una fecha cómoda"*: es **no cambió entre los dos instantes que se
> comparan**.

| Se carga | Tokens | Cuándo se paga |
| --- | --- | --- |
| `SKILL.md` (enrutador) | 5 703 | siempre que se enruta — **no al arrancar** |
| `core/rituals/abrir.md` | 753 | cada sesión, bajo demanda |
| `core/rituals/cerrar.md` | 6 243 | cada cierre |
| `core/rituals/contrastar.md` · `remitir.md` | 3 869 · 9 662 | si hay carta |
| `core/rituals/auditar.md` | 16 314 | cada ~10 sesiones |
| `core/reference/verificar.md` | **22 245** | al medir o publicar una cifra |
| `core/rituals/actualizar.md` | 6 933 | al traer kit nuevo |
| `core/rituals/bootstrap.md` · `configurar.md` | 3 373 · 1 799 | una vez / rara |
| `core/reference/rutas-y-tokens.md` | 2 851 | bootstrap y config |

**Sesión normal (enrutador + abrir + cerrar): 12 699.** Sin cambio en este corpus. **Y ojo con lo que
esa suma NO es** — la sección siguiente: ninguno de esos tres ficheros lo carga la puerta.

**Y conviene leer el resultado sin adornos, porque el titular es incómodo.** Auditar de verdad cuesta
`auditar` **más** `verificar` —lo que se lee al auditar son las dos— y eso son **38 559**, o sea **3,04
veces** una sesión normal. Antes del corte eran 24 999 en un solo fichero. **El corte no abarató el
coste: lo repartió**, y a cambio puso las leyes al alcance de los demás rituales, que era el problema
que venía a resolver.

### Esta tabla NO mide el arranque, y lo dijo durante dos corpus

Los 12 699 de arriba son **los ficheros del kit que toca una sesión completa**. Se presentaron como *el
arranque*, y el arranque es otra cosa: lo que la **puerta** importa antes de que el agente hable. Medido
con el mismo tokenizador, comprobando primero que reproduce las tres cifras selladas de arriba:

| Conjunto | Qué es | Tokens | Corpus |
| --- | --- | --- | --- |
| lo que mide la tabla | `SKILL.md` + `abrir` + `cerrar` | 12 699 | `7ce11a9` |
| lo que **pesan** las plantillas del set de arranque | la puerta + las 4, tal como se distribuyen | 4 781 | `7ce11a9` |
| lo que el adoptante **paga** al bootstrapear | esas mismas, sin los comentarios de instrucción | **3 167** | `7ce11a9` |
| el arranque real de un proyecto vivo | los 4 imports de la puerta, en este repo | **16 013** | **sin sellar** |

**La fila del arranque real no se puede sellar, y eso es parte del hallazgo.** Sus cuatro ficheros son
los docs de trabajo de la instancia, que **no están versionados**: no hay commit que los identifique,
así que la cifra es de un instante y no de un estado —medida **al cerrar la sesión 108, el
2026-08-12**— y envejecerá sin que nada avise. Es exactamente lo que la hacía invisible: **el corpus
caro es el que ninguna herramienta puede fijar**, y por eso la tabla acabó midiendo el barato.

**Ficheros en común entre la fila de la tabla y la del arranque real: cero.** No se solapan poco: son
conjuntos **disjuntos**. La puerta importa `entry`, `gotchas`, `state` y `handover`; `abrir.md` declara
en su primera línea que se lee bajo demanda; y ni la plantilla de la puerta ni la del `entry` nombran
`SKILL.md` una sola vez. La consecuencia práctica es la que duele: **podar el enrutador o el ritual de
cierre —lo que este documento venía proponiendo— no bajaría el arranque ni un token.**

<!-- Las dos frases de arriba decían "la tercera fila" y "el primero y el tercero", y las rompió
     AÑADIR una fila a la tabla en la sesión 109: los ordinales pasaron a apuntar a la fila nueva.
     Misma ley que "mover prosa rompe sus deícticos", por inserción en vez de por movimiento. Por eso
     ahora las filas se nombran y no se numeran. -->

**Lo que sí lo baja es curar el `gotchas`**, que en este repo pesa **más que todo lo demás del set de
arranque junto** —medido al cerrar la sesión 108: 9 562 contra 6 451—. Va en proporción y no en porcentaje fijo a
propósito: el denominador lleva dentro `state` y `handover`, que **fluctúan dentro de una misma
sesión** (un checkpoint abierto multiplica el `handover` por cinco), así que cualquier tanto por ciento
que se escriba aquí envejece sin que nadie lo toque. Por eso la tabla de
presupuestos de más arriba ganó una columna y un tope: el arranque no se abarata donde es cómodo
medirlo, sino donde está la masa.

**Y hay una lección en las dos frases anteriores de esta misma sección**, que es peor que el error.
Una versión decía *"`auditar` es **hoy** 1,44 veces"*, con la tabla ya corpus-fijada encima: la tabla no
mintió —sus cifras siguen siendo verdad de `ce7a5ca`— pero **la frase derivada decía *hoy* y llevaba
sesiones siendo falsa**, porque el fichero había crecido un 73% sin que nada avisara. De ahí salió la
regla: **el identificador protege la tabla, no la prosa que se deriva de ella.** La siguiente versión
—esta— la escribió, la dejó tres párrafos más abajo… y **volvió a fallar en el mismo sitio con la otra
mitad de la frase**: ya no era el adverbio, era el **sujeto**. *Arranque* nombraba un corpus que la
tabla nunca midió, y ninguna cifra estaba mal.

**Una cifra derivada hereda dos cosas de su tabla, y el sello solo cubre una.** El identificador
responde *¿sigue valiendo este número?*; no responde *¿es este el número de lo que estoy diciendo?*
Cuando escribas una cifra derivada, nómbrale el corpus **por sus ficheros**, no por lo que hace: los
ficheros se pueden contar, y *"el arranque"* no.

El tope del enrutador (400 líneas) vive en la sección Presupuestos del manifiesto, junto a los tuyos. Si
se cruza, el sitio de lo que entró es **un ritual**, no el enrutador.

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
Por eso es regla dura, no un juicio. El módulo `producto` la especializa a "antes del primer archivo
de código"; el núcleo usa el `checkpoint_trigger` genérico configurable.

## Adaptar el marco a un proyecto

- **Bootstrap** instancia el marco (ver `core/rituals/bootstrap.md`). Detecta greenfield vs
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
