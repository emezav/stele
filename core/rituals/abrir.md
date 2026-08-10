<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

# Ritual: ABRIR sesión (ponerse al día, barato)

Lee, en orden, SOLO la **lista de arranque** del proyecto (generada; con defaults del módulo de producto):

1. `entry` · 2. `gotchas` · 3. `state` · 4. `handover` — **si su Estado ≠ `SIN_TRABAJO_ACTIVO`**,
respétalo **y compruébalo** antes de editar (abajo). Bajo demanda (grep): `charter` (1ª vez /
orientación), `protocol`, `specs`, `architecture`, `index`/`session`.

**El `handover` es el único doc del set que puede estar caducado, y hay que comprobarlo.** Se escribe
**antes** de la primera edición y no se vuelve a tocar hasta el cierre: en esa ventana su contenido es
**pasado presentado en presente**, y si la sesión murió ahí, miente. El `state` no tiene el problema
—se reescribe al cerrar, así que está al día o no existe.

Con VCS cuesta un comando: **compara su `Sello` con el `HEAD` de ahora** (`git log -1`). Si no
coinciden, hubo trabajo después de escribirlo: **manda el árbol, no el documento** — lee el diff y
dilo al usuario en el saludo, en vez de proponerle trabajo ya hecho. Si ese `handover` no tiene campo
`Sello` (se instanció antes de que existiera), la comprobación equivalente es mirar si hay commits
posteriores al último cierre; añádele el sello la próxima vez que abras un checkpoint.

**El síntoma, para reconocerlo sin comando:** si el `handover` te propone construir algo que **ya
está construido**, caducó. Caso de campo, y de manual: un agente arrancó por una puerta del marco y
propuso *"convertir los dos archivos de la raíz en puertas delgadas"* — leyendo el proyecto **a través
de las dos puertas delgadas ya construidas**. El sello que lo destapaba estaba escrito en el propio
documento; nadie lo había mandado mirar. **Un doc que afirma en vez de comprobar falla en silencio, y
el saludo sale igual de convincente.**

**Sin VCS esto no aplica**, y conviene saber por qué: no hay árbol que consultar, así que el
`handover` es la única fuente de qué quedó a medias y no se le puede pedir que se contraste contra
nada. Ahí lo que sustituye al sello es que el propio documento diga **qué se observa en disco** para
saber por dónde iba.

**Confirma el arranque (visible):** un agente **no puede hablar antes de que el usuario escriba**,
así que la confirmación va **al frente de tu PRIMERA respuesta** — 1-3 líneas: última sesión
(N + título), si quedó trabajo a medias, próximo paso propuesto. En llano, nombrando los archivos
(ver "Cómo se le habla al usuario"). Sin esto, el arranque silencioso es
indistinguible de uno que no corrió. (Se omite si `session_greeting = off`.)
