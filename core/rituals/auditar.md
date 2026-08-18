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

## Lo mínimo para ejecutarlo

> **Este es el ritual más caro del kit** (~19 000 tokens): lo de abajo se abre por pregunta, nunca
> entero.

**Los seis pasos:**

1. **Delimitar el alcance y decirlo en una línea ANTES de leer nada.** El eje es **qué documentos**
   entran, no qué rango de sesiones — el rango describe **cobertura temporal**, y sirve para saber qué
   quedó fuera, no para acotar el trabajo. Si sale caro, el momento de acotar es **ese**.
2. **Barrer** con los detectores. Lo que sale es un **candidato**, no un hallazgo.
3. **Verificar** cada candidato. Aquí se va el grueso. Un hallazgo entra al informe **solo con
   evidencia**: dos punteros que se contradicen. Sin evidencia es **sospecha**, va aparte y **no se
   aplica**.
4. **Clasificar** por clase de drift.
5. **Aplicar** lo aceptado, cada cosa en su hogar y **con su procedencia**.
6. **Segunda pasada, obligatoria** — y **registrar la fila** con su denominador.
7. **Correr la cola de citas del propio cuerpo de reglas y anotar la fracción**, que es una línea en la
   fila: cuántas reglas se citan en 2+ actas, en 1 sola y en ninguna. Es la ley
   *La cola de citas: un cuerpo de reglas no se satura de golpe, y su síntoma no es el que se sospecha*,
   y **se pone aquí porque una serie solo dice algo si tiene puntos**: medida una vez es una foto,
   medida cada auditoría es la única señal de que el corpus dejó de consultarse.
   <!-- El titulo va ENTERO en una linea a proposito, aunque pase de 100 columnas: el detector de
        perchas busca el titulo con grep, y un titulo partido por el ajuste de linea es invisible
        para el. Es "medir prosa por lineas es medir otra cosa", aplicado a la propia percha. -->
8. **Ejecutar los comandos que el propio proyecto publica**, si el entorno puede: extraer cada bloque
   ejecutable, clasificarlo —literal o plantilla, solo-lee o escribe— y **correr los literales**. Es
   *Un comando publicado tiene tantas sintaxis como lenguajes anida, y comprobar la de fuera no dice nada de las de dentro*.
   **La comprobación barata no vale aquí**: un bloque con `awk` o `python` dentro puede pasar `bash -n`
   y reventar al correrlo. **Si el entorno no puede ejecutar, se dice en la fila** — un paso que no se
   pudo dar no es un paso limpio.

**Las siete que se saltan.** Cada una tiene su cadáver abajo:

| Situación | La regla |
| --- | --- |
| Vas a escribir `archivo:línea` en un registro **inmutable** | **Di de quién es el fichero.** El 30% de las referencias podridas nunca dijo su raíz, y eso sí es evitable entero |
| Un detector devuelve **cero** | No vale nada **sin su control positivo**, y el control lleva su **valor esperado** escrito |
| Vas a corregir una afirmación en un doc **ya archivado** | **Se marca la puerta, no la afirmación.** Es O(1); marcar cada una es O(n) y se pudre |
| El barrido dio muchos candidatos | El alcance es el **TECHO**: ninguna segunda pasada lo sube. Lo que quedó fuera **se dice en la fila** |
| Vas a poner una cifra en el informe | `Comprobadas` es el **denominador**. Sin él, dos auditorías con los mismos hallazgos son indistinguibles |
| El usuario decide **no** cambiar algo | Es un **acuerdo**, no un pendiente. Se registra como tal y no se vuelve a proponer |
| Vas a fiarte del recorte que te trajo aquí | **Cautela 0: vuelve a la fuente.** El recorte no es el corpus |

**Dónde está el resto.** Se abre por **pregunta**, nunca entero:

| Si te preguntas… | Sección |
| --- | --- |
| ¿Qué entra y qué queda fuera? | *Alcance* |
| ¿Cómo se comprueba algo? | **No está aquí**: `core/reference/verificar.md`, y su lista de títulos en *Las leyes de verificación viven aparte* |
| ¿De qué clase es este defecto? | *Las clases de drift* |
| ¿Con qué barro? | *Detectores* — y *El séptimo detector* para comprobar contra el mundo |
| ¿Mi detector sirve en otro idioma? | *Los detectores frente al idioma* |
| ¿Por qué una segunda pasada? | *Segunda pasada (obligatoria)* |
| ¿Qué forma tiene el informe? | *Informe (forma fija)* |
| ¿Cuándo toca auditar? | *Cadencia* |

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

### Y esa frase concede una exención que nadie decidió: un corpus se audita cuando un hallazgo tiene dónde caer

**Aporte de campo, y sale de mirar un log de auditorías en vez de la intuición.** Un proyecto llevaba
tres auditorías: la primera tomó **la correspondencia** —17 cartas y su índice—, y las otras dos, los
documentos hogar. **La correspondencia es un registro, es inmutable, y aun así se auditó la primera.**

> Así que lo que exime a un corpus **no es ser un registro**. Si lo fuera, esa primera auditoría no
> existiría.

Lo que la hizo posible está en su resultado: **sus hallazgos se aplicaron a los hogares, no a las
cartas.** Las cartas fueron **fuente**; el objeto corregido fue otro.

> **Un corpus se audita cuando un hallazgo suyo tiene DÓNDE CAER.** Los documentos vivos son
> corregibles. Un registro no lo es — y una auditoría cuyos hallazgos no se pueden aplicar se lee, **por
> adelantado**, como trabajo sin consecuencia.

**Y de ahí el aviso, que es sobre la frase de arriba.** Decir *"fuente y no objeto"* es correcto y
necesario: un registro no se reescribe. Pero **se lee como permiso para no mirarlo**, y eso es otra
cosa. Un `session` no se corrige **y sí se puede auditar**: lo que encuentre cae en el hogar que debió
recibirlo, en `gotchas`, o en la práctica futura. **Antes de excluir un corpus, di dónde caería lo que
encontraras** — si la respuesta existe, la exclusión era pereza; si no existe, la exclusión está
justificada y ahora está **escrita**.

**Ejemplar del lado que funciona**, y lo aporta quien trajo la regla: un barrido de sus propios
registros estuvo nombrado dos sesiones sin correrse, y **se corrió el día que dijeron en voz alta dónde
caería** — en su fichero de trampas y en la práctica, no en las cartas. **Nombrar el destino fue lo que
lo desbloqueó, no la voluntad.**

### Y una vez que el hallazgo tiene dónde caer, queda si la afirmación se puede volver a comprobar

**Caso propio, medido al ir a pagar esa deuda.** Un historial tenía **17** líneas que afirmaban un
cero. Filtrando por las que citaban un comando salieron **3**, y se declaró por escrito que para esas
tres no había excusa. Al correrlas:

| | |
| --- | --- |
| Afirmación de corpus, re-corrible | **1** — y pasó |
| **Narración** de una depuración pasada | 1 |
| Comando citado **sin el patrón** que se buscó | 1 |

**El reparto real no era 3 contra 14: era 1 contra 16.** El filtro contaba como instrumento cualquier
línea que **mencionara** `grep` — uso contra mención otra vez, y en el sitio donde más cara sale,
porque produce **una cifra cierta con una etiqueta generosa** que ya había viajado en una carta.

> **Un comando citado no es un instrumento.** Para que una afirmación se pueda volver a comprobar hace
> falta **comando + patrón + corpus**. Con dos de los tres no se re-corre: se mide otra cosa y se le
> pone el mismo nombre.

**Y de ahí lo que un registro debe guardar, que es más de lo que parece:** *publicar la medición y no
solo el veredicto* es necesario y **no basta**. La medición sin su instrumento **hereda para siempre la
interpretación con la que se publicó** — nadie puede contradecirla, porque nadie puede repetirla.

**La consecuencia incómoda, y conviene decirla antes de que la diga el corpus:** si al instrumento hay
que sumarle el **sustrato** —ver *"las capas se cortan por lo que escribes"*, en este archivo—, entonces
re-correr puede salir más caro que medir de nuevo. Cuando ese sea el caso, **el registro no debe
prometer que se puede re-correr**: debe decir que la cifra es de su día, y ahí acabarse.

### Qué hacer con lo ya escrito: se marca la puerta, no la afirmación

Al aplicar lo anterior aparece un montón de cifras viejas **sin instrumento**, que no se pueden volver
a comprobar y tampoco se pueden borrar porque **un acta no se reescribe**. La salida no es marcarlas
una a una:

> **Un acta es fuente, nunca objeto.** No se marca la afirmación: se marca **la puerta por donde se
> lee**.

Tres razones, y la tercera es la que cierra:

- **Un acta no se lee para decidir.** Una afirmación que nunca subió a un documento vivo ya está
  inerte: nadie construye encima porque nadie la lee. Marcarla no cambia nada.
- **Las que sí se leen son las que subieron**, y ese conjunto está acotado por los documentos vivos,
  que se curan y no crecen solos. La pregunta que termina no es *"marco mis N"* sino **"de cada
  afirmación que un documento vivo sostiene, ¿su fuente se re-corre?"** — que es esta auditoría.
- **Marcarlas una a una es un remedio que hay que mantener**, y por su forma eso no es un arreglo:
  crece con cada acta nueva y se pudre igual que un rango de líneas.

**Queda un residuo, y para ese sí hay que hacer algo:** alguien de fuera que lea el acta directamente,
sin pasar por los documentos vivos. Para eso basta **una línea, una sola vez, en la cabecera del
índice**, diciendo que las afirmaciones anteriores a tal fecha no llevan instrumento y que cada cifra
es de su día. Cuesta O(1) y no hay que mantenerla.

<!-- Regla aceptada de un corresponsal (2026-08-10). La instancia que la recibió llevaba 16 cifras en
     esa situación y su instinto era marcarlas una a una; lo que le faltaba no era otra solución, era
     la razón por la que basta con la puerta. -->

### Y el estado de un documento archivado no se mueve solo

Un fichero que se archiva con una marca de estado —*recibido*, *sin procesar*, *pendiente*— la conserva
**para siempre**, porque el momento en que deja de ser cierta no tiene ningún paso asociado. Es la
clase 1 con una particularidad: **el que la escribió sabía que iba a caducar**, y aun así no dejó quién
la moviera.

**Caso de campo:** un archivo de correspondencia con **21 cartas** marcadas *"archivada sin procesar"*,
varias procesadas sesiones atrás. Las salientes del mismo archivo sí tenían ciclo
—`redactada -> publicada -> entregada`— y estaban al día. **La asimetría es la pista:** el lado con
pasos definidos no derivó y el lado sin pasos derivó entero.

Al auditar, la pregunta barata es: **¿qué marcas de estado hay en el corpus, y quién las mueve?** Si la
respuesta a la segunda es *nadie*, no hace falta comprobar ninguna: ya se sabe el resultado.

## Las leyes de verificación viven aparte, y esta es su lista

**El *cómo se comprueba* vive en `core/reference/verificar.md`.** Aquí queda el **procedimiento** de
auditar; allí, las leyes que gobiernan cualquier comprobación. **La lista va entera a propósito: un
título que se muda deja de ser encontrable por barrido desde donde estaba.**

> **Esta lista es una COPIA y se desincroniza — mantenerla es parte de añadir una ley.** Al escribir una
> nueva en `verificar.md`, su título entra aquí en el mismo cambio. **Se descuadró en la primera ley que
> se añadió tras crearla**, que es lo que hay que esperar de cualquier copia mantenida a mano.
>
> **Y NO se comprueba contando: se comprueba con `comm`**, que es una biyección y no un número. Contar
> da un solo bit —cuadra o no— y **el conteo mismo puede fallar**: un patrón que recoja una línea de más
> del bloque de alrededor da un descuadre inventado, y eso ya ocurrió aquí. `comm` dice **cuál** falta y
> **de qué lado**, que es lo único accionable:
>
> ```bash
> grep '^## ' {kit}/core/reference/verificar.md | sed 's/^## //' | sort > /tmp/leyes
> sed -n '/^## Las leyes/,/^## [^L]/p' {kit}/core/rituals/auditar.md \
>   | grep -E '^- \*' | sed 's/^- \*//; s/\*$//' | sort > /tmp/indice
> comm -23 /tmp/leyes /tmp/indice   # ley sin entrada en el indice
> comm -13 /tmp/leyes /tmp/indice   # entrada sin ley
> # CONTROL POSITIVO: anade una linea inventada a /tmp/leyes y comprueba que sale reportada.
> # Sin el, dos ficheros vacios tambien dan "sin diferencias".
> ```

- *Ni el comando ni su implementación son el patrón, y confundirlos parece sustrato*
- *Un desacuerdo demuestra que algo difiere; nunca demuestra QUÉ*
- *Antes de llamar sustrato a un desacuerdo, mira si alguien está violando una especificación*
- *Una especificación también dice dónde se CALLA, y ahí la divergencia está anunciada*
- *Cuando la especificación es propia, lo que la valida es su FECHA*
- *El nombre de una comprobación no es su cobertura*
- *El sello de una cifra certifica que sigue viva, no que sea la cifra de lo que dices*
- *Una justificación producida al auditar el defecto no tiene procedencia*
- *Declarar una variable de entorno no es controlarla*
- *Medir prosa por líneas es medir otra cosa*
- *Un comando publicado tiene tantas sintaxis como lenguajes anida, y comprobar la de fuera no dice nada de las de dentro*
- *Una tasa mide también la EDAD de su corpus, y dos tasas no se comparan por el denominador*
- *Una cifra sobre tu propio corpus, escrita en el kit, es una FOTO*
- *Un comando que aborta, detrás de una tubería, es un cero*
- *No verifiques a alguien con su propia herramienta*
- *El sustrato que dos corresponsales no pueden variar es el agente*
- *No se enumeran los fallos: se enumeran las CAPAS*
- *Las capas se cortan por lo que ESCRIBES, y lo que no, es el SUSTRATO*
- *"El mismo comando" no es el mismo programa*
- *Mide el producto sobre lo que se distribuye, no sobre tu árbol*
- *Un detector léxico depende del LOCALE, y el mismo patrón da dos respuestas*
- *Un control negativo deja de separar cuando su cadena entra al corpus*
- *`git log` no data un proyecto: data un repositorio*
- *Y comprueba el valor esperado del control, no solo su resultado*
- *Un control corre sobre el PRODUCTO de un instrumento, así que puede acusar al trabajo*
- *Las alternativas de un patrón son su lista de distinciones, y se cuentan*
- *Un detector no monótono no mide el defecto: mide la forma del corpus*
- *Una tasa alta de falsos positivos protege al fallo que la causa*
- *El reverso: cuánto puede callar un detector antes de que su silencio deje de informar*
- *Un número sin expectativa no es información: es decoración*
- *Una ausencia y una no-aplicabilidad dan la misma cifra*
- *Un barrido que filtra por un campo no ve al registro que no lo tiene*
- *Y el error que te quita razón no se busca*
- *Un detector tiene tres superficies, y el control positivo cubre una*
- *Un filtro de bloques falla por tres sitios, no por uno*
- *Un acuerdo entre dos comprobaciones independientes no es evidencia*
- *Un verde tampoco dice QUÉ encontró*
- *Los datos que sostienen una decisión salen juntos*
- *Publicar una ausencia exige más que publicar un hallazgo*
- *Cómo se barre prosa: por palabra rara, por concepto y sin mayúsculas*
- *Un dato puede tener hogar y seguir huérfano*
- *Barrer por alcance: qué queda fuera del encuadre de una regla*
- *El corpus donde todo detector empeora: los documentos que explican la regla*
- *La asimetría no es del error: es del acto que el error autoriza*
- *Separa la afirmación de la regla*
- *Mover prosa rompe sus deícticos, y ninguno da error*
- *Un documento sí ejecuta: se renderiza*
- *Quién ve el rojo, y en qué papel*
- *Prohibir una observación no basta: hace falta que algo obligue a hacerla*
- *Una contradicción interna que sobrevive mide cuántas veces el documento se cargó sin leerse*
- *Cuando un fichero admite dos lecturas por patrón, el control es un invariante y no un patrón mejor*
- *El identificador de una acción es el del instante en que ocurrió, no el de cuando la cuentas*
- *No se pone la medición en el camino: se pone la predicción, también sobre lo que va a salir bien*
- *Una cifra sin su comando no es comprobable, aunque el corpus esté delante*
- *Documentar un experimento en curso puede contaminarlo, porque su ejecutor lee lo mismo que tú*
- *La redundancia de un cuerpo de reglas vive en el USO, no en las palabras*
- *La cola de citas: un cuerpo de reglas no se satura de golpe, y su síntoma no es el que se sospecha*
- *La primera señal de que una comprobación sobra no es el silencio: es el ruido*
- *Un directorio ignorado es invisible a tu buscador y visible en tu contexto*

## Al escribir una ley: di si trae comando, o di que no puede traerlo

**Una ley conocida no impide su caso.** Es la observación más incómoda que ha producido este documento y
la trajo un adoptante con su propio registro: incumplió **tres veces en un día** una ley que tenía
instalada, y **las dos últimas después de haberla aceptado por escrito**. Lo que paró la tercera no fue
recordarla — fue **tener el comando en la mano**.

**Medido sobre este mismo fichero**, para no discutirlo en abstracto:

```text
leyes                    : 59        (medido en b91ce4c, 2026-08-17)
CON comando ejecutable   : 11   (19%)
SIN comando              : 48   (81%)
CON COMANDO **Y** PERCHA :  6

metodo: una ley "trae comando" si tiene al menos una linea que EMPIEZA por una
        invocacion ejecutable (grep, git, find, awk, printf, python...) **DENTRO
        DE UN BLOQUE DE CODIGO**. Un bloque de SALIDA no cuenta: cuenta la invocacion.
control +: "El comando no es el patron"    -> sale CON comando
control -: "La asimetria no es del error"  -> sale SIN comando
control de encuadre: SIN restringir a bloques da 11 en vez de 8, y las tres de mas
        son prosa envuelta que empieza por "locale" (x2) y por "diff" (x1).
```

**La primera cifra publicada fue 9, y eran dos de más.** La remidió un adoptante con su propia lista
de herramientas, dio **7**, y al ir a mirar **cuáles** —que él no hizo y nosotros sí— las dos de
diferencia resultaron **falsos positivos nuestros**: contaban porque el ajuste a 100 columnas había
dejado la palabra `locale` al principio de una línea de **prosa**, y el patrón ancla al inicio.

**Y el 7 tampoco era el número: había un tercero, con otra palabra.** Una línea de prosa que empezaba
por `diff` colaba una ley más, y **la remedición ajena no lo encontró** porque su lista de herramientas
no era la nuestra. Solo apareció al cambiar el **encuadre** del detector —contar únicamente dentro de
bloques de código—, no al repasar la lista de casos.

> **Corregir un detector por la lista de sus fallos lo deja fallando por el mismo sitio.** Los dos
> `locale` y el `diff` son el mismo defecto, y quitar los dos primeros a mano dejó el tercero vivo y la
> cifra con cara de corregida. **Lo que arregla un detector es cambiar lo que MIRA, no tachar lo que
> encontró.**
>
> **Un detector de invocaciones anclado a principio de línea cuenta prosa envuelta.** Es *el ajuste de
> línea fabrica sintaxis que nadie escribió* aplicado a los comandos en vez de a las listas — el reflow
> no solo crea items numerados: también crea invocaciones.

**Y lo que permitió corregirlo fue el método publicado, no la cifra.** Con el método dentro, su 7 llegó
como **réplica**; sin él habría llegado como contradicción, y se habría discutido cuál número era el
bueno en vez de mirar las dos leyes.

## Ampliar una ley o escribir otra: la prueba es el ENUNCIADO

**Ampliar es siempre más barato que añadir**, y por eso hace falta una prueba: si no, un cuerpo de
reglas deja de crecer **no porque esté completo, sino porque ampliar da menos trabajo** — y eso se ve
igual que la disciplina.

**La prueba, por caso, antes de ampliar:** que alguien que lea **solo el enunciado** diga si habría
reconocido esta situación como cubierta.

```text
PASA   -> el titulo ya era bastante general: el caso cabe, y no hay que tocarlo
NO PASA -> el titulo MIENTE desde el momento en que se anade el caso.
           Entonces: se cambia el titulo, o no se amplia
```

**Y el síntoma débil que hay que evitar es contar títulos intactos.** *"Ninguna de mis ampliaciones
tocó el título"* no dice nada por sí solo: puede ser que los enunciados fueran generales, o que se
estén metiendo casos donde no encajan. **La disyunción solo se resuelve caso por caso**, y por eso la
prueba es cualitativa y no una cifra.

> **Aporte de un adoptante, y nació de cazarnos:** propusimos ese síntoma débil y **en la frase
> siguiente publicamos su positivo como tranquilidad**. Al aplicar la prueba afilada a las tres
> ampliaciones de esa semana, **dos fallaron** y hubo que cambiarles el título.

**Cambiar un título tiene coste y hay que asumirlo, no evitarlo:** toca el índice —la biyección se
comprueba en el mismo cambio— y **cualquier documento ajeno que citara el título viejo queda citando
algo que ya no existe**. Los registros inmutables **no se reescriben**: se anota la equivalencia una
vez, donde vive el índice.

**Y los dos renombres posibles NO cuestan lo mismo, que es lo que decide si la tabla hace falta:**

```text
si el titulo nuevo CONTIENE al viejo (solo se anadio)
   -> grep del viejo encuentra la ley misma. La cita ajena se resuelve sola
si se reescribio la CABEZA
   -> grep del viejo encuentra SOLO la fila de equivalencia. Es el unico puente
```

**Medido sobre los dos de esta instancia:** el que solo añadió da **3 aciertos, dos de ellos la ley
misma**; el que cambió la cabeza da **1, y es la fila de la tabla**. **Si esa fila se borra, toda cita
entregada muere en silencio** — quien la busca no encuentra nada y concluye que la ley se retiró.

> **Y los dos hechos se juntan al revés de lo cómodo: los renombres que más falta hacen son los que
> rompen las citas.** Aquí hubo que reescribir la cabeza **precisamente porque la cabeza era lo que
> engañaba**. Aporte de un adoptante, que midió los cuatro literales.

**Equivalencias de esta instancia** (2026-08-17, tras la primera aplicación de la prueba):

| Título viejo | Título vigente |
| --- | --- |
| *El comando no es el patrón, y confundirlos parece sustrato* | *Ni el comando ni su implementación son el patrón, y confundirlos parece sustrato* |
| *No se pone la medición en el camino: se pone la predicción* | *No se pone la medición en el camino: se pone la predicción, también sobre lo que va a salir bien* |

## Y una segunda cifra, peor que la primera: cuántas leyes tienen DÓNDE engancharse

**Una ley con comando y sin percha está igual de sola que una sin comando** — la formulación es de un
adoptante, y la medición salió de su pregunta: *si el kit solo puede ejercer sus leyes donde ya obliga a
hacer algo, ¿cuántas están enganchadas ahí?*

```text
leyes                              : 59   (medido en b91ce4c)
con PERCHA (citadas desde un paso
            de un ritual, no desde
            el indice)             : 11   (19%)
con comando Y percha               :  6

corpus: core/rituals/*.md + core/templates/*.md + modules/*/templates/*.md,
        EXCLUYENDO la seccion del indice y el propio fichero de leyes.
control 1: sin excluir el indice da 59 de 59 -- el indice las cita todas.
control 2: sin excluir el fichero de leyes da 59 de 59 tambien, por la misma
           razon y con otro culpable: ahi es donde viven los titulos.
```

**El 5 que publicamos antes no se sustituye en silencio: no declaraba su corpus.** Decía *"los rituales
y las plantillas que un paso obligatorio lee"*, que no es una lista — y una cifra cuyo corpus se
describe en prosa **no se puede replicar**, ni siquiera por quien la escribió. La de arriba trae la
lista de globs; **la anterior no es refutable, es irrepetible**.

**Y el denominador está mal, lo corrigió el mismo adoptante que pidió la cifra.** Medir *cuántas tienen
percha* **sobre el total castiga a una clase entera por existir**: las leyes que se aplican **cuando
alguien está a punto de concluir** no tienen paso del que colgar, y eso no es un defecto de colocación
sino su naturaleza. La cifra honesta no es *5 de 55*: es **5 de las que podrían tenerla**, y ese
denominador **nadie lo ha contado todavía** — cuenta como deuda declarada, no como cifra pendiente de
pulir.

> **Un porcentaje cuyo denominador incluye lo que no puede estar en el numerador no mide cobertura:
> mide composición.** Y se lee como cobertura, que es lo que lo hace caro.
>
> **La escalera de lo que dispara**, aportada por el mismo adoptante y ordenada por lo que de verdad
> funciona: **(1)** algo que **se niega a seguir** —un tipo, un parámetro obligatorio, un build que
> falla— donde el ejecutor no participa; **(2)** un comando que **cuelga de un paso obligatorio**;
> **(3)** escarmiento reciente, que caza la tercera repetición y **nunca la primera**; **(4)** memoria,
> que no corre.

**Y la consecuencia dura para un kit que es markdown puro: el nivel 1 le está vedado por construcción.**
Un documento no puede negarse a seguir. **Su techo es el 2**, así que la pregunta de diseño de una ley
nueva no es cómo redactarla sino **de qué paso obligatorio cuelga** — y si no cuelga de ninguno, decirlo.

**El corolario incomoda y es de diseño, no de redacción:** el nivel 1 no se escribe, **se compra
cambiando el medio**. Cuando una regla importe de verdad, la pregunta útil puede no ser *"cómo la
escribo"* sino **"qué artefacto del proyecto puede negarse a seguir sin ella"** — un `.gitignore` no
puede; un test, sí; un parámetro obligatorio, sí.

> **Una ley con comando puede impedir un error; una ley sin comando solo puede explicarlo después.** No
> son la misma herramienta y **no se debe confundir cuál se tiene delante**.

**Y lo que esto NO significa, porque el 83% es fácil de leer mal.** *Sin comando* **no es «inútil»: es
«no ejecutable»**. Hay leyes que **no pueden** tener comando por su naturaleza — *una producción se
puede forzar; una precondición negativa solo se puede declarar* trata precisamente de lo que no se
puede forzar, y ninguna herramienta la va a ejercer. Esas cambian **dónde mira uno**, que es un efecto
real y más lento.

**La convención, entonces, es de honestidad y no de jerarquía:**

- **Si la ley tiene un comando que la ejerce, ESCRÍBELO en su cuerpo**, con su control y su valor
  esperado. No en el fichero de al lado: **junto a la ley**, porque es lo que se lee cuando hace falta.
- **Si no puede tenerlo, dilo y di por qué.** Una ley que se presenta como regla y solo puede funcionar
  como posdata **promete algo que no cumple**, y su incumplimiento se lee como descuido cuando es
  estructura.

**La consecuencia dura, para un kit que es markdown puro y sin runtime:** la mayor parte de lo que aquí
se escribe **no puede impedir nada por sí sola**. Conviene saber cuál parte es — y esa es toda la razón
de esta sección.

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

**Y el contador tiene un hermano que ningún barrido de números encuentra: el SUPERLATIVO.** *"El
archivo más caro"*, *"el único que hace X"*, *"el más largo"* **no llevan cifra**, así que los patrones
de arriba pasan de largo — y sin embargo afirman exactamente lo mismo: **una posición en una lista que
sigue moviéndose**. Un contador caduca cuando la lista crece; un superlativo caduca cuando **cualquier
elemento cambia de tamaño**, que pasa mucho más a menudo y sin tocar la lista.

**Caso propio, y el más barato de reconocer:** un `README` afirmaba que cierto documento era *"el
archivo más caro del kit"*. Era cierto el día que se escribió; después se partió otro fichero en dos y
el afirmado pasó a ser **el tercero**, sin que nadie tocara esa línea ni esa lista tuviera un número en
ningún sitio. **El mismo cambio que lo volvió falso es el que se hizo para mejorar el marco.**

> **Un superlativo es un contador con otra forma: sustitúyelo por la comparación que lo sostiene.**
> *"Pesa más que la instalación entera"* se comprueba con dos números y sigue siendo cierto o
> visiblemente falso; *"el más caro"* exige recorrer una lista que nadie recorre.

Su detector va abajo, con los de contadores, y es igual de ruidoso: sus falsos positivos son
superlativos **sobre cosas que no son listas** (*"el caso más incómodo"*, *"el paso más caro"*), así
que sus aciertos se revisan uno a uno como los demás.

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

# clase 1 — el SUPERLATIVO, hermano del contador y sin cifra que buscar. Afirma una
# posición en una lista y caduca cuando cualquier elemento cambia de tamaño, no solo
# cuando la lista crece.
# EL GRUPO OPCIONAL DEL MEDIO NO SOBRA, y su ausencia rompió la primera versión de
# este detector: el caso que lo motivó es "el ARCHIVO más caro del kit", con un
# sustantivo entre el artículo y el "más", y sin ese grupo daba CERO sobre su propio
# caso de campo. Lo cazó el control positivo, que es exactamente su oficio.
# CONTROLES, con su valor esperado: "el archivo más caro del kit" -> 1;
#   "el más caro de los tres" -> 1;  "esto no lleva superlativo" -> 0.
grep -rniE "\b(el|la|los|las)( [a-zá-úñ]+)? m(a|á)s [a-zá-úñ]+ (de|del) " {base} --include="*.md"
#   ...y para cada acierto, preguntar si el sustantivo es un elemento de una LISTA
#   (fichero, ritual, doc) o de algo que no lo es ("el caso más incómodo"). Solo lo
#   primero es candidato; lo segundo es el ruido esperado.

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

<!-- Estas dos secciones volvieron aqui en la sesion 107. La 104 se las habia llevado a
     core/reference/verificar.md dentro de un bloque sin estructura, y no son leyes
     generales: son procedimiento de ESTE ritual. La prueba es que "los seis de arriba"
     dejo de tener seis arriba. -->

## El séptimo detector: comprobar los documentos contra el mundo

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

## Los detectores frente al idioma: estructurales, léxicos y gramaticales

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

## Lo que cada fase esconde

**Las fases están arriba y no se repiten aquí.** Esto es lo que el imperativo no cabe decir.

**Y esa evidencia se pudre, por construcción y sin remedio posterior.** `archivo:línea` es la forma
correcta **y es un puntero**: el número envejece en cuanto el fichero cambia. Aquí es peor que en
otros sitios porque **se escribe dentro de registros inmutables** —el acta, el informe, la carta
entregada—, así que **no hay dónde corregirla**. Lo aportó un corresponsal con su medida (tres de
seis rangos ya no contenían lo señalado); medido el 2026-08-09 sobre 60 referencias de los registros
inmutables de la instancia que lo halló, **solo el 42% resolvía todavía**. Y las que fallaban lo
hacían por **tres causas distintas**, que conviene no mezclar:

| Causa | Qué pasó | ¿Evitable al escribir? |
| --- | --- | --- |
| **Podrida** (22%) | El fichero está, la línea ya no | Solo en parte: anclar en el identificador |
| **Renombrada** (7%) | El fichero cambió de nombre | Ya lo cubre la nota de equivalencia del índice |
| **Sin raíz** (30%) | La referencia **nunca dijo de quién era el fichero** | **Sí, entera** |

<!-- OJO A ESE 30%: es del test INTUITIVO, que sobre-marca. Con el test correcto -unas lineas mas
        abajo- la misma categoria queda en 10 de 62 (16%). Las dos cifras son de la misma medicion de
        2026-08-09 y NO CUADRAN entre si: el denominador aparece como 60 aqui y como 62 alli. No se
        arbitra cual vale porque el corpus de entonces ya no existe; lo que vale es el TEST, que si
        esta explicado. Detectado por la auditoria 10 y anotado en vez de inventado. -->

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
**único** en el árbol y resuelve solo. Con el test correcto quedaron **10 de 62 (16%)** —medido el
2026-08-09 sobre los registros de la instancia que lo halló, que hoy son más de 100: **es una foto,
no un ratio vigente**—, y las diez
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

### Pero el alcance es el TECHO, y ningún número de pasadas lo sube

**Una pasada relee lo que la pasada anterior decidió mirar.** Por eso convergen: no porque el corpus
esté limpio, sino porque **el conjunto que se está releyendo se agota**. Una auditoría que termina
—incluso tras varias pasadas y con la última en blanco— solo ha demostrado que **dentro de su alcance**
no queda nada, y el alcance lo eligió quien audita, antes de mirar y con la información que tenía
entonces.

**Caso de campo, y es el que hace la regla:** un proyecto corrió **siete pasadas** sobre un alcance que
**incluía** el fichero donde vivía el defecto, y ninguna lo produjo. Lo produjo **una pregunta de un
corresponsal**, leyendo una carta. El fichero estaba dentro; lo que faltaba era la pregunta.

> **Un corresponsal no es una cortesía: es el único detector que no comparte tu alcance.** Todo lo
> demás que corres —los barridos, las pasadas, los controles— lo escribiste tú, con tus supuestos sobre
> dónde puede haber algo.

**Y de ahí lo que hay que escribir en el informe y casi nunca se escribe:** *qué quedó fuera*. No como
disculpa —una auditoría acotada es lo correcto— sino porque **la lista de lo excluido es el único sitio
donde un lector puede ver el techo**. Un informe que solo dice lo que miró se lee como si hubiera
mirado todo.

**Cuidado con el falso consuelo de las pasadas.** Que la segunda pasada produzca hallazgos es buena
señal de la pasada y **no dice nada del alcance**: los dos números crecen por motivos distintos, y
confundirlos convierte *"nuestro método funciona"* en la conclusión de un experimento que no se hizo.

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
