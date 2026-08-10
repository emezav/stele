<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

# Ritual: AUDITAR (verificar que lo escrito sigue siendo cierto)

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

## Alcance (qué se relee, y qué no)

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

## Las clases de drift

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

Todas son agnósticas de dominio, y por eso el ritual es del núcleo. Un módulo activo aporta
**detectores atados a sus roles** (el módulo de producto: el par `specs`↔`specs_dir` y los hogares
`specs`/`architecture` — ver su `module.md`).

**Y una forma de clase 1 que se fabrica sola, al escribir sobre la propia tabla: el contador.**
Escribir *"las ocho clases"* copia en prosa un dato que ya está en la tabla —su longitud— y crea un
segundo hogar. Añadir una clase deja la frase falsa **en el mismo cambio que la crea**, y no lo
detecta nada. Vale para cualquier lista: *los tres hogares*, *las cinco rutas*, *ambos contenedores*.

**El remedio no es acordarse de contar: es no escribir el número.** *"Las clases de drift"* dice lo
mismo y no caduca; quien quiera el número lo cuenta en la tabla, que es su único hogar. Repasar los
sustantivos con número sigue sirviendo para lo ya escrito — para lo que escribes hoy, la solución es
de **forma**, no de atención.

**Y el límite, que importa tanto como la regla: esto NO alcanza a los números que son parte del
concepto.** *"Las tres rutas"* no es un contador: son tres por diseño, la cardinalidad es un invariante
y cambiarla sería otro marco. *"Las ocho clases"* sí lo era: esa lista ya creció antes y volverá a
crecer. **La pregunta que separa las dos no es cuántas hay, es si añadir una es un cambio previsto.**
Aplicar esta regla a lo primero es un barrido destructivo que borra información real; el detector de
abajo da positivo en los dos casos y por eso sus aciertos **se revisan uno a uno**, como todos.

**Y el límite tiene su propio límite: *"parte del concepto"* es a su vez una afirmación que caduca.**
Un proyecto escribió *"las dos anclas de la raíz"* dándolo por cerrado por definición —eran el
manifiesto y el loader— y una sesión posterior convirtió el loader en una **lista de puertas**, tantas
como harness haya que atender. El juicio se hizo de buena fe y se volvió falso sin que nadie tocara esa
línea. Peor: la frase que **enunciaba esta misma regla** usaba *"las dos anclas"* como su ejemplo de
lista cerrada, así que el ejemplo falsificó el criterio. La pregunta operativa no cambia —*¿añadir uno
es un cambio previsto?*— pero **su respuesta tiene fecha**, y por eso los ejemplos de lista cerrada son
ellos mismos candidatos de auditoría.

**Ni alcanza a una medición.** *"1562 líneas"*, *"cuatro hogares"*, *"9 hallazgos, 5 falsos"* no son
contadores: son **hechos fechados**, y su forma honesta ya la fija otra regla — van con su corpus al
lado, el identificador y el tamaño. La diferencia no está en el número sino en que **una medición dice
cuándo se tomó y contra qué**, así que no puede caducar en silencio: si el corpus se mueve, la medida
sigue siendo cierta *de aquel corpus*. Un contador, en cambio, afirma un presente que ya no vigila
nadie.

**Y el remedio cambia la clase de fallo, no lo elimina — conviene saber por cuál se cambia.** Quitado
el número, queda una frase que **exige que la lista sea alcanzable desde donde está escrita**, y si no
lo es, *"las clases de drift"* es a la vez la forma correcta y la forma del fallo. Lo destapó un
corresponsal aplicando nuestra propia frase a nuestro propio remedio: *un detector que busca lo que hay
no encuentra lo que sobra* — y aquí lo que falta tampoco. Pero **el cambio es a mejor y es medible**:
un contador **caduca en silencio** y se caza con precisión; un puntero ausente **no caduca —nace mal—**
y se caza con el detector de abajo, más ruidoso. Se pasa de un fallo que llega solo con el tiempo a uno
que está desde el primer día y no empeora.

Caso propio, y lo destapó un corresponsal preguntando *dónde vive vuestra lista*, no una auditoría:
este mismo archivo llevaba el contador en **tres** frases —el encabezado, *"las ocho son agnósticas"*
y *"las otras siete se ven leyendo el doc"*, que además es un número **derivado** y por tanto rompe
por dos motivos distintos— y una **cuarta** copia vivía en otro archivo y otra capa
(`modules/producto/module.md`). Cinco sitios para un dato que solo tiene uno.

**Detector del manifiesto pendiente (clase 1).** Si `módulos` vale `pendiente`, mira el árbol real: si
el proyecto **ya tiene un producto con estructura** —un codebase, un corpus organizado, una colección
con temas—, el manifiesto afirma algo que dejó de ser cierto. No lo actives tú: **repórtalo y que lo
decida el usuario**, porque el criterio no lo puede resolver un `ls`. Si sigue sin haber producto,
`pendiente` sigue siendo correcto y no es un hallazgo.

Este detector existe porque el olfateo del bootstrap se movió aquí. **Allí corría cuando la evidencia
todavía no podía existir; aquí corre cuando ya apareció**, y cada ~10 sesiones en vez de en cada
arranque. Es el mismo `ls`, puesto donde sirve.

**Y el aviso simétrico, que es más fácil de pasar por alto:** un proyecto con `módulos = —` que
también acabó teniendo producto no es drift —se decidió que no— pero **sí conviene nombrarlo una vez**
en el informe, sin insistir. Un "no" de hace cuarenta sesiones se tomó sobre otro proyecto.

**La clase 7 es la que justifica el ritual.** Las demás se ven leyendo el doc con atención; esta no se
ve en **ningún** doc, porque el defecto es una **ausencia**: el dato existe, pero no donde se lee.
Solo aparece contrastando dos sitios. Es la regla "un hogar por dato" fallando en silencio.

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

## Detectores (sin esto, el ritual es decorativo)

Un audit que devuelve "todo se ve bien" no ha auditado. Barre primero, verifica después:

```bash
# ESTOS COMANDOS ESTAN EN EL IDIOMA DEL KIT. Si tu proyecto esta en otro, lo que se
# copia es el COMENTARIO de cada bloque -- que se busca y por que --, no la regex:
# se derivan, no se traducen. Ver "Los detectores no son todos iguales frente al
# idioma", mas abajo, antes de tocarlos.

# clases 1 y 3 — afirmaciones absolutas y criterios que quizá ya no valen
grep -rniE "siempre|nunca|todos los|todas las|ningún|en ningún caso|garantiza|basta con" {base} --include="*.md"

# clases 2 y 6 — marcadores de estado y bloqueos
grep -rniE "pendiente|por confirmar|validado en|en curso|en progreso|provisional|bloquea" {base} --include="*.md"

# clase 3 — vocabulario de refutación en las sesiones del rango (fuente, no objeto)
grep -rniE "en realidad|result(o|ó)|falso negativo|falso positivo|no funciona|descartad|corregi" {history_dir}

# clase 5 — metadatos de sesión en cabeceras, para contrastar contra {index}
grep -rniE "sesi(o|ó)n [0-9]+" {base} --include="*.md"

# clase 4 — secciones reales del detalle, para contrastar con su índice
grep -n "^## " <doc de detalle>

# clase 8 — tamaño contra presupuesto
wc -l <docs vivos>

# clase 1 — afirmaciones sobre el mundo, para comprobarlas FUERA de los docs (opt-in, ver abajo)
grep -rhoE "(/[a-zA-Z0-9._-]+){2,}" {base} --include="*.md" | sort -u   # rutas
grep -rhoE "https?://[^ )\"]+" {base} --include="*.md" | sort -u        # URLs y endpoints
grep -rhoE "\bv?[0-9]+\.[0-9]+\.[0-9]+\b" {base} --include="*.md" | sort -u  # versiones

# clase 1 — contadores en prosa, que son copias de la longitud de una lista
grep -rniE "\b(los|las) (dos|tres|cuatro|cinco|seis|siete|ocho|nueve|diez) [a-záéíóúñ]+" {base} --include="*.md"
grep -rniE "\b(ambos|ambas|el único|la única|los únicos|las únicas)\b" {base} --include="*.md"

# clase 1 — el reverso del contador: PUNTEROS AUSENTES. Al quitar el número, la forma
# correcta y la forma del fallo son IDÉNTICAS ("las clases de drift" está bien si la lista
# es alcanzable desde ahí, y mal si no), así que ningún barrido de números lo ve. Dos pasos:
grep -rn "<frase que nombra una lista que vive en otro fichero>" {base} --include="*.md"
#   ...y para cada acierto, mirar si SU PÁRRAFO lleva la ruta al fichero que la tiene.
# Más ruidoso que los demás y en otra forma: sus falsos positivos son CITAS de la regla y
# ejemplos, no referencias. Se revisan uno a uno. Corre solo sobre el alcance de arriba.

# clase 1 — CITA AMBIGUA. Si un titulo de seccion existe en MAS DE UN fichero, toda cita
# suya tiene que decir CUAL. Ojo: dos punteros que discrepan pueden ser los dos correctos
# --dos capas declaradas, el porque y lo operativo-- asi que lo que se detecta no es la
# discrepancia sino la AMBIGUEDAD, que es defecto siempre. Y hay lista: los encabezados
# del kit son enumerables. Dos pasos:
grep -rn "^#" {base} --include="*.md"     # titulos que aparecen en 2+ ficheros
#   ...y para cada cita de uno de esos titulos, mirar si su parrafo nombra el fichero.

# clase 7 — sigue sin detector LEXICO, y ahora se sabe por que: se probaron tres disenos
# y el mutilador planto el defecto y ninguno lo encontro (tabla abajo). No es calibracion:
# el vocabulario que hace reconocible una leccion la hace indistinguible de su hogar.
# La razon vieja -"no hay cadena que buscar"- era FALSA: oraculo si hay. Se caza
# contrastando dos sitios en las fases, y la salida de un barrido vale como worklist,
# nunca como dictamen.
```

**Y la razón que esta línea daba antes era falsa, no caducada.** Decía *"no hay cadena que buscar"* y
de ahí se seguía que la clase 7 no era detectable **en principio**. La refutó un corresponsal, con el
mecanismo funcionando un piso más abajo y sobre otro material:

> **El oráculo de un detector de ausencias no es un corpus mutilado: es el MUTILADOR.** Una función de
> un corpus sano a uno dañado. **No se guarda el corpus roto — se guarda la transformación**, y se
> aplica al corpus que se tenga.

Eso disuelve la objeción de *"nadie tiene un corpus con ese defecto guardado"*: **nadie necesita
tenerlo**. Y para la clase 7 el mutilador es de una línea — **coger un dato que sí está en su hogar,
borrarlo de ahí y dejarlo en el registro de sesión. Correr. Tiene que encontrarlo.** El corpus sano es
el que ya se tiene.

**Y el dato que lo prueba va contra la intuición que teníamos:** de los tres arreglos que trajo esa
misma carta, el de las vallas es un fallo **de forma** y tenía **cero instancias** en 117 documentos —
el corpus contenía la condición y no disparaba, por la casualidad de tener el mismo número de celdas.
Para ponerlo en rojo hubo que **sintetizar la entrada**. Así que *"de forma"* no significa *"hay
instancias"*, y el eje forma-contra-semántica no era el que decidía.

**Entonces lo que bloquea la clase 7 es nuestro, no de la técnica: la fase 3 exige dos punteros
`archivo:línea`, y una ausencia no tiene dirección.** El detector puede existir y puede probarse; lo
que no puede es **reportar** en el formato que este ritual exige para que un hallazgo entre al informe.
El remedio propuesto, y es del corresponsal: el hallazgo **sí** tiene dirección, solo que no es la de
la ausencia — es la del **dato que debió promoverse** (el registro de sesión donde se quedó) más el
**nombre del hogar donde no está**. Dos punteros, uno de ellos a un fichero y no a una línea.

**Y lo corrimos. El mutilador funciona como oráculo, y lo primero que hizo fue matar el detector.**

Se construyó el detector léxico de clase 7 —lección destilada del acta que no aparece en ningún hogar
durable— en **tres** diseños, y el mutilador **plantó el defecto y ninguno lo encontró**:

| Diseño | Sobre el corpus sano | Sobre el mutilado |
| --- | --- | --- |
| coincidencia **literal** | 27 candidatos de 38 — **inusable** | (no se llegó a probar) |
| solape de palabras contra el **fichero** | 1 candidato — parecía perfecto | **no lo detecta**: 83% en un hogar de 141 KB |
| solape contra el **párrafo** | 3 candidatos | **no lo detecta**: pasa a "reformulada" |

Y la razón no es de calibración, es de fondo:

> **El vocabulario que hace reconocible una lección es el mismo que la hace indistinguible de su
> hogar.** Un huérfano de clase 7 trata **del tema del sitio donde debía estar** — por eso le tocaba
> ese sitio. Exigir literalidad da ruido de reformulación; aflojar a solape da ceguera. **No hay
> umbral entre las dos, porque es la misma señal.**

**Y eso dejó de ser prosa: está medido.** Mutilando **una a una** las lecciones que sí estaban en su
hogar —para obtener el score de una **realmente ausente**— y comparándolo con el de las **presentes
pero reformuladas**:

| Grupo | n | mín | mediana | máx |
| --- | --- | --- | --- | --- |
| **ausente** | 16 | 0.25 | 0.67 | **1.00** |
| **reformulada** | 21 | 0.50 | 1.00 | 1.00 |

**Los rangos se solapan enteros: no existe umbral.** Y el dato que lo cierra no es el solape sino el
máximo: **hay lecciones realmente ausentes que puntúan 1.00** — todas sus palabras distintivas caben
en un párrafo del hogar **que no las contiene**. No es que el umbral esté mal puesto: **es que la
medida no distingue los dos casos ni en el extremo.**

**Y no es que faltara restar.** La resta estaba: los tres diseños calculaban *"lo que el registro
afirma menos lo que afirma algún hogar"*. Lo que falla es **el predicado de igualdad dentro de la
resta** — decidir si dos frases dicen lo mismo. Un conjunto se resta bien solo si se sabe cuándo dos
elementos son el mismo, y esa pregunta **no es léxica**. Así que la conclusión honesta no es *"tres
diseños fallidos"* sino **la herramienta era del tipo equivocado**, y hay que decirlo así para que
nadie gaste un cuarto diseño.

**Lo que salva es el oráculo, y salvó de verdad.** El segundo diseño daba **un** candidato sobre el
corpus sano y se leía como un detector que funciona; sin el mutilador se habría dado por bueno. **Que
la salida parezca razonable no es evidencia de que el detector mida algo.**

**Y un modo de fallo que conviene nombrar aparte:** el diseño de ventana deslizante llegó a dar
**menos** candidatos sobre el corpus mutilado que sobre el sano — borrar una regla hizo el informe más
limpio. Salió de elegir la vecindad por **aritmética** (una ventana de N caracteres, que se desplaza
con cualquier edición ajena) en vez de por una **unidad del texto**. Para un instrumento de auditoría
es el peor comportamiento posible, y es mudo.

**Lo que sí sirvió fue la lista, no el veredicto.** Los tres candidatos del corpus sano se revisaron a
mano y **uno era una clase 7 real**: una regla que el usuario dictó en una sesión, recogida en el acta
y **sin hogar en ningún doc durable** —comprobado con cuatro sondas por concepto y control positivo—.
Ya está promovida. Así que el barrido léxico vale como **worklist** y no vale como **dictamen**, y esa
distinción es la que hay que escribir al lado de cualquier detector de esta clase.

**Con una precondición que ninguno de los dos había mirado, y que es del medio otra vez: hace falta
poder deshacer.** El mutilador necesita un corpus sano que **sobreviva al experimento**. Quien versione
sus documentos lo tiene gratis; quien no —y aquí los docs de trabajo no se versionan— **no necesita
git, necesita una copia**: el mismo `{{artifacts_dir}}` que ya existe para no destruir evidencia sirve
de red. La condición es *poder volver*, no *tener historial*.

**Y la salvaguarda más barata contra la sonda rota es tener el ANTES al lado del DESPUÉS.** Aporte de
campo, funcionando solo: al comprobar si dos reglas habían entrado en un commit, dos de tres sondas
dieron **cero antes y cero después**. Habrían dicho *"no entró"* de dos reglas que sí entraron.

> **Una sonda que da cero en las dos columnas no está midiendo una ausencia: está rota.** Con una sola
> columna, ese cero es indistinguible de un hallazgo.

Es el control positivo **incorporado a la forma del barrido** en vez de puesto al lado, y no cuesta
nada: cuando se comprueba un cambio, se mide contra los dos commits siempre.

**Y hay un modo de fallo del razonamiento, no de la sonda, que este ritual no cubría:**

> **Una aritmética con dos soluciones no identifica un elemento.** Faltaban seis filas de un total de
> once, y once se descompone en 6+5 **de dos maneras** con las cartas disponibles. Se eligió la que
> encajaba con algo que ya se venía diciendo, y se escribió **como hecho y con motivo inventado**.

**Lo que lo vuelve invisible es la segunda mitad:** cuando una de las dos soluciones confirma el relato
anterior, **deja de parecer una elección**. Es la familia de *"el diff prueba la entrada, no la
autoría"* — había evidencia de **cuántas** faltaban y ninguna de **cuál**. Y la prueba de la ambigüedad
estaba impresa **en la misma carta, dos párrafos antes**.

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

**Y esa regla protege la ENTRADA; hay un segundo eje que no cubre: de qué ámbito saca el detector su
REFERENCIA.** Aporte de campo, y es el hueco más fino que ha aparecido. Un detector de columnas recibía
el fichero entero —cumplía la regla al pie— y aun así comparaba cada fila contra la primera fila de
tabla **del fichero**, no de **su** tabla. **El control positivo no puede verlo**, porque el control es
una sola tabla y ahí las dos referencias coinciden. Es la misma familia del cero falso, un piso más
adentro: no falla lo que el detector lee ni lo que reporta, **falla contra qué compara**.

**Y lo que lo vuelve enseñable no es el fallo: es cuánto duró en verde.** Medido después sobre 117
documentos, ese detector daba **356 hallazgos de los que 354 eran ruido** —una tabla bien formada
comparada contra otra distinta—, o sea **menos del 1% de señal durante cinco versiones**, con su
control positivo pasando todo el tiempo. Un control no se queda corto ruidosamente: **certifica**.

> **Un control positivo de una sola instancia no puede validar un detector que compara contra una
> referencia.** Tiene que llevar al menos **dos** de aquello que el detector empareja, y distintas
> entre sí — si no, las dos referencias coinciden por construcción y el verde no dice nada.

**Y la forma buena es más ancha, la devolvió el campo al aplicar la regla contra sí mismo:** *"al menos
dos"* caza el defecto de la referencia y **solo ese**. La formulación que caza los tres es **una
instancia de cada distinción que el check hace**. Preguntarse *¿qué más distingue esta comprobación?*
—dos tablas de distinto ancho, un separador que es contenido, una valla que parece dato— produjo dos
regresiones más que la forma estrecha no habría cubierto. **El control se deriva de las distinciones,
no del tamaño.**

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

## Nunca metas un carácter acentuado dentro de `[...]`

**Un detector de este mismo ritual estuvo roto por esto, y era del producto.** El patrón de la clase 5
decía `sesi[oó]n [0-9]+` y **no puede casar `sesión`**: la vocal acentuada ocupa **dos bytes**, la clase
casa **uno**, y luego el patrón exige la `n` donde está el segundo byte. Medido sobre un corpus
español: **22 líneas con la clase rota, 156 con la alternancia.** Se perdía el **86%**, en silencio y
durante meses.

> **Usa alternancia —`sesi(o|ó)n`— nunca clase.** Y ojo con el caso peor, que es el que no se nota: si
> el carácter acentuado va **al final** del patrón, la clase **sí casa** —consume el primer byte y no
> exige nada detrás—, así que el detector *parece* funcionar. Uno de los dos patrones de aquí estaba en
> cada caso.

**Hay una tercera forma y esa sí es segura, pero por una razón que no es la que uno cree.** Una clase
de rango con acentos **bajo cuantificador** —`[a-záéíóúñ]+`, como en el detector de contadores— sí casa
palabras acentuadas: el `+` deja que los **dos bytes** del acento entren como **dos elementos** de la
clase. Probado: 3 de 3, con control negativo.

> **La condición es el cuantificador, no la clase.** `[a-záéíóúñ]+` funciona; `[a-záéíóúñ]` a secas, o
> seguida de algo fijo, **no**. Quien copie esa clase a un sitio sin `+` se lleva el fallo mudo sin
> tocar nada de lo que se ve.

**Y esto se generaliza a cualquier kit que no esté en inglés:** un detector léxico escrito en un idioma
con diacríticos y probado con un control **sin** diacríticos pasa en verde y no mide nada. El control
positivo tiene que llevar **la forma acentuada**, que es la que el corpus usa de verdad.

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

**Los detectores no son todos iguales frente al idioma, y la diferencia decide qué hay que rehacer.**
Se reparten así:

- **Estructurales** — el barrido de encabezados, `wc -l`, las rutas, URLs y versiones. **No tocan
  idioma.** Funcionan igual en cualquier proyecto y se copian tal cual.
- **Léxicos** — las listas de palabras (clases 1 y 3, 2 y 6, 3). Atados al **vocabulario**.
- **Gramaticales** — el de contadores. Atado a la **gramática**: artículos, género y número. Este es
  el que hay que mirar con cuidado, porque *parece* léxico y no lo es.

**Y no se traducen: se derivan.** Traducir término a término produce detectores malos, y el de
contadores lo demuestra solo: `(los|las) (dos|tres…)` en inglés sería `(the) (two|three…)`, que **no
discrimina ni género ni número** y por tanto no es el mismo detector sino otro, con otra tasa de falsos
positivos. Lo que viaja es **el comentario que encabeza cada comando** —qué se busca y por qué—; el
comando se escribe desde cero en el idioma del proyecto.

**Cada detector derivado se guarda con su control positivo, y sin eso no se guarda.** Una regex recién
escrita y nunca ejercida es la mejor fuente de ceros falsos que existe, y un cero falso aquí se lee
como *"corpus limpio"*. Junto al comando va **una línea de ejemplo que tiene que dar match**: si no lo
da, el detector está roto y sus ceros no valen nada. Es la misma ley del cero de más abajo, aplicada en
el momento de escribir el detector en vez de en el de usarlo.

Su hogar es la sección *Detectores de auditoría* de `protocol`, **no el manifiesto**: son una lista
larga y viva, no un parámetro. Y **no van en *Acuerdos de auditoría***, que es otra cosa — allí viven
decisiones con umbral, y un léxico no tiene umbral ni es una decisión de no cambiar nada.

## Fases

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

   **Y esa evidencia se pudre, por construcción y sin remedio posterior.** `archivo:línea` es la forma
   correcta **y es un puntero**: el número envejece en cuanto el fichero cambia. Aquí es peor que en
   otros sitios porque **se escribe dentro de registros inmutables** —el acta, el informe, la carta
   entregada—, así que **no hay dónde corregirla**. Lo aportó un corresponsal con su medida (tres de
   seis rangos ya no contenían lo señalado); medido aquí sobre 60 referencias de nuestros registros
   inmutables, **solo el 42% resuelve todavía**. Y las que fallan lo hacen por **tres causas
   distintas**, que conviene no mezclar:

   | Causa | Qué pasó | ¿Evitable al escribir? |
   | --- | --- | --- |
   | **Podrida** (22%) | El fichero está, la línea ya no | Solo en parte: anclar en el identificador |
   | **Renombrada** (7%) | El fichero cambió de nombre | Ya lo cubre la nota de equivalencia del índice |
   | **Sin raíz** (30%) | La referencia **nunca dijo de quién era el fichero** | **Sí, entera** |

   **La tercera no está podrida: nació irresoluble**, y es la mitad del problema. Casi todas eran
   citas a ficheros de **otro proyecto** (`main.go:33`), inequívocas para quien las recibía y opacas
   para el archivo propio desde el primer día. Es la misma forma que la **cita ambigua**: un puntero
   sin raíz vale lo mismo que un título que vive en dos ficheros.

   Así que la evidencia se escribe con **tres piezas, y las dos primeras no caducan**: **de quién es
   el fichero** (repo o proyecto, si no es el propio), **el identificador** del commit, y el
   `archivo:línea` **como pista**, no como ancla. Lo ya escrito **no se corrige** —es registro— y por
   eso esto solo protege hacia adelante.

   **Y el test de si una referencia nombra su raíz NO es "¿lleva directorio?".** Es **cuántos ficheros
   casan con ese nombre**: uno solo = puntero válido aunque vaya abreviado; **cero o varios = no es un
   puntero**. Importa porque la versión intuitiva sobre-marca: al construir el detector, *"sin
   directorio = sin raíz"* dio **2,4 veces** los aciertos reales — marcaba `bootstrap.md:107`, que es
   **único** en el árbol y resuelve solo. Con el test correcto quedan **10 de 62 (16%)**, y las diez
   son la misma forma: **citar el fichero de OTRO proyecto sin decir que es suyo**.

   **Y es el mismo test que la cita ambigua**, unas líneas más arriba: allí un título de sección, aquí
   un nombre de fichero. **Un nombre que resuelve a cero o a más de un sitio no es un puntero**, y en
   los dos casos hay lista contra la que comprobarlo.

   **Y hay un tercer eje, que lo cierra: la REVISIÓN.** Si un título puede vivir en más de un fichero y
   un nombre en más de un sitio, **una línea vive en más de una revisión — y todas lo hacen**. Aporte
   de campo, y con el caso encima: una carta citaba *"el em-dash está en `main.go:18`"* dentro del
   párrafo que fijaba que el defecto venía de la **primera** versión, donde esa línea es la **12**. No
   era falso: era **ambiguo**, y cada mitad correcta por separado.

   Eso reencuadra lo de arriba y conviene decirlo así: **una cita sin revisión no envejece, nace sin
   decir de qué habla.** Solo *parece* envejecer porque el fichero se mueve debajo. Por eso el ancla es
   el identificador y el número es la pista — no como mitigación del paso del tiempo, sino porque **sin
   revisión la referencia nunca señaló nada en concreto**.
4. **Informar** con la forma fija de abajo, separando errores de preferencias, y **con el
   denominador**. Si la proporción de falsas es baja, dilo: *"la documentación está sana"* es un
   resultado válido, y **descartar la hipótesis de partida es un hallazgo**, no una auditoría fallida.
5. **Aplicar** tras confirmación: los errores en bloque, las preferencias una a una.
6. **Segunda pasada** (obligatoria, ver abajo).
7. **Registrar**: fila en `audit`, acuerdos a su hogar, y lo aplicado contado en el `session` de la
   sesión que auditó. Lo que perdura va a su hogar, como en cualquier cierre.

## Segunda pasada (obligatoria)

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

**Y hay una superficie que ninguna pasada alcanza, porque nace después de todas: el cierre.** Escribir
el registro de la sesión **genera afirmaciones nuevas** —*"se tocaron estos archivos"*, *"quedó esto
sin persistir"*, *"esto es lo que falta"*— y son las **más frescas y las menos verificadas** que tiene
el proyecto, precisamente porque el ritual ya se declaró terminado y las fases han pasado por encima.
**Cuando audites, el cierre de la propia auditoría entra en el alcance.**

Caso de campo, y lo que lo hace convincente es de dónde salió: un proyecto encontró en su tercera
pasada —la que debía salir vacía— **una afirmación falsa de seis sesiones de antigüedad**, y la
encontró porque escribir el cierre le obligó a afirmar algo comprobable: dijo que cierto fichero estaba
entre los versionados sucios, corrió el comando para enseñar el diff, y el comando devolvió **dos**
donde la frase implicaba **tres**. Tirando de ahí apareció que su doc de proceso llevaba seis sesiones
diciendo que viajaban tres cosas cuando solo viajaba una — y su propio `.gitignore` lo desmentía en un
comentario, desde el primer día.

## Informe (forma fija)

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

## Acuerdos: cuando el usuario decide no cambiar

Un "déjalo así" **se registra con su umbral**, que es lo que lo convierte en decisión en vez de en
aplazamiento. Si no, se rediscute en cada auditoría:

- **Excepción de contenido** (una frase absoluta que sí es absoluta, un estado que se mantiene a
  propósito) → sección *Acuerdos de auditoría* de `protocol`, con fecha y umbral. Se **cura**: al
  cruzarse el umbral, el acuerdo se revisita y se reescribe o se borra.
- **Tope de tamaño de un rol** (clase 8) → eso no es un acuerdo, es un **presupuesto**: va a la
  sección Presupuestos del manifiesto con el ritual `config` ("déjalo entero; revisar si pasa de
  ~1000 líneas" = `specs = 1000`). Ya hay un hogar para ese dato; crear un segundo lo desincroniza.

## Cadencia

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
