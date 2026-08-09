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

Reglas fijas: desactivar un módulo **no** borra sus docs (huérfanos preservados + aviso); colisión
de nombre aborta; cambiar el patrón `session` afecta solo sesiones futuras (el historial es inmutable).

**Cambios de ruta, en concreto:**

- Mover `kit`: mover el directorio y barrer las referencias `{{kit}}` ya resueltas en los docs
  instanciados (`entry`, `protocol`, `loader`). No toca ningún doc de contenido.
- Mover `base`: mover los docs de rol (y `history_dir` completo, con su historial) y regenerar el
  loader, cuyos `@import` son relativos a la raíz. El historial se mueve entero, no se reescribe.
- Cambiar `loader`: insertar el bloque en el archivo nuevo (creándolo o modificándolo, invariante 6)
  y **retirar el bloque del viejo** — no borrar el archivo viejo a ciegas: puede tener contenido del
  usuario. Si al quitar el bloque no queda nada más, entonces sí se borra; si queda algo, se conserva
  y se avisa. Dos loaders **activos** compitiendo es peor que ninguno, pero eso lo resuelve retirar
  el bloque, no destruir el archivo. Verificar antes que el nombre nuevo no colisiona con un rol bajo
  `base`.

