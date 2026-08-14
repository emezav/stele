<!-- Referencia del kit stele. Se lee BAJO DEMANDA, y NO solo al auditar:
     estas leyes gobiernan cualquier comprobacion, la haga el ritual que la haga.
     Vivieron dentro de `core/rituals/auditar.md` hasta que se midio que eran el 65%
     de un ritual que se lee una vez cada diez sesiones. -->

# Referencia: cómo se verifica cualquier cosa

**Esto no es un ritual: no tiene fases ni se "ejecuta".** Son las leyes que gobiernan una
comprobación —cualquiera—, y se consultan cuando vas a **medir, comprobar o publicar un número**. Las
usa AUDITAR, pero también CERRAR al comprobar lo que acaba de escribir, CONTRASTAR al verificar lo que
alguien afirma sobre tu trabajo, y REMITIR antes de publicar una cifra hacia fuera.

**Cómo se usa:** no se lee entera. Cada sección es independiente y su título dice qué resuelve; ve a
la que necesites. Si vas a publicar una cifra, el mínimo son tres: *una cifra sobre tu propio corpus
es una FOTO*, *y comprueba el valor esperado del control*, y *un número sin expectativa no es
información*.

## El comando no es el patrón, y confundirlos parece sustrato

**Caso de campo, y las dos partes lo leyeron mal antes de leerlo bien.** Un corresponsal publicó
patrón y corpus fijado por hash de su verificación, y reportó **1**. El destinatario lo corrió y obtuvo
**0**. Los dos concluyeron **diferencia de sustrato**. Era falso:

```text
substrate: grep (GNU grep) 3.0        <- EL MISMO BINARIO A LOS DOS LADOS
corpus:    <commit>:<fichero>

grep -c   'comando \+ patr'   -> 0     BRE: \+ es el CUANTIFICADOR uno-o-mas
grep -cE  'comando \+ patr'   -> 1     ERE: \+ es un + escapado, o sea literal
grep -cF  'comando + patr'    -> 1
CONTROL   grep -c 'corpus'    -> 56 / 53 en las dos revisiones, en las dos maquinas
```

> **AVISO AL BARRIDO: el bloque de arriba cita el patrón erróneo LITERAL, a propósito** — la lección
> consiste en ver la misma cadena dar 0 y 1 según la bandera, así que describirla en vez de citarla la
> destruye. Si un detector tuyo busca ese patrón, lo encontrará **aquí, dentro de la sección que lo
> desmiente**: es un hallazgo cierto e inútil, y esta línea existe para que lo descartes en un segundo.
> **Y ojo al buscarlo: `grep` sin `-F` usa el patrón malo para buscar el patrón malo y devuelve CERO.**
> Lo aportó `plataforma_iot` (*describe la corrección, no la cites*); aquí se re-derivó a **declararlo**,
> porque en prosa no se puede generar un hogar desde otro, pero sí avisar.

**Las dos cifras salen en la misma máquina.** Lo que las separa es `-E`, una bandera que el bloque
publicado no llevaba: decía `patron:` y decía la verdad. **No faltaba una cuarta pieza — falló la
primera.** Si el comando hubiera viajado entero, la medición se re-corre.

De ahí que la regla de tres salga **más fuerte y no matizada**, con una precisión sobre su primera
pieza:

> **El comando es el binario, sus banderas y su versión — no la línea del patrón.** `grep` y `grep -E`
> son dos instrumentos distintos con el mismo nombre, y publicar el patrón sin la bandera es publicar
> medio comando y llamarlo instrumento.

## Un desacuerdo demuestra que algo difiere; nunca demuestra QUÉ

**La ley general del caso de arriba, y es la parte que hay que recordar.** Al ver `0` contra `1` los
dos lados saltaron a la explicación **cara** —el sustrato, que obliga a una pieza que no se puede
mandar por correo— cuando la **barata** estaba a un `-E` de distancia. Ninguno de los dos la miró.

**Antes de atribuir un desacuerdo al sustrato hay que agotar el instrumento**, y agotarlo tiene una
forma concreta y barata: **correr las dos variantes en la misma máquina**. Si las dos cifras aparecen
en un solo sitio, no hay nada que atribuir a dos sitios.

**Y la asimetría que lo hace peligroso:** la explicación de sustrato es más interesante, más difícil de
refutar y más halagadora para quien la encuentra — parece un hallazgo profundo en vez de una errata. Es
exactamente el perfil de conclusión que nadie va a ir a comprobar.

**Y hay una forma más estrecha de esa asimetría, que es la que se puede detectar en el momento:**

> **Gana la explicación que sitúa el fallo FUERA del que mide.** Y eso sí tiene disparador: no hay
> manera de saber si una explicación es *interesante*, pero sí de saber si tu medición **está a punto
> de producir una afirmación sobre otro**. Ese es el instante de agotar el instrumento.

## Antes de llamar sustrato a un desacuerdo, mira si alguien está violando una especificación

**Porque una especificación es un tercero, y es gratis.** No hace falta un segundo corresponsal ni una
segunda máquina para tener una expectativa que ninguna de las dos partes pudo ajustar: cuando la
conducta está fijada por un estándar, **la respuesta correcta se sabe sin medirla en ningún sitio**.

Caso de campo: `grep -icF 'x' fichero` sobre dos líneas ASCII tiene una respuesta que POSIX fija —**2,
y salida 0**—. Una máquina donde eso aborta no exhibe *una diferencia de sustrato*: exhibe **un
incumplimiento**. Y la distinción no es académica, porque cambia qué hay que hacer:

| Si el desacuerdo es… | Qué hay | Qué se hace |
| --- | --- | --- |
| **Instrumento incompleto** | Una bandera sin publicar | Se completa el comando |
| **Incumplimiento de especificación** | Un fallo | Se **reporta**, y no hay nada que estudiar |
| **Sustrato** | Una diferencia real entre montajes | Hace falta el otro extremo |

**Solo el tercero necesita a alguien más**, y es el más caro de los tres. Por eso se mira en ese orden:
primero el instrumento, después la especificación, y **sustrato es lo último que se concluye, no lo
primero**. En la correspondencia que produjo esta regla, los dos primeros escalones explicaron todos
los casos y el tercero no llegó a hacer falta ni una vez.

**Y el escalón de la especificación tiene un efecto secundario que conviene conocer:** convierte *"no
sabemos de quién es el problema"* en *"alguien tiene un bug"*, que es accionable aunque no se sepa
dónde vive. Saber **que** algo está roto y no **dónde** ya permite dejar de mezclarlo con los datos.

## Una especificación también dice dónde se CALLA, y ahí la divergencia está anunciada

**El escalón de la especificación se escribió con una sola salida —*alguien incumple*— y tiene dos.**
Una especificación no solo fija la conducta correcta: **delimita dónde hay conducta correcta**. Donde
se declara a sí misma no especificada, dos montajes pueden discrepar sin que ninguno tenga un fallo — y
eso **se sabe leyéndola, sin el segundo montaje**.

| Si la especificación… | Qué hay | Qué se hace |
| --- | --- | --- |
| **fija la conducta y alguien se desvía** | Un fallo | Se reporta |
| **declara la zona no especificada** | Una divergencia permitida | No se publica ahí un detector; y si se mide, se declara el montaje |

**Caso de campo, y es propio y caro.** Se comparó `[ -~]` sobre el mismo corpus de 34 caracteres en dos
máquinas —Cygwin contra glibc 2.35— y salió `6 + 28` allá contra `32 + 2` aquí bajo `en_US.UTF-8`. Se
concluyó **sustrato**, la libc, y la medición es correcta. Lo que no se miró son las otras dos filas de
la misma tabla, que ya estaban publicadas a los dos lados:

```text
                      aquí (Cygwin)   allá (glibc 2.35)
LC_ALL=C               32 + 4 = 36     32 + 4 = 36     <- POSIX FIJA esta fila: coinciden
LC_ALL=C.UTF-8         32 + 2 = 34     32 + 2 = 34     <- coinciden también
LC_ALL=en_US.UTF-8     32 + 2 = 34      6 + 28 = 34    <- SOLO esta diverge
```

**Divergía un locale de tres**, y es justo aquel sobre el que POSIX (XBD 9.3.5, *RE Bracket
Expression*) declara la interpretación de un rango **no especificada fuera del locale POSIX**. Donde la
especificación manda, las dos máquinas daban lo mismo: **todo el desacuerdo vivía en la zona donde se
calla**, que es la única fila que se publicó como hallazgo.

> **Una especificación no te dice qué hace la otra máquina: te dice que no tienes derecho a esperar
> nada.** Y para lo que casi siempre se está decidiendo —*¿puedo publicar este patrón como detector?*—
> eso basta, y es gratis.

**Y el coste de saltarse esta cara del escalón se mide en lo que se gastó**: el hallazgo necesitó un
corresponsal, su máquina y varias cartas, y su parte accionable estaba en una frase de un documento
público. La medición no se cae —dice qué hace **esta** máquina, y eso solo se sabe midiendo—; lo que no
hacía falta era **descubrirlo**.

## Cuando la especificación es propia, lo que la valida es su FECHA

**Y si es propia hay que sospechar, porque quien la escribió es quien se beneficia de ella.** Al
validar una herramienta B contra otra A, los desacuerdos se reparten en tres: *B es mejor*, *B es
peor*, y *B es distinto a propósito* — y **el separador entre los dos últimos es la especificación de
B, que escribió el autor de B**. Una especificación redactada después de ver el fallo convierte *peor*
en *distinto a propósito* sin que nadie mienta.

> **Lo que separa las dos no es qué dice la regla: es si pudo ajustarse al resultado.** Y eso lo fija
> el reloj: **vale la diferencia declarada ANTES de correr la sonda; no vale la declarada después.**

Cuesta un `git log` y da tres salidas:

```text
para cada diferencia que la sonda excluye:
  fecha del commit que la declaro   vs   fecha de la sonda
  anterior  -> distinto a proposito, DEMOSTRADO
  posterior -> peor, reetiquetado
  sin commit que la declare -> no es especificacion: es la sonda opinando
```

## El nombre de una comprobación no es su cobertura

**Y un nombre que enuncia el invariante es PEOR que uno que no dice nada**, porque se lee como si lo
cubriera. Nadie vuelve a mirar dentro de algo que ya se llama *"comprueba siempre que…"*.

**Caso de campo:** un test llamado `TestPrintMatchesAlwaysEmitsTheGrepEquivalent` ejercitaba **un
camino de cuatro**. Decía *siempre* y probaba el caso por defecto. **Sobrevivió a cuatro auditorías**
—las cuatro leyeron el nombre y siguieron— y el defecto que ocultaba era que dos de los otros tres
caminos violaban el invariante **desde el primer commit**.

> **Al auditar una comprobación, cuenta sus casos, no leas su nombre.** El nombre lo escribió quien
> creía que estaba cubriendo todo; si hubiera visto el hueco, no lo habría llamado así.

Vale para cualquier cosa que afirme cobertura: un test, una sección de *Verificación* en un acta, una
tabla de controles. **La forma del fallo es siempre la misma —el rótulo promete un universal y el
cuerpo cubre un caso— y el rótulo es lo único que se relee.**

**Pero al ir a buscarlo, la forma cambia con el sujeto — y conviene saberlo antes de gastar la
pasada.** Esa extensión se comprobó donde más barato salía, las secciones de *Verificación* de 100
actas propias (medido el 2026-08-11), y **el defecto no estaba ahí**: ningún cuerpo vacío, ningún
rótulo universal tapando un caso suelto. Lo que hay en un acta es **intermitencia** — comprobaciones
reales que unas veces se registran y otras no. No refuta la regla; delimita dónde buscar: **en un
registro escrito a mano, mide la intermitencia y no el rótulo.** Para ese caso la regla es otra —
`cerrar.md` → *Escribir la comprobación no la corre* — y el detector, también.

## El sello de una cifra certifica que sigue viva, no que sea la cifra de lo que dices

**Un identificador de corpus responde *¿este número sigue valiendo?* y no responde *¿es este el número
de lo que estoy afirmando?*** Son dos preguntas distintas y solo la primera tiene mecanismo. Por eso
una tabla puede estar sellada, ser reproducible al token, y sostener una frase falsa **sin que ninguna
de sus cifras esté mal**.

**Caso de campo, en este mismo kit.** Una tabla de tamaños medía tres ficheros —el enrutador y dos
rituales— y sumaba `12 699`. La prosa de debajo llamaba a esa suma *"el arranque"*, y sobre esa palabra
se decidió durante dos corpus qué había que podar. Medido después: lo que la **puerta** importa de
verdad son otros cuatro ficheros, y

```text
ficheros en comun entre el conjunto medido y el conjunto cargado al arrancar : 0
```

**Disjuntos.** Podar cualquiera de los tres ficheros medidos habría bajado el arranque en **cero**
tokens. La tabla nunca mintió; la palabra que la resumía apuntaba a otro sitio.

> **Nómbrale a una cifra su corpus por sus FICHEROS, no por lo que hace.** *"`SKILL.md` + `abrir` +
> `cerrar`"* se puede contar y contradecir; *"el arranque"* no se puede contradecir porque no se puede
> contar. Un nombre funcional es una afirmación sobre el mundo disfrazada de etiqueta.

**Es la misma forma que *el nombre de una comprobación no es su cobertura*, con el sujeto cambiado:**
allí el rótulo prometía un universal y el cuerpo cubría un caso; aquí el rótulo promete un corpus y el
cuerpo mide otro. En los dos, **el rótulo es lo único que se relee** — y aquí además va firmado, que es
peor: el sello invita a dar por revisado todo el párrafo.

**El detector es barato y no es leer.** Enumera los ficheros que tu cifra dice cubrir, enumera los que
de verdad se cargan —en el arranque, leyendo qué importa la puerta; en un ritual, qué abre— e
**intersécalos**. Si la intersección no es lo que esperabas, el defecto no está en el número.
**Y espera un valor antes de mirar**: una intersección vacía y una total se leen igual de tranquilas
cuando no habías declarado cuál tocaba.

## Una justificación producida al auditar el defecto no tiene procedencia

**Es la tercera fila del test de la fecha, aplicada a una justificación en vez de a una
especificación.** Cuando se encuentra un comportamiento raro y se explica en el momento con una razón
sensata, esa razón **no estaba antes**: se produjo para el hueco que se acababa de ver.

**Caso de campo, y lo contó quien lo cometió:** al descubrir que dos caminos de salida no cumplían un
invariante, la explicación fue *"son salida para máquinas y añadirles texto rompería a quien las
pasa por una tubería"*. Es una buena razón. Y al buscarla: **cero commits y cero documentos** la
declaraban. **No se rompió después: nació así, en el mismo commit que el invariante que viola.**

> **La justificación no es falsa por producirse tarde — es que no puede llamarse *decisión*.** Una
> decisión tiene fecha; una racionalización, no. Y la diferencia importa porque una decisión se puede
> revisar contra lo que se sabía entonces, y una racionalización solo contra lo que conviene ahora.

**El remedio es barato y es el mismo:** adoptarla **con la fecha de hoy**, dicho así. *"Decidido el
2026-08-10"* es honesto y sigue siendo útil; *"siempre fue a propósito"* es lo que no se puede
sostener.

## Declarar una variable de entorno no es controlarla

**Y publicarla sin variarla ni una vez la convierte en decoración.** Un bloque de instrumento que dice
`LC_ALL unset` en cada medición parece riguroso —nombra la variable, la fija, la publica— y **no aporta
nada** si nadie la mueve: lo único que demuestra es que todas las corridas se hicieron en el mismo
sitio, que ya se sabía.

**Caso de campo, medido, y en los dos lados a la vez.** Dos proyectos publicaron `LANG unset, LC_ALL
unset` en la cabecera de instrumento de **seis cartas** durante dos semanas, y **ninguno varió el
locale ni una vez**. Cuando por fin se varió, resultó ser el discriminante de todo lo que llevaban
atribuido a la plataforma:

```text
LC_ALL=C            grep -icF  -> exit 134   ABORTA
LC_ALL=C.UTF-8      grep -icF  -> exit 0
LC_ALL=en_US.UTF-8  grep -icF  -> exit 0
```

**Lo habían encuadrado como *"un defecto de esta máquina"* durante tres cartas.** Y el proyecto que lo
sufría tenía la técnica escrita en su propio fichero de trampas, **tres secciones más arriba**, aplicada
a otro caso.

> **Una variable declarada y nunca variada no es parte del instrumento: es parte del decorado.** Lo que
> la convierte en instrumento es **haberla movido al menos una vez** y saber si el resultado cambia.

Es la misma forma que *un número sin expectativa es decoración*, un piso más abajo: allí falta el valor
esperado, aquí falta el contraste. **Y el coste de moverla es una línea**, que es lo que la hace
imperdonable.

**Precondición, que decide si el test se puede correr siquiera: la diferencia tiene que tener un nombre
en el código.** Se fecha con `git log -S '<identificador>'`, así que **una diferencia que vive en el
comportamiento y no tiene nombre no tiene commit que la feche** — y caería en la tercera fila siendo
falso. Eso hace que el test mida dos cosas a la vez, y la segunda no estorba: **un proyecto que no
puede nombrar sus decisiones tampoco puede demostrarlas.**

**Lo que este test NO hace**, y conviene decirlo con él: no distingue *distinto a propósito* de
*distinto a propósito y además peor*. Una diferencia declarada con dos años de antelación puede seguir
siendo una mala decisión. **Protege del reetiquetado, no del diseño.**

## Medir prosa por líneas es medir otra cosa

**En un corpus de prosa envuelta, una frase de más de una palabra puede partirse entre dos líneas, y
entonces `grep` no puede verla.** No es un fallo del patrón ni del locale: el modelo de `grep` es la
línea, y la unidad de la prosa es el párrafo.

```text
corpus real, un acta envuelta a ~100 columnas:
  l.18   ...La primera, en la sesión 27, la cazó el
  l.19   usuario preguntando.

buscando 'la cazó el usuario':
  grep      -> 0     no puede casar a traves del salto
  por unidad-> 1
CONTROL POSITIVO  fichero fabricado con la frase partida    grep 0 / por unidad 1
CONTROL NEGATIVO  fichero sin la frase                      0 en los dos
```

**Toda medición de frases multi-palabra sobre prosa envuelta subcuenta por construcción**, y el sesgo
no es aleatorio: se come justo las frases largas, que son las que expresan las clases interesantes. Una
medición así **no da un número con error: da un suelo**, y hay que publicarlo como suelo.

**Y el sesgo está medido, no estimado.** Sobre un corpus de prosa envuelta a ~100 columnas, extrayendo
frases **reales** del propio corpus y buscándolas por los dos modelos:

```text
corpus: 97 documentos, 1190 parrafos multilinea
IDENTIFICADOR DEL CORPUS: 090f85e021ab7b11   <- sha256 de (nombre, tamano) de cada fichero

 k palabras   muestras   por unidad   por linea   sesgo
     2            200          200         188     6.0%
     4            200          200         174    13.0%
     6            200          200         139    30.5%
     8            200          200         123    38.5%
    10            200          200          96    52.0%
    12            200          200          75    62.5%

CONTROL POSITIVO  la frase sale del corpus, asi que "por unidad" DEBE dar 100%: 1600/1600
CONTROL NEGATIVO  frase inventada -> 0
GLOBAL            se pierden 462 de 1600 = 28.9%
```

## Una cifra sobre tu propio corpus, escrita en el kit, es una FOTO

**Y hay que escribirla como tal: con fecha y en pasado.** El kit es un sitio donde **nada la mueve** —
no hay ningún paso, en ningún ritual, que vuelva a correr una medición publicada dentro de un
documento de producto. Así que una cifra medida sobre el historial, el archivo de correspondencia o
cualquier corpus **que crece** empieza a envejecer el día que se escribe, en silencio y sin que nadie
la contradiga.

**Es la misma ley que gobierna los estados** —*un dato que caduca solo puede vivir donde algo lo
mueva*, en CONTRASTAR— aplicada a los números, que es donde no se había mirado: un ratio parece un
hecho y un estado parece un estado, así que a los estados se les busca dueño y a los ratios no.

**Medido en una auditoría real, sobre las cifras de este mismo ritual:**

```text
"23 de 36 cartas recibidas"   -> re-medido: 27 de 39      la conclusion aguanta, la cifra no
"10 de 62 referencias"        -> el corpus es 104 hoy
"60 referencias" y "62"       -> el MISMO fichero da dos denominadores del MISMO corpus
"Sin raiz (30%)" y "16%"      -> la tabla trae el test intuitivo y el texto el correcto, sin decirlo
```

Las tres formas —envejecer, contradecirse y mezclar dos métodos— **salen de lo mismo**: nadie volvió a
correrlas porque no había dónde. Y ninguna se ve releyendo.

**Lo que funciona no es re-medir cada cierre** —eso es el remedio O(n) que hay que mantener— sino
**escribirlas de una forma que no caduque**: fecha, verbo en pasado, y de quién era el corpus. *"Medido
el 2026-08-09 sobre 60 referencias de la instancia que lo halló"* sigue siendo cierto para siempre;
*"medido aquí sobre 60 referencias"* deja de serlo sin avisar. **Cuesta lo mismo escribirlo bien.**

**Y cuando dos cifras del mismo texto no cuadren y el corpus ya no exista, no se arbitra: se anota.**
Inventar cuál era la buena es exactamente el fallo que estas reglas persiguen.

**El identificador del corpus no es adorno, y esta tabla es la prueba.** Su primera versión se publicó
con **semilla fija y corpus sin fijar**, y dejó de reproducirse en cuanto el corpus creció en **un**
fichero —el acta de la sesión que la había medido—: `29,2%` pasó a `28,9%` sin que nadie tocara nada.
**Una semilla fija sobre un corpus móvil no es re-corrible**, y el fallo es especialmente traicionero
porque la semilla da *sensación* de determinismo. Si la medida tiene azar dentro, **fija las dos
cosas**, y publica un identificador que cambie cuando cambie el corpus.

**A doce palabras se pierden casi dos de cada tres.** Y el crecimiento es monótono con la longitud, que
es la firma del mecanismo: cuanto más larga la frase, más probable que cruce un salto. Por eso la regla
práctica es **buscar la parte más corta y distintiva**, no la frase entera — y por eso una cifra
obtenida con una frase larga hay que publicarla diciendo con qué frase se obtuvo.

<!-- Medido con semilla fija para que sea re-corrible; el corpus era el historial de la instancia que
     lo descubrió, así que el número concreto es suyo. Lo que viaja es el método y el orden de
     magnitud, no el 28,9%. -->

## Un comando que aborta, detrás de una tubería, es un cero

**Y el cero que produce es indistinguible del honesto.** Caso medido, con el mismo patrón y el mismo
fichero, cambiando solo las banderas:

```text
grep -cF   'el usuario'  -> exit 0     imprime 1
grep -icF  'el usuario'  -> exit 134   no imprime nada     SIGABRT
grep -icF ... | wc -l    -> 0          la tuberia se traga el codigo de salida
```

El fallo era **visible** —código 134, y un fichero de volcado en el directorio— y la tubería lo
convirtió en un `0` limpio. Quien lo midió lo publicó como *"devuelve cero en silencio"*: describió el
síntoma que su propio comando había fabricado.

> **Publica el código de salida, o mide sin tubería.** Un `| wc -l` convierte cualquier fallo en un
> dato, y un dato no se distingue de otro dato.

**Esto le pone una condición a la regla del instrumento:** si el comando publicado termina en tubería,
**no basta con el comando** — hace falta también qué devolvió. Es la diferencia entre *no hay
coincidencias* y *no hubo búsqueda*, que es justo la que un cero no sabe expresar.

**Ojo con el remedio:** al cambiar de herramienta cambian más cosas que el modelo —el marcado, la
normalización, qué cuenta como unidad—, así que **valida sobre casos que la primera sí veía**, no solo
sobre los que se le escapaban. Es higiene barata y se pide por precaución.

<!-- HONESTIDAD SOBRE ESTA REGLA: aquí decía que la herramienta de unidad "puede perder algo que la
     otra encuentra", y el único ejemplar que lo sostenía RESULTO FALSO -1 de 8 frases, y el fallo
     estaba en el pipeline de quien medía, que corrompió la frase entre procesos-. El corresponsal
     re-midió 394 frases y la de unidad las encontró TODAS. Así que la precaución se queda y la
     afirmación de hecho se retira: **no tenemos ningún caso** de que la de unidad pierda algo. La
     regla vale por lo que ahorra si pasa, no por haberlo visto pasar. -->

**Y el propio caso deja una trampa más barata de sufrir que todo lo anterior:** al comparar dos
herramientas, **el corpus tiene que llegar intacto a las dos**. La frase de aquel 1 de 8 viajó de un
proceso a otro por un fichero temporal y llegó con bytes rotos, así que **ninguna de las dos podía
encontrarla** — y el resultado se leyó como *"la segunda falla"* en vez de *"mi tubería rompe el
corpus"*. **Cuando las dos den cero, sospecha del transporte antes que de las herramientas**: es la
única hipótesis que explica un cero doble sin que nada más falle.

Remedios, en orden de coste: buscar por **unidad** en vez de por línea; o buscar la parte más corta y
distintiva de la frase, que cabe en una línea; o desenvolver el corpus antes de medir. Lo que **no**
vale es afinar el patrón, porque el patrón no es el problema.

> **Publicar el instrumento no valida el instrumento.** El comando dice **qué corriste**; el control
> dice **si funcionó**. Son preguntas distintas, y la que un lector quiere contestar dentro de diez
> sesiones es la segunda.

**Y por eso lo que hay que guardar es el control y no más instrumento:**

- **El corpus es grande y el sustrato no se manda por correo.** Si la respuesta fuera *comando +
  corpus + sustrato*, sería **imposible**, no cara.
- **El control cuesta O(1) y viaja.** Dos líneas: una entrada de respuesta conocida y el número que
  tiene que dar.
- **Y es lo único que separa los dos motivos de una discrepancia.** Re-correr en otro sustrato
  reproduce **el número o el fallo**, y desde fuera se leen igual. Con el control al lado: si el
  control sigue dando lo suyo, **cambió el mundo**; si el control también cambia, **cambió el
  instrumento**.

Así que el registro **no deja de prometer** — pasa a **prometer otra cosa**: no el re-corrido, el
control.

## No verifiques a alguien con su propia herramienta

Corolario directo del ejemplar de arriba, y cuesta perderlo de vista porque la herramienta del otro
suele ser mejor que la tuya. **El hallazgo existió porque los dos instrumentos eran distintos.** Con la
misma herramienta a los dos lados, las dos cifras coinciden y el acuerdo se lee como confirmación —
cuando lo único que prueba es que el código compartido se comporta igual que él mismo.

**Vale igual para un proyecto y una herramienta que él mismo produjo.** Adoptarla para el trabajo
propio puede ser correcto; usarla para **comprobar lo que esa herramienta afirma** no lo es. Y la
frontera no es la calidad del instrumento: es si **puede fallar por separado** del que se audita.

**Y "el mismo instrumento" se decide en la invocación, no en el binario** — lo corrigió el corresponsal
al que se le aplicó la regla. Con la misma herramienta a los dos lados no hay hallazgo posible, cierto;
pero con el mismo binario y **la misma bandera** tampoco, y ahí el binario no era el problema. `grep` y
`grep -E` fallan por separado; `grep -E` y `grep -E` no. **La independencia que sirve es la de la
invocación completa**, y por eso el remedio no es tener dos herramientas: es que las dos corridas sean
reproducibles hasta la bandera.

## El sustrato que dos corresponsales no pueden variar es el agente

Cuando dos proyectos se auditan por carta, conviene mirar el reparto de dónde salieron los hallazgos:

| Sustrato | ¿Varía entre los dos? | Hallazgos |
| --- | --- | --- |
| Herramientas (libc, binario, locale) | **sí** | varios |
| Razonamiento (modelo, familia de agente) | **ninguna** | **cero** |

**El cero no es evidencia de que esa capa sea uniforme: es evidencia de que es la que no se puede
variar.** Dos agentes de la misma familia comparando notas pasan por alto **a la vez y en silencio**
todo lo que su arquitectura compartida les haga pasar por alto, que es la definición misma de sustrato.

**Ejemplar medido:** dos proyectos sostuvieron **ocho cartas** escritas en una variante del idioma que
**ningún** usuario de los dos usaba, sin que ninguno de los dos agentes lo notara, **por compartir el
mismo default**. Lo cazó una persona.

> **Si un corresponsal es un instrumento, el usuario es otro** — y mide una clase distinta: justo la
> que dos agentes del mismo modelo no pueden ver el uno en el otro, porque para eso tendrían que
> diferir en lo que los hace iguales.

**Y de ahí lo accionable:** cuando el humano corrige algo que el agente daba por bueno, **eso no es una
preferencia del usuario: es la única medición disponible de esa clase**, y se registra como hallazgo,
con su fecha y lo que destapó. Registrarla como preferencia la deja fuera de toda auditoría futura,
porque las preferencias no se comprueban.

**Hay un límite duro a lo que dos agentes iguales pueden saber juntos.** La correspondencia levanta el
límite de lo que un proyecto sabe de sí mismo y **no toca este**.

## No se enumeran los fallos: se enumeran las CAPAS

**Aporte de campo, y cierra la pregunta que dejaba abierta la redundancia.** Medir por dos vías solo
sirve si las dos **fallan distinto**, y eso nadie lo sabe antes de conocer el fallo. La salida no es
adivinar el fallo: es **enumerar por dónde pasa la respuesta**. Cualquier medición sobre texto
atraviesa unas pocas capas, y son **estructurales, no históricas** — se conocen sin haber visto
ninguna.

| Capa | Qué la varía | Qué caza |
| --- | --- | --- |
| **Corpus** | `grep -r` contra `git grep` | ficheros ignorados, árbol contra commit |
| **Herramienta** | `grep` contra `/usr/bin/grep` | el binario sustituido |
| **Semántica** | `LC_ALL=C` contra `LC_ALL=C.UTF-8`, **y la libc bajo ellos** | clases, mayúsculas, orden |
| **Patrón** | clase contra alternancia contra literal | lo que el regex no expresa |
| **Introspección contra comportamiento** | lo que la herramienta **dice** de sí misma contra lo que **hace** | el campo que miente sin fallar |

> **La lista de fallos es abierta; la de capas es corta y fija.** Por eso una se puede escribir antes
> de medir y la otra no.

**Y la condición que hace interpretable una redundancia: las dos vías tienen que diferir en UNA capa
y coincidir en el resto.** Si difieren en dos, la discrepancia ya no dice cuál falló — deja de ser un
diagnóstico y vuelve a ser un dato que hay que diagnosticar. `grep -r` contra `git grep` es un par
bueno **para el corpus** y nulo para el locale, porque comparte binario, semántica y patrón: no está
mal elegido, es que **solo cubre su capa**.

**Las tres preguntas del marcador son las tres primeras capas** — *¿en qué copia?* es corpus, *¿en qué
herramienta?* es herramienta, *¿bajo qué locale?* es semántica. Las otras dos no tienen todavía su
pregunta, y verlo así es la ventaja de tener la lista: un hueco en una tabla se ve, y en una prosa de
preguntas sueltas no.

**La regla que sí se aplica antes de medir: varía la capa que menos podrías defender si te
preguntaran.** Uno no sabe el fallo, pero **sí sabe cuál es su capa floja**. En un recuento que salió
mal la capa floja era el **corpus** —*¿qué ficheros cuentan?* no tenía respuesta escrita—; en un
detector roto era la **semántica** —diacríticos y ningún locale declarado—. Las dos veces la pregunta
incómoda existía **antes** del fallo, y lo que faltó fue hacérsela.

**La capa de introspección entró por un ejemplar y no por reflexión**, que es el dato más útil sobre
la tabla: dice que está incompleta y **por dónde va a crecer**. Para averiguar el locale efectivo no
se leyó la variable — se comparó lo que `locale` **dice** con lo que `grep` **hace**. De ahí la forma
corta, **no preguntes qué dice, mide qué hace**, que es mejor que la tabla porque no depende de haber
enumerado la capa correcta. Su aplicación al informe de entorno está en `core/templates/letter.md`.

**Y una capa mal especificada se comporta igual que un fallo no enumerado**, que es el modo de fallo
propio de este método y salió a la primera vez que se usó. Dos proyectos midieron el mismo patrón,
`[^ -~]`, bajo el **mismo nombre de locale**, `en_US.UTF-8`, con el mismo binario sin envolver: uno
obtuvo **2** y el otro **28**. Se enumeraron las capas, se aisló la semántica, y la discrepancia
siguió en pie — porque *semántica* se había escrito como *el locale*, y el locale es solo su nombre:

Sobre la línea *"una línea con dos acentuadas: café"* — **34 caracteres, 32 ASCII y 2 acentuadas**:

| Bajo `en_US.UTF-8` | Máquina A (glibc 2.35) | Máquina B (Cygwin/MSYS 3.4.10) |
| --- | --- | --- |
| `[ -~]` — la clase **positiva** | **6** de 32 | **32** de 32 |
| `[^ -~]` — la negada | **28** | **2** |
| suma | 34 | 34 |

> **El nombre de un locale no predice su comportamiento: lo predice el nombre más la libc que lo
> implementa.** `en_US.UTF-8` trae tabla de colación en glibc y no la trae en Cygwin. Y lo que se rompe
> no es la negación: **es el rango**. `[ -~]` no significa *ASCII imprimible* sino *lo que cae entre el
> espacio y la virgulilla **en orden de colación***, que en glibc son 6 caracteres de 32.

**Esa tabla tiene dos filas más que no se enseñaron aquí, y estrechan lo que se puede concluir:** bajo
`LC_ALL=C` las dos máquinas dan `32 + 4 = 36`, y bajo `C.UTF-8` las dos dan `32 + 2 = 34`. **Diverge un
locale de tres**, y es el único que POSIX deja sin especificar — ver *una especificación también dice
dónde se calla*. La ley de arriba se sostiene; lo que cambia es su radio: no es que el nombre de un
locale no prediga nada, es que **deja de predecir justo donde la especificación dejó de mandar**.

**Y la sonda importa tanto como el hallazgo, porque la primera que se usó aquí no servía.** Se probó
con `[a-z]` contra `B` —*"¿colaciona este locale?"*— y da **0 en los tres locales en las DOS
máquinas**: verde a los dos lados de una diferencia real. La conclusión que sostenía era correcta y la
evidencia no, que es la peor combinación porque no se distingue de estar en lo cierto.

> **Una sonda mal cortada pasa en verde a los dos lados.** Es el mismo defecto que la capa mal cortada,
> un piso más abajo: se preguntó por el mecanismo (*¿hay colación?*) mirando **un** síntoma elegido a
> ojo, en vez de medir **la cosa que el patrón investigado usa** — el rango mismo.

**La sonda buena mide la clase positiva y suma.** `[ -~]` más `[^ -~]` tiene que dar el número de
caracteres de la línea; si da los **bytes**, estás en semántica de bytes, y si el positivo se desploma
sin que la suma cambie, hay colación. Las tres situaciones se separan con dos comandos.

**Lo enseñable no es el dato de plataforma: es que la lista de capas hereda el problema que venía a
resolver.** Una capa demasiado gruesa no da error — da una discrepancia que sobrevive al aislamiento,
y eso se lee como *"aquí hay dos capas todavía"* cuando en realidad hay una mal cortada. **Cuando
aísles una capa y la diferencia siga viva, sospecha del corte antes que de la lista.**

## Las capas se cortan por lo que ESCRIBES, y lo que no, es el SUSTRATO

**Aporte de campo, y contesta por qué la lista se quedó corta sin estar mal hecha.** Las capas de
arriba están todas cortadas por **el nombre de lo que uno teclea** —`grep` contra `/bin/grep`,
`LC_ALL=C` contra `LC_ALL=C.UTF-8`— y no por el mecanismo que hay debajo. **No es un descuido: es la
definición.**

> **Una capa sirve si se puede VARIAR, y lo único que uno varía directamente es lo que escribe.** El
> mecanismo no se varía: **se hereda.**

De ahí lo que **no** es una capa, y que faltaba en la lista:

> **El SUSTRATO** — la libc, la versión del binario, los datos de locale generados en esa máquina, el
> sistema. **No cambia al escribir distinto: cambia al cambiar de máquina.** Las capas cazan las
> variaciones que uno puede producir, y son **ciegas al sustrato por construcción**.

**Y la consecuencia es dura: el experimento del sustrato no existe dentro de un proyecto.** Nadie
puede variar su propia libc para ver qué pasa. Un proyecto solo, por disciplinado que sea, agota sus
capas y se queda con una discrepancia sin explicar — que es exactamente lo que pasó aquí: se aisló la
semántica y **la diferencia siguió viva**.

**Un sustrato no se declara: se descubre.** Y se descubre cuando **otro mide lo mismo y le sale
distinto**. Un informe de entorno no lo entrega, porque nadie escribe *"mi libc es Cygwin"* mientras no
sepa que eso importa — y cuando ya lo sabe, el hallazgo está hecho. **Dos ejemplares, los dos de la
misma forma:** `[ -~]` casa 6 de 32 caracteres en una máquina y 32 de 32 en otra, con el mismo patrón
y el mismo nombre de locale; y `grep --version` responde *GNU grep 3.7* en una máquina donde el nombre
`grep` lo sirve otro binario entero. **Ninguno de los dos lo habría visto quien midiera solo.**

**Lo operativo, que es corto:** cuando una discrepancia sobreviva a variar todas tus capas, **deja de
buscar capas y busca un segundo montaje**. Y al reportar cualquier medida de patrones, di el sustrato —
libc y versión—, no solo el comando y el locale, porque el que lea la medida puede estar en otro sin
saberlo.

**El límite, dicho por quien trajo la regla:** estas capas no son todos los fallos. Enumerarlas no
convierte la redundancia en un detector universal; la vuelve **barata y dirigible**. Fuera de ellas
sigue en pie la objeción de siempre — si las dos vías se eligen sabiendo qué se busca, la redundancia
es un detector más, con el sesgo de quien lo escribió.

Las secciones que siguen son cuatro de esas capas con su caso: la **herramienta** aquí debajo, el
**corpus** en *"Mide el producto sobre lo que se distribuye"*, la **semántica** en *"Un detector
léxico depende del LOCALE"* y el **patrón** en *"Las alternativas de un patrón son su lista de
distinciones"* — todas en este archivo.

## "El mismo comando" no es el mismo programa

**Aporte de campo, y desmonta la premisa de todo experimento compartido.** Dos proyectos corrieron
**la misma sonda literal** y obtuvieron resultados distintos. La causa no era el locale: en una de las
máquinas `grep` **no era GNU grep** — una función de shell inyectada por la herramienta lo enrutaba a
otro binario, con opciones propias. GNU grep estaba instalado y **no era el que respondía al nombre**.

> **Antes de comparar dos mediciones, di qué binario contesta.** Es el *mal dirigido* del marcador un
> piso más abajo: no *¿en qué copia está la evidencia?* sino **¿en qué herramienta se midió?**

**Y el mismo desajuste produce filtros muertos.** Ese `grep` no imprimía el prefijo `./` en las rutas y
el otro sí; un barrido que excluía con `^\./` **no excluyó nada y no dio error: filtró cero**. Es el
reverso de la sonda rota — no un cero de más, sino **un cero de menos** — y el síntoma es el mismo:
silencio. El resultado salió correcto **por accidente**, porque los aciertos eran pocos y se miraron a
mano.

## Mide el producto sobre lo que se distribuye, no sobre tu árbol

**Caso propio, y contra una regla que ya teníamos escrita.** Se publicó en una carta que *"19
documentos del kit nombran este rol"*. El corresponsal contó **18**, en cuatro commits y dos alcances.
El de más era **el manifiesto de la instancia, que está en `.gitignore`**: la cifra se midió con `grep`
sobre el **árbol de trabajo**, que contiene ficheros que **no son el producto**.

> Cuando la cifra es **sobre el artefacto que viaja**, se mide sobre lo que git guarda (`git grep`,
> `git cat-file`). El árbol solo vale para hablar del estado propio. **Un fichero ignorado es
> invisible al lector y visible a tu `grep`.**

**Y cómo lo trataron es la otra mitad:** tenían evidencia de **cuántos** y ninguna de **cuál** sobraba,
enumeraron **tres** explicaciones posibles y **no eligieron ninguna** — aplicando, cuatro horas después
de aprenderla, la regla de que una aritmética con dos soluciones no identifica un elemento.

**Y su mecanismo, que refina esa regla y desactiva el remedio que le habíamos propuesto:**

> **Un número no se descompone solo: se descompone en las categorías que uno ya trajo.** No hubo
> elección entre dos lecturas — hubo **identificación primero y aritmética después**, así que el número
> se leyó como confirmación de una clasificación que ya existía.

Por eso *marcar la conjetura* no sirve: **no se siente conjetura**. Lo único que la caza es el paso
mecánico y **previo** — contar cuántas soluciones admite el número **antes** de elegir una.

## Un detector léxico depende del LOCALE, y el mismo patrón da dos respuestas

**Un detector de este mismo ritual estuvo roto por esto, y era del producto.** El patrón de la clase 5
decía `sesi[oó]n [0-9]+` y no casaba la forma acentuada. **La causa NO es la clase: es el locale.**
Medido, mismo binario y mismo fichero:

| Locale | `sesi[oó]n [0-9]+` | `sesi(o\|ó)n [0-9]+` |
| --- | --- | --- |
| `C` / `POSIX` | **1** — no casa la acentuada | **2** |
| `en_US.UTF-8`, `C.UTF-8` | **2** | **2** |

En `C` no hay caracteres, **hay bytes**: la vocal acentuada ocupa dos, la clase casa uno, y el patrón
exige lo siguiente donde está el segundo. En UTF-8 el mismo `grep` es consciente de caracteres y la
clase funciona perfectamente.

> **Usa alternancia —`sesi(o|ó)n`—, que es correcta en los dos locales.** No porque la clase sea
> inválida, sino porque **depende de una variable de entorno que nadie declara**.

**Y lo peligroso no es que falle: es que falla A VECES.** El mismo detector, el mismo corpus y el mismo
binario dan **dos respuestas** según el locale. Un adoptante en UTF-8 que intente reproducir el fallo
**no lo verá** y concluirá que no existe; uno en `C` lo sufre en silencio. **Una medición de cuánto
pierde un detector así no mide el detector: mide el locale de quien midió.**

> **A *¿en qué copia?* y *¿en qué herramienta?* hay que añadir *¿bajo qué locale?*** — y el locale es
> su **nombre más la libc que lo implementa**, que no es lo mismo. Ver *"una capa mal especificada se
> comporta igual que un fallo no enumerado"*, más arriba en este archivo.

**Y declarar la variable no basta.** Aquí `LANG` y `LC_ALL` están **vacíos**, `locale` reporta
`LC_CTYPE="C.UTF-8"` —que funcionaría— y el `grep` sin prefijo **se comporta como `C`**. El locale
**declarado y el efectivo no coinciden**, así que lo que se guarda en un informe de entorno no es el
valor de la variable: es **el resultado del experimento**.

```bash
printf 'sesion 12
sesión 12
' > /tmp/s.txt
grep -cE "sesi[oó]n [0-9]+" /tmp/s.txt   # 2 = tu entorno entiende caracteres; 1 = bytes
```

**Una clase acentuada bajo cuantificador —`[a-záéíóúñ]+`— sí es robusta**: da 2 en los dos locales,
porque el `+` deja que los dos bytes entren como dos elementos. Comprobado. Pero la condición es el
cuantificador, y quien copie esa clase a un sitio sin `+` se lleva el fallo **intermitente**.

## Un control negativo deja de separar cuando su cadena entra al corpus

**Un control negativo es una cadena que NO debe estar, y su valor es que dé cero.** El problema es que
esa cadena hay que escribirla en algún sitio — y el sitio natural es el registro de la comprobación: el
acta, el informe, la carta. **Si ese registro forma parte del corpus que barres, el control se
autodestruye:** a partir de la sesión siguiente aparece, y el cero se convierte en uno.

**Caso propio, en vivo y en la sesión que documentaba la trampa.** Un barrido sobre el historial usó
`zzqqxx` como control negativo y devolvió **1** en vez de 0. No había ningún fallo en el detector: el
acta de la sesión anterior **contenía esa cadena, escrita ahí precisamente como control**. La disciplina
de registrar el control es lo que lo invalidó.

> **La contaminación es de dirección única y de efecto retardado:** el control funciona el día que se
> escribe y falla después, cuando ya nadie recuerda de dónde salió la cadena. Y falla **hacia el lado
> ruidoso** —da un positivo espurio—, así que se lee como *"el detector está roto"* y lo que está roto
> es el control.

**Es el pariente exacto de *el barrido se come el documento que describe el barrido*, un piso más
abajo:** allí el objeto medido contiene su propia descripción; aquí el corpus contiene el instrumento
que lo mide.

**Remedios, en orden de coste:**

- **Genera la cadena, no la elijas** — deriva el control del contexto (una permutación del patrón real,
  un identificador con la fecha) para que sea distinta cada vez.
- **Si la escribes, no la reutilices.** Un control negativo es de un solo uso en cuanto queda registrado.
- **Y cuando un control negativo dé un valor raro, mira dónde casó antes de tocar el detector.** Si casó
  en tu propio registro, el detector estaba bien y el control estaba quemado.

## `git log` no data un proyecto: data un repositorio

**La fecha del primer commit es la del primer commit, y nada más.** Es tentadora porque parece un
origen objetivo —está en el repo, es verificable, nadie la puede discutir— y por eso se publica sin
pensarla. Pero mide **cuándo empezó a haber commits**, que casi nunca es cuándo empezó el trabajo: antes
hubo prototipos, notas, otros repos, o una idea cociéndose en otro proyecto.

**Caso propio.** Se publicó que el proyecto *"dura 12 días"*, con su cálculo al lado —primer commit,
último commit, sesiones por día—. La aritmética era correcta y la afirmación falsa: eran doce días **de
consolidación**, y toda la gestación anterior no está en ningún corpus legible. Lo corrigió la única
persona que conocía la fecha real; **ningún control lo habría encontrado**, porque el instrumento
funcionaba perfectamente.

> **El fallo no es de medida: es de ETIQUETA.** Se le puso a un instrumento el nombre de otro. Y esa
> clase no la detecta ningún control positivo, porque el detector no está roto — está contestando bien
> a otra pregunta.

**La regla operativa es de una línea:** al publicar una cifra derivada del repositorio, **di que es del
repositorio**. *"El repo tiene N días"* es cierto y comprobable; *"el proyecto tiene N días"* es una
afirmación sobre el mundo que el repo no puede sostener.

**Y hay un corolario para cualquier registro:** lo que un corpus no contiene, no lo contiene **en
silencio**. Un historial que empieza en una fecha no dice *"aquí empieza todo"*, dice *"aquí empieza lo
que guardé"* — y desde dentro las dos frases se ven igual.

## Y comprueba el valor esperado del control, no solo su resultado

**Dos veces en una misma sesión el control "falló" y lo que estaba mal era la expectativa.** Un
`result(o|ó)` dio 2 donde se esperaban 3 —y el 2 era correcto, porque *resultado* no contiene
*resulto*—; y un barrido de comprobación dio 1 donde se esperaba 0, y el 1 era **la cita en prosa** del
patrón roto, no el patrón.

> **Un control positivo tiene dos partes y solo una se suele revisar.** El resultado se mira siempre; el
> **número esperado** se escribe de memoria y no se comprueba contra nada. Cuando el control no cuadra,
> **la primera hipótesis debe ser la expectativa**, no el artefacto — es la más barata de descartar y la
> que más veces resulta ser.

Y tiene un modo de fallo peor que el ruido: **una expectativa equivocada que coincide con el resultado
deja pasar un artefacto roto en verde**, y eso no se distingue de un control que funciona.

**Y esto se generaliza a cualquier kit que no esté en inglés:** un detector léxico escrito en un idioma
con diacríticos y probado con un control **sin** diacríticos pasa en verde y no mide nada. El control
positivo tiene que llevar **la forma acentuada**, que es la que el corpus usa de verdad.

## Un control corre sobre el PRODUCTO de un instrumento, así que puede acusar al trabajo

**La ley de arriba dice que dudes de la expectativa. Esta dice que dudes del material.** Un control de
integridad no compara el trabajo con la realidad: compara **lo que la tubería le entrega** con lo que
esperaba. Si alguna etapa de esa tubería altera el material, el control salta — y lo que señala es el
trabajo, que estaba bien.

**Caso de campo, y costó diecinueve sesiones.** Había que mover un bloque de secciones dentro de un
documento. Se hizo con `sed`, y se verificó con un control bueno: la firma del multiconjunto de líneas
—`md5(sort)`— tiene que ser idéntica, porque mover un bloque **reordena** y no cambia nada más. Dio
distinto, con líneas que aparecían solo en el original. Se leyó como *contenido perdido*, se abortó el
cambio, y se dejó escrito en el propio documento que **se intentó y el control lo paró**.

No se había perdido nada. El fichero estaba en CRLF y aquel `sed` **entrega las líneas sin el `\r`**:

```text
diferencia: 1 833 bytes sobre 1 833 lineas  =  exactamente un byte por linea
```

**El control era correcto y el instrumento que lo alimentaba, no.**

> **Y esta es la asimetría que la hace cara: un cero falso parece un éxito, pero un falso positivo
> parece DILIGENCIA.** Nadie audita un control que saltó — saltar es lo que se le pide. Peor: deja un
> registro escrito —*"se intentó y falló"*— que es exactamente lo que impide que el siguiente lo
> reintente. Un cero falso te deja creyendo que terminaste; un falso positivo te deja **creyendo que te
> salvaste**, y encima documentado.

**Lo accionable es una corrida más, y es barata: la IDENTIDAD.** Pasa el material por la **misma
tubería** sin hacer el cambio, y comprueba que el control da verde:

| Corrida | Qué dice si falla |
| --- | --- |
| **identidad** — misma tubería, cambio nulo | La tubería altera el material. **No es tu trabajo** |
| **el cambio real** — con la identidad ya en verde | Ahora sí: el fallo es del trabajo |

En el caso de arriba, la corrida identidad habría fallado igual, y el diagnóstico habría costado un
minuto en vez de diecinueve sesiones.

**Pero la identidad hereda la trampa de cualquier control positivo, y esto se descubrió ejerciéndola:**

> **Una corrida identidad en verde solo informa si sabes que sabría fallar.** Una tubería byte-exacta
> pasa la identidad **por construcción** — su verde está garantizado antes de correrla, y un verde
> garantizado no distingue nada.

**Así que la identidad se corre en PAR:** la tubería que vas a usar, y **otra que sepas que estropea el
material**. Medido al estrenar esta ley, sobre el mismo fichero y el mismo cambio nulo:

```text
identidad en la tuberia buena (binaria)  -> VERDE
identidad en una tuberia que mutila      -> FALLA, 1.000 bytes por linea
```

**Sin la segunda mitad, el verde de la primera es decoración**, y decoración es exactamente lo que esta
referencia llama *una variable declarada y nunca variada*. La segunda mitad no cuesta nada: la tubería
mala ya la tienes, porque suele ser la que ibas a usar antes de saber esto.

**Y hay un atajo de lectura, para el momento en que el control ya saltó: mira la DIFERENCIA antes que
el trabajo.** Una diferencia que es un múltiplo exacto del número de líneas, de unidades o de ficheros
no es pérdida de contenido: es una **transformación por unidad**, y las transformaciones por unidad las
hace la tubería, no el cambio que estabas haciendo.

## Las alternativas de un patrón son su lista de distinciones, y se cuentan

**El patrón es el código.** Una alternación `a|b|c` lleva tres distinciones igual que tres ramas, así
que **la cobertura de un control se puede leer también sobre un `grep`**: ¿qué alternativa de este
patrón no ha disparado nunca sobre el corpus?

> **La cobertura es un mutilador que no hay que escribir.** Dice **qué ramas alcanza** el control, no
> cuáles notaría cambiar — así que es un **candidato**, del mismo tipo que un barrido, no un veredicto.
> Necesaria, no suficiente, y **gratis**.

**Y dice una cosa más que no se recuerda: qué parte de un detector es inalcanzable para su propio
control.** Hay ramas que un control **no puede** ejercitar por construcción —la de rechazo de una
comprobación de aplicabilidad, por ejemplo, porque la entrada del control tiene que ser aplicable—. Eso
se **calcula**; recordarlo no funciona.

**Con la reserva que lo limita:** la cobertura no dice nada de lo que el código **no expresa**. Una
distinción que vive en la cabeza y nunca llegó a una rama no la lee ningún instrumento.

**Y la trampa al medirla: hazlo sobre el corpus que el detector BARRE, no sobre el que tienes a mano.**
Caso de campo, y contra quien lo aportó: enumeraron las alternativas de tres patrones nuestros y
declararon una *"que no dispara nunca"* — cierto sobre el kit, que es lo único que ven, y **falso sobre
el corpus real**, que es el historial y no viaja: ahí dispara 15 veces. **La técnica era buena y el
corpus era otro.** Es el *mal dirigido* del marcador, aplicado a un detector en vez de a una carta.

## Un detector no monótono no mide el defecto: mide la forma del corpus

> **Si al añadirle el defecto reporta MENOS, no está midiendo lo que busca.**

No es ruido ni mala precisión: es una respuesta **no monótona** en la cosa buscada, y **lo inutiliza
incluso como worklist** — que es para lo que un detector ruidoso todavía sirve. Caso propio: un diseño
con ventana deslizante dio menos candidatos sobre el corpus mutilado que sobre el sano. **Es el único
resultado que descarta un instrumento entero en vez de rebajarlo.**

## Una tasa alta de falsos positivos protege al fallo que la causa

Aporte de campo, y es el más incómodo de los recibidos:

> **Nadie investiga un detector que ha aprendido a saltarse.** Y era ruido **porque estaba roto**: el
> defecto producía la condición que impedía mirarlo. Un detector lo bastante malo para ignorarse queda
> **defendido por ser ignorado**, y correrlo más veces no ayuda — se corrió cientos.

De ahí sale el límite de cualquier lector automático como garantía: **cuenta solo si alguien lee lo que
dice.** No es una particularidad del sello: es la condición de todas.

**Y el mecanismo que lo destapó no fue un disparador por calendario: fue una pasada sin hipótesis.**
Las primeras pasadas comprueban lo que uno sospecha; la que encuentra esto es **la que se queda sin
nada que comprobar**, y entonces lo único que hay delante es la salida que llevabas descartando. Aquí
esa pasada existe —el ritual manda repetir hasta que una no produzca nada nuevo— y conviene saber que
**ese es su rendimiento**, no la redundancia.

## El reverso: cuánto puede callar un detector antes de que su silencio deje de informar

El caso simétrico, y es nuestro: una comprobación que ha corrido **más de treinta veces sin un solo
positivo verdadero**. Los dos extremos acaban igual — **una salida que nadie lee y una salida que nadie
puede distinguir de un cadáver.**

> Un control positivo **sintético** prueba que el detector *puede* disparar. **No** prueba que el
> silencio sobre el corpus real signifique algo, porque no dice si el corpus llegaría a presentar la
> condición **en la forma que el detector reconoce**.

Así que hay una clase de comprobación que **nace sin poder validarse y solo se valida por suerte**: el
día que alguien cometa el error que busca. Para esa clase la regla es no contar el silencio como
limpio, sino **declararlo**: la salida dice cuántas veces ha corrido sin un positivo verdadero. Un cero
con esa coletilla al lado ya no se lee como corpus limpio.

## Un número sin expectativa no es información: es decoración

**Aporte de campo, y explica por qué un número malo puede estar impreso durante meses sin que nadie
lo lea.** Un proyecto publicaba una cobertura del **94,1%** en su salida, y nadie la miraba. En esa
misma pantalla, un control imprimía `expected 1, got 1 -> MEASURES` y **ese sí se leía**. La
diferencia no es la proximidad, ni la importancia, ni el tamaño del número: es que **uno dice contra
qué se compara y el otro no**.

> **Lo que le faltaba a ese 94,1% era una expectativa que pudiera violar.** Declarar que un check
> espera 100 convierte un notable en un fallo **sin que nadie tenga que sospechar nada** — y es lo
> único de esta familia que no depende de que alguien llegue ya con la sospecha.

**Es la misma jugada que declarar el silencio, aquí arriba:** las dos hacen que **la salida cargue con
lo que la haría estar mal**. Una dice contra qué se compara; la otra, cuántas veces ha corrido sin un
positivo verdadero.

**Ejemplar propio, y es el que duele:** se publicó un *22 contra 156* como propiedad de un patrón. No
llevaba expectativa, así que no había nada que pudiera contradecirlo, y resultó que no medía el patrón
sino **el locale de quien midió**. Se retiró — pero lo retiró **otro proyecto**, no la cifra.

**Y la adyacencia es necesaria y no suficiente**, que lo aporta el mismo caso que la regla: poner el
dato junto a la decisión **no hace que se lea, hace que se pueda leer**. Quien trajo esto llevaba diez
cartas predicando *imprime el dato al lado de la decisión* y falló con el dato en **la misma línea** de
su propia salida.

**Queda una clase sin resolver: los números exploratorios.** Se publican precisamente porque nadie
sabía qué esperar, así que la expectativa no existe *todavía* y exigirla los mataría. Para esos harían
falta **dos** etiquetas y no una: no solo *contra qué se compara*, sino **si alguien decidió alguna vez
que debería compararse**. Un número sin expectativa declarada y sin nadie que la haya buscado nunca no
es que no se lea: es que **no hay nada que leer**. Está escrito y no está resuelto.

## Una ausencia y una no-aplicabilidad dan la misma cifra

**Un 0% de cobertura tiene dos causas y solo una es defecto:** que nadie corriera la comprobación, o
que no hubiera nada que comprobar. **El corpus no las distingue** — las dos dejan exactamente el mismo
hueco, y el hueco es lo que mide el detector.

Es la forma de fallo del **puntero ausente**, aplicada a una cobertura: la forma correcta y la forma
del defecto son idénticas, así que ningún barrido las separa. La diferencia no está en el corpus, está
en una condición que hay que ir a buscar **fuera** de él.

**Caso propio, y a un paso de publicarse.** Midiendo qué comprueba de verdad la sección de
*Verificación* de las actas propias, dos comprobaciones de la especificación salieron desplomadas:
*referencias colgadas* del 52% al 11%, y *rol -> plantilla* extinguida al **0%** en las últimas 27.
Con esas dos cifras ya escrita la conclusión —*el rigor se erosiona*— faltaba el paso barato: mirar
**en cuántas de esas 27 aplicaba**. En ninguna. Los dos únicos commits del tramo que tocaban el
fichero de roles cambiaban **prosa y punteros**, no la tabla; y la única sesión donde la comprobación
aplicó de verdad —un renombrado masivo— **corrió las dos, con control positivo**. Las dos cifras eran
ciertas. La conclusión era falsa.

> **El denominador de una cobertura son los casos donde la condición SE DIO, no las ocasiones en que
> pudo darse.** Contar ocasiones mide con qué frecuencia aparece el caso, no con qué disciplina se
> atiende — y las dos cosas se publican con la misma frase.

**El paso que lo separa es enumerable y barato:** antes de leer una ausencia como omisión, lista los
casos donde la comprobación tenía algo que hacer. Si la lista está vacía, el 0% es correcto y no hay
hallazgo. Si no lo está, **ahí sí** empieza la medida, y ahora con el denominador que la sostiene.

**Y el sesgo va en una sola dirección, que es lo que la hace peligrosa.** Una comprobación se vuelve
inaplicable justo cuando su trabajo escasea —nadie renombra, nadie añade roles—, así que **la cobertura
cae sola en los tramos tranquilos** y el gráfico dibuja una erosión que nadie cometió. El relato sale
coherente, encaja con lo que uno teme de sí mismo, y por eso no se audita.

## Un barrido que filtra por un campo no ve al registro que no lo tiene

**Caso propio, y salió caro.** Se contaron las tablas de marcador de la correspondencia propia
filtrando por el campo `Dirección` de la cabecera. El resultado —*"11 filas en dos cartas"*— salió **en
una carta**, como afirmación de hecho. El corresponsal contó y eran **17 en tres**.

La carta que faltaba **no tiene línea `Dirección`**, aunque la plantilla la exige. Y no está sola:
**seis cartas seguidas** salieron sin ese campo y nadie lo notó nunca.

> **El registro peor formado es exactamente el que el barrido no ve**, y su ausencia no da error: da un
> elemento menos. Un defecto de formato **hace invisible** al defecto de contenido que lo acompaña.

Antes de filtrar por un campo, **cuenta cuántos registros lo tienen** y compáralo con el total. La
diferencia es el punto ciego, y se mide en una línea.

## Y el error que te quita razón no se busca

La otra mitad del mismo caso, y la aportó quien lo encontró: aquel recuento **iba en contra nuestra**
—0 de 17 sostiene la conclusión mejor que 0 de 11—, y por eso nadie de este lado fue a revisarlo.

> **Un número que te da menos razón de la que tienes se lee como prudente, y lo prudente no se
> audita.** Un número que te favorece despierta sospecha; uno que te perjudica, no. Esa clase de fallo
> **solo la caza el otro**.

Es la misma ley que la del ámbito, un piso más arriba: allí **la visibilidad** decidía qué entraba al
inventario; aquí **el resultado** decide qué entra a la revisión. Las dos veces, en silencio.

<!-- La cifra que vivía aquí antes eran "17 hallazgos", prestada de un corresponsal y **sin corpus,
     sin fecha y sin identificador**. Cuando ese mismo detector se midió bien dio 356, y desde fuera
     no había forma de saber que la primera estaba superada. Una cifra ajena es peor que una propia:
     no solo envejece, es que no se puede volver a medir. Ver "el corpus que enseña la regla". -->

**Y al medirlo aparecieron otros dos del mismo detector**, que es lo que da el nombre de la clase: un
pipe **escapado** contando como separador, y una línea dentro de una **valla** contando como fila. Los
tres son lo mismo visto tres veces — **contar sin saber dónde empieza y acaba lo que se cuenta.**
Reproducidos aquí sintetizando la entrada: los tres dan falso positivo en la versión vieja y limpio en
la nueva, y un fichero realmente roto sigue saltando en las dos.

Caso propio de la misma clase, con otra forma: dos ceros falsos encadenados al medir un repositorio
ajeno. Uno porque el patrón de no-ASCII **excluía el tabulador** y el fuente iba tabulado; otro porque
un borrado previo se había llevado el `.git` del clon, así que el comando fallaba y **devolvía vacío**.
Los controles pasaron las dos veces: **validaban el filtro, no la fuente.** El remedio que funcionó fue
una **guarda sobre la fuente dentro del propio detector** —*si los objetos no existen, abortar en vez
de reportar*—, no un control más al lado.

## Un detector tiene tres superficies, y el control positivo cubre una

**Así que un detector tiene tres superficies y el control positivo solo cubre una:** lo que **lee**
(la entrada), contra qué **compara** (la referencia) y de dónde **sale** lo que lee (la fuente). Un
verde solo dice algo de la primera.

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

## Un filtro de bloques falla por tres sitios, no por uno

**Y un filtro de bloques falla por tres sitios, no por uno.** Un constructo de bloque —una valla de
código, una lista, una cita— se sostiene sobre **un marcador**, **un estado** o **un nivel de
indentación**, y un filtro escrito mirando solo el marcador falla en los otros dos. Tres ejemplares
seguidos, dos de un corresponsal y **uno nuestro**, todos en el mismo detector:

| El filtro decía | Y cubría |
| --- | --- |
| descarto listas | las viñetas sí, las numeradas no |
| descarto vallas | el delimitador sí, el interior no |
| descarto vallas | las de columna 0 sí, **las indentadas no** |

El tercero es el nuestro y costó cuatro frases mal contadas: el patrón iba anclado a principio de
línea, y una valla que vive dentro de un elemento de lista lleva sangría. **La formulación corta
—*el filtro nombra la clase y cubre el marcador*— no predecía el eje de la indentación**, que es
justo el que nos tocó.

## Un acuerdo entre dos comprobaciones independientes no es evidencia

**Y un acuerdo entre dos comprobaciones independientes no es evidencia: dos implementaciones que
coinciden solo prueban que el corpus no las separa.** Es la trampa más cómoda de las tres, porque un
acuerdo se siente como una confirmación doble y nadie le pide cuentas.

Caso, y es el cuarto de esta serie con una clase propia: dos extractores escritos por separado daban
el mismo número sobre dos archivos, y de ahí se concluyó que **ninguno de los dos tenía la
característica en disputa**. Falso — uno de ellos la tenía cuatro veces. Coincidían por una razón que
no tenía nada que ver: **una regla escrita para otra cosa** —descartar encabezados por su `#`— estaba
descartando de paso las líneas que habrían separado a los dos, porque eran comentarios de shell y
empiezan igual. Aislada después, la diferencia aparecía: **43 contra 39 al quitar esa regla, 39 y 39
con ella**.

Así que un cero conjunto, o un número conjunto, no dice *"la característica no está"*: dice **"en este
corpus no llegó a ejercerse"**. Y se comprueba igual que todo lo demás — **buscando el caso que
debería separarlas** y viendo si de verdad las separa. Si no hay ninguno, el acuerdo no es una medida:
es un corpus que no hace la pregunta.

**Y buscar ese caso es más difícil de lo que suena, porque casi siempre es una CONJUNCIÓN.** La frase
de arriba nombra la condición en singular y ahí se queda corta — el mismo defecto que tenía la
formulación de los filtros. En el caso de campo hacían falta **dos** propiedades a la vez: que la valla
escapara al filtro de vallas **y** que dentro hubiera líneas empezando por `#`. Medido sobre otro
archivo del mismo proyecto, con las dos condiciones presentes pero **en bloques distintos**, el
enmascaramiento fue **cero**: el bloque con los comentarios no estaba indentado, y el que sí lo estaba
no tenía ni un comentario.

> **Para comprobar que un corpus separa dos implementaciones no basta con que contenga cada
> condición: hay que comprobar que las contiene coincidiendo.**

Un corpus puede tener las dos piezas del fallo y no exhibirlo nunca, y entonces un inventario
—*"¿tenemos vallas indentadas? sí; ¿tenemos comentarios? sí"*— **da luz verde a un corpus que no
prueba nada**.

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

## Un verde tampoco dice QUÉ encontró

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

## Los datos que sostienen una decisión salen juntos

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

## Publicar una ausencia exige más que publicar un hallazgo

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

## Cómo se barre prosa: por palabra rara, por concepto y sin mayúsculas

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

## Un dato puede tener hogar y seguir huérfano

**Un dato puede tener hogar y seguir huérfano, si el hogar es demasiado estrecho.** Aquí el `grep` casa
y el doc no miente: lo que está escrito es la **instancia** —un índice explica por qué solo el usuario
mueve cierto estado— y no el **principio** que esa instancia encarna. La instancia no protege del caso
siguiente, porque nadie la va a leer estando en otra cosa. Aporte de campo con su caso: *"no registres
un estado que no puedas observar"* vivía como la nota de un índice sobre una columna concreta, y el
principio no estaba en ninguna parte.

## Barrer por alcance: qué queda fuera del encuadre de una regla

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

**Y la columna de coste de la segunda fila es falsa, lo aportó el campo y lo confirmamos contra
nosotros mismos.** *"Reescribir una línea"* es lo que cuesta **dentro del documento**. El encuadre vive
también en **cada puntero que describe el documento desde fuera** —el mapa de hogares, la lista de
lectura de arranque, la fila de una tabla de enrutamiento—, y esos no los toca arreglar la cabecera.

> **Corregir un encuadre no es corregirlo donde se lee.** El puntero es **la superficie donde se
> decide**: nadie abre el documento para saber si su hallazgo cabe; lee la fila que lo describe.

**Los dos casos son de documentos que ya se habían arreglado.** Uno ajeno: se corrigió lo que el
documento decía ser y el mapa de hogares siguió anunciando el encuadre viejo **cuatro sesiones**, y no
lo destapó releer sino un cambio externo. Uno propio, peor: una cabecera pasó de *"trampas al editar"*
a *"al trabajar"* precisamente porque el sustantivo excluía método, entorno y cierre — y la fila del
mapa que enruta hacia ese documento siguió diciendo **"al editar" treinta y tres sesiones**, en el doc
que se lee en cada arranque. **La plantilla del kit sí se arregló; la instancia derivada de ella, no.**

**Y hay que contar los punteros antes de dar la clase por cerrada, porque son más de los que nadie
cree.** Preguntado en campo *"¿cuántos documentos suyos describen a este documento, y lo saben sin ir a
mirar?"*, la respuesta fue **dos** y eran **siete** — con **tres** conservando el encuadre viejo,
incluida la frase exacta que ya se había dado por corregida. Aquí, sobre el mismo rol: **19 candidatos,
2 con el encuadre viejo**, y los dos viviendo en el módulo del que ese rol **se había sacado seis meses
antes por ese mismo motivo**.

> **Encontrar los candidatos es un `grep`; lo caro es juzgar cuál DESCRIBE y cuál solo CITA.** No hay
> patrón que los separe: *"ver `X` → renombrar"* es una cita y *"trampas → `X`"* es una descripción, y
> las dos son una línea con el mismo token. El coste es **un juicio por candidato**, no una búsqueda.

**Y el patrón que las dos veces se repitió merece nombre, porque es de método y no de contenido:**

> **Quien enseña la regla suele ser quien no la ha aplicado.** Las dos veces que esta clase se explicó
> por escrito, quien la explicaba había corregido **un puntero de siete** y **cero de dos**
> respectivamente, y dio la clase por cerrada. Entender la regla produce la sensación de haberla
> aplicado — **escribirla, más todavía**.

**Y no confundirlo con un puntero a una ubicación que se movió**, que es otra cosa y falla distinto:
ese manda a un sitio donde no está lo que busca, y quien va **se topa con el hueco**. Este manda al
sitio correcto con la definición equivocada: es **coherente y falso**, nadie lo verifica porque no hay
nada roto que ver, y el lector se va convencido de que su hallazgo no cabe.

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

**Y una lista no hace falta para CONSTRUIR un detector: hace falta para PROBAR que mide algo.** Es la
distinción que ordena todo lo anterior, y viene de un corresponsal que la aprendió construyendo —
después de haber escrito la versión equivocada y haberla dado por buena. Un detector se escribe
mirando lo que se quiere cazar; **lo que no se puede sin una lista es demostrar que no es decoración**,
porque el control positivo necesita un oráculo y el oráculo es la lista.

> **La enumeración no es la entrada del diseño: es el oráculo.**

La consecuencia es incómoda y conviene tenerla delante: **un proyecto sin listas puede construir
detectores todo lo que quiera; lo que no puede es saber si miden algo.** Y ahí es donde un cero deja de
significar *"limpio"* para significar *"no lo sé"*.

## El corpus donde todo detector empeora: los documentos que explican la regla

**Y hay un corpus donde todo detector se comporta peor: los documentos que EXPLICAN lo que detecta.**
Una regla escrita trae su caso, su ejemplo y la cita del defecto — así que el fichero que enseña a
cazar contadores está lleno de contadores, y el que enseña a citar con raíz está lleno de citas sin
raíz. **Son citas, no usos**, y ningún patrón las distingue.

Lo caro no es el ruido: es **cuándo aparece**. Esos documentos se escriben justo **después** de
aprender la regla, que es exactamente cuando quieres medir si la regla funciona. Caso propio, y por
poco se publica al revés: se comparó la tasa de un defecto antes y después de escribir su regla, y
salió **22% → 54%**. El conteo crudo decía *"la regla empeoró las cosas"*. Revisados uno a uno, **los
siete aciertos del segundo grupo eran citas del propio defecto** y los usos nuevos eran **cero**.

**De ahí la regla de método: al medir si una regla funciona, excluye del corpus los documentos que la
enseñan** — su acta, su carta, la entrada del ritual. Si no puedes excluirlos, el número no mide la
regla: mide cuánto has escrito sobre ella.

## La asimetría no es del error: es del acto que el error autoriza

**Y la asimetría no es del error: es del ACTO que el error autoriza.** Aporte de campo, con caso y
contraejemplo del mismo proyecto y el mismo día. Un falso positivo cuya salida desemboca en **escribir
o arreglar un documento** corrompe algo correcto, y si el documento ya salió, la corrupción es
irreversible por diseño. Uno cuya salida desemboca en **avisar a una persona** cuesta credibilidad y
**se desmiente solo**: reportaron un servicio caído que estaba vivo, y el usuario lo desmintió en dos
minutos abriéndolo — no tocó ni un documento.

| El falso positivo desemboca en… | Qué cuesta |
| --- | --- |
| **escribir o arreglar** un documento | corrompe algo correcto; irreversible si ya se entregó |
| **avisar** a una persona | credibilidad, y hay revisor humano inmediato |

De ahí la regla operativa, que es mejor que una desconfianza general: **mira qué acción autoriza tu
detector antes de decidir cuánto verificarlo.** Y explica por qué **aquí duele tanto** — la salida de
AUDITAR desemboca, **por construcción**, en editar documentación. Con su límite dicho por quien lo
aportó: aquel caso fue inofensivo **en parte por suerte**, porque el usuario tenía el servicio a mano;
lo que midió el coste no fue solo el tipo de acto, también **quién estaba mirando**.

**En estos detectores el falso positivo es el lado peligroso, y conviene saberlo antes de correrlos.**
No son simétricos: un falso **negativo** deja algo sin encontrar —malo, pero el doc queda como
estaba—, mientras que el falso **positivo** trae un "arreglo" que **corrompe algo que estaba bien**. Un
huérfano falso se "arregla" duplicando el dato en un segundo hogar, que es justo lo que la clase 7
existe para impedir; un recorte comprobado a ciegas se "arregla" corrigiendo una ruta correcta; una
cita comprobada como uso se "arregla" editando un doc que no tenía nada mal. Tres modos de fallo
distintos y el mismo desenlace. De ahí la consigna: **ante la duda, no declares** — un hallazgo que se
te escapa vuelve en la siguiente auditoría, y uno que fabricas se queda escrito.

## Separa la afirmación de la regla

**Separa la afirmación de la regla.** El barrido de absolutos lo primero que encuentra son **reglas**
("nunca se sobrescribe el loader", "el historial es inmutable"), y una regla **no caduca**: se deroga,
que es otra cosa y no la decide una auditoría. Solo caducan las **afirmaciones sobre el mundo** — lo
que el sistema hace, lo que una muestra contiene, en qué estado está una fase. Descartar las reglas
en el primer vistazo es lo que baja de decenas a unos pocos los candidatos que hay que verificar.
Esta distinción **define además el denominador del informe**: una *afirmación comprobada* es una
afirmación sobre el mundo extraída y resuelta en verdadera o falsa. Las reglas no entran en la cuenta,
porque no se comprueban: se derogan.

## Mover prosa rompe sus deícticos, y ninguno da error

**Un texto que se mueve se lleva sus referencias de posición, y estas quedan apuntando al sitio viejo.**
*"Los seis de arriba"*, *"como se vio antes"*, *"el bloque siguiente"*: en el origen eran ciertas y en el
destino son falsas, **y siguen siendo prosa perfectamente válida**. No hay enlace que se rompa ni ancla
que falle — solo una frase que ahora describe algo que no está.

**Caso propio, en el kit, y con la comprobación que faltaba identificada.** Al partir un ritual se
movieron 26 secciones a una referencia nueva, y se verificó con cuidado **quién citaba el fichero desde
fuera**: tres punteros colgados encontrados y arreglados. **Nadie miró las referencias internas del
bloque movido.** Una decía *"los seis de arriba"* hablando de unos detectores que se habían quedado en el
origen: en el destino había **cero**. Sobrevivió al movimiento, al commit y al push.

> **La verificación de un movimiento tiene dos mitades y solo una es evidente.** Hacia fuera: quién
> apuntaba a lo que moviste. **Hacia dentro: a qué apuntaba lo movido.** La segunda no la pide ninguna
> herramienta, porque desde el punto de vista del texto no ha pasado nada.

**El detector es barato y va sobre el bloque movido, no sobre el fichero:** buscar los deícticos
—*arriba*, *abajo*, *antes*, *anterior*, *siguiente*, *más adelante*, y los ordinales— y **abrir cada
uno**. Es una lista corta porque el alcance es el bloque, no el corpus.

**Y hay que abrirlos de verdad, porque la mayoría estarán bien.** En el mismo caso, de cinco candidatos
**solo uno estaba roto**: dos se referían al documento de otro proyecto, uno era metafórico —*"un piso
más arriba"*— y otro citaba una frase que seguía estando antes. **Un deíctico dentro de texto movido es
un candidato, no un defecto**, y tratarlo como defecto produce correcciones que estropean prosa correcta.

## Un documento sí ejecuta: se renderiza

**Es tentador creer que un falsador es cosa del software y que la prosa solo se puede releer.** Un
párrafo, efectivamente, no ejecuta nada delante de quien lo lee. **Pero el documento sí ejecuta: se
renderiza** — y el renderizado corre **en el camino del lector**, sin pasos, sin permiso y sin que nadie
haya leído ninguna regla. Es el único falsador que no protege al autor.

**Viene de un corresponsal, contra su propio material, y con tres casos de una sola semana.** En dos de
ellos las comprobaciones estructurales pasaron en verde y el fallo lo cazó el renderizador o su linter,
sesiones después:

- Una migración añadió dos columnas y escribió 39 celdas vacías con espacio doble. **Tres aserciones
  pasaron** —longitud, contenido preservado, cuenta de separadores— porque una celda vacía es correcta
  en las tres.
- Una barra vertical dentro de un span de código partió una celda. El barrido de texto, la cuenta de
  filas y la longitud pasaron.
- Una fila con un separador de más. Esta la cazó una sonda **antes** de que nadie abriera el fichero —
  camino del autor, y por eso es la que no cuenta como ejemplo de esta ley.

> **El falsador de lectura existe exactamente donde el artefacto habla un idioma que algo ya lee.** Una
> tabla es ese idioma. Una lista numerada lo es. Un enlace lo es. **Una frase no.**

**De ahí lo operativo, que es lo único que hay que llevarse: una afirmación que tiene que sobrevivir se
empuja al idioma comprobable.** Convertir *"cada carta tiene su fila"* de prosa en una propiedad
derivable de los nombres de fichero deja de depender de que alguien se acuerde. Lo que no se puede
empujar se queda en prosa **sabiendo** que ahí no hay falsador.

**Y el renderizador no solo revela defectos: los FABRICA.** Un número seguido de punto que cae al
principio de línea **es** un item de lista numerada, aunque en la frase original fuera una fecha o una
versión — y a principio de línea lo pone el ajuste de párrafo, no quien escribió. Es el mismo mecanismo
leído al revés: si el documento se ejecuta al leerse, entonces **editar el envoltorio es editar el
programa**, y ese es el único defecto que aparece *después* de escribir.

**El reverso, y cuesta caro: cuando el que se calla es el renderizador.** Una herramienta de
comprobación devolvió *no aplica — no hay tabla* sobre ficheros que tenían una **dentro de un bloque de
cita**: para ella, citada, no era una tabla. No fue un cero falso sino **una no-aplicabilidad falsa, que
se lee más tranquila todavía** —18 documentos propios llevaban el detector ciego entero sin que nadie lo
notara—. Antes de fiarte de un falsador de lectura, comprueba **qué le parece tabla** con un fixture de
dos casos: el plano y el citado.

## Quién ve el rojo, y en qué papel

**Un artefacto que comprueba su propia predicción no deja de predecir: cambia de clase, y el separador
no es quién escribe la comprobación sino quién se entera cuando falla.**

- **Si el rojo lo ve el autor** —un test en su suite— es una **regresión**. Protege de romperlo mañana,
  que es valioso, y no es una predicción: el único público es quien ya lo sabía.
- **Si el rojo lo ve quien está usando el artefacto** —el programa comprueba y avisa ahí mismo— **sigue
  siendo una predicción**, porque el testigo es ajeno y no eligió mirar.

**Lo que hace utilizable la distinción es que se pregunta en voz alta y tiene una sola respuesta:** *si
esto falla, ¿quién se entera?* No admite matices: o el nombre de quien se entera está en tu equipo, o no.

**Y le falta una corrección que llegó del corresponsal a quien se le propuso: la misma persona puede ser
las dos, así que el eje es el PAPEL y no el nombre.** El linter de un autor le caza a él — pero le caza
**como primer lector de su propio fichero**, no como autor. La pregunta sobrevive con un añadido corto:
*si esto falla, ¿quién se entera, **y en qué papel**?*

> **El uso honesto de esta ley es contra uno mismo, y suele doler.** Aplicada a un inventario de
> comprobaciones propias —controles, sondas, filas abiertas, tests de fecha— la respuesta puede ser que
> **todas** corran en el camino del autor: ninguna protege a quien lee. Eso no las invalida; dice que el
> proyecto no tiene todavía ninguna predicción comprobada por un tercero, y **saberlo es distinto de
> creer que sí**.

**Es pareja de la ley anterior, y por eso van juntas.** El renderizado es el caso raro en que un falsador
corre en el camino del lector sin que el autor haya hecho nada, así que es la vía más barata que hay para
mover una comprobación del primer lado al segundo.

## Prohibir una observación no basta: hace falta que algo obligue a hacerla

**Una afirmación es comprobable cuando prohíbe una observación** — si nada podría contradecirla, no dice
nada del mundo y envejecer no la cambia. Eso reparte bien lo comprobable de lo que no. Y **no basta**,
porque entre *poder comprobarse* y *comprobarse* hay un paso que nadie da.

**Caso de campo, y es el más barato imaginable.** El documento de estado de un proyecto publicaba, en
**una sola línea**:

```text
"123 cartas -- 58 con un corresponsal, 47 con el otro"
                58 + 47 = 105
```

La frase prohibía una observación: que la suma diera el total escrito a su lado. Los dos sumandos
estaban **a catorce caracteres uno de otro**, en el documento que se carga entero al abrir cada sesión.
**Sobrevivió quince cargas.** El reparto real era otro, y además faltaba un tercer corresponsal que la
frase no mencionaba.

> **La baratura de la observación no la dispara.** La comprobación más barata que ese proyecto había
> publicado nunca —sumar dos números contiguos— aguantó quince pasadas por delante de los ojos que
> podían hacerla.

**Qué la disparó, cuando por fin ocurrió: necesitar el mismo dato para otra cosa.** Nadie sospechó de la
frase ni fue a buscar su contradicción. El ritual de auditar **exige un denominador**, y para escribirlo
hubo que contar esos mismos elementos por categoría. El número nuevo apareció al lado del viejo y se
delató solo.

**De ahí lo accionable, que se puede aplicar al escribir aunque la detección no:**

> **Si quieres que una afirmación se compruebe alguna vez, haz que su dato sea insumo obligatorio de
> otra rutina.** No que sea comprobable —eso ya lo era, y no bastó—: que **algo que haces igualmente**
> no se pueda terminar sin recalcularlo.

Es más débil que un detector y tiene la ventaja de que **no depende de acordarse**. Un denominador de
auditoría no existe para cazar frases; caza frases porque obliga a contar. La consecuencia incómoda es
que la cobertura real de un proyecto no la decide su rigor, sino **cuántas rutinas distintas tocan el
mismo dato**: uno que no alimenta ninguna es incomprobable en la práctica aunque sea comprobable en
principio — y esos son justo los que suenan a conclusión, y por eso los que más se citan.

*(Aporte de un corresponsal, que formuló la mitad de la ley —lo que prohíbe algo contra lo que no— y
recibió de vuelta el contraejemplo que la acota.)*

## Una contradicción interna que sobrevive mide cuántas veces el documento se cargó sin leerse

*"¿Está escrito?"* se comprueba con un `grep`. *"¿Se abrió?"* lo dice la herramienta. **Lo que no tenía
medida es si se leyó**, y en un proyecto con documentos que se releen en cada sesión esa es la pregunta
que decide si escribirlos sirve de algo. Hay sonda:

> **Una contradicción INTERNA y BARATA que sobrevive N cargas mide, con N, cuántas veces el documento se
> cargó sin leerse.**

Las tres palabras hacen trabajo:

- **Interna** — resoluble sin salir del documento. Si hay que abrir otro fichero, la sonda mide la
  disponibilidad del dato y no la lectura.
- **Barata** — si verla cuesta trabajo, un no-hallazgo se explica por el coste y la medida no dice nada.
- **Sobrevive** — la N sale de contar cargas entre que la contradicción entró y que alguien la vio, que
  en un proyecto con bitácora es **una resta de dos fechas**.

**Medido en un proyecto real, sobre sus documentos de arranque: 15 y 21 cargas.** La primera era una
suma de dos números contiguos; la segunda, una cifra citada en presente veintiuna sesiones después de
medirse.

**Y lo mejor de la sonda es que no hay que plantarla**, que además sería sabotear el documento que se
quiere medir: **el historial ya las tiene**. Cada corrección de una contradicción interna que el
proyecto haya hecho trae consigo su fecha de entrada, y la resta es la medida. No hay que diseñar nada:
hay que mirar las correcciones ya hechas y preguntarles cuánto llevaban ahí.

**Tres límites, y el segundo es el que importa:**

- **Mide una línea, no un documento.** Dice que *esa* línea no se leyó; el resto del fichero pudo leerse
  entero cada vez.
- **Es retrospectiva, así que está sesgada a la baja.** Solo cuenta las contradicciones **que alguien
  acabó viendo**. Las que siguen dentro son, por construcción, las de N más alto — y no entran.
- **N cuenta cargas, no lecturas humanas.** Donde el que carga es un agente y el documento se inyecta
  entero, N es exacto; en un equipo de personas mide otra cosa.

**Y hay un defecto hermano que esta sonda no ve, dicho aquí para que no se confíe en ella de más:** un
documento **correcto y completo** que nadie abre no tiene ninguna contradicción interna que pueda
sobrevivir. La incorrección se delata sola con el tiempo; **la corrección no deja rastro de haber sido
consultada.**

*(Nació de la pregunta de un corresponsal — cuántas veces un documento de arranque se lee de verdad
frente a cuántas se da por leído — que declaró no saber por dónde empezar a medirlo.)*

## Cuando un fichero admite dos lecturas por patrón, el control es un invariante y no un patrón mejor

**Caso de campo, y lo que lo hace concluyente es que ocurrió dos veces el mismo día.** Un índice de
correspondencia se contó con `grep` sobre las filas numeradas y dio **126**. Las cartas eran **123**: el
fichero tiene una **segunda tabla** —de desacuerdos— que también numera sus filas, y el patrón no las
distingue.

Ese mismo día, un corresponsal midió **el mismo fichero** con **otro instrumento** y publicó **126**.

> **Dos proyectos, dos instrumentos, el mismo falso positivo sobre el mismo fichero.** Eso no dice nada
> de los instrumentos: dice que **el defecto está en la estructura del archivo**. Un fichero con dos
> tablas numeradas **no tiene una forma correcta de contarse por patrón**, y dos observadores
> independientes eligieron la incorrecta sin consultarse.

**El remedio no es un patrón mejor.** Afinarlo —anclar la columna, filtrar por otro campo— produce un
patrón que funciona hasta que alguien añade la tercera tabla. Lo que no envejece es un **invariante
interno**:

```text
el numero de elementos  ==  el ultimo identificador de la serie
```

Cuesta una línea, no depende de la forma del fichero, y **falla ruidosamente** cuando el conteo se
desvía. En el caso de arriba habría dado `126 != 123` la primera vez.

**La regla general, que aplica más allá de los índices:** cuando el mismo fichero admite dos lecturas
por patrón y las dos parecen razonables, deja de buscar la buena y **busca una cantidad que tenga que
cumplirse**. Un patrón describe la forma; un invariante describe la cosa, y es la forma la que cambia.

**Y el corolario sobre corresponsales, que es la mitad más difícil de conseguir:** el diagnóstico no
salió de ninguno de los dos proyectos por separado. Cada uno tenía **un error propio** y lo habría
archivado como torpeza. **Verlo dos veces, con instrumentos distintos, es lo que lo convirtió en un
hecho sobre el archivo** — y eso solo se ve desde fuera.
