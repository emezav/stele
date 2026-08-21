<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

# Ritual: BOOTSTRAP (instanciar el marco en un proyecto)

**Modo:** *greenfield* (no hay docs → scaffold) o *adopción* (ya existen → mapear a roles sin
sobrescribir contenido; solo generar lo que falte).

## Lo mínimo para ejecutarlo

> **Este ritual lo ejecuta un agente que acaba de conocer el marco, una sola vez y sin ningún
> precedente que copiar.** Por eso el bloque va aquí arriba: en los demás rituales ahorra lectura; en
> este es lo único que hay.

**Antes de nada: no pares a preguntar.** El usuario pidió instalar; **instalar es el ritual entero, no
traer el kit.** Lo que se decide aquí tiene defaults, y lo que no los tiene **se declara en el eco**,
donde el usuario lo ve y lo corrige si quiere. **Un bootstrap que termina pidiendo una frase mágica no
terminó.**

**Los diez pasos:**

1. **Resolver** `idioma`/`módulos`/`persistencia` y las tres rutas `kit`/`base`/`loader`, con defaults.
   `persistencia = git` solo si hay `.git` **en la raíz** — uno anidado deja la raíz sin versionar.
2. **Eco del layout ANTES de escribir nada**, siempre, incluso si no preguntaste nada.
3. **Validar los invariantes de ruta** (`core/reference/rutas-y-tokens.md` → *Las tres rutas*).
4. **Resolver nombres** por rol y módulo. Los roles de un módulo inactivo van desactivados, no fuera.
5. **Escribir `stele.config.md`** en la raíz.
6. **Instanciar cada plantilla** por rol bajo `base`, resolviendo los tokens.
7. **Sembrar** `state` y `handover` (`SIN_TRABAJO_ACTIVO`), `index` vacío — **con la FORMA VACÍA que
    traen las dos plantillas**, no con una sesión inventada. *Ninguna cerrada todavía* es el estado
    real de todo proyecto recién instalado, y es el único que ninguna plantilla decía.
8. **Generar UNA PUERTA POR CADA NOMBRE de `loader`**, en la raíz. `loader` es una **lista**, no un
   archivo.
9. **Validar** (ritual CONFIG, fase 5).
10. **Confirmar y COMPROBAR**: pídele que reabra el editor y te salude, **y dile de antemano qué verá
    si falla**. Este paso no afirma un resultado: lo comprueba.

**Las cinco que se saltan.** Cada una tiene su cadáver en el cuerpo:

| Situación | La regla |
| --- | --- |
| Vas a preguntar el módulo de producto | **No se olfatea y tampoco se bloquea:** se resuelve con criterio y **se declara en el eco**. Medido: la instrucción de *preguntar* se ignoró dos veces de dos, porque **preguntar se percibe como un coste** |
| Vas a escribir una puerta | Se escriben **todas** las de la lista. Una fila nueva entra como **testimonio**, y solo pasa a **corrida** cuando alguien reabre con ese agente y **le saludan sin pedirlo** |
| Vas a decirle algo al usuario | **En sus palabras, no en las del marco.** `base` es *dónde te dejo los documentos*; `kit`, `loader` y *greenfield* **no se preguntan**: son detalles internos que resuelves tú |
| Vas a sembrar `state` y `handover` | **No inventes una sesión anterior.** Usa la forma vacía de la plantilla: *ninguna cerrada todavía*. Es el estado en que nace **todo** bootstrap, y estuvo sin escribir hasta que dos agentes de campo lo improvisaron bien — que es como un hueco se queda abierto |
| Vas a dar el paso 10 por hecho | **El saludo es el único observable que distingue un marco activo de uno apagado**, y quien lo ve primero es la persona: tú ya tienes el contexto y no puedes notar su falta |

**Pasos:**

1. Elegir `idioma`/`módulos`/`persistencia` y las **tres rutas** (`kit`/`base`/`loader`) con
   defaults sensatos. Auto-detectar `persistencia = git` si hay `.git` **en la raíz del proyecto** —
   no vale uno anidado en un subdirectorio, que deja la raíz sin versionar —, si no `ninguna`
   (avisando de la consecuencia).

   **`loader` no es un archivo: es una LISTA DE PUERTAS**, y se escriben **todas**. Cada puerta lleva
   el nombre exacto que auto-carga un harness, y todas apuntan al mismo `entry`.

   | Puerta | La auto-carga | Cómo se sabe |
   | --- | --- | --- |
   | `CLAUDE.md` | Claude Code | **corrida** — saludó al reabrir, aquí y en un adoptante |
   | `AGENTS.md` | Codex y la mayoría de los demás | **testimonio** — nadie ha reportado una corrida |
   | **la tuya** | **añádela si tu harness lee otro nombre** | la tuya entra como **testimonio** |

   **La tercera columna no es adorno: es lo que hace falsable la fila.** Un nombre dicho no se puede
   comprobar desde el kit, y una fila equivocada es **peor que ninguna** — hace que el bootstrap
   escriba con confianza una puerta que nadie lee. Por eso una fila nueva entra como **testimonio**, y
   solo pasa a **corrida** cuando alguien reabre la carpeta con ese agente y **le saluda con la última
   sesión sin pedírselo**. Si una corrida con ese agente no saluda, **la fila se cae**.

   Y mira cuál es la fila floja: la ancha. *"La mayoría de los demás"* es la afirmación **menos
   verificada y más amplia** de la tabla, y es justo la que nadie va a dudar.

   **La tabla es necesaria y no suficiente, y el segundo eje lo aportó el campo.** El nombre lo impone
   el harness, pero **la ubicación la puede cambiar el proyecto**: un adoptante sacó `AGENTS.md` de la
   raíz a propósito, y su agente siguió funcionando porque la puerta que él lee lo importa desde ahí.
   **Cualquier otro que auto-cargue `AGENTS.md` de la raíz allí no encuentra nada — y no lo encuentra
   en silencio.** Así que hay dos preguntas y no una: *¿tiene la puerta el nombre que lee tu agente?* y
   **¿sigue en el sitio donde ese agente la busca?** Un proyecto puede haberla movido por razones
   buenas y entonces el nombre correcto apunta a un hueco.

   **Escribe las dos que el kit conoce Y la tuya si es distinta.** Las dos mitades hacen falta y
   ninguna basta sola:

   - *Solo la tabla* deja al kit con un registro del mundo exterior que **no puede verificar y que
     envejece**: cada harness nuevo la deja corta.
   - *Solo tu introspección* **reproduce el fallo**, y esto es campo y no conjetura: en cuatro
     instalaciones observadas con el mismo agente, **acertó su propio nombre dos veces de tres**.

   **Sumarlas es lo que hace que equivocarse deje de ser fatal:** si yerras con la tuya, otra puerta
   sigue abierta.

   **Caso de campo, el más caro de los observados:** un agente que no lee `CLAUDE.md` instaló el marco
   entero correctamente —manifiesto, docs, bloque de arranque, todo bien— y escribió **una sola**
   puerta, con ese nombre. Al reabrir la carpeta **no arrancó**: saludó en genérico, sin sesión, sin
   estado y sin próximo paso, y se ofreció a rehacer trabajo ya terminado. **Nada falla, no hay error,
   y el proyecto queda con la documentación perfecta y el mecanismo apagado.**

   **El módulo de producto NO se auto-detecta: se RESUELVE con criterio y se DECLARA en el eco**, y
   son tres respuestas. El criterio es
   *"¿este proyecto va a tener un **producto con estructura propia y decisiones que convenga registrar
   por tema**?"*, y se ilustra con ejemplos porque en abstracto no se puede contestar:

   > *Una tesis con su corpus, un codebase, un kit de plantillas: **sí**. Un cuaderno de notas
   > sueltas o una bitácora personal: **no**. Y si acabas de crear la carpeta y todavía no lo sabes:
   > **todavía no**, que es una respuesta legítima y no un aplazamiento.*

   | Respuesta | Qué se escribe |
   | --- | --- |
   | sí | `módulos = producto` |
   | no | `módulos = —` |
   | todavía no | `módulos = pendiente` |

   **Por qué no se olfatea.** El criterio es de dominio general y el detector que había buscaba
   `Cargo.toml`/`package.json`/`src/` — o sea, un compilador, que es justo lo que el criterio dice que
   **no** importa. Y en greenfield cualquier olfateo es inútil por construcción: **corre en el único
   momento en que la evidencia todavía no puede existir**. Dos proyectos reales se quedaron sin
   `specs` ni `architecture` por eso, y uno de ellos organizó su corpus en cuatro carpetas que no
   quedaron escritas en ningún doc.

   **`pendiente` no es lo mismo que `—`, y esa es toda su razón de ser.** `—` dice *"se decidió que
   no"*; `pendiente` dice *"falta decidirlo"*. Si los dos casos escribieran lo mismo, nadie volvería a
   mirar nunca. Vive en el manifiesto, que **no se poda**, y no en los pendientes de `state`, que se
   reescriben en cada cierre — ahí ya murió una trampa real de campo.

   Quien vuelve a preguntar es **AUDITAR**, no ABRIR: que el manifiesto diga `pendiente` mientras el
   árbol ya tiene un producto es drift de clase 1 de manual, y comprobarlo cada ~10 sesiones no cuesta
   nada. ABRIR se mantiene en su tamaño.

   **Zero-question sigue siendo posible**, y esto es un cambio de opinión con dato detrás. Se probó a
   exigir que se preguntara; un agente leyó la regla, se quedó con *"no se auto-detecta"*, descartó
   *"se pregunta"* y resolvió solo — y encima acertó, y lo reportó como virtud: *"no se interrumpió al
   usuario con preguntas"*. **Preguntar se percibe como un coste, y por eso la instrucción se cae.**
   Así que la decisión no se bloquea: se **declara en el eco** del paso 2, donde el usuario la ve
   antes de que se escriba nada y puede corregirla. Si calla, tu resolución vale.
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
   puertas      -> CLAUDE.md, AGENTS.md  (una por harness; añade la tuya si lee otro nombre)
   persistencia -> git        (cómo se guarda el trabajo al cerrar)
   módulos      -> pendiente  (¿hay producto con estructura? sí / no / todavía no)
   ```

   Coste cero y ataja la mala interpretación antes del scaffold, no después. Si el kit ya se
   vendorizó en la ruta equivocada, moverlo aquí es trivial; después no.

   **La fila de `módulos` va aquí a propósito, y sustituye a pedir que preguntes.** Se observó tres
   veces que este eco **sí** se emite, y dos veces que una instrucción de *preguntar* se ignora: un
   agente leyó *"no se auto-detecta: se PREGUNTA"*, se quedó con la mitad negativa y resolvió solo.
   Poner la decisión en un paso que ya se cumple la vuelve **visible y corregible** sin depender de
   que alguien se acuerde de preguntar. Si el usuario no dice nada, tu resolución vale.
3. Validar los **invariantes de ruta** (ver `core/reference/rutas-y-tokens.md` → "Las tres rutas";
   **no `guide.md`**, que aquí no hace falta y pesa más que toda la instalación). Violación = abortar y
   re-preguntar.
4. Resolver nombres (defaults de rol + módulo; override libre). **Los roles de un módulo inactivo se
   escriben igual, con el centinela `—` (o `---`) en la columna Archivo — no se omiten.** Omitir una
   fila significa *"déjala en su default"*, que es lo contrario de *"está apagada"*, y las dos se ven
   igual desde fuera. Lo mismo con las features del módulo: van a `off`, no encendidas. Ocurrió en
   campo un `effort_log = on` con el módulo inactivo y sin `esfuerzo.md` en ninguna parte.
5. Escribir `stele.config.md` en la raíz (plantilla `core/templates/config.md`), con la sección
   Rutas ya resuelta y **`kit_origen` anotado**: la URL o ruta de la que acabas de traer el kit. Es
   el único momento en que se sabe con certeza, y sin ella ACTUALIZAR no puede correr después.
6. Scaffold: instanciar cada template por rol → nombre configurado bajo `base`, **resolviendo
   tokens** (incluido `{{kit}}`). En adopción, saltar los docs que ya existen.
7. Semilla: `state` y `handover` (`SIN_TRABAJO_ACTIVO`) iniciales, `index` vacío, y **`effort` vacío si
   `effort_log = on`** — si el toggle está encendido y el fichero no existe, el primer cierre escribe
   una fila en un archivo que nadie creó. **`audit` no se instancia**: lo crea la primera auditoría, y
   su ausencia es el dato (nunca se auditó).

   **Y aquí se decide si `base` viaja o no, que es lo que decide si existe.** Si el proyecto usa un VCS
   y `base` **no** se versiona —opción legítima y la que toma el propio kit—, la línea que lo ignora
   **no se escribe sola**: va con **la declaración de qué respaldo tiene**, en el mismo comentario.

   ```text
   # La documentacion de trabajo NO se versiona: vive solo en disco.
   # RESPALDO: <ninguno | repo propio con remoto en X | copia periodica a Y>
   # Sin respaldo declarado, esta linea deja el trabajo del proyecto sin red.
   base/
   ```

   **Las dos mitades van juntas o no van.** Ignorar `base` es correcto y no se discute; lo que no puede
   quedarse sin decidir es dónde está su copia — y el sitio donde se decide es **la línea que lo
   causa**, no un aviso en otro documento. Un aviso está *a la vista*; esto está *en el camino*, porque
   no hay forma de escribir la exclusión sin escribir al lado su consecuencia (ver CERRAR → *a la vista*
   contra *en el camino*).

   **Y esa línea cuesta DOS cosas, no una.** La segunda no se ve y muerde a diario: **el buscador de tu
   agente respeta el `.gitignore`**, así que a partir de aquí **no encuentra ni una palabra de los
   documentos que él mismo carga enteros al arrancar**. Un barrido sobre tu propia documentación
   devuelve **cero**, sin error y sin aviso, y ese cero es indistinguible de un corpus limpio. Se
   trabaja con `grep` de shell o con la bandera que incluye ignorados; el buscador integrado queda para
   el código, que sí está versionado. La ley y su caso, en
   `{{kit}}/core/reference/verificar.md` → *Un directorio ignorado es invisible a tu buscador y visible
   en tu contexto*.

   **`RESPALDO: ninguno` es una respuesta válida** y a menudo la correcta al principio. Lo que no es
   válido es **no responder**: una ventana sin red declarada se puede vigilar, y una que nadie escribió
   solo se descubre cuando ya se perdió algo.

   **Y es un TOKEN con valor enumerado, no una frase — la diferencia es el remedio entero.** Caso de
   campo, de un proyecto que aplicó esta convención a un `.gitignore` que ya existía: la línea **no
   estaba vacía**, decía *"su respaldo es aparte del de este repo"*. Eso **no declara nada** —no dice
   dónde, ni quién, ni cada cuánto— y sin embargo **suena a decisión ya tomada**, así que nadie la
   comprobó en **39 sesiones**. No había ninguna copia, en ninguna parte.

   > **Un hueco se ve; una frase tranquilizadora, no.** Y las dos producen el mismo estado del mundo:
   > cero copias. Si el remedio fuera *"explica en el comentario qué respaldo tienes"*, ese `.gitignore`
   > **ya cumplía**.

   **`RESPALDO: ninguno` funciona porque es incómodo de escribir**: obliga a elegir un valor de una
   lista corta y a mirar si es verdad. La prosa libre es cómoda, y por eso sobrevive sin que nadie la
   lea.

   > **Por qué esto es declarativo y no ejecutivo, a propósito.** Sería fácil hacer que el ritual
   > corriera un `git init`, y sería un error: el kit es **markdown puro y sin runtime**, y
   > `persistencia` admite `ninguna` y `comando`. Un ritual que ejecuta operaciones de VCS deja de ser
   > texto y amarra el marco a una herramienta concreta. **La obligación es escribir la decisión, no
   > tomarla por el usuario.**

   **Caso de campo, y es de la peor clase — la que no da error.** Un proyecto recibió el marco de otro
   (ver *siembra*, abajo) y heredó su `.gitignore` con `base/` dentro. Durante horas, `base/` fue **lo
   único que existía en el proyecto**: doce documentos, ningún repositorio todavía, ninguna copia — y
   una importación masiva de otro árbol prevista encima. La convención heredada era sana, la práctica de
   *documentación primero* también, y **juntas producen una ventana en la que el proyecto entero cabe en
   un directorio sin red**. Nadie se equivocó en ningún paso.
8. Generar derivados: **una puerta por cada nombre de la ruta `loader`**, en la raíz (plantilla
   `autostart.md`) + mapa-doc en `entry`. **Si alguna puerta ya existe** (`CLAUDE.md`, `AGENTS.md`…
   escritos a mano antes de adoptar el marco), **léela primero e inserta** el bloque
   `STELE:INICIO`/`STELE:FIN` conservando todo lo demás — invariante 6. Igual que en adopción con
   cualquier otro doc: nunca reemplazar contenido que no escribiste.
   **Y con varias puertas ese invariante tiene varias superficies donde fallar: compruébalo EN CADA
   UNA.** Es la regla que ya destruyó un `CLAUDE.md` real cuando solo había una.
   **Emite la marca pelada, siempre.** El bloque queda protegido por default y tú no tienes que
   decidir nada: no escribas `LIMPIO` — esa marca la escribe quien haya comprobado con un diff que el
   bloque no dice nada que la plantilla no diga, y aquí acabas de instanciarlo, no de compararlo.
   **Los comentarios `STELE:*` conviene conservarlos, pero nada depende de que lo hagas.** Se observó
   tres veces que el bloque instanciado sale con la marca pelada y sin su comentario — la tercera con
   una instrucción explícita de conservarlo delante. **Y está bien: la protección es el default, no el
   comentario.** Lo único que se pierde es que quien lea su loader no descubra la marca `LIMPIO`, y eso
   no rompe nada. El comentario `PLANTILLA` de la cabecera sí sobra siempre: habla del kit, no del
   proyecto.
9. Validar (ver ritual CONFIG, fase 5).
10. **Confirmar + COMPROBAR.** Dile al usuario que **reabra el editor y te salude**, y qué tiene que
    ver:

    > *"Reabre el editor y escríbeme cualquier cosa. Debería contestarte con la última sesión y el
    > próximo paso, sin que se lo pidas. **Si te contesta en genérico, ninguna puerta tiene el nombre
    > que lee tu agente**: dímelo y la añado."*

    **Este paso decía antes "reabrir el editor → el loader se auto-carga", y eso AFIRMA un resultado
    que nadie mira.** Era la única frase del ritual sobre algo que no se comprueba, y es exactamente
    donde un fallo de campo pasó desapercibido: el marco quedó instalado perfecto y mudo, sin error.
    **El saludo es el único observable que distingue un marco activo de uno apagado**, y quien lo ve
    primero es la persona, no tú — tú ya tienes el contexto en esta sesión y no puedes notar su falta.
    Por eso el síntoma va **dicho de antemano**: sin él, un saludo genérico se lee como cortesía.

## Siembra: cuando el marco lo instala OTRO proyecto, desde fuera

Los diez pasos de arriba se leen **desde dentro** del proyecto que arranca. Hay un caso que no es ese y
que ocurre en campo: **un proyecto que ya usa el marco le monta la bitácora a un proyecto hermano**, con
su código y sus trampas delante, antes de que el hermano tenga una línea de código. El hermano no
ejecuta BOOTSTRAP: **se lo ejecutan**.

No es un modo distinto ni cambia los pasos. Lo que cambia es que **cuatro campos suponen un pasado que
todavía no existe**, y quien siembra los tiene que resolver solo si esto no está escrito:

| Campo | Qué pasa al sembrar | Qué se escribe |
| --- | --- | --- |
| `persistencia` | El proyecto **va a** usar VCS, pero el repositorio aún no existe | El valor **de destino** (`git`), y en `state` **el primer paso de la sesión 1**: crear el repositorio. No se pone `ninguna` para "arreglarlo" luego: se documenta la ventana |
| `Sello` del `handover` | Sus dos formas suponen trabajo a medias; aquí no hay trabajo a medias, hay un proyecto que no empieza | Su tercera forma: **qué ficheros existen ya y no deben pisarse**. Es lo que el que retoma necesita, y aquí *"por dónde ibas"* no significa nada |
| Saludo de ABRIR (`N + título`) | No hay sesión previa ni acta que citar | *"Ninguna: el marco lo sembró `<proyecto>` el `<fecha>`"*, **y por qué**. El saludo sigue siendo el observable de que el marco arrancó, así que **no se omite** |
| `index` y `effort` | Nacen con **cero filas**, y no por ser nuevos | Una línea dentro de cada uno diciendo que **el trabajo que los llenaría ocurrió en otro proyecto**. Sin ella, un lector los toma por un proyecto que no ha hecho nada |

**El respaldo de `base` es más agudo aquí que en cualquier otra adopción, y por eso vale releer el paso
7.** Al sembrar, `base` es **lo primero que existe** — durante horas es lo **único** que existe. Si
además se ignora y el repositorio todavía no está, el proyecto entero vive sin red justo cuando va a
recibir una importación masiva de otro árbol encima. **Declara el respaldo en la misma línea que la
exclusión.**

**Y una obligación que solo existe al sembrar: DECLARA QUÉ VERSIÓN DEL KIT ESTÁS COPIANDO, en el
momento de copiarla.** El identificador va al `state` del proyecto sembrado, no a la memoria de quien
siembra: **entre la copia y el momento de contarlo puede haber trabajo**, y entonces se cuenta el
identificador de *ahora* en vez del de *entonces*. Ocurrió en campo y lo destapó el receptor con dos
horas: se le dijo que tenía una versión y tenía la anterior, por once minutos.

**Importa más de lo que parece cuando el proyecto sembrado es el sujeto de algo.** Si alguien va a
observar cómo se comporta ese proyecto —y sembrarlo suele venir con esa intención—, **el kit que lleva
es la variable independiente**, y sin su identificador ningún resultado se puede atribuir. La ley, en
`{{kit}}/core/reference/verificar.md` → *El identificador de una acción es el del instante en que
ocurrió, no el de cuando la cuentas*.

**Lo que NO cambia, y conviene decirlo:** los invariantes de ruta, la protección de las puertas
existentes y el eco del layout antes de escribir. Sembrar no relaja ninguna comprobación; **relaja la
suposición de que hay alguien dentro con historia.**

> **Y una advertencia sobre lo que la siembra no demuestra.** Que la bitácora quede escrita no dice que
> el marco vaya a arrancar allí: eso lo dice **la primera sesión del hermano**, con su saludo. Hasta que
> alguien la abra, lo sembrado es una hipótesis bien redactada — el mismo paso 10, aplicado a un
> proyecto que todavía no ha hablado.

*(Caso de campo aportado por un proyecto que sembró a su hermano y escribió antes de saber si funciona,
que es la parte difícil de reportar.)*
