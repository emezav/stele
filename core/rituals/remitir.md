<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

# Ritual: REMITIR (escribir hacia fuera lo que aprendiste)

**Cuándo.** Cuando encuentras algo que **no es tuyo**: un hallazgo cuyo hogar correcto está en el
proyecto de otro. Es el espejo de CONTRASTAR y comparte con él la carta, el archivo y el índice.

## Lo mínimo para ejecutarlo

**Antes de nada:** ¿hay **caso** —algo que pasó, en un terreno, con un coste? Sin caso no hay carta.

1. **Comprobar que hay caso.**
2. **Redactar con la plantilla `letter`.** El **caso primero**; la propuesta es opcional y va marcada
   como lo que es: la parte menos valiosa.
3. **Rellenar "qué NO demuestra este caso".** Es el campo que más veces ha faltado.
4. **Tachar** rutas internas, nombres de máquinas y datos de personas — y **decir en la carta** que algo
   va tachado. Fase obligatoria, no buena práctica.
5. **Pasarle el linter de markdown** al fichero de la carta — y **el marcador va arriba y sin citar**:
   dentro de un bloque de cita, perder un `>` saca una fila de la tabla y **no da error**.
6. **RE-CORRER las filas comprobables, ya con la carta escrita y justo antes de entregarla.** No
   revisarlas: **correrlas**. Es *Una comprobación puede CADUCAR sin fallar, y ningún control lo cubre*:
   los dos controles hablan del detector y **ninguno habla del momento**, así que una fila exacta el día
   que se redactó puede ser falsa el día que sale. **La ventana es más corta cuando la afirmación es
   sobre un corpus ajeno**, porque la mueve el otro y no tú. Caso medido: una carta redactada el 13 y
   enviada el 18 llevaba dentro un `no existe` que para entonces **sí existía**.
7. **Enviar lo decide el usuario, nunca tú.**

**Y las cinco del marcador, que son las que se saltan.** Cada una tiene su cadáver documentado abajo:

| Situación | La regla |
| --- | --- |
| Una afirmación que el otro **puede ir a mirar** | **Ni se omite ni se marca *no***, y menos si le acusa. Mira dónde vive **su copia**, no dónde vive el dato |
| Una fila con **dos afirmaciones de evidencia distinta** | Son **dos filas** |
| Una fila que dice *sí* y apunta a un sello **que aún no existe** | **No se escribe**: o se sella antes, o dice `SIN SELLAR` en el cuerpo |
| Una fila que manda a comprobar a **una copia que no puede responder** | Di **en qué copia** |
| El estado (`redactada`/`publicada`/`entregada`) | **No va en la carta.** Vive en la fila del índice, y solo el usuario mueve la tercera |

**Dónde está el resto.** Se abre por **pregunta**, nunca entero:

| Si te preguntas… | Sección |
| --- | --- |
| ¿Esto es siquiera para otro proyecto? | *El disparador* |
| ¿Cómo digo que algo lo cambié yo? | *Una respuesta está hecha de afirmaciones sobre acciones propias* |
| ¿A qué copia le mando a mirar? | *El tercer modo de fallo* |
| ¿Y lo que afirmo sobre ÉL? | *El ángulo muerto* |
| ¿Parto esta fila? | *Una fila que junta dos afirmaciones* |
| ¿Por qué linter, si es prosa? | *La forma de una carta se rompe donde nadie la comprueba* |
| ¿Qué NO me da el marcador? | *Lo que el marcador NO hace* |
| ¿Va con nombre o anónima? | *Público o privado* · *El remitente* |
| ¿Cuándo pasa a entregada? | *Una carta saliente tiene tres estados* |
| ¿Le copio su forma de hablar? | *Espeja el registro, no el dialecto* |

## El disparador, que es la parte difícil

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

## Lo que cada paso esconde

**Los pasos están arriba y no se repiten aquí** — repetirlos sería el segundo hogar que este kit
persigue en todas partes. Aquí va **solo lo que el imperativo no cabe decir**:

- **Paso 2, la propuesta.** No hace falta traer solución para escribir. Una carta con caso y sin
  propuesta vale; una con propuesta y sin caso, no.
- **Paso 4, tachar.** **El seudónimo del remitente no anonimiza el cuerpo** — un informe de campo va
  lleno de tus tripas: rutas, nombres de máquinas y servicios, datos de personas. Por eso es fase y no
  buena práctica.
- **Paso 5, el linter.** Una carta es un documento y su forma también se rompe — y ahí se rompe **hacia
  fuera**. El porqué, en *"La forma de una carta se rompe donde nadie la comprueba"*.
- **Paso 6, el canal y el ORDEN.** El canal da igual y el marco no opina: pegar el texto en la sesión
  de otro agente, un correo, un issue, un PR. Copiar y pegar es el suelo y funciona siempre. **Lo que
  sí importa es el orden: si la carta afirma cambios en algo que el destinatario puede obtener, dile
  cuándo obtenerlo** — antes de leerla, o mientras. Eso es parte de la carta, no logística.

## Una respuesta está hecha de afirmaciones sobre acciones propias

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

**Y hay una forma de escribirlo que no caduca cuando el par cambia: meter la condición DENTRO de la
fila.** Un `Sí` pelado es verdad para ese lector y mentira para cualquier otro, así que el día que la
carta se publique, se reenvíe o el par deje de compartir disco, la columna pasa a ser falsa **sin que
nadie la toque**. Cuesta cinco palabras evitarlo:

```text
| lo que afirmo | ¿lo puedes comprobar? |
| ...           | Si -- su repo publico                              |  <- vale para cualquiera
| ...           | Si, SI LEES el arbol privado D:\...\Proyecto       |  <- vale, y dice para quien
| ...           | No -- vive en un repo privado. Bajo palabra        |  <- vale para cualquiera
```

**La fila del medio es la que se suele escribir mal**, y las dos formas equivocadas fallan en
direcciones opuestas: `Sí` a secas **promete a un lector futuro algo que no podrá hacer**, y `No` a
secas **le niega a este lector una comprobación que sí puede hacer hoy**. La condición escrita conserva
la riqueza del par sin heredarle la mentira a nadie.

**Y una consecuencia práctica: escribir la condición obliga a nombrar la ruta**, que es justo lo que
hace la fila resoluble. Una fila que dice *"sí, en tu copia"* sin decir cuál no manda a ningún sitio —
ver el tercer modo de fallo, más abajo.

## El tercer modo de fallo: marcar bien y apuntar a un corpus que no puede responder

Los dos primeros son de **cobertura** —el estrecho deja fuera lo comprobable, el generoso mete dentro
lo que no lo es—. El tercero no falla en la columna: **falla en la dirección**.

**Caso de campo, y es de los dos lados a la vez.** Una carta afirmó que cierta cabecera de una carta
propia **no llevaba un campo**, lo marcó `Sí` y mandó al destinatario a comprobarlo **en su copia
archivada**. La copia del destinatario **sí lo llevaba** — pero escrito desde su lado (`entra` en vez
de `sale`), así que no era demostrablemente la original. Ni confirma ni desmiente: **el sitio al que
manda la fila no puede contestar la pregunta.**

| Modo | Qué hace |
| --- | --- |
| **estrecho** | deja fuera lo comprobable -> no se comprueba |
| **generoso** | mete dentro lo que no lo es -> se confirma la mitad y se supone el resto |
| **mal dirigido** | marca `Sí` **correctamente** y apunta a un corpus que no puede responder |

**Y es el más difícil de evitar de los tres, porque la ceguera está del OTRO lado.** Quien escribe no
puede saber cómo archiva el otro; quien archiva no sabe por qué campo barre el otro. Medido el
2026-08-09 en la instancia que lo halló: **23 de 36 cartas recibidas conservaban una sola cabecera —la
suya— y no la del remitente**,
así que para dos tercios del corpus **nuestro archivo no puede testificar qué decía el original**. Nadie
lo decidió; salió así.

> **No basta con preguntar *"¿puede el otro comprobarlo?"*. Falta *"¿en qué copia?"*.** Cuando la
> evidencia es un artefacto que existe **de los dos lados**, hay que decir de cuál se habla — las dos
> copias pueden diferir sin que ninguno lo sepa.

## El ángulo muerto: el marcador inventaría lo que afirmas sobre TI

**Al llenar la tabla uno enumera lo que estuvo escribiendo, y lo que estuvo escribiendo es su propio
lado.** Las afirmaciones sobre el destinatario se quedan fuera sin que nadie lo decida — y son, a la
vez, **las únicas que él puede verificar** y **las que lo acusan**.

**Caso ajeno, y es el peor de los observados.** Una carta abrió con *"cero afirmaciones comprobables
por ustedes en esta carta"* y cerró con *"dónde comprobar: en ningún sitio"*. Sus tres filas hablaban
del remitente y eran correctas. Pero la misma carta llevaba **cuatro afirmaciones sobre el kit del
destinatario**, una de ellas una acusación, que el marcador no cubría. El destinatario fue a mirar
igual y **dos de las cuatro eran falsas**. Si hubiera hecho caso al marcador, seguirían en pie: el
aviso *"aquí no hay nada que comprobar"* quedó puesto **justo encima de lo único comprobable**, y no
solo omite — **disuade**.

**Caso propio, medido, y el resultado engaña.** Nueve cartas con tabla, **52 filas**: **8 hablan del
destinatario** y van marcadas *sí, es su repositorio*. Parece que aquí el defecto no existe. **No es
virtud del formato: es un accidente de visibilidad.** Las ocho están en las cartas a un corresponsal
con **repo público**; en las dos dirigidas a uno con **repo privado**, las once filas hablan del
remitente y **ninguna del destinatario**.

> La columna pregunta *"¿puedes comprobarlo?"*. Cuando el repo del otro no se ve, **todo lo suyo
> responde que no y sale solo del inventario** — en silencio, y justo cuando la afirmación lo acusa.

**La regla no es "marcar todo"**, que es imposible en una carta larga: un marcador es por fuerza un
resumen. Es esta:

> **Lo que no puede quedar fuera del marcador es lo que el otro sí puede ir a mirar** — y muy en
> especial si le acusa de algo.

Y la prueba de que un marcador resume bien es de qué lado cae lo que omite: **omitir testimonio es
correcto; omitir lo comprobable es el fallo.** Los dos ejemplares conocidos caen a lados distintos, y
por eso la regla no sale de uno solo.

## Una fila que junta dos afirmaciones con evidencia distinta son dos filas

**Es el fallo contrario y también es real.** Una fila propia decía *"la comprobación no tenía control
positivo, **ni escrito ni en la práctica**"* → **Sí**, con dos identificadores. El diff prueba *"ni
escrito"*; *"ni en la práctica"* —siete sesiones corriéndola sin él— sale de las actas, **que no
viajan**. Marcada entera como comprobable.

| | Omitir lo comprobable | Marcar entera una fila mitad comprobable |
| --- | --- | --- |
| Qué provoca en quien lee | **no comprobar** lo que sí se podía | comprobar, confirmar la mitad y **dar por buena la otra** |
| Cómo se manifiesta | una afirmación falsa que sobrevive al marcador | un lector que sale creyendo probado lo que no se probó |

El daño del segundo es menor —acompaña una afirmación que puede ser cierta—, pero **el mecanismo es el
mismo: el campo dice más de lo que sostiene**. Se parte limpia: *"no estaba escrito"* → **sí**, con su
identificador; *"no se corrió con él"* → **no**, bajo palabra.

**Lo caro del caso es que la misma tabla trataba el problema dos veces y solo acertó una:** dos filas
más abajo, un reparto medido sobre las propias actas iba marcado *"sí… así que **no**. Bajo palabra"*.
La distinción estaba entendida en el mismo documento, a diez líneas. **No se evita habiéndolo
entendido; se evita partiendo la fila.**

## La forma de una carta se rompe donde nadie la comprueba

**Caso propio, y en carga ya entregada.** Una carta salió con la **última fila del marcador fuera del
bloque de cita**: perdió su `>`, así que no renderiza dentro de la tabla. Tres filas lo llevaban y esa
no. Lo cazó el destinatario leyendo, no nosotros escribiendo.

> **Y era detectable por máquina.** Una fila suelta debajo de una tabla citada queda como una tabla de
> una fila sin línea en blanco delante —**MD058**— y el linter la marca. Comprobado con un repro
> mínimo de dos casos: el roto marca y el mismo con su `>` sale limpio, que es lo que convierte esto
> en un detector y no en una corazonada.

**Lo que fallaba no era el detector: era el alcance.** El linter se le pasa a los docs y a una carta
no, porque una carta **se siente correspondencia y no documentación**. Es el mismo punto ciego que
deja sin barrer cualquier fichero que viaje sin parecer un manual: **la lista de lo que se comprueba
es todo lo que se escribe, no todo lo que parece un doc.**

**Y la fila que se cayó era la que retiraba una afirmación propia**, o sea la única del marcador que
iba en contra nuestra. Eso no se eligió: la única prueba de que un marcador se leyó entero es que
alguien note **qué falta**, y lo que falta se nota menos cuanto menos te favorece. Es *"el error que
te quita razón no se busca"* —`core/reference/verificar.md`— en la capa de la forma.

**Y el remedio no es el linter: es que la tabla no vaya citada.** El linter es la red que caza el fallo
después de escribirlo; lo que lo hace imposible es quitarle la marca por línea. Una tabla dentro de un
bloque de cita solo se sostiene si **cada** línea lleva su `>`, así que su forma depende de una marca
que se repite tantas veces como filas haya y que no falla en voz alta al perderse. Fuera de la cita esa
clase de fallo no existe. **Va en la plantilla `letter` desde este caso** — el marcador arriba y sin
citar — y las cartas anteriores **no se tocan**: están entregadas o recibidas.

**Lo que no cambia es el alcance, que era el defecto de fondo.** Aunque la forma ya no se rompa por ahí,
el linter sigue siendo paso del ritual: lo que fallaba no era el detector sino a qué se le pasaba, y una
carta seguirá sin parecer documentación el día que se rompa por otro sitio.

**El archivo entregado no se retoca**, y no es una preferencia: lo que recibió el otro y lo que
guardas tú tienen que ser el mismo texto, o el registro deja de servir para comprobar nada. Se anota
al lado, marcado como posterior.

## Lo que el marcador NO hace, dicho por quien lo propuso

**Marcar un bloque como no comprobable no obliga a comprobarlo.** Caso de campo, contado contra sí
mismo: una cifra inventada salió **por debajo de una marca correcta** que encabezaba su bloque, y la
cazó ir a verificar, no la marca. **Funciona como etiqueta, y una etiqueta no lee lo que hay debajo.**

Lo que sí hace es de otra clase, y es la razón de mantenerlo: **deja un rastro que se puede auditar
después.** El barrido que encontró el ángulo muerto de arriba no lo encontró leyendo prosa — lo
encontró **mirando marcadores**. Un campo explícito y equivocado es un objeto contra el que chocar;
una intuición implícita no lo es.

**Y hay un número al que esto obliga:** si el otro puede mirar tu árbol, tus cifras dejan de ser
comprobables por otra razón — **el árbol se mueve**. Una medida sobre un repo sin decir sobre qué
bytes no la reproduce nadie, ni él ni tú mañana. Un mismo fichero dio 803, 862 y 763 en tres momentos
del mismo día. **Ningún número sale de un repositorio sin su corpus fijado al lado: el identificador
del commit y el tamaño.**

**Fijar el corpus es lo único que puedes prometer. Congelar un fichero vivo, no.** El impulso que sigue
al identificador es prometer además que no se toca —*"queda congelado mientras dure la comparación"*—
y esa promesa **no la puede sostener quien la hace**: la rompe un cambio de **otro hilo de trabajo**,
cuyo autor no sabe que ese fichero es corpus de nadie, y al que ningún recordatorio dirigido a quien
prometió le llega. Caso propio, con la peor cronología posible: una carta salió sellada con `X`
prometiendo congelar dos cosas, y **el hijo directo de `X`** reorganizó el kit y dejó el fichero central
de la comparación en 250 líneas de las 1845 que tenía, **sin una sola valla de código dentro** — de seis
delimitadores a cero. El corresponsal lee ese árbol: de haber medido, habría obtenido un **cero
legítimo** —ni detector roto ni filtro mal escrito— indistinguible de los ceros falsos que las dos
partes llevaban seis cartas persiguiendo.

**El corolario invierte el valor de la frase: decir "está congelado" es peor que no decir nada**, porque
el otro deja de comprobar. Es la forma de la promesa sin sello, unos párrafos más arriba — desde el lado
del que recibe, una promesa y una afirmación no verificable se ven igual.

**Y el remedio que parecía obvio era peor que el problema.** Propusimos un **centinela** —*el corpus
fijado tiene N líneas y M vallas; si el árbol da otra cosa, no es un aviso, es una carta*— y el
corresponsal lo desmontó con una pregunta: **¿contra qué compara?** Si contra las cifras escritas en la
carta, esas cifras pasan a vivir en **dos** sitios —la carta entregada e inmutable, y el centinela—,
que es exactamente el defecto que venía a vigilar. Y si contra el blob del identificador, entonces **no
necesita cifras**: necesita el hash, y lo que detecta ya no es *"el corpus cambió"* sino **"alguien
está midiendo contra el árbol en vez de contra el commit"**, que es el fallo de verdad.

**Con eso la promesa no es solo insostenible: es redundante.** Un corpus fijado por identificador es
inmutable por construcción y **no necesita que nadie prometa nada**; el coste es decir el hash cada
vez, que es lo que la regla ya obliga a hacer. La formulación es del corresponsal y es mejor que la
nuestra: la promesa *no añadía garantía, solo la sensación de una*. Nosotros habíamos escrito que no se
puede sostener; falta la mitad — aunque se sostuviera, no serviría para nada.

**Y hay una capa que solo se ve al reincidir.** Esa misma carta ya diagnosticaba la clase —*cuando se
congela un corpus, se congela lo que se recuerda de él, no lo que lo hace corpus*— y no protegió nada,
porque se escribió **sobre el fichero que se acababa de descubrir** en vez de sobre la clase: el que se
rompió fue el otro. Y el pendiente que vigilaba el asunto nombraba también ese fichero, así que
**cuatro sesiones de vigilancia apuntaron al fichero sano**. Una deuda anotada con el nombre equivocado
es peor que no anotarla — cada lectura confirma que ya se sabe dónde está el riesgo.

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

## Público o privado

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

## Una carta saliente tiene tres estados, y el agente solo puede mover dos

**`redactada` -> `publicada` -> `entregada`.** El agente escribe la fila al redactar y mueve la
segunda; **solo el usuario mueve la tercera**, porque el cartero es él. Un agente **no puede
comprobar** que una carta salió: puede saber que la escribió y nada más, así que anotar "enviada" al
archivarla es una suposición disfrazada de registro — y el índice es justo el doc que no debe
contenerlas.

Ocurrió: una carta estuvo dos sesiones en el cajón mientras el índice decía "enviada", y lo descubrió
el usuario preguntando si ya se había contestado. Con los estados, esa pregunta **se responde
mirando** en vez de preguntando: una fila `redactada` es una conversación que no ha salido.

**El estado de en medio existe porque el fallo caro no es el de la entrega, es el del orden.** Cuando
una carta afirma cambios en algo que al otro le llega, su identificador de publicación tiene que estar
**dentro**; y publicar lo hace el agente mientras entregar lo hace la persona, así que **el orden entre
los dos pasos no lo controla ninguno de los dos por separado**. Escribir *"esta carta espera al sello"*
dentro de la carta no ata a nadie: se probó, y la carta salió igual — con esa frase dentro, ya falsa,
en una copia que no se puede reescribir.

**Y volvió a pasar con el mecanismo ya escrito, que es el dato que importa.** Dos cartas salieron con
el identificador **sin rellenar**: un marcador con tres `{SELLO}` literales dentro, en las filas que
prometían *"sí, lo pueden comprobar"*. El aviso existía y estaba **en un comentario HTML del
archivo** — o sea invisible al renderizar, más débil todavía que la frase que este ritual ya declaraba
insuficiente.

> **Un hueco que se puede rellenar más tarde se entrega más tarde.** El estado no lo mueve el
> documento: lo mueve quien lleva la carta, y a esa persona el archivo no le dice nada.

De ahí la forma que sí aguanta: **el hueco no se escribe como hueco, se escribe como defecto visible
para el destinatario.** Un `{SELLO}` parece plantilla y se cuela; `SIN SELLAR — no publicado` en el
cuerpo del marcador **avergüenza al que la lee**, que es el único lector garantizado. Y la alternativa
más barata todavía: **no escribir esas filas hasta tener el identificador** — un marcador con una fila
menos es honesto, y uno con una fila que apunta a nada no lo es.

### Cómo se comprueba que algo está publicado, que son dos pasos

**El primero es de red y el segundo es local, y hacen falta los dos.** `git ls-remote` da **la punta**;
la pertenencia la da `git merge-base --is-ancestor <id> <punta>`:

```sh
PUNTA=$(git ls-remote origin refs/heads/main | cut -f1)   # hecho de red
git merge-base --is-ancestor <identificador> "$PUNTA"     # hecho local: esta dentro?
```

**Y el control positivo no puede ser un identificador cualquiera.** `ls-remote` lista **puntas de
referencia**: con una sola rama, el único identificador que puede aparecer ahí es el sello mismo. Un
control positivo que no sea la punta **devuelve exactamente lo mismo que el control negativo** — los
dos nada — así que la comprobación pasa en verde sin haber separado nada. Caso de campo: dos proyectos
en correspondencia sellaron así durante varias cartas, y lo destapó el destinatario al reproducir la
cabecera antes de creerla. El control que **sí** separa es un identificador conocido-publicado que **no
sea la punta**, comprobado por ancestría, con un hash inventado como negativo.

**Ojo con la trampa de la regla anterior:** la formulación que produjo el fallo decía *"sella contra
`ls-remote`, nunca contra `git log`"*, y es una prohibición que se pasa de frenada. La ancestría lee la
base de objetos local —la familia de `git log`— y **es justo la pieza que faltaba**. Lo que hay que
sacar de la red es **la punta**, no cada comprobación.

**Y bajo `ls-remote` a secas el sello caduca solo.** Un identificador publicado deja de aparecer ahí en
cuanto alguien empuja algo encima, porque ya no es la punta: la carta sigue siendo correcta y su
verificación falla, sin que nada haya ido mal. **Un método de comprobación que expira cuando el otro
publica no es un método**, y por eso el paso local no es opcional.

`publicada` **es observable por el agente** —un identificador contra el servidor, no contra la copia
local— y por eso puede exigirse. **Una fila `entregada` sin haber pasado por `publicada`, cuando la
carta afirma cambios, es una deuda visible en el índice** en vez de un descubrimiento una carta
después. Es el mismo mecanismo que ya justifica los otros dos estados, aplicado al paso que faltaba.

**No aplica a toda carta:** una que no afirma cambios en ningún artefacto va de `redactada` a
`entregada` y ahí se acaba. Y **las filas viejas no se reescriben** para añadirles el estado: el índice
registra lo que pasó, y lo que pasó es que entonces no existía.

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

**Y el reverso también pasó, con el mismo campo `Dirección` y coste real: seis cartas seguidas
salieron SIN él.** La plantilla lo exige, nadie lo comprobó nunca, y el defecto durmió hasta que un
barrido de la propia correspondencia **filtró por ese campo** y se dejó fuera una carta entera. El
recuento equivocado salió publicado en una carta, y lo corrigió el destinatario contando.

> **Los campos de la cabecera no son ceremonia: son el índice por el que se barrerá esta carta dentro
> de un año.** La que no los lleva es invisible al barrido que existe para encontrarla, y su ausencia
> **no da error**: da un elemento menos.

Cuesta una línea comprobarlo antes de archivar —¿están los cinco campos?— y no hay forma barata de
notarlo después. **Las que ya salieron se anotan y no se retocan**, por la asimetría de arriba: una
cabecera incompleta es un fósil visible; una completada a posteriori no la delata nada.

## El remitente, y por qué son dos claves

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

## Espeja el registro, no el dialecto

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

**Y debajo hay un supuesto que puede no cumplirse: que tu proyecto tenga variante declarada.** La regla
dice *escribe en la variante de tu proyecto*; si nadie la eligió, sale la del modelo **y la regla se
lee como cumplida**. Segundo caso de campo, con los dos lados a la vez y descubierto al preguntar: dos
proyectos llevaban cinco cartas cada uno en una variante que no había elegido ninguna persona. Uno la
tenía declarada y **recayó**; el otro **no podía recaer, porque nunca la había decidido** — y desde
fuera los dos se ven idénticos. La regla no cortó el bucle en ninguno de los dos, porque cada uno la
leyó como una instrucción sobre su propia variante y uno no tenía ninguna.

Así que antes de la pregunta al otro va una comprobación de una línea sobre lo propio: **¿está
declarada la variante de este proyecto, y la eligió una persona?** Si la respuesta es no, la regla no
te está protegiendo de nada — te está devolviendo el default con cara de decisión.
