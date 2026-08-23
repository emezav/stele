<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

# Ritual: CONFIG (adaptar nombres/parámetros — único renombrador sancionado)

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

   **Y el barrido tiene un LÍMITE que hay que escribir, porque sin él se come el registro.** No se
   barren `session`, `index` ni la correspondencia **recibida**: son inmutables y mencionan los
   nombres viejos **a propósito** — corregirlos falsea el registro, y además destruye la única
   prueba de por qué se renombró. La equivalencia vieja→nueva **se anota una vez, en la cabecera de
   `index`**, y ahí se acaba. Lo que se barre es lo **vivo**: los docs de rol, el `loader`, el
   manifiesto.
   **Es la frontera entre reescribir y anclar, y no es un matiz: es la misma pregunta con dos
   respuestas legítimas según de qué lado caiga el fichero.** Un adoptante la resolvió de las dos
   maneras opuestas **el mismo día** —barrió sus actas al renombrar y las ancló al planear una
   promoción— y las dos veces con argumento; lo que no encontró fue la regla, porque no estaba
   escrita aquí. **Barrer lo vivo, anclar lo inmutable.**

   **Y el acotado del barrido es en las DOS direcciones**, que es donde se falla:

   | Se pasa… | Síntoma | Guarda |
   | --- | --- | --- |
   | **hacia dentro** | el `sed` alcanza el doc que describe el renombrado y lo deja diciendo *"de `x` a `x`"* | excluye ese fichero, o escribe las equivalencias **después** |
   | **hacia fuera** | el `sed -i` corre sobre todos los `.md` versionados y toca el producto | acota la lista de ficheros **antes**, no el patrón |
   | **corto** | un rol **creado en la misma operación** no está en el mapa viejo→nuevo | los añadidos entran en la lista de renombrado, no solo los movidos |

   **Los tres son el mismo defecto:** el reemplazo se pensó como *qué cambiar* y no como **dónde
   puede aterrizar**. Los tres, medidos en la misma sesión de campo.

   > **En Windows el renombrado por MAYÚSCULAS no llega al índice de git.** El disco es insensible
   > al *casing* y los repositorios suelen llevar `core.ignorecase=true`, así que `mv X.md x.md` no
   > hace nada y `git` lo reporta como *modificado* **conservando el nombre viejo**. Hay que pasar
   > por un nombre temporal, o usar `git mv` en dos pasos. **Esto alcanza a todo adoptante en
   > Windows que actualice desde antes de `4e73392`**, que es cuando este kit bajó sus propios
   > documentos a minúscula: el fichero parece renombrado en disco y no lo está en el repositorio.
5. **Validar**: `grep` del nombre (o ruta) viejo = 0; cada nombre resuelve a un archivo; ningún rol
   activo apunta a faltante; los invariantes de ruta se cumplen.

   **Y si el cambio tocó `persistencia` o la raíz, se re-comprueba lo que BOOTSTRAP comprobó una vez:
   `git` solo vale si hay `.git` en la RAÍZ** — uno anidado en un subdirectorio deja la raíz sin
   versionar, y entonces el manifiesto declara `git` mientras el cierre cubre una fracción del árbol.
   **Nada falla:** los commits salen bien, el `state` se escribe, y lo que queda fuera no da señal
   ninguna. Lo trajo un adoptante que iba a promover su raíz: la comprobación existía, pero **solo se
   leía en el ritual que se ejecuta una vez en la vida del proyecto**. Es la misma ley de *barre por
   RUTA, no solo por texto*: tres rituales tocan `persistencia` —BOOTSTRAP, CONFIG y CERRAR— y solo
   el primero la validaba.

   **Y si el cambio tocó una puerta o el `entry`, esa validación no alcanza: es toda textual.** Un
   nombre de puerta equivocado pasa las cuatro comprobaciones de arriba —resuelve a un archivo, no
   rompe ningún invariante— y deja el marco **instalado y mudo**. Acaba como BOOTSTRAP (paso 10),
   pidiendo el único observable que hay:

   > *"Reabre el editor y escríbeme cualquier cosa. Debería contestarte con la última sesión y el
   > próximo paso. **Si te contesta en genérico, la puerta no tiene el nombre que lee tu agente**:
   > dímelo."*

   Al **quitar** una puerta el síntoma es el contrario y también se dice: *si el agente que la leía
   deja de saludarte, era la suya.*

Reglas fijas: desactivar un módulo **no** borra sus docs (huérfanos preservados + aviso); colisión
de nombre aborta; cambiar el patrón `session` afecta solo sesiones futuras (el historial es inmutable).

**Cambiar `idioma` no es solo cambiar una celda, y es el caso que más fácil se queda a medias.** Si el
proyecto tenía detectores derivados en la sección *Detectores de auditoría* de `protocol`, quedan en el
idioma anterior. **No los traduzcas** —traducir término a término produce detectores malos, y los
gramaticales no tienen equivalente uno a uno—: **márcalos como caducados y dilo**, que la siguiente
auditoría los vuelva a derivar. Un detector en el idioma que ya no se escribe **no da error: da cero**,
y ese cero se lee como *"corpus limpio"*.

**Activar un módulo, en concreto** — y esto es tan procedimiento como desactivarlo:

1. **Añadir sus filas a *Nombres*** con `Origen` = el módulo, resolviendo el nombre de cada rol
   (default del módulo salvo que el usuario pida otro). Las filas que estaban en `—` dejan de estarlo.
2. **Instanciar los docs que faltan** desde `modules/<mód>/templates/`, con los tokens resueltos.
   **Nunca sobrescribir uno que ya exista**: si el módulo se desactivó antes, sus docs quedaron
   huérfanos y siguen ahí con contenido real — se **readoptan**, no se recrean.
3. **Crear sus contenedores** solo si el módulo los declara y su regla no dice "nace del uso".
4. **Encender sus features** (p. ej. `effort_log`) y aplicar su `checkpoint_trigger` si lo especializa.
5. **Regenerar la lista de arranque y el mapa-doc** si el módulo aporta algún rol `obligatorio` o
   algún `trigger` — portando el delta a mano al bloque protegido del loader y del `entry`.
6. **Avisar de lo que NO se hizo:** los docs nuevos nacen vacíos, y un doc vacío no es lo mismo que un
   doc que no existe. Decirle al usuario cuáles son y que hay que llenarlos.

**La asimetría era real y estaba escrita al revés.** Durante mucho tiempo aquí solo se explicaba cómo
**desactivar**, porque desactivar da miedo —borra filas, deja huérfanos— y activar parecía trivial. No
lo es: activar toca el manifiesto, el disco, la lista de arranque y dos bloques generados. Y con la
respuesta `pendiente` de BOOTSTRAP, **activar deja de ser el caso raro y pasa a ser la carretera
principal**. La dirección peligrosa se llevó las reglas y la común se quedó sin ninguna.

**Cambios de ruta, en concreto:**

- Mover `kit`: mover el directorio y barrer las referencias `{{kit}}` ya resueltas en los docs
  instanciados (`entry`, `protocol`, `loader`). No toca ningún doc de contenido.
- Mover `base`: mover los docs de rol (y `history_dir` completo, con su historial) y regenerar el
  loader, cuyos `@import` son relativos a la raíz. El historial se mueve entero, no se reescribe.
- **Mover la RAÍZ** (una *promoción*: el proyecto pasa a colgar de un directorio superior) es el caso
  que no está en la tabla del manifiesto, porque **la raíz no es un valor configurado**: es el origen
  del sistema de coordenadas, el CWD con que se abre el agente. Por eso no se persiste —igual que el
  layout— y por eso **ningún ritual la cambiaba**. Detalle abajo.
- Cambiar `loader` es **añadir o quitar puertas**, porque el valor es una lista.
  **Añadir** una: insertar el bloque en ese archivo —creándolo o modificándolo, invariante 6— con el
  mismo contenido que las demás. **Varias puertas activas NO compiten: es el diseño.** Todas llevan la
  misma lista de lectura y apuntan al mismo `entry`; lo que estaría mal es que **dijeran cosas
  distintas**, y eso lo evita portarles el mismo delta a todas.
  **Quitar** una: retirar su bloque, **nunca borrar el archivo a ciegas** — puede tener contenido del
  usuario. Si al quitar el bloque no queda nada más, entonces sí se borra; si queda algo, se conserva
  y se avisa.
  Verificar antes que ningún nombre nuevo colisiona con un rol bajo `base`.
  **Y avisar de lo que quitar implica:** cerrar una puerta deja mudo al agente que la lee. Si el
  usuario trabaja con dos y se cierra una, lo notará la próxima vez que abra con ese — y lo notará
  como un saludo genérico, no como un error.

## Mover la raíz (promoción), el cambio de ruta que no es un valor

**Qué es.** El proyecto pasa a colgar de un directorio superior: lo que era la raíz queda un nivel
abajo y el marco sube con ella. Ocurre cuando el subdirectorio que se adoptó resulta ser **uno de
varios procesos** del trabajo real, y la documentación tiene que cubrir el árbol entero.

**Lo que NO cambia, y explica por qué la operación es legal.** El marco viaja completo, así que `kit`,
`base` y `loader` **conservan su valor** —son relativos a la raíz, y la raíz nueva es la suya—, los
seis invariantes se siguen cumpliendo, y `{{history_dir}}` se mueve entero, con lo que sus enlaces
Markdown internos sobreviven sin tocarlos (`{{kit}}/core/reference/rutas-y-tokens.md` → *Dos clases de
ruta*). Un layout con nombre promovido sigue siendo ese mismo layout.

**Lo que sí se rompe es exactamente lo que el marco no parametriza:** las referencias al **contenido
del proyecto** —carpetas, scripts, plantillas propias— escritas a mano en los docs. El marco tiene
tokens para sus rutas y **ninguno** para las del proyecto, así que ninguna operación de rutas las
puede barrer sola. Reciben el prefijo de la raíz vieja, a mano y con el `grep` de control después.

**Y el historial no se corrige: se ancla.** Las rutas citadas en `session` e `index` eran ciertas
cuando se escribieron; después de la promoción apuntan a una raíz que ya no existe. Es drift que el
marco **prohíbe reparar**, porque un registro no se reescribe. La salida ya existe y es la misma que
para un renombrado: **se anota la equivalencia una vez, en la cabecera de `index`** —*"a partir del
DD-MM la raíz es X; las rutas de las sesiones 1..N se resuelven anteponiendo `P/`"*— y ahí se acaba.
No en `protocol`: el anclaje vive donde vive el índice de lo anclado.

**Antes de escribir nada**, el mismo eco del bootstrap, con dos líneas más: **raíz** (antes → después)
y **prefijo**. Corregir la interpretación cuesta cero antes y caro después.

**Y su verificación no cabe en la sesión que la ejecuta.** Las puertas se auto-cargan **al abrir**, así
que el observable de que la raíz nueva funciona es el saludo de la sesión **siguiente** — igual que el
paso 10 de BOOTSTRAP, y por la misma razón. Se cierra pidiéndoselo al usuario, no afirmándolo.

> **Con `persistencia = git`, promover la raíz mueve también lo que git ve.** Un `.gitignore` de raíz
> escrito **antes** de mover el repositorio es parte de la operación, no una mejora posterior: el
> commit de re-enraizado es el que decide qué entra, y lo que entre ahí ya está dentro. Los
> `.gitignore` anidados siguen aplicando a su subárbol, así que lo ya excluido no se rehace.

### Y lo que de verdad cambia no son las rutas: es el REFERENTE

**Una promoción no mueve el proyecto: lo amplía.** Antes, *"el proyecto"* era el subdirectorio; después
es el árbol entero, del que aquello es una parte. Las rutas cambian de **valor** y se ve; las
afirmaciones cambian de **referente** y no se ve ninguna, porque **la frase sigue leyéndose
perfectamente**.

**El caso concreto está en el historial, y es peor que las rutas rotas.** Un acta que dice *"se decidió
X para el proyecto"* no tenía ninguna ruta que arreglar y aun así dejó de ser cierta: aquello se
decidió para **uno de varios** procesos, y ahora la misma frase promete un alcance que nadie acordó. Una
ruta rota se detecta con un `grep`; **esto no se detecta con nada**, y no se puede corregir, porque un
registro no se reescribe.

**Por eso el anclaje de la cabecera de `index` es de ALCANCE y no solo de prefijo.** No basta con
*"antepón `P/` a las rutas de las sesiones 1..N"*: hay que decir **qué era el proyecto entonces** —
*"hasta el DD-MM, «el proyecto» significaba `P/`; a partir de ahí significa el árbol entero"*—. Cuesta
una frase más y es la que hace legible el registro viejo.

**Y hay docs vivos que sí se reescriben, precisamente porque no son registro.** Su sujeto acaba de
cambiar y ninguna operación de rutas los toca:

| Doc | Qué le pasó |
| --- | --- |
| `charter` | Describe el propósito de la parte, no del todo. Es el que más miente y el primero que se relee |
| `state` | *Dónde estamos* pasa a medirse contra un proyecto mayor: sin tocarlo, **subestima lo que falta** |
| `gotchas` | Sus trampas son las de un proceso; ahora conviven con las de los demás |
| `specs` · `architecture` | Su alcance declarado es el viejo, y su título no lo dice |

**Al cerrar, `audit completo`**, que es lo que el kit ya pide después de una migración estructural — y
aquí con un encargo explícito, porque su detector habitual no lo cubre: **buscar afirmaciones que
siguen siendo ciertas de la parte y falsas del todo.**
