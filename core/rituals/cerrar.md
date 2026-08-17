<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

# Ritual: CERRAR sesión (dejar registro durable)

## Lo mínimo para ejecutarlo

> **Este ritual se ejecuta CADA sesión**, así que un bloque mal cortado se paga a diario.

1. **`session`** (nuevo): qué se hizo, decisiones, archivos, verificación, notas para retomar, y
   `## Esfuerzo equivalente` (si `effort_log`). `NNN` con padding a 3 dígitos, y la fecha **medida, no
   recordada** — ver *De dónde sale la fecha*, abajo.
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

**Las siete que se saltan.** Cada una tiene su cadáver documentado abajo:

| Situación | La regla |
| --- | --- |
| Vas a poner la fecha | **Se mide con un comando**, no se recuerda ni se copia del contexto. Dos actas de este proyecto llevan un día de más |
| Vas a actualizar el `state` | **Se reescribe ENTERO con `Write`.** Nunca `Edit`, nunca prepend |
| Vas a hacer un reemplazo en lote | **Imprime su diff, no su recuento.** Un recuento correcto acompaña a un reemplazo equivocado sin decir nada |
| Vas a copiar en el `state` algo que vive en otro doc | **Apunta, no copies.** Lo copiado es lo que se queda viejo |
| Vas a registrar un estado | Solo si **puedes observarlo**. *"Enviada"* no lo es: eso lo sabe el usuario, no tú |
| Terminaste de escribir el cierre | **Comprueba lo que acabas de escribir** antes de persistir: **su contenido, su destino y su tamaño**. Un `printf >>` con la ruta mal compuesta **crea el fichero que falta** y no da error; y los topes del set de arranque **se miden, no se recuerdan** — el bloque está en `protocol`, y la primera vez que se corrió, **dos de los cuatro llevaban rotos desde siempre**. En el mismo sitio y en la misma tanda va **el respaldo declarado de `base`**, si está excluido del control de versiones: comprueba que **alguien contestó**, no que exista |
| Escribiste la comprobación en el doc | **Escribirla no la corre.** El paso es correrla, y su cero no vale sin control positivo |

**Dónde está el resto.** Se abre por **pregunta**, nunca entero:

| Si te preguntas… | Sección |
| --- | --- |
| ¿De dónde saco la fecha, y por qué no de `git log`? | *De dónde sale la fecha* |
| ¿Por qué entero y no un `Edit`? | *El `state` se reescribe entero* |
| Voy a saltarme un paso por eficiencia | *La barrera se rodea por eficiencia, no por descuido* |
| ¿Cómo verifico un reemplazo masivo? | *Un reemplazo en lote imprime su diff* |
| ¿Qué va en el `state` y qué no? | *El `state` apunta a lo que caduca* · *No registres un estado que no puedas observar* |
| ¿Qué compruebo antes de commitear? | *Antes de persistir, comprueba lo que acabas de escribir* |
| ¿Y si no hay git? | *Persistir el cierre, según el modo* |

## De dónde sale la fecha

**La fuente es el reloj de la máquina, y se mide con un comando** — `date '+%Y-%m-%d %z'` en POSIX,
`Get-Date -Format 'yyyy-MM-dd K'` en PowerShell. Vale con VCS y sin VCS. **No la pongas de memoria y
no copies la que traiga tu contexto.**

**Porque la fecha que inyecta el harness suele ser UTC, y el proyecto no vive en UTC.** Medido en una
instancia real: el contexto decía `2026-08-10` mientras el reloj local marcaba `2026-08-10 05:45 -0500`.
Cinco horas de desfase significan que **entre las 19:00 y la medianoche locales el agente ya está en el
día siguiente**, y la fecha del acta es el eje del historial: dos actas de esa franja quedaron
adelantadas un día y **no se corrigen**, porque un acta no se reescribe.

**Y ojo con el sustituto que parece obvio: `git log -1` no da hoy, da el día del último commit.** Solo
coincide si commiteaste dentro de la sesión y lo lees *después* de commitear; abierta la sesión sobre un
commit de anteayer, devuelve anteayer con toda confianza. Git no sabe qué día es: lee el mismo reloj del
sistema que `date` y lo estampa, así que **`git log` es `date` con un rodeo y una condición extra**.
Aparece como regla porque fue **donde se vio el error** —las actas adelantadas se destaparon comparando
contra él—, y de ahí se ascendió un detector a fuente. **Un detector que destapa un error no es por eso
la fuente del dato.**

**El caso que rompe las dos señales a la vez es que el shell no corra en la máquina del usuario** —
agente remoto, contenedor, ejecución en la nube. Esos suelen ir en UTC, así que `date` da UTC **y** los
commits que haga ese agente se estampan en UTC: las dos fuentes fallan igual y **coinciden**. Es el peor
modo de fallo, porque la coincidencia se lee como confirmación; son dos instrumentos correlacionados
validándose entre sí.

**La sonda, si hay VCS**, es comparar tu zona con la de quien commitea:

```sh
date '+%z'                                               # la zona del shell donde corres
git log -20 --format='%ad' --date=format:'%z' | sort -u  # la zona de quien commitea
```

Si difieren, no estás donde vive el proyecto: **pregunta la fecha**. Y **valida la sonda antes de
creerle**, que cuesta una línea: `TZ=UTC date '+%z'` tiene que discrepar del offset de los commits. Si
ni siquiera así discrepa, la sonda no separa y su acuerdo no vale nada.

**Sin VCS no hay sonda, y no pasa nada** — la jerarquía degrada al último escalón, que siempre está.
Son tres, en orden:

1. **El reloj de la máquina.** La fuente.
2. **El control de zona**, si hay VCS. Solo dice si la fuente es de fiar aquí.
3. **Preguntar al usuario** — si no hay VCS, o si el control discrepa. Cuesta una línea y es la única
   fuente con autoridad sobre qué día es donde vive el proyecto.

**Vale para toda fecha que estampes**, no solo la del acta: la fila del `index`, la del `effort`, la del
`audit`, el nombre de una carta. Y AUDITAR exige fecha en todo hallazgo por la misma razón por la que
esto importa — **un dato fechado mal envejece como un dato cierto**.

## El `state` se reescribe entero, también fuera del cierre

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

## La barrera se rodea por eficiencia, no por descuido

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

## Un reemplazo en lote imprime su diff, no su recuento

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

**Y al preguntar por qué el diff del commit tampoco lo cazó, la respuesta desmonta la pregunta: ese
diff no se miró nunca.** Lo que hubo fue una cadena de comprobaciones que miraban **cuántos**
—los contadores—, **cuáles** —los nombres de archivo de `git status --short`— y **dónde**, y
**ninguna miraba qué**. Un nombre de archivo no muestra contenido, así que el carácter que cambió no
tuvo ocasión de ser visto. Conviene tenerlo claro antes de fiarse de un techo que nadie ha medido:
**la regla del diff no falló ahí, no llegó a ejercerse.**

**El remedio que elimina la clase entera es más barato que los dos que la detectan: nombra el
carácter en vez de contenerlo.** Un texto que dice *"conserva el centinela `—`"* es carne del barrido
que sustituye ese carácter, y lo que queda es una frase **coherente y falsa**. Un texto que dice
*"conserva el centinela em-dash"* es inmune. Y cuando el carácter **tiene** que aparecer —porque es
el token que alguien debe teclear—, se escribe **con su nombre al lado**: entonces el barrido produce
una contradicción visible (*"`--` (em-dash)"*) en vez de una mentira que se lee bien.

**Aplicado aquí encontró una nuestra, en el kit y en un contrato de parseo:** la plantilla del
manifiesto explicaba el centinela de *rol desactivado* **conteniéndolo**, así que un barrido masivo
de no-ASCII —el que la propia documentación pide correr— habría dejado el contrato afirmando lo
contrario de la única excepción que existe para protegerlo. Ya lleva el nombre al lado.

**Y el detector propio no cubre esto**, que es lo que lo hace insustituible: un detector de no-ASCII
mira una dirección, y esta corrupción va **hacia** ASCII. El barrido que la causa es el mismo que la
verificación daría por limpia.

## El `state` apunta a lo que caduca, no lo copia

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

## No registres un estado que no puedas observar

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

**Y esta regla tiene dos hermanas, una antes y una después**: *antes de construir, comprobar si ya está
construido*, y *después de un paso cuyo efecto ocurre fuera del texto, comprobar que ocurrió y decir qué
se ve si no*. Las tres están enunciadas juntas en `SKILL.md` ("Comprobar en vez de dar por hecho"); aquí
vive el caso de la del medio, que es la del registro.

## Antes de persistir, comprueba lo que acabas de escribir

**Antes de persistir, comprueba lo que acabas de escribir contra las convenciones de texto de tu
proyecto** (si las tiene: solo-ASCII, terminología, lo que sea). **El marco no impone ninguna** —este
mismo kit está escrito en prosa acentuada a propósito—: el paso se parametriza con **las tuyas**, y si
tu proyecto no tiene convenciones de texto, no hay nada que comprobar. Lo aclaramos porque un lector
cuidadoso, con el kit delante, entendió lo contrario. No es una formalidad, y hay dos sitios
donde se escapa siempre: **las filas append-only** y **el mensaje de commit**. Son los dos únicos
momentos del cierre en que se redacta **prosa narrativa hacia un archivo**, con el mismo impulso con el
que se le habla al usuario — y ahí el registro equivocado se cuela sin que nadie lo note, porque el
resto de lo que se escribe son identificadores y rutas, donde el error salta solo. Compruébalo con un
comando, no releyendo: es lo que hace la diferencia entre una regla escrita y una regla aplicada. **Y
esa diferencia se puede medir** — ver *Escribir la comprobación no la corre*, al final.

## Persistir el cierre, según el modo

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

## Escribir la comprobación no la corre

**Una comprobación que hay que acordarse de hacer se hace a veces.** No es una afirmación sobre la
disciplina de nadie: es lo que sale al medirlo. Cada vez que un cierre falla en algo, el reflejo es
escribir la comprobación que lo habría cazado — en el ritual, en el protocolo, en el doc de trampas —
y darlo por resuelto. **Lo que queda escrito es la intención; lo que se corre es otra cosa.**

**La firma que las separa es la CURVA, no el nivel.** Si una comprobación se corriera por estar
escrita, saltaría a casi todas las sesiones **en la siguiente a su decisión**: el día que se decide, se
sabe. Si en cambio sube despacio a lo largo de decenas de sesiones y se estanca sin llegar al total, lo
que la sostiene no es la regla — es la costumbre de quien estaba delante. Y una costumbre tiene techo.

**Mídelo sobre tu propio historial, que ya tiene los datos.** Es un procedimiento y no una opinión:

1. Toma las comprobaciones que tu proyecto decidió **tras un fallo concreto** — las que tienen fecha de
   nacimiento. Sin esa fecha no hay antes ni después y no se puede medir nada.
2. Para cada una, cuenta en qué fracción de las sesiones **posteriores a su decisión** queda registro
   de que se corrió.
3. **Control obligatorio:** la misma fracción en las sesiones **anteriores** a su decisión. Si no sube,
   tu detector está midiendo otra cosa y la cifra no vale.
4. Mira la forma, no solo el número: dónde salta y dónde repta.

**Lo que mide es MENCIÓN, no ejecución, y eso lo convierte en un suelo.** Una comprobación puede
haberse corrido sin dejar rastro en el acta. Pero el suelo sigue informando, porque el registro es
justo lo que queda: si no está escrita, **quien lea el acta no puede saber que se corrió**, y desde
fuera *no se hizo* y *se hizo sin decirlo* son el mismo documento.

> **FOTO — corpus propio de este kit, 100 actas, medido el 2026-08-11.** Cuatro comprobaciones, cada
> una nacida de un fallo real y escrita en su documento (texto no-ASCII, columnas de una fila
> append-only, publicación del sello, fecha medida): se registran en el **66-70%** de las sesiones
> posteriores a su decisión. **Ninguna llega al 100%.** Las cuatro suben en el control. La más antigua
> tardó **más de 40 sesiones** en pasar del 90%.
> **Es una foto, no una constante:** un solo proyecto, un solo equipo de agentes, y un detector con
> falsos negativos conocidos. Lo transferible es el método y la forma de la curva; **el número de tu
> proyecto es tuyo y sale del paso 2.**

**El remedio no es insistir: es cambiar dónde vive la comprobación.** Una comprobación **a la vista**
—en una lista, en un párrafo, en un recordatorio— depende de que alguien la recuerde en el momento
exacto. Una comprobación **en el camino** se corre porque no hay forma de dar el paso sin darla: el
control positivo **dentro** del mismo bloque de comandos que la medición, el valor esperado escrito al
lado del comando que lo produce, el chequeo pegado al `printf` que escribe la fila. **La diferencia no
es la visibilidad, son los pasos** — y la vía segura tiene que costar lo mismo que la insegura, o se
paga la diferencia en olvidos.

**Al añadir una comprobación al cierre, la pregunta no es dónde escribirla sino de qué paso colgarla**
— y si no cuelga de ninguno, dilo al escribirla, para que el siguiente sepa que lo que tiene es un
recordatorio y no una garantía.
