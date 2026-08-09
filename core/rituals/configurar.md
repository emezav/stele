<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

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
