---
name: stele
description: >
  Marco modular y configurable de documentación y continuidad para trabajar en un proyecto a
  través de muchas sesiones sin perder contexto y con coste de tokens acotado. Sirve para software
  y para trabajo no-software (materiales, planeación, investigación). Úsalo al INICIAR una sesión
  (ponerse al día), al CERRAR (registro durable), antes de un cambio interrumpible (checkpoint),
  para AUDITAR la documentación (verificar que lo escrito sigue siendo cierto), para INICIALIZAR el
  marco en un proyecto (bootstrap), para ACTUALIZARLO a una versión nueva del kit (actualizar), o
  para ADAPTARLO (config: nombres, módulos, parámetros). Núcleo agnóstico + módulos (producto) +
  config (stele.config.md).
---

# stele — rituales de sesión

> Hoja operativa. El *por qué* y las fronteras están en `guide.md` (leer una vez). Plantillas en
> `core/templates/` y `modules/<mód>/templates/`. Roles en `core/roles.md` y `modules/<mód>/roles.md`.
> Regla madre: **un hogar por dato; el estado se sobrescribe; el historial es append-only; lee poco
> al arrancar.** Los nombres de archivo salen del manifiesto `stele.config.md`.

## Mapa de documentación (GENERADO — consúltalo antes de leer/escribir)

El mapa "necesito X → tal archivo" **no se escribe a mano**: se genera en el doc `entry` desde los
`triggers` de los roles activos + el binding de la config. Regla de generación:

- **Roles activos** = roles del núcleo (`core/roles.md`) + roles de los módulos activos, menos los
  que tengan nombre `—` en la config.
- **Lista de arranque** = roles `startup: obligatorio`, ordenados por `order`, nombre resuelto
  (`{base}/[{history_dir}/]{nombre}`). El resto va a la nota "bajo demanda con grep".
- **Tabla de enrutamiento** = una fila `trigger → nombre` por cada rol activo.

Se regenera en `config` y al activar/desactivar módulos o renombrar. **Regla de oro anti-tokens:**
no leas un archivo entero "por si acaso"; usa el mapa y `grep -n` + lectura por rango.

## El punto inicial significa "maquinaria", y conviene decirlo

El kit vive en un directorio oculto (`.stele`, `.claude/skills/stele`) y tus documentos no. **Eso no
es casualidad: el punto marca lo que el marco se autoriza a sustituir entero.** Lo que empieza por
punto es reemplazable y no se edita a mano; lo que no, es tuyo y el marco no lo toca jamás.

Estaba en vigor desde el principio y nunca se había escrito, que es como una convención deja de
comunicar.

**Con un límite que hay que reconocer:** la convención del punto **es legible exactamente para quien
no la necesita**. Quien programa ya sabe que lo oculto es herramienta; quien no, no tiene por qué
saberlo — y es justo el público al que se le recomienda `agrupado`. Por eso declararla **no sustituye**
a que las dos carpetas se llamen distinto: lo acompaña.

**Consecuencia práctica que sorprende:** un directorio oculto **no lo tratan igual todas las
herramientas**. `ripgrep` y muchos buscadores lo saltan por defecto; `grep -r` de GNU entra. Así que
un mismo comando de barrido da resultados distintos según qué tenga instalado quien lo corre — y por
eso **todo comando que publiques debe acotar dónde busca** en vez de fiarse del default.

## Precedencia frente a los defaults del harness

**Dónde vive lo que produces en este proyecto lo decide el marco, no la herramienta que te ejecuta.**
Muchos harnesses inyectan un área de trabajo propia —scratchpad, memoria del agente, temporal de
subagente— y la marcan como prioritaria. Es un default razonable *para la herramienta*, y es el
equivocado *para el proyecto*: lo que se escribe ahí no lo ve el siguiente agente, no lo ve el humano,
y no queda en el registro. Ante conflicto, manda el mapa de documentación y el hogar de artefactos.

**Y de esa lista, la memoria del agente es la que más engaña, porque el motivo de arriba no le
aplica.** Un scratchpad se siente desechable; una memoria se siente **lo contrario** —permanente,
curada, con su índice y su formato—, y es verdad que **el siguiente agente de esa misma herramienta sí
la lee**. Ahí la razón correcta es otra, y es más estrecha: **no la lee nadie más.** Ni otro agente, ni
el humano que abra el repo, ni quien clone el proyecto en otra máquina — y no queda en el registro. Un
dato que solo sobrevive mientras nadie cambie de herramienta no está guardado: está prestado.

**La prueba es la ruta, no la intención.** Si el destino lleva el nombre de este proyecto y vive bajo
la configuración del agente (`…/<agente>/projects/<este-repo>/memory/` y equivalentes), es memoria
privada **de proyecto** por construcción — da igual lo general que parezca el dato que ibas a escribir
ahí. El hogar de una convención de trabajo es el doc de proceso; el de una trampa, el de gotchas.

**El límite, y hay que respetarlo:** esta precedencia cubre **el destino de los archivos**. No alcanza
a las reglas de seguridad del harness, ni a cómo usa sus herramientas, ni a sus permisos. Una regla
que reclama más de lo que le toca se ignora entera, y con razón.

Esto no es una regla de estilo: es la que evita que el marco pierda en silencio. Ha hecho falta
escribirla dos veces por el mismo motivo — un default externo vació una regla del marco que nunca dijo
quién manda, y las dos veces lo descubrió una persona leyendo, no el mecanismo.

## Rutas y tokens (los invariantes; la exposición está aparte)

Tres rutas independientes, todas **relativas a la raíz del proyecto** y **sin `/` final**:
`kit` (el marco, reemplazable) · `base` (tus documentos, intocables) · `loader` (**las puertas** de
auto-arranque, siempre en la raíz). **Invariante duro: `base` nunca puede quedar dentro de `kit`.**
Los valores vivos están en el manifiesto `stele.config.md`, no aquí.

Las plantillas usan tokens (`{{state}}`, `{{history_dir}}`, `{{kit}}`…) que bootstrap y `config`
resuelven a nombres. **`{{kit}}` colapsa cuando `kit = .`**: se escribe `SKILL.md`, no `./SKILL.md`.
Los contenedores ya traen `base` y su `/` final, así que se concatenan directos.

**El detalle —los casos de colapso, los layouts con nombre, qué se interpola y qué se consulta— vive
en `core/reference/rutas-y-tokens.md`**, y solo hace falta al instanciar o al cambiar nombres:
BOOTSTRAP y CONFIG. No se lee para trabajar.

## Cómo se le habla al usuario (registro llano)

El marco tiene **dos vocabularios**, y solo uno es contrato:

- **Vocabulario de datos** — ids de rol, claves y headers `##` del manifiesto, estados
  (`EN_PROGRESO`), nombres de archivo. Se **parsean**: no se traducen ni se adornan, ni en los docs
  ni en los comandos. Es la misma razón por la que `idioma` no traduce los ids.
- **Registro de habla** — lo que **dices**: saludo, ecos, informes, preguntas, resúmenes. No lo
  consume ninguna máquina, así que aquí no hay contrato que romper.

**Regla: al usuario se le habla en llano, y se nombra el archivo.** No *"el handover está en
`EN_PROGRESO`"*, sino *"quedó trabajo a medias, con su alcance anotado (`handover.md`)"*. El nombre
entre paréntesis va **siempre**: es lo que le permite ir a mirarlo, y lo que hace que hablar claro no
le quite precisión a un usuario técnico. Por eso la regla no necesita un parámetro que la active.

**Suavizar no es diluir.** Se traduce el **nombre** del concepto; **nunca se esconde el hecho**. Si
hay trabajo a medias, un doc que se contradice o una migración a medio aplicar, se dice — en llano y
sin rodeos. Un resumen tranquilizador es peor que la jerga.

**Y todo número que le muestres lleva qué se espera de él, aunque la respuesta sea "nada".** Hablar
llano dice *cómo* hablarle; no dice **qué necesita saber**, y un dato claro que no le sirve sigue
siendo hablarle de más. El caso peligroso es el **contador**: una persona que ve *"van 8, umbral 10"*
lee **cuota** —plan gratuito, batería, límite—, porque es lo que significa en todos los sitios donde ha
visto uno. Caso real de campo: un usuario leyó el aviso de auditoría del cierre y preguntó si eso
quería decir que **no debía cerrar más sesiones**. El aviso nombraba un coste —*"queda pendiente de
auditar"*— y ningún beneficio, así que **desincentivaba el hábito que el marco más quiere**.

La forma correcta dice las tres cosas: el número, qué pasa al llegar, y qué se espera de la persona.
*"Van 8 sesiones desde la última revisión de la documentación; a la 10 conviene hacer una. No cambia
nada de lo tuyo: cerrar sesiones siempre suma, y la revisión se pide cuando quieras."* Esto **no** es
"no des números": a un usuario técnico el número le sirve. Lo que faltaba era la consecuencia.

**Y vale para toda la contabilidad del marco**, no solo para el aviso de auditoría — presupuestos,
tamaños, sesiones transcurridas, cartas sin contestar. Si el número existe para que **el agente**
decida algo, la pregunta antes de decirlo en voz alta es si le cambia algo **a quien lo escucha**; si
no, no se dice, y si sí, se dice qué.

**Y la forma tira más que la advertencia escrita al lado.** El contador es un caso de algo más general:
cuando la **forma** de una cosa sugiere una lectura, decir en palabras que no es esa lectura casi nunca
basta. *"Van 8, umbral 10"* se lee como cuota aunque la frase siguiente lo niegue, porque la forma
—un número que sube hacia un tope— ya significó algo antes de que nadie leyera nada. Aporte de campo con
un caso incómodo: un corresponsal se reprochó haber presentado la cifra de una sola población como si
fuera una tasa, y **dos párrafos después puso dos poblaciones distintas en columnas contiguas**, con la
advertencia al lado. No fue descuido —**la tabla dice *compara* más fuerte de lo que el texto dice *no
compares***—. Así que cuando veas que una forma induce a error, **cambia la forma**: parte la tabla,
saca el número, reescribe la fila. La nota que la desmiente llega después y pierde.

**Una pregunta de la persona que el `manual` no contesta se contesta y se escribe ahí** (si el rol está
activo). No al cerrar, no "cuando haya varias": **antes de seguir**, mientras se sabe qué se preguntó y
con qué palabras. Es lo que hace que ese doc no sea genérico — nace corto y **crece con las preguntas
reales**, no con las que imaginamos. Y la señal de que funciona no es su tamaño, es que las preguntas
dejen de repetirse. Si te descubres explicando lo mismo dos veces, la primera vez no se escribió.

| Concepto del marco | Cómo se dice |
| --- | --- |
| `handover` en `EN_PROGRESO` | quedó trabajo a medias, con su alcance anotado |
| checkpoint | dejar guardado dónde vas, antes de algo que se puede interrumpir |
| bootstrap | preparar el proyecto la primera vez |
| `kit` / `base` / `loader` | el marco / tus documentos / **las puertas** que arrancan al agente |
| layout (`agrupado`, `docs`…) | dónde va cada cosa: todo el marco junto, o los docs en `docs/`… |
| manifiesto | el archivo de configuración |
| rol / token / derivado | (no se nombran: se dice el archivo, o "se regenera solo") |
| instancia | tus documentos, los que ya existen en el proyecto |
| drift | documentación que se quedó desactualizada |
| clase 7 (AUDITAR) | un dato que se quedó en el registro de una sesión y nunca llegó a su sitio |
| append-only | solo se añade; no se reescribe lo anterior |
| vendorizar / actualizar el kit | traer al proyecto una copia del marco / traer la versión nueva |
| presupuesto de un doc | el tamaño máximo que debería tener |
| `audit_every_n_sessions` / auditoría vencida | cada tantas sesiones conviene revisar la documentación — no es un límite, y cerrar sesiones siempre suma |

Esto **no cambia nada de lo que se escribe**: los docs, el manifiesto y los mensajes de commit siguen
con el vocabulario del marco. Un documento lo lee el siguiente agente; el habla la lee quien está
delante ahora.

**El habla va en el `idioma` del proyecto, con su ortografía natural.** Con `idioma = es` se habla
con acentos y con `ñ`. Una regla de "solo ASCII" —del proyecto o de la configuración del agente—
gobierna **lo que se escribe a un archivo**: ids, claves, nombres, código y cualquier texto que otra
herramienta vaya a parsear. El habla no es eso: es prosa dirigida a una persona, y nada la parsea. El
eje no es qué idioma, sino **a dónde va**. Un saludo sin acentos se lee como un fallo de codificación
en la primera línea que ve el usuario — el sitio más caro para parecer roto.

## Lo que llega al lector es el texto, no el razonamiento que lo produjo

**Dudar de los propios argumentos es parte de razonar; publicar esa duda no lo es.** Al escribir se
sopesa, se concede, se anticipa la objeción — y todo eso es trabajo legítimo que ocurre **antes** del
texto. El lector no estaba ahí: no ve la deliberación, ve la frase. Una salvedad que existía para
tranquilizar a quien escribía llega a destino convertida en **contenido**, y con el mismo peso que lo
demás.

**La forma del fallo es el párrafo final que se desdice.** Se enuncia una regla, se sostiene con su
caso, y en la última línea se añade la concesión que se sintió al escribirla — *"aunque esto también
vale para este mismo documento"*, *"aunque quizá sea otra cosa"*. Quien lo escribe lo vive como
honestidad. Quien lo lee recibe una regla que se desautoriza sola, sin nada que hacer con esa
información.

> **La distinción que salva la regla, y hay que hacerla bien: NO se trata de esconder incertidumbre.**
> La del **contenido** —*es un suelo y no una cifra*, *es una foto de un solo corpus*, *esto no se
> midió*— **se publica siempre**, porque le dice al lector qué puede y qué no puede hacer con lo que
> tiene. La que sobra es la del **autor sobre su propio texto**, que solo informa de cómo se sentía
> quien lo escribió.

**La prueba es de una línea:** ¿esta salvedad le permite al lector hacer algo distinto —recalcular,
desconfiar de un número, ir a medirlo— o solo le cuenta que quien escribía no las tenía todas consigo?
Lo primero es contenido y se queda. Lo segundo va al registro de decisiones **con su fecha**, donde
alguien puede retomarlo; **borrarlo sería la otra forma de perderlo.**

**Caso de campo, propio:** dos reglas nuevas salieron con su concesión pegada al final —una admitía
que el techo que describía alcanzaba al propio ritual, la otra explicaba con una teoría no medida por
qué el resultado salió así—. Las dos eran ciertas y ninguna publicable. **Lo cazó el usuario leyendo,
no un detector.**

## Regla dura: checkpoint antes de un cambio interrumpible

Deja `handover` en `EN_PROGRESO` con objetivo + alcance + verificación prevista + **sello** (plantilla
`core/templates/handover.md`) **{{checkpoint_trigger}}**. No es opcional ni depende del tamaño: una
sesión puede cortarse en cualquier momento y el checkpoint (~20 líneas) siempre cuesta menos que
reconstruir el contexto desde el diff. (El módulo `producto` especializa el trigger a "antes del primer
archivo de código".)

**Exención:** cambios que SOLO tocan el **contenido** de la documentación. **No exime una migración
estructural** — mover, renombrar o reestructurar docs, es decir los rituales CONFIG y ACTUALIZAR —
aunque no toque una línea de código: si se corta a la mitad, media instancia está en un sitio, el
manifiesto ya declara otro y los comandos de cierre apuntan a donde no hay nada.

**Y aquí también van las trampas de ESTE salto**, no solo el objetivo y el alcance: lo que sabes que
puede salir mal en lo que estás a punto de hacer. No es adorno, es el sitio donde una advertencia
llega a tiempo.

**El sello es la cuarta pieza y la más barata: el `HEAD` al abrir el checkpoint, con la instrucción de
compararlo.** Este doc se escribe *antes* de la primera edición y no se vuelve a tocar hasta el cierre,
así que es **el único del set de arranque que puede estar caducado** — y un checkpoint caducado no falla:
propone trabajo ya hecho, con toda la seguridad del mundo. La comprobación va en ABRIR
(`core/rituals/abrir.md`), y la ley es que **cuando discrepan manda el árbol**.

La evidencia de campo tiene los dos lados, que es raro y por eso vale. Un proyecto anotó en su
checkpoint que una sustitución textual podía alcanzar al kit, hizo el cambio y **lo esquivó** — y
reconoce que sin ese aviso probablemente habría caído, porque los dos cambios equivalentes que había
hecho ese mismo día fueron ingenuos. Aquí, en cambio, la trampa equivalente estaba en `gotchas` —que
se lee al abrir cada sesión— y **no impidió el fallo que describía**, cometido una sesión después de
escribirla y encontrado cinco sesiones más tarde por una auditoría.

**Lo que decide no es solo que esté escrita: es la distancia al momento de uso.** Con un matiz que el
propio corresponsal aportó y que rebaja la conclusión — **su trampa estaba también en `gotchas`**, no
solo en el checkpoint. Así que la comparación limpia no es *"doc de arranque contra checkpoint"* sino
**"doc de arranque solo" contra "doc de arranque más checkpoint"**, y lo que muestra es que
**añadirlo funciona**, no que el doc de arranque sobre. Escríbela en los dos sitios si hace falta: el
de arranque te informa, el del salto te detiene.

## Comprobar en vez de dar por hecho: tres momentos

Una sola desconfianza en tres puntos del tiempo. Las tres cuestan un comando y las tres fallan **en
silencio**, que es lo que las hace caras.

- **Antes de hacerlo — ¿ya está hecho?** No construyas sin mirar si existe. Dos casos de campo: un
  agente se ofreció a crear un entregable **que ya estaba verificado**, y otro propuso construir las
  puertas del marco **leyendo el proyecto a través de esas puertas**.
- **Al escribir que está hecho — ¿lo está?** Sobre todo cuando lo hecho es tuyo: entre decidir *"voy a
  hacerlo"* y dar por hecho que se hizo no hay distancia perceptible. Desarrollado en CERRAR.
- **Después de un paso cuyo efecto ocurre fuera del texto — ¿ocurrió, y qué se ve si no?** Un paso que
  **afirma** un resultado que nadie mira no da error: sale igual de convincente. Se arregla con
  comprobación **más síntoma declarado** — ver BOOTSTRAP paso 10 y el `Sello` del `handover`.

**El síntoma es la parte que se olvida**, y sin él la comprobación no existe: *"comprueba que
funcionó"* no sirve si quien mira no sabe qué aspecto tiene el fallo. Y quien mira suele ser el
usuario, no tú — tú ya tienes el contexto que oculta la falta.

## Los rituales, y dónde vive cada uno

**Este archivo no contiene los rituales: los enruta.** Cada uno vive en `core/rituals/` y **se lee
solo cuando se invoca**. Es deliberado y tiene una medida detrás: cuando los nueve vivían aquí, este
archivo pesaba **1845 líneas / 36 242 tokens** (medido en `27a41bd`) y crecía de forma monótona —de veinticinco cambios
seguidos, **ninguno** lo redujo—, mientras el ritual que se usa **cada** sesión ocupaba 12 líneas y el
que se usa **cada diez** ocupaba 615. **La masa era inversamente proporcional a la frecuencia de uso**,
y eso contradice el principio del marco: *coste de tokens acotado*.

| Ritual | Cuándo | Dónde |
| --- | --- | --- |
| **ABRIR** | al iniciar sesión — ponerse al día, barato | `core/rituals/abrir.md` |
| **CERRAR** | al terminar — dejar registro durable | `core/rituals/cerrar.md` |
| **AUDITAR** | se invoca; verificar que lo escrito sigue siendo cierto | `core/rituals/auditar.md` |
| **CONTRASTAR** | llega un informe externo sobre tu trabajo | `core/rituals/contrastar.md` |
| **REMITIR** | escribir hacia fuera lo que aprendiste | `core/rituals/remitir.md` |
| **BOOTSTRAP** | una vez: instanciar el marco en un proyecto | `core/rituals/bootstrap.md` |
| **ACTUALIZAR** | traer una versión nueva del kit | `core/rituals/actualizar.md` |
| **CONFIG** | adaptar nombres/parámetros — único renombrador sancionado | `core/rituals/configurar.md` |

**El checkpoint no está en la tabla porque no es un ritual: es una regla dura**, y por eso vive arriba,
en este archivo, donde se lee sin invocar nada.

**Y hay un tercer tipo de archivo, que no es enrutador ni ritual: la REFERENCIA.** Un ritual lo invoca
un **momento** del trabajo; una referencia la abre una **pregunta**, y puede hacerlo desde dentro de
cualquier ritual. Se leen por secciones, nunca enteras.

| Referencia | Cuándo se abre | Dónde |
| --- | --- | --- |
| **cómo se verifica cualquier cosa** | vas a medir, comprobar o publicar un número | `core/reference/verificar.md` |
| **rutas y tokens** | instanciar, renombrar o componer una ruta | `core/reference/rutas-y-tokens.md` |

**Una ley de verificación no es de AUDITAR aunque se use ahí**: la necesita igual quien cierra una
sesión, contesta una carta o publica una cifra. Si dudas de si algo es ritual o referencia, la pregunta
es **quién más lo va a necesitar**.

**Lo que queda aquí es lo que se necesita sin haber invocado nada:** las rutas, la convención de
tokens, cómo se le habla al usuario, la precedencia frente al harness y el checkpoint. Si al escribir
una regla dudas de si va aquí o en un ritual, la pregunta no es de qué trata sino **cuándo hay que
saberla**: si hace falta antes de elegir ritual, va aquí; si solo importa dentro de uno, va con él.

**Y una advertencia para quien añada:** este archivo tiene el mismo incentivo que tuvo antes. Cada
párrafo que entra está justificado por su cuenta y el agregado es lo que rompe el principio. Antes de
añadir aquí, comprueba si el sitio correcto es un ritual.

## Operaciones de bajo coste (preferir siempre)
- Apéndice de una fila → `printf '...' >> archivo` (sin `Read` previo).
- Archivo pequeño de formato fijo → un `Write` completo (no varios `Edit`).
- Buscar en archivo grande → `grep -n` y luego leer solo el rango.
- Volumen mecánico grande (dividir un doc de 1000+ líneas) → delegar a un subagente. **Dile dónde
  escribir:** un subagente trae su propio temporal privado, aún menos visible que el tuyo, así que
  esta recomendación multiplica el problema que resuelve el hogar de artefactos si no se le nombra el
  destino de forma explícita.
