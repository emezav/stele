<!-- Referencia del kit stele. Se lee BAJO DEMANDA, sobre todo desde BOOTSTRAP y CONFIG.
     `SKILL.md` guarda solo los invariantes; la exposición vive aquí. -->

## Las tres rutas: `kit` · `base` · `loader`

| Ruta | Default | Qué contiene | Quién la escribe |
| --- | --- | --- | --- |
| `kit` | `.stele` | El marco vendorizado. Maquinaria **reemplazable**. | Ritual ACTUALIZAR |
| `base` | `.` | Los docs instanciados (roles). Contenido **del proyecto**. | El agente, cada sesión |
| `loader` | según el agente | Auto-arranque, siempre en la raíz. GENERADO **por bloque**. **No hay default universal:** se llama como el archivo que auto-carga el harness que abre la sesión — `CLAUDE.md` en Claude Code, `AGENTS.md` en Codex y la mayoría. Elegirlo mal deja el marco instalado y **mudo**. | `bootstrap`/`config` |

**Invariantes** (validar en `bootstrap` y en `config`, antes de escribir):

1. `base` **nunca** dentro de `kit`: actualizar reemplaza el directorio del kit entero, y se llevaría
   los docs por delante. Violación = abortar. **Excepción: modo auto-hospedado** (`kit = .`), cuando
   el proyecto **es** el marco — el repo del kit. Ahí el kit no se vendoriza: se desarrolla en sitio
   y nunca se borra, así que la razón del invariante no aplica y `base` es por fuerza un
   subdirectorio suyo. En ese modo `base` debe ser un subdirectorio propio, nunca `.` (ver 2).
2. `kit` == `base` = abortar (misma razón, caso degenerado).
3. `kit` dentro de `base` (p. ej. `base = stele`, `kit = stele/.stele`) es legal pero **se avisa**:
   los `grep` del ritual de apertura empiezan a encontrar plantillas del marco como si fueran docs
   del proyecto.
4. El `loader` vive en la raíz y no puede colisionar con el nombre de un rol resuelto bajo `base`
   (con `base = .` y `loader = AGENTS.md` chocaría con `entry`). Colisión = abortar.
5. `stele.config.md` y el `loader` son las **dos anclas fijas de la raíz**: no siguen a `base`.
6. **Si el archivo del `loader` ya existe, se MODIFICA — nunca se crea de cero.** Su contenido es del
   usuario: se conserva íntegro y el bloque del marco se **inserta** entre las marcas
   `STELE:INICIO` / `STELE:FIN`. Solo ese bloque se reescribe después. Sobrescribir el archivo entero
   destruyó el `CLAUDE.md` de un proyecto real. Vale igual en `bootstrap` y en `config`.
   **Y ese bloque tampoco se reescribe entero: por default se porta el delta a mano**, conservando lo
   que el proyecto haya añadido dentro. Solo se regenera entero si la marca de apertura dice `LIMPIO`.
   Vale en `config` igual que en `actualizar` — los dos rituales tocan ese bloque. La marca antigua
   `RICO` significa lo mismo que el default y se respeta. Razón en el ritual ACTUALIZAR.

**El loader es derivado en parte, no desechable.** Lo generado es el bloque; el **archivo** puede ser
compartido con contenido del proyecto — muchos equipos ya tenían un `CLAUDE.md` o un `AGENTS.md`
escrito a mano antes de adoptar el marco. Se le aplica la misma regla de adopción que a cualquier doc
de rol: mapear y añadir, jamás reemplazar.

### Layouts con nombre (vocabulario, no parámetro)

Cuatro combinaciones de `kit` + `base` cubren casi todos los proyectos. Son **atajos de
conversación**: se resuelven a las tres rutas y **nunca se guardan en el manifiesto**. El layout es
derivable de la sección Rutas; guardarlo crearía un segundo hogar del mismo dato, que se
desincroniza en cuanto alguien mueva una ruta.

| Layout | `kit` | `base` | Para quién |
| --- | --- | --- | --- |
| `default` | `.stele` | `.` | Docs en la raíz, marco invisible |
| `agrupado` | `.stele` | `bitacora` | Tus docs juntos en una carpeta visible |
| `docs` | `.stele` | `docs` | Proyecto con carpeta de docs ya establecida |
| `skill` | `.claude/skills/stele` | `bitacora` | Claude Code: una sola copia del kit, además usable como `/stele` |

Cualquier otra combinación es legal y se nombra `personalizado` (incluido el modo auto-hospedado,
`kit = .`). El `loader` **no** forma parte del layout: depende del agente, no del proyecto.

**Por qué `base` no se llama como el marco.** El nombre obvio para `agrupado` sería `stele/`, y estuvo
así hasta que un proyecto lo señaló al agrupar: **`.stele/` y `stele/` se diferencian en un punto**, y
son cosas opuestas — una es maquinaria que se sustituye entera al actualizar, la otra son **tus
documentos**, los que editas cada sesión.

Y el fallo **no es simétrico**: escribir en tus docs creyendo que es el kit no rompe nada; **escribir
dentro del kit creyendo que son tus docs hace que el cambio desaparezca en la próxima actualización,
sin error y sin aviso**. Un nombre distinto elimina la confusión de raíz, sin depender de que nadie
entienda la convención del punto.

**Aviso antes de elegir `agrupado` o `docs`:** si el `entry` se llama `AGENTS.md`, hay agentes
(Codex y otros) que lo **auto-cargan desde la raíz** igual que Claude Code carga `CLAUDE.md`. Sacarlo
de la raíz con `base != .` no rompe nada visible —el loader sigue funcionando— pero esos agentes
dejan de leer el `entry` por su cuenta. Falla en silencio. Si trabajas con alguno de ellos, o
`base = .`, o renombra el `entry` a algo que no sea `AGENTS.md`.

Se usan de tres maneras:

- **En el eco**, siempre: nombrar el layout resuelto dice más, y más rápido, que tres rutas sueltas.
  Un usuario que pidió "agrupado" detecta `layout -> default` al instante.
- **Como menú**, solo cuando el ritual ya iba a preguntar (BOOTSTRAP paso 1, "ante duda real"). No
  convierte el bootstrap en un cuestionario: sin ambigüedad se aplican los defaults sin preguntar.
- **Como entrada**: "bootstrapea con layout agrupado" o "pásame a layout docs" son peticiones
  válidas; se traducen a valores de ruta y se previsualizan como tales.

## Convención de tokens en plantillas

Las plantillas se escriben por **rol** y usan tokens que bootstrap/`config` resuelven a nombres:
`{{rol}}` → nombre del rol (p. ej. `{{state}}`→`latest.md`); `{{history_dir}}`, `{{specs_dir}}`,
`{{artifacts_dir}}` y `{{correspondence_dir}}` → **rutas** de los roles contenedores;
`{{budget:rol}}` → tope de líneas; `{{effort_unit}}` y
`{{checkpoint_trigger}}` → valores de Features/Wording; `{{kit}}` y `{{loader}}` → rutas
(sección Rutas). Los toggles como `session_greeting` **se consultan, no se interpolan**: no hay
token para ellos.
Los bloques marcados `<!-- GENERADO -->` los produce el marco, no se editan a mano.

**Toda ruta interpolada es relativa a la raíz del proyecto**, nunca al doc que la contiene: los
agentes operan con el CWD en la raíz y es lo que hace `grep`, así que el valor no depende de dónde
quedó cada archivo. De ahí dos reglas de composición:

- `{{kit}}` se escribe **sin `/` final** y se usa como `{{kit}}/SKILL.md`. **Con `kit = .` el prefijo
  colapsa**: `{{kit}}/SKILL.md` → `SKILL.md`, no `./SKILL.md`.
- Los **contenedores** (`{{history_dir}}`, `{{specs_dir}}`, `{{artifacts_dir}}`, `{{correspondence_dir}}`) resuelven **con `base` delante y con `/`
  final**, así que se concatenan directos, sin barra intermedia: `{{history_dir}}{{index}}` →
  `stele/history/index.md` con `base = stele`, y `history/index.md` con `base = .` (el prefijo
  colapsa igual que en `{{kit}}`). En el manifiesto el valor configurado es solo el nombre de la
  carpeta (`history/`); es el token el que le antepone `base` al resolverse.
- **Los tokens de doc (`{{entry}}`, `{{memory}}`, `{{protocol}}`, `{{state}}`…) también llevan `base`
  delante.** `{{entry}}` con `base = bitacora` es `bitacora/AGENTS.md`, no `AGENTS.md`. En el
  manifiesto el valor guardado es solo el nombre del archivo, igual que en los contenedores: **valor
  configurado != token resuelto**. Esta línea existe porque faltaba, y la falta salió cara — la lista
  de arriba solo nombraba los contenedores, y un agente concluyó, con toda la lógica del mundo, que
  los demás tokens se emiten pelados. Ver la trampa de abajo.

> **La trampa: un token de doc sin `base` colisiona con el `loader`, en silencio.** Ocurrió al
> bootstrapear con `base = bitacora` y `loader = AGENTS.md`: el loader generado quedó diciendo
> *"consulta el mapa de documentación en `AGENTS.md`"* y *"el checklist de cierre de `protocol.md`"*.
> El primero **apunta al propio loader**, así que quien lo siga nunca llega al mapa; el segundo no
> apunta a nada. Nada falla: el archivo se ve correcto y el marco arranca.
>
> El invariante 4 de arriba **no lo atrapa** porque compara rutas resueltas —`AGENTS.md` contra
> `bitacora/AGENTS.md`, que no chocan— mientras la colisión ocurre en el puntero *renderizado*.
>
> **El control está en el mismo archivo:** los `@import` del bloque de arranque sí salieron con
> `base` (`@bitacora/AGENTS.md`). Que unos acierten y otros no, en el mismo loader, es lo que prueba
> que el fallo es de los tokens inline y no de la resolución en general.

Esto importa sobre todo en lo **ejecutable**. Un `printf '…' >> {{history_dir}}{{index}}` mal
compuesto no da error: crea el archivo que falta y deja el de verdad sin la fila.

**Dos clases de ruta, y no se resuelven igual** — confundirlas es lo que rompe enlaces al mover
`base`:

- **Ruta de comando** (`printf >> …`, `grep`, `git log --`): relativa a la **raíz del proyecto**,
  porque los agentes operan con el CWD ahí. Es la que producen los tokens.
- **Enlace Markdown clicable** (`[index.md](./index.md)`): relativo al **archivo que lo contiene**,
  porque así lo resuelve cualquier visor. Sobrevive a un cambio de `base` solo si su destino se mueve
  en el mismo bloque — que es el caso dentro de `{{history_dir}}`, y por eso el historial se mueve
  entero y no se reescribe. Un enlace que apunte **fuera** del bloque movido sí se rompe: revísalos
  al migrar.
