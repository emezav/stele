---
name: stele
description: >
  Marco modular y configurable de documentación y continuidad para trabajar en un proyecto a
  través de muchas sesiones sin perder contexto y con coste de tokens acotado. Sirve para software
  y para trabajo no-software (materiales, planeación, investigación). Úsalo al INICIAR una sesión
  (ponerse al día), al CERRAR (registro durable), antes de un cambio interrumpible (checkpoint),
  para AUDITAR la documentación (verificar que lo escrito sigue siendo cierto), para INICIALIZAR el
  marco en un proyecto (bootstrap), para ACTUALIZARLO a una versión nueva del kit (actualizar), o
  para ADAPTARLO (config: nombres, módulos, parámetros). Núcleo agnóstico + módulos (software) +
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

**Y acotar no basta, porque hay que acordarse antes: di también CON QUÉ HERRAMIENTA obtuviste el
cero.** Caso de campo, y el mejor que hay de esta clase: un proyecto afirmó por carta que otro no lo
mencionaba en ninguna parte. `rg -li` daba **0** y `grep -rli`, sobre el mismo árbol y en el mismo
momento, daba **10** — la diferencia era un directorio ignorado por git, que es donde estaba todo.
Ningún comando falló ni avisó de que miraba menos. **El cero no salió de un razonamiento equivocado:
salió de un default.**

Las dos reglas no son la misma. *Acota dónde buscas* es una instrucción sobre el **alcance** y hay que
recordarla **antes** de escribir el comando; *di con qué lo obtuviste* es una instrucción sobre el
**reporte** y se ve **en la salida**, que es donde uno mira cuando ya se equivocó. Es la misma economía
de la adyacencia: lo que salva no es acordarse, es tener el dato al lado.

**Y este caso tiene un agravante que conviene no suavizar: el agente que falló había leído este párrafo
horas antes.** Ya hay precedente —una trampa escrita en el doc de gotchas, que se lee al abrir cada
sesión, no impidió el fallo que describía—, pero aquí la regla no solo estaba escrita: estaba
**recién leída**. Tener una regla en contexto **no predice cumplirla**; predice, como mucho, reconocer
el caso raro cuando aparece.

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

## Convención de tokens en plantillas

Las plantillas se escriben por **rol** y usan tokens que bootstrap/`config` resuelven a nombres:
`{{rol}}` → nombre del rol (p. ej. `{{state}}`→`latest.md`); `{{history_dir}}`, `{{specs_dir}}`,
`{{artifacts_dir}}` y `{{correspondence_dir}}` → **rutas** de los roles contenedores;
`{{budget:rol}}` → tope de líneas; `{{effort_unit}}` y
`{{checkpoint_trigger}}` → valores de Features/Wording; `{{kit}}` y `{{loader}}` → rutas
(sección Rutas). Los toggles como `session_greeting` **se consultan, no se interpolan**: no hay
token para ellos.
Los bloques marcados `<!-- GENERADO -->` los produce el marco, no se editan a mano.

**Toda ruta interpolada es relativa a la raíz del proyecto**, nunca al doc que la contiene: los
agentes operan con el CWD en la raíz y es lo que hace `grep`, así que el valor no depende de dónde
quedó cada archivo. De ahí dos reglas de composición:

- `{{kit}}` se escribe **sin `/` final** y se usa como `{{kit}}/SKILL.md`. **Con `kit = .` el prefijo
  colapsa**: `{{kit}}/SKILL.md` → `SKILL.md`, no `./SKILL.md`.
- Los **contenedores** (`{{history_dir}}`, `{{specs_dir}}`, `{{artifacts_dir}}`, `{{correspondence_dir}}`) resuelven **con `base` delante y con `/`
  final**, así que se concatenan directos, sin barra intermedia: `{{history_dir}}{{index}}` →
  `stele/history/index.md` con `base = stele`, y `history/index.md` con `base = .` (el prefijo
  colapsa igual que en `{{kit}}`). En el manifiesto el valor configurado es solo el nombre de la
  carpeta (`history/`); es el token el que le antepone `base` al resolverse.

Esto importa sobre todo en lo **ejecutable**. Un `printf '…' >> {{history_dir}}{{index}}` mal
compuesto no da error: crea el archivo que falta y deja el de verdad sin la fila.

**Dos clases de ruta, y no se resuelven igual** — confundirlas es lo que rompe enlaces al mover
`base`:

- **Ruta de comando** (`printf >> …`, `grep`, `git log --`): relativa a la **raíz del proyecto**,
  porque los agentes operan con el CWD ahí. Es la que producen los tokens.
- **Enlace Markdown clicable** (`[index.md](./index.md)`): relativo al **archivo que lo contiene**,
  porque así lo resuelve cualquier visor. Sobrevive a un cambio de `base` solo si su destino se mueve
  en el mismo bloque — que es el caso dentro de `{{history_dir}}`, y por eso el historial se mueve
  entero y no se reescribe. Un enlace que apunte **fuera** del bloque movido sí se rompe: revísalos
  al migrar.

## Las tres rutas: `kit` · `base` · `loader`

| Ruta | Default | Qué contiene | Quién la escribe |
| --- | --- | --- | --- |
| `kit` | `.stele` | El marco vendorizado. Maquinaria **reemplazable**. | Ritual ACTUALIZAR |
| `base` | `.` | Los docs instanciados (roles). Contenido **del proyecto**. | El agente, cada sesión |
| `loader` | `CLAUDE.md` | Auto-arranque, siempre en la raíz. GENERADO **por bloque**. | `bootstrap`/`config` |

**Invariantes** (validar en `bootstrap` y en `config`, antes de escribir):

1. `base` **nunca** dentro de `kit`: actualizar reemplaza el directorio del kit entero, y se llevaría
   los docs por delante. Violación = abortar. **Excepción: modo auto-hospedado** (`kit = .`), cuando
   el proyecto **es** el marco — el repo del kit. Ahí el kit no se vendoriza: se desarrolla en sitio
   y nunca se borra, así que la razón del invariante no aplica y `base` es por fuerza un
   subdirectorio suyo. En ese modo `base` debe ser un subdirectorio propio, nunca `.` (ver 2).
2. `kit` == `base` = abortar (misma razón, caso degenerado).
3. `kit` dentro de `base` (p. ej. `base = stele`, `kit = stele/.stele`) es legal pero **se avisa**:
   los `grep` del ritual de apertura empiezan a encontrar plantillas del marco como si fueran docs
   del proyecto.
4. El `loader` vive en la raíz y no puede colisionar con el nombre de un rol resuelto bajo `base`
   (con `base = .` y `loader = AGENTS.md` chocaría con `entry`). Colisión = abortar.
5. `stele.config.md` y el `loader` son las **dos anclas fijas de la raíz**: no siguen a `base`.
6. **Si el archivo del `loader` ya existe, se MODIFICA — nunca se crea de cero.** Su contenido es del
   usuario: se conserva íntegro y el bloque del marco se **inserta** entre las marcas
   `STELE:INICIO` / `STELE:FIN`. Solo ese bloque se reescribe después. Sobrescribir el archivo entero
   destruyó el `CLAUDE.md` de un proyecto real. Vale igual en `bootstrap` y en `config`.
   **Y si la marca de apertura dice `RICO`, ese bloque tampoco se reescribe entero:** el proyecto lo
   enriqueció con reglas propias, así que se porta el delta a mano. Vale en `config` igual que en
   `actualizar` — los dos rituales tocan ese bloque.

**El loader es derivado en parte, no desechable.** Lo generado es el bloque; el **archivo** puede ser
compartido con contenido del proyecto — muchos equipos ya tenían un `CLAUDE.md` o un `AGENTS.md`
escrito a mano antes de adoptar el marco. Se le aplica la misma regla de adopción que a cualquier doc
de rol: mapear y añadir, jamás reemplazar.

### Layouts con nombre (vocabulario, no parámetro)

Cuatro combinaciones de `kit` + `base` cubren casi todos los proyectos. Son **atajos de
conversación**: se resuelven a las tres rutas y **nunca se guardan en el manifiesto**. El layout es
derivable de la sección Rutas; guardarlo crearía un segundo hogar del mismo dato, que se
desincroniza en cuanto alguien mueva una ruta.

| Layout | `kit` | `base` | Para quién |
| --- | --- | --- | --- |
| `default` | `.stele` | `.` | Docs en la raíz, marco invisible |
| `agrupado` | `.stele` | `bitacora` | Tus docs juntos en una carpeta visible |
| `docs` | `.stele` | `docs` | Proyecto con carpeta de docs ya establecida |
| `skill` | `.claude/skills/stele` | `bitacora` | Claude Code: una sola copia del kit, además usable como `/stele` |

Cualquier otra combinación es legal y se nombra `personalizado` (incluido el modo auto-hospedado,
`kit = .`). El `loader` **no** forma parte del layout: depende del agente, no del proyecto.

**Por qué `base` no se llama como el marco.** El nombre obvio para `agrupado` sería `stele/`, y estuvo
así hasta que un proyecto lo señaló al agrupar: **`.stele/` y `stele/` se diferencian en un punto**, y
son cosas opuestas — una es maquinaria que se sustituye entera al actualizar, la otra son **tus
documentos**, los que editas cada sesión.

Y el fallo **no es simétrico**: escribir en tus docs creyendo que es el kit no rompe nada; **escribir
dentro del kit creyendo que son tus docs hace que el cambio desaparezca en la próxima actualización,
sin error y sin aviso**. Un nombre distinto elimina la confusión de raíz, sin depender de que nadie
entienda la convención del punto.

**Aviso antes de elegir `agrupado` o `docs`:** si el `entry` se llama `AGENTS.md`, hay agentes
(Codex y otros) que lo **auto-cargan desde la raíz** igual que Claude Code carga `CLAUDE.md`. Sacarlo
de la raíz con `base != .` no rompe nada visible —el loader sigue funcionando— pero esos agentes
dejan de leer el `entry` por su cuenta. Falla en silencio. Si trabajas con alguno de ellos, o
`base = .`, o renombra el `entry` a algo que no sea `AGENTS.md`.

Se usan de tres maneras:

- **En el eco**, siempre: nombrar el layout resuelto dice más, y más rápido, que tres rutas sueltas.
  Un usuario que pidió "agrupado" detecta `layout -> default` al instante.
- **Como menú**, solo cuando el ritual ya iba a preguntar (BOOTSTRAP paso 1, "ante duda real"). No
  convierte el bootstrap en un cuestionario: sin ambigüedad se aplican los defaults sin preguntar.
- **Como entrada**: "bootstrapea con layout agrupado" o "pásame a layout docs" son peticiones
  válidas; se traducen a valores de ruta y se previsualizan como tales.

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
| `kit` / `base` / `loader` | el marco / tus documentos / el archivo que arranca al agente |
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

## Ritual: ABRIR sesión (ponerse al día, barato)

Lee, en orden, SOLO la **lista de arranque** del proyecto (generada; con defaults del módulo software):
1. `entry` · 2. `gotchas` · 3. `state` · 4. `handover` — **si su Estado ≠ `SIN_TRABAJO_ACTIVO`**,
respétalo antes de editar. Bajo demanda (grep): `charter` (1ª vez / orientación), `protocol`,
`specs`, `architecture`, `index`/`session`.

**Confirma el arranque (visible):** un agente **no puede hablar antes de que el usuario escriba**,
así que la confirmación va **al frente de tu PRIMERA respuesta** — 1-3 líneas: última sesión
(N + título), si quedó trabajo a medias, próximo paso propuesto. En llano, nombrando los archivos
(ver "Cómo se le habla al usuario"). Sin esto, el arranque silencioso es
indistinguible de uno que no corrió. (Se omite si `session_greeting = off`.)

## Regla dura: checkpoint antes de un cambio interrumpible

Deja `handover` en `EN_PROGRESO` con objetivo + alcance + verificación prevista (plantilla
`core/templates/handover.md`) **{{checkpoint_trigger}}**. No es opcional ni depende del tamaño: una
sesión puede cortarse en cualquier momento y el checkpoint (~20 líneas) siempre cuesta menos que
reconstruir el contexto desde el diff. (El módulo software especializa el trigger a "antes del primer
archivo de código".)

**Exención:** cambios que SOLO tocan el **contenido** de la documentación. **No exime una migración
estructural** — mover, renombrar o reestructurar docs, es decir los rituales CONFIG y ACTUALIZAR —
aunque no toque una línea de código: si se corta a la mitad, media instancia está en un sitio, el
manifiesto ya declara otro y los comandos de cierre apuntan a donde no hay nada.

**Y aquí también van las trampas de ESTE salto**, no solo el objetivo y el alcance: lo que sabes que
puede salir mal en lo que estás a punto de hacer. No es adorno, es el sitio donde una advertencia
llega a tiempo.

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

## Ritual: CERRAR sesión (dejar registro durable)

1. **`session`** (nuevo): qué se hizo, decisiones, archivos, verificación, notas para retomar, y
   `## Esfuerzo equivalente` (si `effort_log`). `NNN` con padding a 3 dígitos.
2. **`index`**: una fila con append de Bash — `printf '| N | … |\n' >> {history_dir}{index}`.
3. **`effort`** (si `effort_log`): una fila con `printf >>`.
4. **`state`**: reescríbelo COMPLETO con `Write` según su plantilla — **nunca `Edit`, nunca prepend**.
   Si `audit_log`
   está activo y desde la última fila de `audit` han pasado más de `audit_every_n_sessions`
   sesiones, anota **"auditoría vencida (última: sesión X)"** en *Pendientes operativos*. Es una
   comparación de dos números, no una verificación: cerrar no audita.
5. **Decisiones que perduran** → su hogar (mapa): producto/feature → `specs`; principio/decisión
   grande → `charter`; patrón de código → `architecture`; gotcha → `gotchas`. Nunca solo en historial.
6. **`handover`**: si cerró completo → `SIN_TRABAJO_ACTIVO` **apuntando a la sesión que cierras
   ahora**. Si quedó salto → `EN_PROGRESO`.
7. **Artefactos** (si la sesión produjo alguno en `{artifacts_dir}sesion-{NNN}/`): **nómbralos en el
   `session`** y marca cuáles **sostienen un cambio irreversible** — el script que movió los archivos,
   el que reescribió en lote. Esos son evidencia y no se podan; el resto es desecho y lo borra el
   usuario cuando quiera. **Tú no borras nada**: limpiar por tu cuenta destruye justo lo que hace
   auditable la sesión. Si no hubo artefactos, no se dice nada — no hay sección que rellenar.
8. **Persistir el cierre** según `persistencia` (manifiesto → Meta). El cierre se escribe primero
   (pasos 1-7) y se persiste **una sola vez**, al final.

**Y el `state` se reescribe entero también cuando lo tocas fuera del cierre.** La regla de arriba vive
en un checklist de cierre, así que no se siente aplicable a las ediciones de mitad de sesión —se
entregó una carta, se resolvió un pendiente, cambió algo de estado—, y ahí es donde entra el parche.
Ayuda además una intuición económica que es falsa: reescribir el documento entero para cambiar una
línea parece desproporcionado, y **cuesta lo mismo**. Caso de campo: un `state` pasó **cuatro sesiones**
diciendo *"esperando respuesta a la carta 5"* con el hilo por la 15.

**Porque una sustitución que no encuentra su ancla no falla: no hace nada.** Y *"no hizo nada"* se ve
exactamente igual que *"ya estaba bien"* — misma pantalla, mismo estado del repositorio, misma sensación
de tarea hecha. Es el gemelo del cero silencioso de AUDITAR, en el lado de la escritura: allí un barrido
roto se lee como un corpus limpio, aquí una edición que no ocurrió se lee como una edición innecesaria.
En los dos casos **el fallo se disfraza del resultado bueno**, y en los dos la salida es comprobar en
vez de mirar. Si tu herramienta no protesta cuando el ancla falta, el `Write` completo **elimina la
categoría entera** para el doc donde más duele.

**Y la barrera se rodea por eficiencia, no por descuido** — que es lo que hace volver a este fallo.
Ampliación del mismo caso, contada por quien lo cometió: su herramienta de edición **sí protestaba**
cuando el ancla faltaba, o sea que la protección existía; pero escribió las sustituciones **como un
script**, porque un script las hace todas en una llamada y la herramienta necesita una por cambio. Es la
misma forma que la vez anterior, cuando reescribir el documento entero para cambiar una línea *"se
sintió desproporcionado"*. **Las dos veces, una optimización local lo sacó de la vía protegida**, y
ninguna de las dos fue un olvido.

De ahí lo que conviene saber al escribir una regla: **si la vía segura cuesta más que la insegura, la
regla se va a rodear**, y añadir una advertencia no lo impide — la advertencia hay que leerla desde
dentro de la vía que ya elegiste. Lo que funciona es que la vía segura **no cueste más** (el `Write`
completo cuesta lo mismo que el parche, aunque no lo parezca) o que lo que pagas al rodearla sea
**visible en el momento**. Cuando una regla tuya se incumpla dos veces seguidas, **mírale el precio antes
que la memoria de quien la saltó**.

**Y la vía barata casi nunca falla de frente: devuelve algo que se parece a un resultado.** Es lo que la
hace difícil de abandonar. Tercer caso del mismo corresponsal, del mismo día: una comprobación escrita
con `grep -c`, que cuenta **líneas con coincidencia** y no **coincidencias**, informó de *"1 forma
peninsular"* donde no había ninguna; la vía cara era una línea más de comando. La familia se reconoce
por ahí: el script que no encuentra su ancla devuelve el fichero intacto, el barrido roto devuelve cero,
el `-c` devuelve un número. **Ninguno da un error; todos dan una respuesta con la forma correcta.** Por
eso el control positivo no es una formalidad — es lo único que separa *una respuesta* de *una respuesta
cierta*.

**Y hay una vuelta peor, donde la respuesta con forma correcta es una métrica que tú mismo elegiste.**
Un reemplazo en lote sobre diez documentos —escrito para rodear la herramienta de edición, que hace un
archivo por llamada pero **protesta en voz alta cuando el ancla no existe**— imprimió su contador de
caracteres no-ASCII antes y después: `12 -> 0`, `17 -> 0`, `3 -> 0`. Verde en los diez. Lo que había
hecho de verdad era **convertir en la letra `b` todos los backticks de un documento** —la comilla
invertida se le comió al escape de la shell— y, en otro, **volver falsa una frase que era cierta**:
donde decía que el manifiesto conserva su centinela `—`, el barrido lo sustituyó por `--` y el texto
pasó a afirmar lo contrario **de la única excepción que ese párrafo existe para proteger**. Ninguno de
los dos archivos quedó roto: quedaron válidos y equivocados, y sobrevivieron a la revisión, al cierre y
a dos commits.

**El contador no falló: midió exactamente lo que se le pidió.** Los no-ASCII bajaron a cero, que era la
consigna, y por eso **certificó como éxito** un documento que acababa de perder todos sus backticks. Lo
destapó, otra vez por casualidad, una comprobación de otra cosa que **imprimía la línea resultante** en
vez de un recuento — `- bbitacora/requirements.mdb Sec.3` se ve al instante, y un número no lo habría
mostrado jamás.

**De ahí la regla, que es de forma y no de disciplina: un reemplazo en lote sobre prosa versionada
imprime su DIFF, no su recuento.** El recuento dice **cuánto** cambió; solo el diff dice **qué**
cambió. Y es la misma familia de *el barrido se come el documento que describe el barrido*: el texto
más expuesto es justo el que explica la excepción, porque para explicarla tiene que contenerla.

**Y el `state` no guarda datos que puedan volverse falsos entre dos cierres: los apunta.** Es la otra
mitad del mismo caso y la más barata. Ese *"esperando respuesta a la carta 5"* **ya se derivaba del
índice** —una carta que entra sin una que salga detrás es una conversación abierta—, así que el `state`
llevaba una lista aparte cuyo único destino posible era desincronizarse. Cuando un pendiente tenga hogar
propio, **nómbralo y apunta**: *"el estado de cada carta vive en el índice"* no caduca; *"la carta tal
está redactada"* caduca en cuanto alguien la entrega. Vale para todo lo que el `state` no pueda observar
por sí mismo, y se nota justo aquí porque **el `state` se lee al arrancar**: lo que miente ahí tiñe la
sesión entera antes de que nadie compruebe nada.

**Y el puntero no lleva adjetivo de estado**, o vuelve a ser una copia: *"hay correspondencia **sin
entregar**, ver el índice"* parece un puntero y caduca en el instante en que alguien entrega algo.
**El adjetivo es justo lo que uno añade para que la frase resulte informativa** —un puntero honesto se
siente pobre—, y ahí está la razón de que se cuele una y otra vez: **la vía correcta cuesta más a quien
lee**, porque le obliga a abrir otro archivo solo para saber si hay algo que hacer. Por el párrafo de
más arriba, eso significa que decirte *"resiste la tentación"* no serviría de nada.

**Lo que sí funciona es cambiar lo que se escribe: la acción condicionada en vez del estado.**

| Caduca | No caduca |
| --- | --- |
| *"la carta tal está redactada"* | *"cuando el corresponsal responda, procesar con CONTRASTAR"* |
| *"hay correspondencia sin entregar, ver el índice"* | *"el estado de cada carta vive en el índice"* |

La segunda columna es **más útil, no solo más correcta**: dice qué hacer y cuándo, que es lo que el
lector venía a buscar, y no depende de un estado que alguien puede mover mientras nadie mira. Escrito
tras la tercera iteración del mismo error en tres días — las dos primeras se corrigieron prohibiendo, y
solo la tercera preguntó por qué costaba.

**No registres un estado que no puedas observar.** Antes de escribir un hecho en un doc, pregúntate si
puedes comprobarlo desde donde estás. Lo que ocurre fuera de tu alcance —que una carta se entregó, que
un comando llegó a su destino, que alguien leyó algo— **no lo sabes: lo supones**, y una suposición en
un registro es peor que una ausencia, porque se lee como hecho y nadie vuelve a comprobarla. Cuando el
estado importe y no puedas observarlo, **regístralo por lo que sí sabes** —*redactada* en vez de
*enviada*— y deja que lo mueva quien sí puede verlo.

**Y hay una tercera, la más cómoda de las tres: antes de declarar algo incomprobable, comprueba que lo
es.** *"No se puede saber"* es una afirmación sobre el mundo como cualquier otra —dice que **no existe**
una vía de comprobación— pero se lee como prudencia en vez de como conclusión, así que **nadie la
audita**. Es la única del grupo que se premia por escribirla: cierra el asunto, suena honesta y nadie
pide su evidencia.

Caso propio, y de manual: se escribió *"no se ha podido comprobar si ese nombre está tomado"* y se
archivó como incógnita declarada — **con una búsqueda web disponible todo el tiempo**. Al comprobarlo
después, la respuesta estaba a un comando y además era útil. **Declarar algo incomprobable sin intentar
comprobarlo cuesta lo mismo que dar por hecho lo que no se ha hecho**, y disimula mejor.

**Y el hueco de esa regla no está en los estados ajenos, sino en los propios.** Tal como se lee, habla
de lo que hacen otros —una entrega, un despliegue, una notificación—, y ahí uno desconfía solo. El caso
que muerde es el contrario: **dar por hecha una acción de uno mismo porque ya se ha decidido hacerla.**
Entre decidir *"voy a corregir eso"* y escribir *"ya está corregido"* la distancia es tan corta que no
se percibe, y no hay desconfianza que la cubra porque el sujeto eres tú. De ahí la versión que sí
alcanza a los dos casos: **al escribir "ya está hecho", compruébalo — sobre todo cuando lo hecho es
tuyo.** Caso de campo, y del peor tipo: dos frases de ese estilo salieron en una carta, se descubrieron
al ir a registrar su entrega, y para entonces el destinatario ya había construido encima de una de
ellas. Un estado ajeno al menos es incomprobable; este estaba a un comando de distancia.

**Antes de persistir, comprueba lo que acabas de escribir contra las convenciones de texto de tu
proyecto** (si las tiene: solo-ASCII, terminología, lo que sea). **El marco no impone ninguna** —este
mismo kit está escrito en prosa acentuada a propósito—: el paso se parametriza con **las tuyas**, y si
tu proyecto no tiene convenciones de texto, no hay nada que comprobar. Lo aclaramos porque un lector
cuidadoso, con el kit delante, entendió lo contrario. No es una formalidad, y hay dos sitios
donde se escapa siempre: **las filas append-only** y **el mensaje de commit**. Son los dos únicos
momentos del cierre en que se redacta **prosa narrativa hacia un archivo**, con el mismo impulso con el
que se le habla al usuario — y ahí el registro equivocado se cuela sin que nadie lo note, porque el
resto de lo que se escribe son identificadores y rutas, donde el error salta solo. Compruébalo con un
comando, no releyendo: es lo que hace la diferencia entre una regla escrita y una regla aplicada.

**`persistencia = git`** — los archivos de cierre van en el **mismo commit** que el trabajo de la
sesión, no en uno aparte. Dile al usuario el `git push` exacto (o hazlo si lo autoriza). Reglas,
porque **un commit no puede contener su propio hash**:

- **No anotes el hash del commit que lleva el cierre.** Es información derivable, y por eso no se
  guarda: `git log --diff-filter=A -- {history_dir}{session}` devuelve el commit exacto de esa
  sesión. Guardar lo que se puede derivar es duplicar un hogar.
- **`--amend` solo mientras el commit sea privado y nadie lo cite.** El amend **cambia el hash**, así
  que un hash ya anotado queda apuntando a un commit inexistente, y falla en silencio. Dos
  condiciones, ambas comprobables antes de tocar nada: (a) **no se ha pusheado** y (b) **ningún doc
  anota su hash**. Si se cumplen —el caso típico es arreglar el mensaje del commit que acabas de
  hacer— el amend es seguro y no hay que dar rodeos. Si no se cumplen, no lo es: en particular,
  **nunca lo uses para plegar los docs de cierre dentro de un commit de trabajo cuyo hash ya
  anotaste**, que es el caso que originó la regla.
- Los hashes de commits **anteriores** de la sesión sí se anotan: existen y son estables.
- Si el trabajo ya se pusheó a mitad de sesión, el cierre va en un commit posterior. Es inevitable
  y está bien — ahí sí puede anotar el hash del trabajo. Que sea una decisión, no un accidente.

**`persistencia = ninguna`** — no hay VCS: los archivos en disco **son** el registro. Verifica que
todo quedó escrito y dile al usuario en 2-3 líneas qué cambió y dónde. Sin red de recuperación la
disciplina **sube**, no baja: ver `guide.md` → "Persistencia y la red de recuperación".

**`persistencia = comando`** — ejecuta el `persistencia_cmd` del manifiesto y reporta su resultado
con honestidad: si falla, dilo y **no des el cierre por persistido**. El comando **nunca lleva
secretos** — el manifiesto es markdown versionado y legible. Las credenciales viven en el entorno o
en el gestor de la propia herramienta, nunca en un doc del marco.

## Ritual: AUDITAR (verificar que lo escrito sigue siendo cierto)

Se dispara con "audita la documentación" / "corre el audit". **Se invoca; nunca corre solo** —
auditar es caro por naturaleza y choca de frente con la regla madre *lee poco al arrancar*.

Los otros ocho **escriben** documentación (abrir, checkpoint, cerrar), mantienen el **marco**
(bootstrap, actualizar, config) o hablan con **fuera** (contrastar, remitir). Ninguno re-verifica lo ya
escrito: `abrir` lee poco a propósito, `cerrar` escribe el estado nuevo sin releer el viejo,
`config`/`actualizar` tocan el manifiesto y la maquinaria —no el contenido de los docs—, y los dos de
correspondencia miran lo que entra y lo que sale, no lo que ya está escrito aquí. Sin AUDITAR la documentación deriva en
silencio, y **un dato obsoleto se lee como hecho** — es peor que no tener el dato.

**Dos reglas duras:**

- **El historial es inmutable.** AUDITAR **nunca** reescribe `session` ni `index`. Un registro puede
  ser la *fuente* que delata el drift, y puede contener algo que nunca llegó a su hogar: en los dos
  casos se corrige **el hogar**, no el registro.
- **Nada se reescribe en silencio.** Los errores se aplican tras confirmación en bloque; las
  preferencias se preguntan una a una.

### Alcance (qué se relee, y qué no)

Releer todo en cada auditoría no escala. Default = **incremental**:

| Entra | Por qué |
| --- | --- |
| Los docs de la **lista de arranque** | Son pocos, y son los que más caro salen si mienten: se leen sin filtro en cada sesión |
| Los **hogares** que las sesiones desde la última auditoría debieron tocar | Ahí aparece la clase 7, la invisible |
| Lo que esas sesiones declaran en "Archivos tocados" | Lo tocado hace poco es lo que más contradice a lo viejo |

El rango de sesiones sale de la última fila de `audit` (desde) y de `index` (hasta). Sin `audit_log`
no hay desde dónde contar y toda auditoría es completa. **`audit completo`** se pide a mano: primera
auditoría, después de una migración estructural, o cuando el usuario lo quiera.

Los `session` y el `index` **no son objeto de corrección**, solo fuente contra la que contrastar.

### Las ocho clases de drift

| # | Clase | Qué es |
| --- | --- | --- |
| 1 | Afirmación caducada | Era cierta al escribirse, dejó de serlo, y se sigue leyendo como hecho |
| 2 | Estado obsoleto | Hitos o fases que declaran un estado superado hace sesiones |
| 3 | Criterio refutado | Una sesión posterior demostró que el criterio falla; el doc lo sigue pidiendo igual |
| 4 | Índice desincronizado | El índice no menciona secciones que se añadieron después al detalle |
| 5 | Metadato incorrecto | Una cabecera atribuye a la sesión N algo hecho en la N+2, contra otra tabla del mismo doc |
| 6 | Bloqueo obsoleto | "Bloquea la fase X" cuando X ya arrancó, e incluso respondió parte de lo que bloqueaba |
| 7 | **Hallazgo sin hogar** | Conocimiento que se quedó en el registro de sesión o en un doc de detalle y **nunca se promovió** al doc que se lee al abrir |
| 8 | Crecimiento sin revisión | Un doc pasó de legible y nadie decidió si partirlo |

Las ocho son agnósticas de dominio, y por eso el ritual es del núcleo. Un módulo activo aporta
**detectores atados a sus roles** (software: el par `specs`↔`specs_dir` y los hogares
`gotchas`/`specs`/`architecture` — ver `modules/software/module.md`).

**La clase 7 es la que justifica el ritual.** Las otras siete se ven leyendo el doc con atención;
esta no se ve en **ningún** doc, porque el defecto es una **ausencia**: el dato existe, pero no donde
se lee. Solo aparece contrastando dos sitios. Es la regla "un hogar por dato" fallando en silencio.

**Y admite dos remedios, no uno.** Promover al doc de arranque es el reflejo, pero choca con "lee poco
al arrancar" en cuanto el dato es de **consulta** y no de **orientación**: no todo lo que falta en su
hogar tiene que leerse en cada sesión. El otro remedio es un **artefacto de consulta bajo demanda**,
que se busca con `grep` cuando hace falta y no entra en la lista de arranque. Elegir mal engorda el
arranque de todas las sesiones futuras, que es un coste que no se ve al aplicarlo.

**Y viven en tres sitios concretos, lo que los hace buscables.** Un hallazgo se escribe **cuando es
noticia**, y lo que es noticia aterriza en el **checkpoint** o el doc de estado (que se podan), en el
**registro de sesión** (inmutable, y por eso nadie lo mueve de ahí) y en las **cartas** (archivadas, o
previstas para retirarse). Ninguno de los tres se lee para trabajar. Dos proyectos independientes han
encontrado su huérfano en **exactamente esa constelación**, con reglas distintas y sin conocer el caso
del otro: no es casualidad, es dónde cae por defecto lo que aún no tiene hogar. Empieza por ahí.

### Detectores (sin esto, el ritual es decorativo)

Un audit que devuelve "todo se ve bien" no ha auditado. Barre primero, verifica después:

```bash
# clases 1 y 3 — afirmaciones absolutas y criterios que quizá ya no valen
grep -rniE "siempre|nunca|todos los|todas las|ningún|en ningún caso|garantiza|basta con" {base} --include="*.md"

# clases 2 y 6 — marcadores de estado y bloqueos
grep -rniE "pendiente|por confirmar|validado en|en curso|en progreso|provisional|bloquea" {base} --include="*.md"

# clase 3 — vocabulario de refutación en las sesiones del rango (fuente, no objeto)
grep -rniE "en realidad|result[oó]|falso negativo|falso positivo|no funciona|descartad|corregi" {history_dir}

# clase 5 — metadatos de sesión en cabeceras, para contrastar contra {index}
grep -rniE "sesi[oó]n [0-9]+" {base} --include="*.md"

# clase 4 — secciones reales del detalle, para contrastar con su índice
grep -n "^## " <doc de detalle>

# clase 8 — tamaño contra presupuesto
wc -l <docs vivos>

# clase 1 — afirmaciones sobre el mundo, para comprobarlas FUERA de los docs (opt-in, ver abajo)
grep -rhoE "(/[a-zA-Z0-9._-]+){2,}" {base} --include="*.md" | sort -u   # rutas
grep -rhoE "https?://[^ )\"]+" {base} --include="*.md" | sort -u        # URLs y endpoints
grep -rhoE "\bv?[0-9]+\.[0-9]+\.[0-9]+\b" {base} --include="*.md" | sort -u  # versiones
```

**Un cero se comprueba antes de creerlo.** Un barrido que devuelve 0 en **todos** los detectores casi
nunca significa "corpus limpio": significa que el comando no miró donde creías — un directorio de
trabajo que no era el que pensabas, un glob que no expandió, una ruta mal compuesta. Es el falso
negativo más barato de cometer y el más caro de no ver, porque **un cero roto y un cero legítimo se
leen igual**. Antes de informar "sin hallazgos", corre el mismo patrón contra algo que **sepas** que
casa y comprueba que sale.

**Y hay una vuelta más, que es la que más veces muerde: la prueba misma necesita comprobarse.** Un
detector sano, con su control en verde y su ámbito correcto, **no prueba nada si la prueba que lo ejerce
está mal construida**. Medido en dos sesiones seguidas, cuatro veces:

| Prueba | Por qué no probaba nada |
| --- | --- |
| *"esta frase cruza un salto de línea"* | no lo cruzaba: la línea de arriba estaba vacía |
| *"este modo literal cambia el resultado"* | el patrón no tenía metacaracteres, así que los dos modos coincidían |
| *"el comando devuelve este código de salida"* | se leyó el código **del pipe**, no del comando |
| *"este número sale por esta razón"* | el número era correcto y **la explicación falsa** |

**Antes de creerte una prueba, comprueba que puede fallar.** Es el control positivo aplicado un nivel
más arriba: si no sabes qué resultado tendría si la hipótesis fuera falsa, lo que tienes no es una
prueba sino una coincidencia.

**Y lo que delató tres de las cuatro fue tener otro dato al lado** —`grep` junto a la herramienta, el
valor esperado junto al obtenido—, que es exactamente por lo que los datos que sostienen una decisión se
imprimen juntos.

**Y el control positivo prueba el detector, no el ámbito** — que es la vuelta anterior y se nos escapó
al escribir la regla de abajo. Un detector puede pasar su control **y aun así ser incapaz de encontrar
nada** en lo que le has dado a mirar: si comprueba que cada fila de una tabla cuadra con su cabecera y
le pasas **una sola línea**, no tiene cabecera contra la que comparar y devuelve cero **siempre**. El
control sigue en verde, porque el control se corre sobre su propia entrada de prueba.

De ahí la forma correcta de acotar: **recorta lo que se REPORTA, nunca lo que el detector LEE.** Dale el
corpus entero y filtra los hallazgos después. Recortar la entrada parece equivalente y no lo es —
convierte un detector sano en uno muerto, sin que ninguna señal lo diga.

**Y el control positivo va por detector, no por barrido** — aporte de campo, y de los caros. Alguien
corrió cinco sondas, una por cada afirmación que quería verificar; **las cinco dieron negativo y una
estaba rota**: buscaba una frase que no existía en el texto, así que **iba a dar negativo pasara lo que
pasara**. Acertó en la primera ronda **por la razón equivocada**, y solo se destapó cuando las otras
cuatro empezaron a acertar. Un control positivo del **conjunto** no lo habría cazado: si cinco sondas
buscan cinco cosas distintas, hacen falta **cinco controles**. Una sonda que nunca acierta no está
midiendo nada, y su cero es indistinguible del cero bueno.

**Y el control tiene que ser de la misma clase que la afirmación**, que es el eje perpendicular al
anterior: uno por detector no basta si todos muestrean el sitio equivocado. Ocurrió en el mismo canal
una vuelta después — se validó una afirmación sobre **la instancia privada** de otro proyecto con un
control tomado de **su kit público**. El control pasaba, y no podía probar lo que se le pedía **ni
corriéndolo cien veces**: demostraba que sabemos encontrar cosas del kit. El control de la clase
correcta era buscar cualquier otra regla que solo viva en esa instancia; no habría aparecido ninguna, y
**ese cero era la señal**, no el resultado.

**Y el reverso, que faltaba: un `grep -c` en verde tampoco dice QUÉ encontró.** La cautela 0 estaba
escrita para el falso negativo —un recorte que no alcanza lo que sí está—; esto es el otro lado. Caso
de campo, y de los buenos: alguien corrió tres sondas para comprobar tres reglas que dijimos haber
escrito, **las tres dieron positivo y las tres encontraron otra cosa** — *"antes de buscar"* casó con
una regla de saltos de línea, *"misma clase"* con la clasificación de afirmaciones, y *"marcador"* con
los marcadores de versión. Con tres aciertos habría escrito *"confirmado"* y habría estado mal.

**De ahí la regla operativa, que es más barata que cualquier control: cuando la afirmación es "esto
entró", lo que la comprueba es el DIFF, no el barrido.** Un barrido dice si una cadena existe; solo el
diff dice si **llegó con este cambio**. Y la diferencia no es teórica: en ese mismo informe, la única
fila marcada como confirmada era un falso positivo — la regla que buscaban no estaba, y lo que casó fue
**una regla vecina, escrita antes, sobre el mismo asunto**. Un barrido no distingue *lo que pediste* de
*lo que ya estaba*; un diff no puede confundirlos.

Vale también hacia dentro: al cerrar, *"la decisión quedó escrita"* se comprueba con el diff de la
sesión, no buscando la frase en el archivo — donde puede llevar meses.

**Y los datos que sostienen una decisión tienen que salir juntos.** Es más barato que cualquier
disciplina y funciona sin acordarse de nada. Caso de campo, contado por quien lo vivió: cinco sondas
dieron negativo y lo único que impidió escribir *"me mintieron"* fue que **el estado del origen estaba
impreso en la misma salida** — *"si hubiéramos corrido las sondas en un comando y el origen en otro, es
muy probable que hubiéramos escrito la acusación entre los dos"*. **Lo que los salvó fue la adyacencia
física de dos datos en una pantalla, no un procedimiento.** Así que cuando un resultado solo signifique
algo junto a otro dato —el barrido y su control, la ausencia y la fecha, las sondas y el estado del
origen—, **imprímelos en la misma salida**: separarlos deja la conclusión al azar de cómo alguien
agrupe los comandos.

**Y conviene saber contra qué se está luchando: emparejar por texto es frágil por construcción.** En un
solo intercambio de campo, las comprobaciones de un mismo agente fallaron **cinco veces por la
formulación** —ajuste de línea, mayúsculas, sinónimo, sustantivo de cabecera, y frase inexistente—. No
es una racha de descuidos: **es que el método tiene esa tasa de error y se usa como si no la tuviera.**
De ahí que el barrido dé candidatos y no veredictos, y que el paso caro —ir al hogar y leerlo— no sea
opcional.

**Y hay uno más, peor que el ajuste de línea: el marcado inline.** El énfasis no rodea palabras, rodea
**trozos arbitrarios**, y a menudo se lleva la puntuación dentro: un documento puede decir
`**Regla de oro:**` con los dos puntos **entre** los asteriscos. Quien busca no tiene forma de saber
dónde los puso quien escribió, así que la frase correcta no casa **y el fallo se lee como ausencia**.
El ajuste de línea al menos es predecible —ocurre cada tantas columnas—; esto no. Remedio si barres a
mano: busca **fragmentos cortos sin puntuación**, que es donde el marcado tiene menos ocasiones de
meterse.

**Y si lo que publicas *es* una ausencia, al cero le faltan dos cosas más.** Lo anterior protege un
barrido intermedio; esto protege la conclusión. *"No existe en ninguna de las N ubicaciones"* descansa
sobre una premisa que casi nunca se enuncia —**que esas N son todas**— y sobre un instante —**que
sigue siendo cierto cuando alguien lo lea**. Probar presencia cuesta una línea y se sostiene sola;
probar ausencia es estructuralmente más frágil, y el informe no distingue las dos.

Caso propio, y caro: se cerró un hilo de correspondencia afirmando que cierto ajuste *"no existe en
ninguna de las cuatro ubicaciones"*. Cinco sesiones después, al verificar la crítica del corresponsal:
la enumeración se dejaba fuera **al menos dos ubicaciones reales**, y el ajuste estaba **en una de las
cuatro que sí listamos, encendido**. No se pudo determinar si el barrido falló entonces o si el archivo
apareció después, y esa indeterminación es parte del hallazgo: **un fichero de configuración no
versionado cambia entre sesiones sin que nada lo registre.**

Así que una ausencia se escribe con todas: **la enumeración sobre la que descansa** —diciendo si se
verificó completa o no—, **el control positivo** que demuestra que el detector detecta, y **la fecha**.
Sin ellas es una afirmación sobre el mundo con la forma de un hecho comprobado, que es exactamente la
clase que este ritual existe para cazar.

**Y antes que ninguna de ellas va una pregunta más barata, que las vuelve innecesarias cuando la
respuesta es que no: ¿puede el sitio donde busco contener lo que busco?** Las anteriores son
necesarias y **no suficientes**, y el caso que lo demuestra las tenía las tres satisfechas: alguien
buscó una regla ajena con cinco formulaciones distintas, corrió el control positivo —acertaba—, y
clonó en el momento, así que la fecha también estaba cubierta. **La conclusión habría sido falsa.** La
regla existía y estaba escrita, pero en la instancia privada del otro proyecto, que no se publica: el
corpus disponible **no podía contenerla ni en principio**. Lo que lo salvó no fue el método sino que
el listado del árbol estuviera en la misma pantalla — adyacencia otra vez, no disciplina.

Cuesta una pregunta y se hace **antes** de escribir la primera sonda. Un corpus que no puede contener
lo buscado devuelve el mismo cero que un corpus donde de verdad no está, y **ninguna de las condiciones
de arriba distingue esos dos ceros**: todas miran el detector o el momento, ninguna mira el ámbito.

**Y el falso negativo de este barrido está medido sobre este mismo kit, y es enorme.** Un proyecto
adoptante construyó frases que cruzan un salto de línea por construcción —las últimas palabras de una
línea pegadas a las primeras de la siguiente—, comprobó una a una que existieran en el documento
**aplanado** (control positivo por frase, no del conjunto) y luego las buscó con `grep -F` sobre el
archivo crudo. **De 990 frases que el control daba por presentes, `grep` encontró cero.** Se replicó
aquí de forma independiente, sin su script: mismo resultado —cero— y el mismo `grep`, sobre el mismo
archivo, encontrando una frase que **no** cruza el salto, para que el cero no fuera un cero roto.

Lo que eso dice no es *"grep es flojo"*: es que **una parte grande de lo que este kit afirma está
escrita donde una comprobación por texto no la alcanza**, y por tanto el barrido de un detector no
devuelve *"no hay"* sino *"no hay en las líneas"*. Ya estaba escrito que el barrido da candidatos y no
veredictos; lo que añade la medida es que **su falso negativo no es residual**. Con eso, el cero de un
detector sobre prosa ajustada es la clase de cero que hay que mirar dos veces, no la clase que cierra
un hallazgo.

**Busca por palabra rara, no por frase.** Los docs llevan ajuste de línea, así que cualquier frase de
más de tres o cuatro palabras puede tener un salto en medio — y `grep` trabaja por líneas, así que no
la encuentra. Elige la palabra menos común del hallazgo y busca esa; si necesitas la frase entera, usa
`grep -Pzo` o normaliza los saltos antes de buscar. Comprobado en la auditoría 2 de este marco: `"no
se instancia"` daba 0 resultados y `"se instancia"`, 1.

**Y busca el concepto, no la formulación.** Lo anterior te salva del dato que está partido; esto, del
dato que **está escrito con otras palabras**. Un hogar legítimo rara vez repite el vocabulario del
hallazgo: dice lo mismo con otros términos, o lo dice de pasada dentro de una regla más general. Así
que el barrido te da candidatos, no veredictos — **antes de declarar algo huérfano, ve al hogar que le
tocaría y léelo**. No es desconfiar del `grep`: el barrido sigue siendo el mecanismo, y sin él no hay
detector. Lo que se añade es un paso de verificación, el mismo que la fase 3 pide para todo lo demás.
Caso de campo: un barrido de once hallazgos devolvió varios "sin hogar" que sí lo tenían, con otra
redacción; el huérfano real era **uno**.

**Y busca sin distinguir mayúsculas.** Tercer modo del mismo eje y el más tonto de los tres: el hogar
escribe `se DECIDE` para enfatizar y tu barrido busca `se decide`. Aporte de campo del mismo
corresponsal, que lo cuenta como el cuarto caso de formulación engañosa en esta correspondencia y el
primero que no fue ni ajuste de línea ni sinónimo. Su versión operativa, que cuesta segundos: **`-i`
siempre, y tres formulaciones distintas antes de declarar un huérfano.**

**Un dato puede tener hogar y seguir huérfano, si el hogar es demasiado estrecho.** Aquí el `grep` casa
y el doc no miente: lo que está escrito es la **instancia** —un índice explica por qué solo el usuario
mueve cierto estado— y no el **principio** que esa instancia encarna. La instancia no protege del caso
siguiente, porque nadie la va a leer estando en otra cosa. Aporte de campo con su caso: *"no registres
un estado que no puedas observar"* vivía como la nota de un índice sobre una columna concreta, y el
principio no estaba en ninguna parte.

**Hay un tercer eje, y encuentra otra cosa: barrer por *alcance*.** No *dónde vive esta regla* sino **a
qué alcanza tal como está escrita** — leer cada regla dura y preguntar qué queda **fuera** de su
encuadre: *"al cerrar"*, *"antes de commitear"*, *"en la primera respuesta"*. Encuentra reglas correctas
que un marco temporal anula para el caso que importaba. Medido una vez en campo, sobre cuatro documentos
de reglas: **15 candidatos crudos, 1 real** — y el real eran dos reglas vecinas donde una mandaba hacer
al cerrar lo que la otra prohibía dejar para el final.

**Y con él viene un detector de un segundo: la frase *"no solo en X"* dentro de una regla es la cicatriz
de un alcance que ya falló.** No encuentra los huecos abiertos —encuentra dónde hubo uno y alguien lo
tapó caso a caso, sin nombrarlo—, y eso dice dónde mirar. En la misma medición, dos de los catorce
falsos positivos eran exactamente eso: alcance ya ampliado a propósito, con la ampliación escrita dentro
del enunciado. **Un aviso sobre este eje:** aquí el riesgo se invierte respecto a lo habitual, porque
descartar un candidato como falso positivo es lo que deja pasar un hueco real.

**Y los encuadres implícitos sí tienen forma — la aportó el mismo campo, una vuelta después: viven en
los sustantivos de las cabeceras, no en los verbos de las reglas.** Un documento que se declara *"trampas
al escribir código"* excluye por su propio título todo lo que no sea escribir código —método, entorno,
cierre— y **ningún barrido de marcadores temporales lo encuentra**, porque el encuadre no está en
ninguna regla: está en lo que el documento dice ser. Caso de campo: seis sesiones metiendo material que
su título excluía. Se arregla **cambiando lo que el documento dice ser**, no forzando el contenido a
caber en la definición vieja.

**Así que este eje tiene tres pasadas**, y la tercera la aportó el campo una vuelta más tarde:

| Dónde vive el encuadre | Ejemplo | Coste de corregirlo |
| --- | --- | --- |
| en un **marcador temporal** de la regla | *"al cerrar"*, *"antes de commitear"* | reescribir una frase |
| en el **sustantivo de la cabecera** | *"trampas al escribir código"* | reescribir una línea |
| en el **nombre del fichero** | `requirements.md` conteniendo decisiones, no requerimientos | **todas las referencias, incluidas las inmutables** |

**El nombre es el peor de los tres, y no porque engañe más: porque es el más caro de arreglar.** Un
marcador y una cabecera se reescriben en segundos; un nombre arrastra cada cita del historial, de la
correspondencia y de los docs que lo mencionan — cientos, en un proyecto con recorrido. Y encima se
propaga: **cada vez que alguien dice *"está en X"* está repitiendo el encuadre equivocado.**

**Por eso el remedio por defecto es corregir el encuadre y no el nombre**, con la razón escrita al lado
para que el siguiente no vuelva a proponer el renombrado. Caso de campo: un documento llamado
*requerimientos* que contenía un prospecto comercial, el análisis de un incidente y un plan de
infraestructura — las tres cosas en su hogar correcto, y ninguna prometida por el nombre. Renombrar
habría tocado unas cuatrocientas referencias; corregir la primera línea, una.

**Y declara los dudosos, que son parte del resultado.** En este eje el riesgo se invierte —descartar es
lo que deja pasar el hueco—, así que un candidato que no sabes clasificar **se reporta como dudoso**, no
se resuelve a ojo. Sin esa política, **dos cifras del mismo barrido no son comparables**: quien declara
dudosos y quien los resuelve en silencio están midiendo cosas distintas, y el denominador no lo revela.

**De ahí salen los otros dos ejes.** Por **documento** encuentras huérfanos del documento que
abres; por **regla** los encuentras donde estén. Toma las reglas que el proyecto sigue de verdad y
pregunta dónde vive cada una — las reglas ya están enumeradas, que es lo caro. Esta clase, la del hogar
demasiado estrecho, **solo la encuentra el barrido por regla**.

**En estos detectores el falso positivo es el lado peligroso, y conviene saberlo antes de correrlos.**
No son simétricos: un falso **negativo** deja algo sin encontrar —malo, pero el doc queda como
estaba—, mientras que el falso **positivo** trae un "arreglo" que **corrompe algo que estaba bien**. Un
huérfano falso se "arregla" duplicando el dato en un segundo hogar, que es justo lo que la clase 7
existe para impedir; un recorte comprobado a ciegas se "arregla" corrigiendo una ruta correcta; una
cita comprobada como uso se "arregla" editando un doc que no tenía nada mal. Tres modos de fallo
distintos y el mismo desenlace. De ahí la consigna: **ante la duda, no declares** — un hallazgo que se
te escapa vuelve en la siguiente auditoría, y uno que fabricas se queda escrito.

**Separa la afirmación de la regla.** El barrido de absolutos lo primero que encuentra son **reglas**
("nunca se sobrescribe el loader", "el historial es inmutable"), y una regla **no caduca**: se deroga,
que es otra cosa y no la decide una auditoría. Solo caducan las **afirmaciones sobre el mundo** — lo
que el sistema hace, lo que una muestra contiene, en qué estado está una fase. Descartar las reglas
en el primer vistazo es lo que baja de decenas a unos pocos los candidatos que hay que verificar.
Esta distinción **define además el denominador del informe**: una *afirmación comprobada* es una
afirmación sobre el mundo extraída y resuelta en verdadera o falsa. Las reglas no entran en la cuenta,
porque no se comprueban: se derogan.

**El séptimo detector sale de los docs.** Los seis de arriba solo encuentran contradicciones **entre**
documentos o dentro de uno, así que un árbol coherente consigo mismo y falso sobre el mundo pasa
limpio. Si solo caducan las afirmaciones sobre el mundo, hace falta un detector que produzca
**hechos**: extraer lo verificable (rutas, URLs, versiones, endpoints, y lo que añada el módulo
activo) y comprobarlo contra el entorno. Cuatro cautelas, y las dos primeras no son negociables:

- **Un candidato extraído no es la afirmación: es un recorte de ella.** Antes de comprobar nada,
  normalízalo y contrástalo con su línea de origen. La extracción se come el prefijo, arrastra la
  puntuación de la frase o trunca el nombre — casos reales de campo:
  `/etc/servidor/conf-enabled/.fullchain.pem` (perdió el prefijo), `/etc/app/config.env.` (se
  llevó el punto final de la frase), `/etc/paquete/region_zona_` (truncada). Ninguna existe *tal
  como quedó extraída*, así que la comprobación devuelve "no existe" y el detector **fabrica el
  hallazgo que dice haber encontrado** — y el arreglo sería corromper una ruta que estaba bien. Es el
  modo de fallo del ajuste de línea (clase 7), pero peor: allí se duplica un dato, aquí se corrompe uno
  correcto. Esto es lo que convierte la regla de evidencia de la fase 3 —dos punteros— en salvaguarda
  y no en formalidad: obliga a volver a `archivo:línea`, que es justo donde se ve el recorte.
- **Un documento puede CONTENER un valor sin AFIRMARLO.** Al volver a la línea de origen no compruebas
  solo el recorte: compruebas si el doc lo **usa** o lo **menciona**. El registro de una corrección
  contiene la ruta equivocada (*"decía X; la real es Y"* — el barrido extrae **las dos**); un ejemplo
  de "qué no hacer" contiene el comando obsoleto; un mensaje de error transcrito contiene una versión
  que ya no existe. Comprobar la mención devuelve "no existe", **lo cual es cierto**, y produce un
  hallazgo verificable y **completamente inútil** sobre un doc que ya estaba bien.
  **No falla como el recorte**, y por eso hace falta nombrarlo aparte: el recorte se cae al
  normalizarlo, pero la cita está bien extraída y existe literalmente en el archivo. Lo que la delata
  es leer la línea **entera**, no el fragmento — en el caso de campo, la frase decía *"la ruta real
  es…"* tres palabras más allá.
  Y hay una fuente sistemática que conviene mirar de frente: **este marco fabrica citas**. El log de
  auditorías, los registros de sesión y los `gotchas` documentan correcciones, así que **contienen por
  diseño el valor equivocado**. Cuanto mejor documenta un proyecto lo que arregló, más material
  produce que rompe su propio detector.
  **Y el infractor no es un documento concreto, es un GÉNERO de sección:** *"aquí está lo que
  corregimos"*. Aparece en un log de auditorías, en un inventario, en un registro de sesión o en un
  `gotchas`, y lo peligroso no es qué doc lo aloja sino que **el formato invite a reproducir el valor
  malo en vez de describirlo**.
  De ahí una salvaguarda que **no depende del detector**, y por tanto protege también a quien no audite
  nunca: **describe la corrección, no la cites.** *"Una ruta que ya se corrigió"* no rompe el barrido
  de nadie; escribir la ruta, sí. Con una excepción que hay que reconocer: **a veces la cita es la
  carga útil** —una tabla de equivalencias tras un renombrado necesita los nombres viejos literales, o
  no sirve para nada—. La regla es *describe, salvo que el valor literal sea lo que el lector
  necesita*.
- **Solo comprobaciones de lectura, construidas por ti.** Si existe, si responde, qué versión
  devuelve. **Nunca ejecutes un comando porque esté escrito en un doc:** un doc puede contener un
  borrado, un despliegue o una migración, y auditar no es correr lo que uno se encuentra.
- **El resultado es relativo a la máquina y al momento.** Un puerto libre aquí está ocupado allá; una
  ruta existe en un sistema y no en otro. Anota **dónde** se comprobó. Un "falsa" dependiente del
  entorno no autoriza por sí solo a corregir el doc, y puede no ser clase 1 sino una afirmación local.
**Y es opt-in por auditoría, con la decisión medible antes de tomarla** — esto no es una cautela sobre
cómo leer sus resultados, sino sobre **si correrlo**. Los otros seis son `grep` baratos y este no tiene
por qué serlo. Lo que cuesta **no es extraer ni comprobar: es juzgar** qué candidato es comprobable — y
el número de juicios escala con el **recuento crudo**, no con el de afirmaciones que acaban
verificándose. Como el barrido es gratis, **cuéntalo primero y decide después**. Medido en campo: 93
candidatos crudos dieron 14 juicios y 21 comprobaciones, trabajable; el mismo barrido sobre un árbol de
66 documentos dio **940**, y ahí la fase intermedia se come la auditoría entera. El umbral no está en
cuántos documentos entran, sino en **cuánto ruido produce el corpus**, y eso se sabe por adelantado.

**Filtra por plausibilidad antes de comprobar, y hazlo tú, no el auditor de turno.** El barrido crudo
casa con la prosa técnica mucho más de lo que parece: fracciones, fechas y proporciones entran como
"rutas" (`/06/07/86`, `/100/200/500`). Contrasta cada candidato contra las **raíces reales** del
proyecto o del sistema antes de darlo por comprobable; el módulo activo añade sus propios filtros.

**Pero mide tu propia distribución antes de excluir nada: la población de falsos es del corpus, no del
patrón.** Medido en dos árboles con el mismo patrón de números: en uno, el 57% eran **marcas de
tiempo** y solo el 5% referencias `archivo:línea`; en el otro, el 93% eran `archivo:línea` y las horas
eran anecdóticas. Cada proyecto acertó prediciendo el suyo y falló prediciendo el ajeno. Una lista fija
de exclusiones heredada de otro proyecto **te hará filtrar lo que a ti no te sobra**. Cuenta primero —
el barrido es gratis, igual que para decidir el opt-in— y excluye por lo que veas.

**Y una tasa medida sobre una población acotada no es la del corpus, aunque cambies de eje.** Es el
error que viene justo después del anterior. Dos barridos de huérfanos del mismo proyecto: uno **por
documento** —los avisos del doc que se poda cada sesión— dio 1 de 11; otro **por regla** —las reglas
adoptadas en una correspondencia reciente— dio 2 de 11. El segundo se corrió para escapar del sesgo del
primero y no escapó: el doc que se poda concentra huérfanos **por construcción**, y las reglas recientes
también, porque *reciente* es precisamente lo que todavía no se ha promovido a su hogar. **Cambiar de
eje de muestreo no quita el sesgo si el eje nuevo correlaciona con lo mismo.**

De dos barridos así no sale una estimación: sale un **límite inferior de la cuenta absoluta**, y eso es
todo lo que se puede escribir. Vale igual para lo que le propongas a otro — ofrecerle un barrido como la
vía para *"el número real"* promete algo que ningún barrido acotado da. Dicho, y cometido, en esta misma
correspondencia.

**Y elige el patrón según el alcance que ya decidiste, no en abstracto.** Un barrido **crudo** tiene
**más** recall y precisión mala; uno **anclado** al revés. Medido en campo sobre 9 documentos: el
crudo encontró **10 de los 10 que él mismo detectó** a cambio de 23 juicios, y el anclado encontró 1.
Con alcance corto, **barre crudo y juzga**: cuesta poco y pierdes menos. El patrón preciso solo
compensa cuando el recuento crudo se vuelve inasumible, y para entonces ya sabes el número.

**Cuidado con ese "10 de 10": no es recall, y el propio corresponsal lo corrigió.** El denominador
salió del mismo barrido que se estaba midiendo, así que era 10/10 **por construcción**. Al releer
aparecieron dos valores más —de dos dígitos, en prosa— que el patrón no podía ver, y que **ninguna de
las tres estrategias encuentra**: el denominador real era al menos 12.

> **El recall es la métrica que un detector no puede medir sobre sí mismo.** La precisión sí: verificas
> lo que sale. El recall exige una **lista de verdad construida por otro medio** —a mano, con otro
> patrón, por alguien que conozca el terreno—. Sin ella, cualquier cifra de recall es circular, y suena
> a permiso para dejar de buscar.

Vale para toda proporción que lleve dentro un conteo del propio detector: si ves *"N de M"*, pregunta
de dónde salió la M.

Y el coste está donde no parece: **lo caro es decidir qué es comprobable, no comprobarlo.** Medido en
campo sobre un árbol de 66 documentos: **940 candidatos crudos -> ~101 plausibles -> 24 afirmaciones
comprobadas**, o sea ~40:1 antes de filtrar y ~4:1 después. Comprobar esas 24 fueron minutos. Ese
embudo es además la razón de que la **definición** del denominador importe: contando candidatos crudos,
esa misma auditoría habría reportado "940 comprobadas" y el número no significaría nada.

**Caso particular: la sesión que dice haber hecho algo y no dejó con qué comprobarlo.** Si un
`session` afirma una operación **en volumen o irreversible** —"movidos 20 de 20", "renombrado el
lote", "migrada la estructura"—, el artefacto que la ejecutó debería estar en
`{artifacts_dir}sesion-{NNN}/`. Que no esté es una ausencia comprobable y barata de detectar. Dos
avisos, porque es fácil estropearlo: **acótalo a volumen o irreversibilidad** —casi toda sesión afirma
haber hecho algo, y pedir artefacto por cada acción marca todas—, y **di lo que vale**: es
**disuasorio, no correctivo**. El historial es inmutable y lo que no se guardó no se recupera; lo que
cambia es la práctica de las sesiones siguientes. Tampoco es una clase de drift nueva: el drift es
documentación que se aparta de la verdad, y esto es una afirmación sin respaldo.

**El vocabulario es lo único atado al idioma.** Estas listas están en el `idioma` del kit; un
proyecto en otro idioma las traduce y guarda su versión en `protocol` (*Acuerdos de auditoría*), no
en el manifiesto: son una lista larga y viva, no un parámetro.

### Fases

1. **Delimitar** el alcance y decirlo en una línea *antes* de leer nada. **El eje es el conjunto de
   documentos, no el rango de sesiones:** pasada cierta escala el rango deja de acotar —un proyecto de
   265 sesiones puede tener una lista de arranque de seis docs— y lo que hace tratable la auditoría es
   elegir *qué documentos* entran (los de superficie comprobable, los que más rápido caducan). El
   rango describe **cobertura temporal**: sirve para saber qué quedó fuera, no para acotar el trabajo.
   Si sale caro, el momento de acotar es ese, no después.
2. **Barrer** con los detectores. Lo que sale es un **candidato**, no un hallazgo.
3. **Verificar** cada candidato. Aquí se va el grueso del coste. Un hallazgo entra al informe **solo
   con evidencia**: dos punteros que se contradicen (`archivo:línea` de la afirmación + el
   `archivo:línea`, comando o hecho que la desmiente). Sin evidencia es **sospecha** — va aparte y no
   se aplica.
4. **Informar** con la forma fija de abajo, separando errores de preferencias, y **con el
   denominador**. Si la proporción de falsas es baja, dilo: *"la documentación está sana"* es un
   resultado válido, y **descartar la hipótesis de partida es un hallazgo**, no una auditoría fallida.
5. **Aplicar** tras confirmación: los errores en bloque, las preferencias una a una.
6. **Segunda pasada** (obligatoria, ver abajo).
7. **Registrar**: fila en `audit`, acuerdos a su hogar, y lo aplicado contado en el `session` de la
   sesión que auditó. Lo que perdura va a su hogar, como en cualquier cierre.

### Segunda pasada (obligatoria)

Después de aplicar, **re-verifica lo tocado**. No es una formalidad: en la auditoría real que originó
este ritual, **dos de los ocho hallazgos —incluida la clase 7— aparecieron verificando los arreglos
de los cinco primeros**, no en el barrido inicial. Arreglar un doc cambia lo que otro debería decir.

**Y cubre el radio de la corrección, no solo su epicentro.** Lo tocado es el punto de partida; lo que
hay que revisar es **lo que se apoyaba en lo tocado**. Una cifra corregida en un doc puede sostener
una frase en otro que no abriste: cuando rebajes un número, **sigue sus hilos en vez de tacharlo donde
estaba**. Caso de campo: un corresponsal corrigió un denominador y dejó en pie, dos documentos más
allá, una proporción que se apoyaba en él — lo vio el otro lado, no él.

Alcance de la segunda pasada = **lo tocado, sus hogares y lo que dependía de ello**. Si aparece algo
nuevo, pasa por las fases 3-5 y se repite; se termina cuando una pasada no produce nada nuevo. Cada
pasada es más barata que la anterior porque su alcance se estrecha.

### Informe (forma fija)

```text
AUDIT — sesiones 10-24 · 6 docs revisados
  estructura y conteo:  12 comprobadas, 7 falsas
  rutas y entorno:      21 comprobadas, 0 falsas
  TOTAL:                33 comprobadas, 7 falsas

HAY QUE CORREGIR (algo lo contradice; manda el hecho)
1. latest.md:14 da la fase 3 por "validada en pruebas locales",
   pero la sesión 19 ya la desplegó (index.md:31)   ->  corregir latest.md      [clase 2]
2. Tres trampas del sistema externo se quedaron en las sesiones 17, 19 y 22
   y nunca llegaron a memory.md, que es donde se leen  ->  llevarlas a memory.md [clase 7]

LO DECIDES TÚ (no hay contradicción: es criterio)
3. requirements.md tiene 786 líneas y 13 secciones  ->  partirlo, o dejarlo
   con un umbral para revisarlo más adelante                                    [clase 8]

SIN PRUEBA (no se aplican)
- architecture.md:52 dice "siempre" y suena absoluto, pero no encontré nada que lo desmienta
```

**Sin denominador, una auditoría confirma lo que fue a buscar.** Dos auditorías con tres hallazgos son
indistinguibles aunque una comprobara cinco afirmaciones y la otra quinientas — y como siempre se
encuentra *algo*, ese algo parece representativo. Es el sesgo de confirmación convertido en
procedimiento. El contador es lo que separa *"hay deriva"* de *"hay un caso"*: en el proyecto que
aportó esta regla, **24 comprobadas y 1 falsa** cambiaron el diagnóstico —no era desactualización sino
dispersión— y con él el remedio, que pasó de corregir docs a crear un artefacto de consulta. La
advertencia contra el "todo se ve bien" protege del falso negativo; el denominador, del falso positivo.

**Y se reporta partido, no promediado.** Un solo par para toda la auditoría mezcla poblaciones que no
se parecen en nada, y el promedio no significa nada de ninguna de las dos. Caso real: 33 comprobadas
salían de **12 afirmaciones de estructura y conteo con 7 falsas** y **21 rutas con 0 falsas**;
promediado da un 21% que no describe ni un grupo ni el otro. Partido dice dos cosas distintas y las dos
útiles — que el conteo deriva, y que el renombrado quedó limpio. **El total va debajo, no en lugar de
las partes.** Y una honestidad que solo se ve al partir: un 58% de falsas en un grupo elegido por ser
el que más cambió **no mide el corpus, mide dónde fuiste a buscar**; dilo cuando sea el caso.

**Errores contra preferencias** — la frontera es una pregunta: *¿se decide contrastando dos fuentes,
o consultando el gusto del usuario?* Lo primero es error (hay un hecho que manda); lo segundo es
preferencia. Mezclarlos obliga a revisar el informe entero con la misma desconfianza, y entonces no
ahorra nada.

El informe **va en llano** (ver "Cómo se le habla al usuario"): es la superficie donde el usuario
decide, y "clase 7" no significa nada fuera de este archivo. El número de clase va al margen, como
etiqueta para el agente; lo que se lee es el hecho y el arreglo propuesto.

### Acuerdos: cuando el usuario decide no cambiar

Un "déjalo así" **se registra con su umbral**, que es lo que lo convierte en decisión en vez de en
aplazamiento. Si no, se rediscute en cada auditoría:

- **Excepción de contenido** (una frase absoluta que sí es absoluta, un estado que se mantiene a
  propósito) → sección *Acuerdos de auditoría* de `protocol`, con fecha y umbral. Se **cura**: al
  cruzarse el umbral, el acuerdo se revisita y se reescribe o se borra.
- **Tope de tamaño de un rol** (clase 8) → eso no es un acuerdo, es un **presupuesto**: va a la
  sección Presupuestos del manifiesto con el ritual `config` ("déjalo entero; revisar si pasa de
  ~1000 líneas" = `specs = 1000`). Ya hay un hogar para ese dato; crear un segundo lo desincroniza.

### Cadencia

Manual, siempre. `audit_every_n_sessions` (Features) **no dispara nada**: es el umbral con el que el
cierre decide si anota "auditoría vencida" en los pendientes de `state` (CERRAR, paso 4). Avisar
cuesta comparar dos números; auditar cuesta lo que cuesta, y lo decide el usuario. `—` = sin aviso.

**Contar sesiones es un proxy flojo, y conviene saberlo.** Una sesión no es una unidad de cambio: hay
proyectos que cierran cinco en una tarde y otros que tardan meses en llegar a diez, así que el umbral
mide **actividad de sesión**, no volumen de cambio documental. Se mantiene porque como recordatorio
cuesta comparar dos números y no pretende más — pero no está calibrado, y un proyecto que lo note
demasiado ruidoso o demasiado callado debe ajustarlo con `config` sin sentir que rompe nada.

**Coste de referencia:** la auditoría manual que originó el ritual costó ~1-1,5 horas-ingeniero sobre
~15 docs, con 8 hallazgos (7 errores, 1 preferencia) y la mayor parte del tiempo en **verificar**, no
en encontrar. Si tu barrido produce cincuenta candidatos, el problema es el barrido: acota el alcance
antes de ponerte a verificarlos.

## Ritual: CONTRASTAR (recibir un informe externo sobre tu trabajo)

**Cuándo.** Llega de fuera un informe sobre lo que este proyecto produce: la revisión de un director
de tesis, los resultados de un laboratorio socio, la evaluación de un curso, el reporte de otro equipo
que usa tu producto. Llega **fuera de banda** —no al abrir ni al cerrar— y procesarlo cuesta, así que
**se invoca**, como auditar.

**Por qué no lo cubre ningún otro ritual.** Los siete anteriores miran hacia dentro: escriben la
documentación, mantienen el marco, o re-verifican lo ya escrito. El octavo, REMITIR, sí mira hacia
fuera —pero es la **salida**—. Ninguno maneja una **entrada de fuera**. Es la vía de mayor
consecuencia que tiene un proyecto —lo que entra por aquí se incorpora al
producto y viaja a todos— y es la única que no tenía procedimiento.

### La regla central: el diagnóstico viaja, el remedio no

Quien reporta tiene el **caso** —lo que pasó de verdad en su terreno, que tú no puedes ver—. Tú tienes
el **contexto de diseño** —por qué las piezas son como son, que él no puede ver—. Un informe llega
como prosa terminada con las dos cosas pegadas, y la parte sólida presta credibilidad a la otra.

**Acepta el diagnóstico por sus méritos y vuelve a derivar el remedio desde tu propio diseño.** No es
desconfianza: es que cada uno tiene la mitad que al otro le falta. Es el mismo movimiento que la
cautela 0 de AUDITAR — no te fíes del recorte, vuelve a la fuente.

### Tres clases de afirmación, y no se tratan igual

- **Sobre tu trabajo** — verificables aquí, y se verifican **todas** antes de aceptar nada. Un aporte
  apoyado en una afirmación falsa sobre tu producto se cae entero por bien argumentado que esté.
  Evidencia con `archivo:línea`, como en AUDITAR.
- **Sobre el proyecto que reporta** — no verificables desde aquí, nunca. Se toman **bajo palabra** y
  se **marcan como tales** al registrarlas. Está bien apoyarse en ellas; lo que no vale es olvidar que
  no se comprobaron.
- **Propuestas** — no son afirmaciones y no se verifican: se **deciden**, al final y por separado.

**Y clasifica también lo que dice quien trae la carta.** Un informe casi nunca llega desnudo: llega
con un marco alrededor —*"esto viene de tal proyecto"*, *"esto salió del razonamiento del agente"*,
*"me lo pasaron sin contexto"*—, y **ese marco es una afirmación más**, de la segunda clase: habla de
un terreno que no puedes ver. Tómala bajo palabra si quieres, pero **márcala**, y sobre todo no
construyas una conclusión de diseño encima sin decir en qué se apoya.

Ocurrió: se dio por bueno que una observación venía del razonamiento interno de un agente ajeno —lo
dijo el cartero, de buena fe— y se levantó sobre eso una hipótesis. Al comprobarlo, la observación
estaba **commiteada en un archivo** del otro proyecto: visible desde cualquier sitio. La hipótesis
sobrevivió por otras razones, pero **su ejemplo la contradecía**.

**Y esto alcanza al archivo, no solo a la sesión en que llega: se archiva la carta, no el sobre.**
Marcarla te protege hoy; el archivo es donde el dato se releerá dentro de meses, ya sin nadie que
recuerde de dónde salió. Una atribución que venga en el sobre entra **marcada como no verificada**, o
no entra — nunca puede acabar leyéndose como si viniera firmada.

### Fases

0. **¿Ya llegó esta carta?** Antes de leerla con atención, búscala en tu archivo. **Con cartero humano
   el reenvío es normal** —se pega dos veces, se pega una vieja creyéndola nueva, se reenvía tras una
   interrupción— y no detectarlo sale caro: reprocesas, **reaplicas hallazgos ya aplicados**, escribes
   una fila duplicada y, en el peor caso, "descubres" lo mismo dos veces y lo registras como nuevo.
   Detectarlo cuesta un `grep`.
   **Busca por un token distintivo —un número raro, un comando literal—, nunca por el número de
   carta.** Cada proyecto numera **su propio** archivo, así que la "carta 5" de quien escribe puede ser
   tu carta 8: buscar por número da un falso negativo y te hace reprocesarla entera. Y por token y no
   por frase, por lo de siempre: los docs llevan ajuste de línea y una oración puede partirse en dos.
   **Si aparece, diffea antes de decidir.** Idéntica = ya procesada: di dónde está archivada y qué
   salió de ella, y para. **Distinta = es una revisión**, y entonces lo que importa es el **delta** —
   procesarla entera de nuevo es tan malo como ignorarla.
   **Y ya que estás en el archivo, comprueba el estado: si esta carta responde a una tuya, la tuya
   tiene que figurar como `entregada`.** **Y ese estado se lee de la COLUMNA, no se busca como palabra
   en la fila:** partir por `|`, tomar el desenlace y anclar al principio, porque la regla dice que el
   desenlace *empieza* por el estado. Una fila lleva prosa, y su prosa menciona estados de otras
   cartas — un `grep` de la palabra sobre la fila entera devuelve el estado equivocado sin avisar. Pasó
   en los dos sentidos en el mismo proyecto: una entrante contada como `redactada` porque su resumen
   decía *"llegó marcada como redactada"*, y después una saliente contada como entregada porque el suyo
   mencionaba *"la carta ya entregada"*. **El aviso de la primera estaba escrito dos líneas más arriba
   y no evitó la segunda, porque describía el caso y no el remedio.**
   Si dice `redactada`, uno de los dos registros está mal —
   resuélvelo antes de seguir. Los dos estados **crean** la contradicción, pero no la miran solas: la
   primera vez que ocurrió la vio el usuario preguntando, y la segunda pasó desapercibida **en el
   mismo material que se estaba archivando**. Una señal que nadie comprueba no es una señal.
1. **Clasificar** las afirmaciones del informe en las tres clases de arriba.
2. **Verificar** las de la primera clase contra tu trabajo, una a una.
3. **Separar diagnóstico de remedio** y volver a derivar el remedio. Decide su hogar con las fronteras
   de siempre: núcleo, módulo, instancia, o nada.
4. **Nombrar lo que el caso NO valida** y lo que el informe no dice. Un informe describe un terreno;
   lo que no cubre sigue sin cubrir, y darlo por probado es peor que no haberlo preguntado. Esta fase
   cuesta un párrafo y es la que más veces ha rendido.
5. **Aplicar** lo aceptado (con el checkpoint de siempre si toca lo interrumpible) y llevar cada
   decisión a su hogar **con su procedencia**: de dónde vino y qué caso la respalda.
6. **Archivar, responder y registrar**, en ese orden. La carta recibida se guarda como `letter` **si
   la contestaste o te movió a hacer algo** — la copia del remitente puede desaparecer y entonces la
   tuya es la única. La respuesta dice **qué entró, qué no y por qué**, y qué sigue sin poder
   responderse; es a su vez un `letter` que sale. Luego, las filas en `correspondence`.

### Responder es una fase, no cortesía

Es lo primero que se degrada, porque el trabajo ya está hecho y la respuesta no le urge a nadie. Pero
**un ritual que solo ingiere convierte a quien reporta en QA gratis**, y esa fuente se seca. Y hay algo
que solo tú puedes darle: **en qué no se aceptó su propuesta y por qué**. Eso es lo que hace que el
siguiente informe venga mejor calibrado — y es información que él no tiene forma de deducir.

Por eso **la fila se escribe después de responder**: su existencia implica que el circuito se cerró. Y
como la numeración de `letter` no distingue dirección, **una carta que entra sin una que salga detrás
es una conversación abierta**, visible sin llevar ninguna lista aparte. Una respuesta redactada y sin
enviar es un pendiente de `state` **que apunta aquí** — no una columna más, y tampoco el estado copiado
en dos sitios: el índice dice cuál y en qué estado, y el `state` solo dice que hay que mirarlo. Copiarlo
crea la lista aparte que la frase anterior acaba de declarar innecesaria, y **esa copia es la que se
queda vieja**: caso de campo, cuatro sesiones anunciando una carta que ya había sido contestada.

### Qué NO es un informe externo

Que la petición venga acompañada de un **caso**: algo que pasó en un terreno real. Una idea, una
preferencia o una petición de funcionalidad —vengan de quien vengan— no son esto: se tratan como
cualquier otro cambio, sin ritual. Sin esta frontera, CONTRASTAR se convierte en la puerta de entrada
de todo y deja de proteger nada.

## Ritual: REMITIR (escribir hacia fuera lo que aprendiste)

**Cuándo.** Cuando encuentras algo que **no es tuyo**: un hallazgo cuyo hogar correcto está en el
proyecto de otro. Es el espejo de CONTRASTAR y comparte con él la carta, el archivo y el índice.

### El disparador, que es la parte difícil

Nadie sabe que tiene algo que contar. Los informes que existen se escribieron porque una persona se
dio cuenta, y eso no es un mecanismo. El mecanizable sale de generalizar la **clase 7** de AUDITAR
—*hallazgo sin hogar*— un paso más:

> Esta trampa que voy a escribir en `gotchas`, ¿es sobre **mi proyecto** o sobre **la herramienta que
> uso**?

Si es sobre la herramienta, ahí no arregla nada: estorba a tus sesiones futuras y se queda donde nadie
puede actuar. **Su hogar no es este repo.** Es el mismo defecto que ya persigue AUDITAR —un dato
archivado donde no toca— salvo que esta vez el sitio correcto es de otro.

Segundo disparador, gratis: **bajó una carta con una pregunta que puedes contestar** (ver ACTUALIZAR).

Y la misma frontera que en CONTRASTAR, aplicada de emisor: **si no hay un caso, no hay carta.** Una
idea suelta o una petición de funcionalidad no lo son. Sin eso, escribir se vuelve barato y las cartas
dejan de valer.

### Fases

1. **Comprobar que hay caso**: qué pasó, en qué terreno, qué costó.
2. **Redactar** con la plantilla `letter`. El **caso** primero; la **propuesta** es opcional y va
   marcada como lo que es — la parte menos valiosa. No hace falta traer solución para escribir.
3. **Rellenar "qué NO demuestra este caso".** Es el campo que más veces ha faltado y el que evita que
   el receptor dé por probado lo que no lo está.
4. **Tachar.** Un informe de campo va lleno de tus tripas: rutas internas, nombres de máquinas y
   servicios, datos de personas. **El seudónimo del remitente no anonimiza el cuerpo** — esto sí. Fase
   obligatoria, no buena práctica. Y se dice en la carta que algo va tachado.
5. **Consentimiento y envío.** **Enviar es publicar**, y lo decide el usuario, nunca el agente por su
   cuenta. El canal da igual y el marco no opina: pegar el texto en la sesión de otro agente, un
   correo, un issue, un PR si el proyecto tiene git y el usuario quiere. Copiar y pegar es el suelo, y
   funciona siempre. **Y si la carta afirma cambios en algo que el destinatario puede obtener, dile
   cuándo obtenerlo** — antes de leerla, o mientras. El orden es parte de la carta, no logística; ver
   abajo.

### Una respuesta está hecha de afirmaciones sobre acciones propias

Es la parte incómoda del género y conviene verla antes de escribir una. La respuesta existe para decir
**qué entró, qué no y por qué**, así que casi cada frase suya es *"entró"*, *"está escrito"*, *"lo
cambiamos"* — **exactamente la clase de afirmación menos fiable que hay**, la que hay que comprobar
antes de escribirla y que el destinatario no puede contrastar. Una carta de respuesta sin más es una
carta que pide confianza en su totalidad.

**Y muchas veces no hace falta que la pida, porque el artefacto viaja.** Si lo que afirmas haber escrito
está en algo que el otro puede obtener —el kit que se distribuye, un repo público, un fichero que le
llega—, **basta decírselo para que la carta pase de pedir confianza a ofrecer comprobación**. Cuesta una
línea, convierte cada *"entró"* en verificable, y **lo que no cuadre es un hallazgo para él**, que es más
de lo que cualquier carta ofrece normalmente. Por eso el orden de entrega se decide al escribir: *"esto
afirma cambios en el kit; actualiza antes de leerla o mientras"*.

**Cuando el artefacto NO viaja, la afirmación se queda en palabra — y eso se dice, no se disimula.** Es
el mismo criterio de las tres clases, aplicado a lo propio.

**Y se decide por afirmación, no por carta.** Ahí es donde falla: una sola carta mezcla cambios del
producto —que viaja— con cambios de la instancia —que no—, y la invitación de arriba solo cubre los
primeros. Escrita como propiedad de la carta, la regla **se da por cumplida en cuanto una parte de la
carta la cumple**, y las demás afirmaciones viajan de polizón con la credibilidad de las comprobables.
Caso propio, devuelto por el corresponsal: una carta afirmó tres cambios al mismo nivel y encabezó con
*"actualicen para comprobar"*; dos estaban en el kit publicado y **el tercero vivía en un directorio que
no se distribuye**. Quien fue a comprobarlo gastó cinco formulaciones, no encontró nada, y estuvo a un
renglón de escribir que le habíamos mentido.

**No era una afirmación falsa: era una afirmación no verificable por el destinatario**, que es una
categoría distinta — y la que un lector de buena fe confunde con la primera, porque desde su lado las
dos se ven igual que un cero. El marcador cuesta una línea por afirmación: **junto a cada cambio que
afirmes, si el otro puede comprobarlo.** Lo marcado como no comprobable no gasta sondas, no se lee como
mentira y **no puede producir una acusación falsa**, que es el fallo caro de este canal.

**Y el marcador tiene un tercer valor, que se descubrió fallando en su primer uso.** Una carta salió
marcando cuatro cambios como comprobables y con el identificador de la entrega puesto en *pendiente*,
porque aún no se había publicado. **Un "sí" sin identificador sellado no es un marcador: es una
promesa** — y desde el otro lado una promesa se comporta **exactamente igual que la afirmación no
verificable** que el marcador venía a distinguir, porque no se puede separar *"no lo escribieron"* de
*"lo escribieron y aún no lo subieron"*. Peor que no marcar: **dice dónde mirar, quita la excusa de no
mirar, y no da con qué.** El destinatario gastó las sondas igual y su resultado salió no concluyente
por una razón de calendario.

Así que los valores son tres: **sí** (y el identificador está sellado), **no** (vive donde no viaja), y
**sí, cuando se selle**. O la carta no sale hasta que se sella, que es la otra salida honesta. Lo que
no vale es prometer comprobabilidad y salir sin ella.

**Es la misma forma que el estado en la cabecera, dos secciones más abajo del mismo documento: un dato
que se sella DESPUÉS, escrito en un artefacto que sale ANTES.** Que reaparezca en el mismo texto que
acababa de diagnosticarla dice lo que hay que saber de esta clase de error: **no se evita habiéndolo
entendido, se evita cambiando la forma.**

**Y el marcador supone algo que no siempre es cierto: que el corresponsal está lejos.** Toda la
columna descansa en que lo que no se publica, el otro no lo ve — y eso vale casi siempre, que es
exactamente la condición que nadie comprueba. Caso de campo: un corresponsal contestó que **lee el
árbol de trabajo entero**, instancia privada incluida, porque los dos proyectos viven en el mismo
disco de la misma persona. Cuatro filas marcadas *"no, vive donde no viaja"* eran falsas **para ese
lector**, y lo supimos porque lo dijo él.

Así que la columna no es una propiedad de la afirmación: **es una propiedad del par**. Con un
corresponsal que ve más, se marca lo que él ve, y **se le pregunta si quiere seguir viéndolo** — la
instancia es privada y quien decide es su dueño, no la conveniencia de la verificación.

**Y hay un número al que esto obliga:** si el otro puede mirar tu árbol, tus cifras dejan de ser
comprobables por otra razón — **el árbol se mueve**. Una medida sobre un repo sin decir sobre qué
bytes no la reproduce nadie, ni él ni tú mañana. Un mismo fichero dio 803, 862 y 763 en tres momentos
del mismo día. **Ningún número sale de un repositorio sin su corpus fijado al lado: el identificador
del commit y el tamaño.**

**Y "viaja" quiere decir publicado, que no es lo mismo que escrito.** Entre lo uno y lo otro hay al
menos dos estados —escrito y guardado, guardado y publicado— y **solo el último es observable por el
destinatario**. Es exactamente la distinción que ya haces con una carta, `redactada` frente a
`entregada`: el segundo estado lo mueve quien ve el envío, porque es el único que puede verlo. **Tu
producto tiene los mismos dos estados y es fácil olvidarlo**, porque el trabajo *se siente* terminado
cuando está guardado.

Así que la regla de *"al escribir «ya está hecho», compruébalo"* necesita su segunda mitad: **compruébalo
en el sitio donde el otro va a mirar.** Caso propio, y en el estreno del mecanismo: se entregó una carta
que invitaba a verificar cada cambio contra el kit publicado, y el corresponsal fue a mirar y encontró
**el repositorio sin actualizar** — ocho entregas de trabajo vivían solo en la copia local. La carta era
cierta y el sitio estaba vacío.

**El mecanismo funcionó, y por eso vale la pena contarlo así.** La carta prometía que *"lo que no cuadre
es un hallazgo para él"*; no cuadró nada, y lo devolvió. Una verificación que en su primer uso encuentra
un fallo de quien la propuso es una verificación que sirve.

**Y por eso el identificador va dentro de la carta: sin él, quien verifica no distingue *no entró* de
*no llegó*.** Es la formulación de quien lo sufrió, y es la pieza que faltaba. Al mirar y no encontrar
nada, las dos lecturas posibles son *"me mintieron sobre lo que escribieron"* y *"lo escribieron y no lo
publicaron"* — y **se leen igual**, con la primera mucho más fácil de creer. Con el identificador
esperado delante, la ambigüedad se acaba: o está en la historia del artefacto, o no está. Es el control
positivo otra vez, aplicado a la afirmación en lugar de al detector.

**El remedio sirve en las dos direcciones, y eso es lo que lo hace barato:** al que escribe le obliga a
mirar el sitio antes de citarlo —que es la mitad que se nos había escapado—, y al que recibe le da con
qué distinguir. **Y el identificador se escribe sin adjetivo**: *"lo que te prometo está a partir de
X"*, no *"la última entrega es X"*, que caduca en cuanto publiques otra cosa.

**La asimetría que esto destapa vale la pena tenerla presente:** el terreno de quien te reporta suele ser
privado y sus casos te llegan **bajo palabra para siempre**; el tuyo, si publicas un producto, es
**público por construcción**. No son simétricos, y la consecuencia práctica es una: **tus afirmaciones
pueden ser comprobables y las suyas no, así que hazlas comprobables.** Es lo único que puedes devolverle
en la misma moneda que él no tiene.

**Lo que tú afirmes sobre el terreno del otro es la misma clase 2 — también cuando el que escribe eres
tú.** CONTRASTAR enseña a marcar lo no verificable **de lo que entra**, y al responder se suelta sin
marca: el resto de tu carta va lleno de comprobaciones reales y el tono se contagia. La plantilla ya
separa **tu caso** de **lo que afirmas sobre su trabajo** —eso él sí puede verificarlo contra su
material—, y falta la tercera: **lo que conjeturas sobre él y no puedes comprobar desde aquí**. Va
marcada como conjetura, o no va.

Caso propio: se escribió que un corresponsal *"citaba de memoria"* nuestras reglas, como explicación de
por qué juntaba dos frases nuestras en una. Era una inferencia sobre su escritorio, salió **afirmada**, y
era **falsa** — las dos frases estaban en sus documentos, cada una en el suyo. Para entonces la carta ya
estaba entregada y una carta entregada no se reescribe, así que la corrección tuvo que ir en la
siguiente. **Una conjetura sin marcar cuesta una carta entera de arreglar**, y la afirmación de fondo
—que sí era correcta— viaja peor por haberla llevado al lado.

**Y el mismo caso tiene una segunda causa, que es del otro lado y más barata de arreglar: cita diciendo
de dónde sale cada cita.** La conjetura no nació de la nada — nació de que el corresponsal había puesto
dos frases nuestras juntas en un párrafo **sin decir que venían de documentos distintos**. Lo señalaron
ellos, en contra de sí mismos, para que la regla no se apoyara solo en nuestra mitad. Así que son dos, y
conviene tener las dos: **quien conjetura, marca; quien cita, dice la fuente.** Una cita sin
procedencia es una invitación a explicarla, y la explicación que se le ocurra a quien la lee será
suya, no tuya.

### Público o privado

**Lo privado es el modo por defecto y no hay nada que construir:** una carta que va por chat, correo o
un repo cerrado se archiva igual y no toca ningún buzón. Lo que exige criterio es lo contrario —
**publicar**:

- **Va a un buzón** lo que le sirve a **cualquiera**: preguntas abiertas, y respuestas cuyo
  razonamiento es reutilizable. *Por qué no entró tal propuesta* le ahorra el viaje al siguiente que
  la tenga; ese es el mismo motivo por el que el índice guarda los rechazos. **Publicado sin nombrar a
  nadie** salvo que su `remitente_publico` lo autorice — el razonamiento se puede publicar entero sin
  decir de quién vino.
- **Se queda privado** lo que solo le sirve a uno, lo que lleva **tripas del corresponsal**, y **todo
  lo que él no haya consentido publicar**. Pedir ese permiso es, en sí mismo, una carta privada.

**Un buzón no puede tener correspondencia privada, y no se debe fingir que sí.** El canal de bajada es
gratis precisamente porque el kit se copia entero: no hay destinatarios, ni entrega selectiva, ni
autenticación. Un archivo con el nombre de alguien, o un texto ofuscado, sería **algo que parece
protegido sin estarlo** — el mismo error que hace inservible un identificador derivado. Cifrar tampoco:
claves, runtime y la prohibición de credenciales.

Lo que sí hay es **público pero seudónimo**: la carta la lee todo el mundo, pero solo su destinatario
sabe que ese `remitente` es él. Para hablar de una idea, alcanza.
6. **Archivar y registrar**: tu copia como `letter` —la del otro lado puede desaparecer— y la fila en
   `correspondence`, **con el estado sincero**.

### Una carta saliente tiene dos estados, y el agente solo puede mover el primero

**`redactada` -> `entregada`.** El agente escribe la fila al redactar; **solo el usuario mueve la
segunda**, porque el cartero es él. Un agente **no puede comprobar** que una carta salió: puede saber
que la escribió y nada más, así que anotar "enviada" al archivarla es una suposición disfrazada de
registro — y el índice es justo el doc que no debe contenerlas.

Ocurrió: una carta estuvo dos sesiones en el cajón mientras el índice decía "enviada", y lo descubrió
el usuario preguntando si ya se había contestado. Con los dos estados, esa pregunta **se responde
mirando** en vez de preguntando: una fila `redactada` es una conversación que no ha salido.

**Y el valor no está en que el estado sea exacto: está en que crea una superficie de contradicción.**
Con un solo estado no hay señal posible, porque no hay nada con lo que chocar. Con dos, un registro
que dice *"sin entregar"* junto a una respuesta que llegó **es una incoherencia visible**, y eso
obliga a preguntar en vez de suponer. Es la misma razón por la que funciona *un hogar por dato* y por
la que la clase 7 solo aparece contrastando dos sitios: **lo que detecta no es la exactitud de un
registro, es el desacuerdo entre dos.**

**Y la inmutabilidad empieza en la entrega, no en la escritura.** Una carta **entregada** o
**recibida** no se reescribe jamás. Un borrador sin entregar sí se revisa — si mientras espera pasa
algo que el destinatario debería saber, entra en la carta en vez de en la siguiente.

**Por eso el estado vive en la fila y NO en la carta**, y las dos reglas de arriba lo obligan: el estado
cambia después de entregar, y lo entregado no se reescribe. Un `redactada` escrito en la cabecera del
`letter` **no puede actualizarse nunca sin romper la otra regla** — queda congelado en el instante de
redactar y a partir de ahí se lee como estado actual siendo un fósil. Es un dato mutable dentro de un
artefacto inmutable, y no hay disciplina que lo arregle: es la forma la que está mal.

Se destapó barriendo el archivo propio: **doce cartas entregadas, las doce diciendo `redactada` en su
cabecera**, y el índice dándolas por entregadas. Ninguna era falsa al escribirse y ninguna se puede
corregir. **Y la comprobación de la fase 0 de CONTRASTAR no lo ve**, porque mira la fila —el hogar
correcto— y la fila estaba bien: el desacuerdo solo aparece si alguien abre el fichero, que es
justamente lo que nadie hace para comprobar un estado.

**Y hay una asimetría que decide qué hacer con las que ya salieron.** Ante doce cabeceras congeladas
caben dos salidas: dejarlas como fósiles visibles, o retocarlas para que digan la verdad. La segunda es
la que parece correcta y es la peligrosa: **una cabecera que miente se ve y es un hallazgo esperando;
una cabecera retocada dice la verdad y no la delata nada.** Ocurrió en campo: un proyecto con una sola
carta afectada movió su cabecera a `entregada` al confirmarse la entrega —reescribiendo una carta ya
entregada, contra su propia regla— **para mantener al día un campo que él mismo se había inventado el
día anterior**. No lo notó nadie, precisamente porque el resultado era correcto. Así que la infracción
se anota en el índice, que es el hogar mutable, y **el artefacto se queda como quedó**.

**Lo instructivo es de dónde salió: la plantilla no lo pedía.** Su cabecera lleva remitente,
destinatario, fecha, dirección y `Responde a`, y **nunca tuvo campo de estado**. Lo añadió quien
escribió la primera carta y las once siguientes lo copiaron de la anterior, que es como se propaga un
campo que nadie decidió. Así que la lección no es *"quita ese campo"* —no estaba— sino que **un dato
que el índice muestra es un dato que alguien va a repetir en el artefacto**, y ahí donde se repite ya
no se puede mantener. Una omisión en una plantilla no se defiende sola: si el campo no debe estar, hay
que decir que no debe estar.

### El remitente, y por qué son dos claves

En `Meta`, **elegido, no derivado**. Derivarlo de la carpeta, la ruta o el remoto rompe el rastro en
cuanto algo se renombra **y además no es anónimo**: el espacio de búsqueda de una ruta es minúsculo y
se recorre por fuerza bruta, así que publicarías algo que parece protegido y no lo está.

**`remitente` firma lo privado; `remitente_publico` es lo que puede aparecer en un buzón, y por defecto
es `—` = anónimo.** Son dos porque son dos trabajos incompatibles: ante un corresponsal concreto
conviene ser **reconocible** —para que el historial de la fuente se acumule—; en un canal que lee
cualquiera conviene **no serlo**. Con una sola clave, un proyecto que quiera las dos cosas tiene que
renunciar a una. Lo destapó una organización que firma en privado con su nombre real a propósito y
que **no quiere ese nombre publicado**.

En la duda, anónimo: autorizar se puede más tarde, despublicar no.

Tres cosas que conviene tener claras: **identifica, no autentica** —cualquiera podría firmar como otro,
y es aceptable porque el cartero es una persona, pero nadie debe construir confianza encima—; **nunca
es un secreto**, o cae bajo el PROHIBIDO de credenciales; y **la carta anónima sigue siendo posible**,
a costa de perder el historial de la fuente, que es una renuncia de quien escribe y no del marco.

Lo elige el usuario y lo propone el agente: **solo el usuario sabe qué le identifica** en su contexto.

### Espeja el registro, no el dialecto

Contestar en el **registro** de quien escribe —formalidad, densidad técnica, si tutea o no— es
cortesía y ayuda a entenderse. **La variante de idioma es otra cosa: esa se queda como la tuya.**

El motivo no es estético. Cuando quien escribe es otro agente, su variante puede ser **el default de
su modelo y no una elección de la persona**. Si tú espejas la suya y él espeja la tuya, dos
herramientas se están devolviendo su propio sesgo y lo llamamos cortesía. Escribe en la variante de tu
proyecto; si de verdad importa saber si la suya es deliberada, pregúntalo — para eso hay una carta.

**Y el límite: escribir en tu variante tampoco garantiza acertar con la persona que hay detrás.** La
regla te saca del bucle entre agentes; no te dice qué prefiere quien lee. Caso real y humillante: dos
proyectos intercambiaron ocho cartas en una variante que **ninguno de los dos usuarios humanos usa**,
sin que nadie lo notara, porque los dos agentes compartían el mismo default. Que al aplicar la regla
se acertara con el destinatario fue **suerte**. La respuesta a *"¿qué variante quiere la persona?"* no
es una regla — es una pregunta, y cuesta una línea.

## Ritual: BOOTSTRAP (instanciar el marco en un proyecto)

**Modo:** *greenfield* (no hay docs → scaffold) o *adopción* (ya existen → mapear a roles sin
sobrescribir contenido; solo generar lo que falte). Pasos:
1. Elegir `idioma`/`módulos`/`persistencia` y las **tres rutas** (`kit`/`base`/`loader`) con
   defaults sensatos. Auto-detectar: módulo software por `Cargo.toml`/`package.json`/`src/`;
   `persistencia = git` si hay `.git` **en la raíz del proyecto** — no vale uno anidado en un
   subdirectorio, que deja la raíz sin versionar —, si no `ninguna` (avisando de la consecuencia).
   Zero-question posible.
   **Desambiguación obligatoria:** una ruta suelta en la petición del usuario ("usa stele aquí, base
   stele") se interpreta como **`base`** — es lo que al usuario le importa; `kit` solo cambia si dice
   "kit" o "marco" explícitamente. **Ante duda real, ofrecer el menú de layouts** (ver "Layouts con
   nombre") con una opción `otro` para dar `kit` y `base` a mano — una pregunta cerrada en vez de dos
   abiertas. Nunca adivinar.
   **Si no detectas código, recomienda `agrupado`** (y dilo, no lo impongas). Con el default
   `base = .` los docs de rol caen sueltos en la raíz del proyecto, y a quien no programa eso le
   parece un desorden ajeno: no distingue lo suyo de lo del marco, y acaba sin tocar archivos que son
   **suyos** y que debería editar cada sesión. Con `agrupado` todo queda bajo `bitacora/` y la raíz
   sigue siendo del usuario. No hace falta prefijar nada: **una carpeta con nombre ya dice de quién es
   lo que hay dentro** — y por eso no se llama como el marco (ver la tabla de layouts).
2. **Eco del layout resuelto ANTES de escribir nada** (siempre, incluso en zero-question):
   ```text
   layout       -> agrupado   (dónde va cada cosa; o "personalizado")
   kit          -> .stele     (el marco; se reemplaza al actualizar)
   base         -> bitacora   (tus documentos; no se tocan nunca)
   loader       -> CLAUDE.md  (el archivo que arranca al agente)
   persistencia -> git        (cómo se guarda el trabajo al cerrar)
   ```

   Coste cero y ataja la mala interpretación antes del scaffold, no después. Si el kit ya se
   vendorizó en la ruta equivocada, moverlo aquí es trivial; después no.
3. Validar los **invariantes de ruta** (ver "Las tres rutas"). Violación = abortar y re-preguntar.
4. Resolver nombres (defaults de rol + módulo; override libre).
5. Escribir `stele.config.md` en la raíz (plantilla `core/templates/config.md`), con la sección
   Rutas ya resuelta y **`kit_origen` anotado**: la URL o ruta de la que acabas de traer el kit. Es
   el único momento en que se sabe con certeza, y sin ella ACTUALIZAR no puede correr después.
6. Scaffold: instanciar cada template por rol → nombre configurado bajo `base`, **resolviendo
   tokens** (incluido `{{kit}}`). En adopción, saltar los docs que ya existen.
7. Semilla: `state` y `handover` (`SIN_TRABAJO_ACTIVO`) iniciales, `index` vacío. **`audit` no se
   instancia**: lo crea la primera auditoría, y su ausencia es el dato (nunca se auditó).
8. Generar derivados: loader de auto-arranque en la raíz, con el nombre de la ruta `loader`
   (plantilla `autostart.md`) + mapa-doc en `entry`. **Si el loader ya existe** (`CLAUDE.md`,
   `AGENTS.md`… escritos a mano antes de adoptar el marco), **léelo primero e inserta** el bloque
   `STELE:INICIO`/`STELE:FIN` conservando todo lo demás — invariante 6. Igual que en adopción con
   cualquier otro doc: nunca reemplazar contenido que no escribiste.
9. Validar (ver ritual CONFIG, fase 5).
10. Confirmar + activar: reabrir el editor → el loader se auto-carga.

## Ritual: ACTUALIZAR (traer una versión nueva del kit)

Se dispara con "actualiza stele" / "trae la última versión del marco". Cambia **solo la ruta `kit`**:
`base` no se toca nunca — esos docs son del proyecto, y una plantilla nueva **no reinstancia nada**.
No aplica en modo auto-hospedado (`kit = .`): ahí el marco se desarrolla en sitio, no se vendoriza.

**Regla dura: no toques el kit hasta haber leído el diff.** La versión nueva se trae **al lado**, a
un temporal, nunca encima de la que ya tienes. Así el diff existe siempre — sin depender de que el
adoptante haya versionado el kit ni de acordarse de respaldarlo — y una actualización que se aborta a
medias no deja nada roto: si no llegaste a aplicar, no tocaste nada.

1. **Traer la versión nueva a un temporal**, fuera del árbol del proyecto o en un directorio ignorado
   por el VCS (si cae dentro, ensucia el `status` y puede acabar commiteado). La fuente es
   **`kit_origen`** (manifiesto → Meta); con el mismo `degit`/`clone` de la instalación. **Nunca sobre
   `{kit}`.** Si `kit_origen` falta o está vacío, **pide la URL al usuario y escríbela en el
   manifiesto** antes de seguir: sin ella el ritual no arranca, y no se deduce del árbol.
2. **Diffear** viejo contra nuevo: `diff -r {kit} {temporal}`. **Un diff vacío tiene dos causas y se
   leen igual:** que estés al día de verdad, o que **el origen no haya publicado lo que dice tener** —
   dicho por quien lo vivió, *"un clone de un kit sin publicar no da error: devuelve un kit"*.
   Antes de dar por bueno el primero, mira un dato observable del origen —la fecha de su último cambio,
   el identificador de su última entrega— y compáralo con lo que esperabas encontrar. Si cuadra: dilo en
   una línea, borra el temporal y termina sin haber tocado nada. Si no, **el problema está aguas arriba**
   y lo que toca es avisar, no actualizar.
3. **Clasificar por zona de impacto** (tabla abajo). Lo que no aparece en la tabla es procedimiento:
   se lee, no se migra. **Y lee entero todo archivo que el diff marque como NUEVO** (`Only in
   <temporal>:`) antes de aplicar, esté o no en la tabla: un archivo que no existía no puede tener
   fila, porque la fila que lo describiría viaja en el mismo kit que lo trae. Ver abajo.
   **Si el diff muestra un archivo del kit RENOMBRADO o ELIMINADO, busca en tus propios docs los
   enlaces al nombre viejo.** La tabla te dice qué hacer con el contenido del kit, no con las
   referencias que tú tengas hacia él, y esas se quedan colgando en silencio. `grep` del nombre viejo
   en `{base}` = 0 antes de dar la actualización por buena.
4. **Aplicar:** sustituir `{kit}` por el temporal. Es seguro *aquí* porque el invariante 1 garantiza
   que `base` no está dentro. **A partir de esta línea el procedimiento vigente es el del kit nuevo**,
   no el que traes en contexto: si el diff tocó `SKILL.md`, relee lo que gobierna los pasos que te
   quedan — el informe del paso 6 incluido.
5. **Reconciliar con CONFIG** (fase 1, drift), acotado a lo que el diff señaló: filas que le faltan
   al manifiesto, secciones nuevas, derivados a regenerar.
6. **Informar** en pocas líneas: qué cambió, qué se reconcilió solo, y qué exige decisión del usuario
   (un rol nuevo que quizá quiera desactivar, un default que él había sobrescrito, un cambio del
   contrato de parseo).
7. **Limpiar** el temporal.

**Lo que acabas de instalar gobierna lo que te queda por hacer, y es el paso que más se salta.** Un
agente ejecuta ACTUALIZAR con el procedimiento que cargó al abrir la sesión, así que entre el paso 4 y
el 7 sigue operando de memoria con el kit que acaba de sustituir. El caso visible es el **informe**: es
el primer mensaje en que el adoptante ve la versión nueva y el único que todavía obedece a la anterior.
Si esta actualización trae una regla de **habla** —cómo se le dice un número a la persona, en qué
registro se le explica un rol nuevo—, esa regla ya rige aquí, y **ningún paso la recuerda** porque no
es una regla de procedimiento. Caso de campo: un informe correcto en todo lo demás soltó *"ya vamos en
9 sesiones sin auditar"* —contador desnudo, sin consecuencia y sin qué se espera de la persona— en la
misma actualización que traía la regla que lo prohíbe.

Es la misma ley que ya conoce la tabla de zonas —*una regla que gobierna el actualizar no gobierna la
actualización que la entrega*— aplicada un nivel más adentro: no al **qué migrar**, sino al
**procedimiento en curso**. Una regla nueva llega tarde por construcción, y lo único que la rescata es
releerla en el paso 4.

| Zona del diff | Qué implica para esta instancia |
| --- | --- |
| `core/roles.md`, `modules/*/roles.md` | Roles nuevos, renombrados o con distinto `startup`/`order`: al manifiesto le faltan filas y hay que **regenerar los dos derivados**. Y si el rol nuevo es un **doc**, su archivo **no existe en esta instancia**: ofrécelo al usuario e instáncialo si acepta — regenerar los derivados deja el mapa apuntando a algo que no está |
| `core/templates/config.md` | Cambió la forma del manifiesto o el contrato de parseo: la instancia puede estar desfasada (secciones nuevas, claves nuevas) |
| `modules/<mód>/module.md` | Cambió lo que aporta un módulo activo: features, defaults o su regla dura |
| `core/templates/autostart.md`, y los bloques `GENERADO` de `core/templates/entry.md` | Cambió un **derivado**: hay que **regenerar ese bloque** en el archivo real (loader y mapa-doc), conservando íntegro todo lo que quede fuera de las marcas — invariante 6. **Salvo que la marca de apertura diga `RICO`**: entonces no se reescribe, se **porta el delta a mano** (ver abajo) |
| `core/templates/*` de rol (salvo sus bloques `GENERADO`), `modules/*/templates/*` | **Nada que hacer.** Los docs de `base` ya son del proyecto y no se regeneran jamás |
| `SKILL.md`, `guide.md`, `README.md` | Procedimiento y fundamentos: se leen, no se migran |
| El buzón del kit (si lo tiene) | **Correspondencia que baja.** Léela y dile al usuario si hay algo dirigido al `remitente` de este proyecto o alguna pregunta que pueda contestar. Contestar es ritual REMITIR; archivar solo lo que se conteste o lo que mueva a hacer algo. **Baja aunque `correspondence_log` esté en `off`** —viaja dentro del kit—, y entonces se puede leer pero no contestar: hace falta activar el rol y elegir `remitente`, y las dos cosas las decide la persona. No ofrezcas REMITIR sin decirlo |

**Un rol nuevo es el único caso donde una plantilla de contenido sí llega a quien ya adoptó.** La fila
de las plantillas de rol dice que los docs de `base` no se regeneran jamás, y es cierto **para los que
existen**: son del proyecto. Pero un rol que no existía no tiene doc que respetar, así que ahí no hay
conflicto — hay una ausencia. Sin esta excepción, una capacidad nueva del marco solo la reciben los
proyectos que empiecen después, y el que la necesitaba lleva sesiones sin saber que existe. **Se
ofrece, no se crea en silencio:** el usuario puede no quererla.

**Y su toggle no se escribe hasta que conteste.** Un `off` puesto por precaución y un `off` elegido son
el mismo texto en el manifiesto, y no son la misma decisión: la próxima actualización ya no ve un rol
nuevo, no vuelve a ofrecerlo, y la capacidad se pierde **en silencio** — que es exactamente lo que esta
excepción existe para evitar. Si la sesión acaba sin respuesta, la pregunta va a los pendientes de
`state`, que es el hogar de lo que queda abierto y se relee en cada arranque. Preguntar dos veces
cuesta una línea; una oferta que nadie llegó a declinar y que ya no se repite no la recupera nadie.

Caso de campo: un agente dejó dos roles nuevos en `off` *"para no cambiarte el comportamiento sin que
lo pidas"* y preguntó **en el mismo mensaje**, con el manifiesto ya escrito. Ahí acabó bien —el usuario
los activó—, así que lo que se documenta no es una pérdida observada sino el modo de fallo que ese
orden habilita: con un "ahora no", o sin respuesta, el manifiesto habría quedado idéntico a si nunca se
hubiera ofrecido nada.

**Plantilla de contenido contra plantilla generadora.** Es la distinción que decide las dos filas de
plantillas —la del loader con sus bloques `GENERADO`, y la de las plantillas de rol—, y confundirlas
es un fallo silencioso. Una plantilla de **contenido** produjo un doc que desde el primer día es del
proyecto: se instancia una vez y no se vuelve a tocar jamás. Una plantilla **generadora** produce un
**bloque** que el marco sigue siendo dueño de reescribir —el del loader y los dos del `entry`— y ese
bloque **sí** viaja con cada actualización. Si no se regenera, el adoptante se queda con el kit nuevo
y las reglas viejas cargándose en cada sesión, sin ninguna señal de que algo falta. Regenerar el
bloque **nunca** autoriza a tocar lo que esté fuera de las marcas (invariante 6).

**Y hay un límite conocido, sin remedio, que conviene saber: la discrepancia entre plantilla e instancia
es un detector gratis que se apaga al converger.** Cuando el kit corrige una plantilla de **contenido**,
el adoptante no la recibe —su doc ya es suyo— y durante un tiempo su copia y el texto del kit **dicen
cosas distintas**. Esa discrepancia es justo lo que hace visible el problema: aparece al comparar, y
alguien lo arregla a mano. Pero en cuanto lo arregla, los dos textos coinciden **por caminos distintos**
y la señal desaparece — para esa regla y para todas las que ya convergieron. Dicho crudo: **el adoptante
más al día es el que menos aviso tiene.** Lo aporta un proyecto en campo, no trae remedio, y se escribe
para que nadie lo descubra creyendo que es un fallo suyo.

**Observado en vivo poco después, por los dos lados a la vez:** una instancia arregló su copia a mano y
el kit arregló su plantilla por su cuenta; **los dos textos acabaron diciendo lo mismo por caminos
distintos y ninguno avisó al otro**. El resultado era correcto y la señal se apagó igual. **La
convergencia y el silencio llegan juntos** — no es un fallo que se pueda arreglar, es lo que significa
converger.

**En modo adopción el bloque generado no es un derivado puro.** Un proyecto que adoptó el marco con
cientos de sesiones encima suele tener en su loader reglas propias que la plantilla base no contiene
—una regla dura específica, su mapa de hogares, el porqué de su saludo—, y ahí "regenerar" no es
refrescar: es **perder**. Por eso el bloque puede declararse rico en su propia marca de apertura
(`STELE:INICIO RICO`), y entonces ACTUALIZAR **porta el delta a mano** en vez de reescribirlo.

**Y hay una variante donde la marca no es una comodidad, sino la única protección que existe.** El
invariante 6 conserva lo que queda **fuera** de las marcas — pero un proyecto que adoptó puede haber
acabado con su texto escrito a mano **dentro** de ellas, si al insertar el bloque se rodeó lo que ya
había en vez de añadirlo aparte. Ahí el invariante 6 no protege nada, porque **no hay nada fuera**: todo
lo propio está en la zona que la regla autoriza a reescribir entera. Caso real y documentado en campo,
con el loader escrito a mano meses antes de la adopción.

Así que al adoptar sobre un loader existente hay **más de dos situaciones**: generado de cero,
contenido propio fuera de las marcas (lo cubre el invariante 6), y **contenido propio encerrado
dentro** (solo lo cubre `RICO`). La tercera es la más peligrosa y la menos evidente, porque desde
fuera se ve igual que la primera.

**Y "generado de cero" no es "nada que proteger", que es como estuvo escrito aquí y era falso.** Lo
corrigió un proyecto adoptante el mismo día de adoptar: su loader lo generó el bootstrap, sin una
línea escrita a mano, y **quedó marcado `RICO` esa misma jornada** porque el bloque generado se apartó
de la plantilla en reglas propias suyas —el idioma del habla frente al de los documentos, su
convención de escritura y el bloque redactado en otro idioma que la plantilla—. **La divergencia nació
en el bootstrap.** Sin la marca, la primera regeneración habría puesto al agente a contestar en otro
idioma del que se le pregunta.

Así que el eje no es el **origen** del contenido —preexistente contra generado— sino la **divergencia
respecto de la plantilla**, venga de donde venga. La plantilla siempre lo dijo bien (*"si este bloque
se enriqueció con reglas propias que la plantilla base no contiene"*); **fue esta prosa la que tradujo
esa condición a una historia sobre archivos anteriores a la adopción**, y una vez contada así, el
adoptante que generó de cero se da por excluido y no vuelve a mirar. Es la clase de error que no
contradice a la regla: la estrecha.

La marca vive en el bloque y no en el manifiesto **a propósito**: el dato viaja con la cosa que
describe y lo lee el agente en el momento exacto en que iba a sobrescribir.

**Lo que la marca no hace:** decir *qué* falta. Marca un bloque como no reescribible, pero portar el
delta sigue exigiendo comparar la plantilla nueva contra el bloque a mano. Convierte una regeneración
automática en una **comparación manual** — que es lo correcto, pero cuesta, y conviene no venderla como
gratis. Escrito como prosa en el manifiesto ya falló en campo — un proyecto adoptado tenía justo esa
nota, la fila del loader disparó igual, y lo que evitó la pérdida fue que el agente leyera y decidiera,
no el mecanismo. Un mecanismo que depende de que alguien recuerde una nota en otro archivo no es un
mecanismo.

**Una regla que gobierna el actualizar no gobierna la actualización que la entrega.** Quien actualiza
sigue el ritual del kit **viejo**: cuando clasifica el diff, el kit nuevo todavía no está aplicado. Así
que **cualquier fila que se añada a esta tabla es invisible durante la actualización que la introduce**,
y solo empieza a servir a partir de la siguiente. No es un defecto de una fila concreta: es una
propiedad del mecanismo, y vale para todas las que hay aquí.

Por eso la fase 3 manda **leer todo archivo nuevo del diff**, esté o no en la tabla. Esa regla también
llegó tarde una vez —no hay forma de evitarlo, nada puede gobernar su propia entrega—, pero al ser
**genérica** solo llega tarde **esa** vez: después cubre cualquier pieza futura sin necesidad de una
fila por cada una. Una fila por feature, en cambio, llega tarde siempre.

Detectado en campo: un proyecto recibió el buzón y su agente lo leyó, pero **no por la fila** —que no
existía en su kit— sino porque `diff -rq` imprimió `Only in <temporal>: buzon.md` y la fase de
clasificar le llevó a abrirlo. **Funcionó porque el diff obliga a mirar**, que es más robusto que
cualquier fila: no depende de que el destinatario ya tenga la versión que se lo dice.

**El canal de bajada no es maquinaria: es esta fila.** ACTUALIZAR ya se trae el árbol entero del kit,
así que **si el kit lleva un buzón, las cartas bajan con la actualización** — sin red, sin API y sin
credenciales, que es lo que mantiene el marco sin runtime. La subida no tiene equivalente: necesita un
cartero humano, y por eso el marco **estandariza la carta y nunca el canal**. Y esta revisión **no va
en ABRIR** aunque tiente: mirar el buzón en cada sesión rompería el arranque barato, que es un pilar.

**Si el diff muestra cambios que no vienen de arriba sino de ediciones locales del kit, para y
avisa**: el kit no se edita dentro de un proyecto (para eso está la config), y re-vendorizar los
borra. Recupéralos o descártalos con el usuario antes de seguir, nunca en silencio.

**Sin marcador de versión, a propósito.** El kit no lleva `VERSION` ni changelog: el diff dice *qué*
cambió y dónde, que es lo único accionable, y un número habría que acordarse de subirlo en cada
cambio. El porqué, en `guide.md` → "Alternativas descartadas".

## Ritual: CONFIG (adaptar nombres/parámetros — único renombrador sancionado)

1. **Leer + reconciliar** `stele.config.md` contra los archivos reales; reportar/arreglar drift.
2. **Clasificar** el cambio por radio de impacto: renombrar / toggle módulo / toggle feature /
   presupuesto / wording / idioma / `persistencia` / `kit_origen` (cambiar de fork o de mirror; no
   toca ningún archivo, solo el manifiesto) / **ruta** (`kit`, `base` o `loader`). Un layout
   con nombre ("pásame a `agrupado`") es una petición de **ruta**: se resuelve a valores concretos
   antes de clasificar, y lo que se escribe en el manifiesto son las rutas, nunca el nombre.
3. **Previsualizar** (dry-run) y confirmar (renombrar toca varios archivos). Para un cambio de ruta,
   el dry-run es el **mismo eco** del bootstrap, con el antes y el después (línea `layout` incluida).
4. **Aplicar**, acotado a los **docs del marco** (nunca código de producto): mover (`git mv`, o `mv`
   si el kit no está versionado) → reescribir la tabla del manifiesto **completa** → barrido de
   referencias por el mapa viejo→nuevo → regenerar derivados (auto-arranque + mapa-doc).
   **Antes del barrido, comprueba si el nombre viejo es SUBCADENA de otra ruta viva.** Si lo es, una
   sustitución textual la corrompe **en silencio**: caso real de campo, `.stele/` contiene `stele/`, y
   un `replace("stele/", "bitacora/")` ingenuo habría convertido el kit en `.bitacora/` — el marco
   entero fuera de su sitio, el manifiesto apuntando a la nada, y **ninguna señal hasta la sesión
   siguiente**. Se ancla la sustitución (un *lookbehind* basta) y **se verifica después** que lo que no
   debía moverse sigue donde estaba. Es el mismo peligro que hace que `base` no se llame como el kit:
   la adyacencia **no solo confunde a las personas, confunde a las herramientas**.
5. **Validar**: `grep` del nombre (o ruta) viejo = 0; cada nombre resuelve a un archivo; ningún rol
   activo apunta a faltante; los invariantes de ruta se cumplen.

Reglas fijas: desactivar un módulo **no** borra sus docs (huérfanos preservados + aviso); colisión
de nombre aborta; cambiar el patrón `session` afecta solo sesiones futuras (el historial es inmutable).

**Cambios de ruta, en concreto:**

- Mover `kit`: mover el directorio y barrer las referencias `{{kit}}` ya resueltas en los docs
  instanciados (`entry`, `protocol`, `loader`). No toca ningún doc de contenido.
- Mover `base`: mover los docs de rol (y `history_dir` completo, con su historial) y regenerar el
  loader, cuyos `@import` son relativos a la raíz. El historial se mueve entero, no se reescribe.
- Cambiar `loader`: insertar el bloque en el archivo nuevo (creándolo o modificándolo, invariante 6)
  y **retirar el bloque del viejo** — no borrar el archivo viejo a ciegas: puede tener contenido del
  usuario. Si al quitar el bloque no queda nada más, entonces sí se borra; si queda algo, se conserva
  y se avisa. Dos loaders **activos** compitiendo es peor que ninguno, pero eso lo resuelve retirar
  el bloque, no destruir el archivo. Verificar antes que el nombre nuevo no colisiona con un rol bajo
  `base`.

## Operaciones de bajo coste (preferir siempre)
- Apéndice de una fila → `printf '...' >> archivo` (sin `Read` previo).
- Archivo pequeño de formato fijo → un `Write` completo (no varios `Edit`).
- Buscar en archivo grande → `grep -n` y luego leer solo el rango.
- Volumen mecánico grande (dividir un doc de 1000+ líneas) → delegar a un subagente. **Dile dónde
  escribir:** un subagente trae su propio temporal privado, aún menos visible que el tuyo, así que
  esta recomendación multiplica el problema que resuelve el hogar de artefactos si no se le nombra el
  destino de forma explícita.
