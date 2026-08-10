<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

# Ritual: BOOTSTRAP (instanciar el marco en un proyecto)

**Modo:** *greenfield* (no hay docs → scaffold) o *adopción* (ya existen → mapear a roles sin
sobrescribir contenido; solo generar lo que falte). Pasos:

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
   **no `guide.md`**, que aquí no hace falta y es el archivo más caro del kit). Violación = abortar y
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
7. Semilla: `state` y `handover` (`SIN_TRABAJO_ACTIVO`) iniciales, `index` vacío. **`audit` no se
   instancia**: lo crea la primera auditoría, y su ausencia es el dato (nunca se auditó).
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
