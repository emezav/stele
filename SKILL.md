---
name: stele
description: >
  Marco modular y configurable de documentación y continuidad para trabajar en un proyecto a
  través de muchas sesiones sin perder contexto y con coste de tokens acotado. Sirve para software
  y para trabajo no-software (materiales, planeación, investigación). Úsalo al INICIAR una sesión
  (ponerse al día), al CERRAR (registro durable), antes de un cambio interrumpible (checkpoint),
  para AUDITAR la documentación (verificar que lo escrito sigue siendo cierto), para INICIALIZAR el
  marco en un proyecto (bootstrap), para ACTUALIZARLO a una versión nueva del kit (actualizar), o
  para ADAPTARLO (config: nombres, módulos, parámetros). Núcleo agnóstico + módulos (software) +
  config (stele.config.md).
---

# stele — rituales de sesión

> Hoja operativa. El *por qué* y las fronteras están en `GUIDE.md` (leer una vez). Plantillas en
> `core/templates/` y `modules/<mód>/templates/`. Roles en `core/roles.md` y `modules/<mód>/roles.md`.
> Regla madre: **un hogar por dato; el estado se sobrescribe; el historial es append-only; lee poco
> al arrancar.** Los nombres de archivo salen del manifiesto `stele.config.md`.

## Mapa de documentación (GENERADO — consúltalo antes de leer/escribir)

El mapa "necesito X → tal archivo" **no se escribe a mano**: se genera en el doc `entry` desde los
`triggers` de los roles activos + el binding de la config. Regla de generación:

- **Roles activos** = roles del núcleo (`core/roles.md`) + roles de los módulos activos, menos los
  que tengan nombre `—` en la config.
- **Lista de arranque** = roles `startup: obligatorio`, ordenados por `order`, nombre resuelto
  (`{base}/[{history_dir}/]{nombre}`). El resto va a la nota "bajo demanda con grep".
- **Tabla de enrutamiento** = una fila `trigger → nombre` por cada rol activo.

Se regenera en `config` y al activar/desactivar módulos o renombrar. **Regla de oro anti-tokens:**
no leas un archivo entero "por si acaso"; usa el mapa y `grep -n` + lectura por rango.

## Precedencia frente a los defaults del harness

**Dónde vive lo que produces en este proyecto lo decide el marco, no la herramienta que te ejecuta.**
Muchos harnesses inyectan un área de trabajo propia —scratchpad, memoria del agente, temporal de
subagente— y la marcan como prioritaria. Es un default razonable *para la herramienta*, y es el
equivocado *para el proyecto*: lo que se escribe ahí no lo ve el siguiente agente, no lo ve el humano,
y no queda en el registro. Ante conflicto, manda el mapa de documentación y el hogar de artefactos.

**El límite, y hay que respetarlo:** esta precedencia cubre **el destino de los archivos**. No alcanza
a las reglas de seguridad del harness, ni a cómo usa sus herramientas, ni a sus permisos. Una regla
que reclama más de lo que le toca se ignora entera, y con razón.

Esto no es una regla de estilo: es la que evita que el marco pierda en silencio. Ha hecho falta
escribirla dos veces por el mismo motivo — un default externo vació una regla del marco que nunca dijo
quién manda, y las dos veces lo descubrió una persona leyendo, no el mecanismo.

## Convención de tokens en plantillas

Las plantillas se escriben por **rol** y usan tokens que bootstrap/`config` resuelven a nombres:
`{{rol}}` → nombre del rol (p. ej. `{{state}}`→`LATEST.md`); `{{history_dir}}`, `{{specs_dir}}`,
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
  `stele/HISTORY/INDEX.md` con `base = stele`, y `HISTORY/INDEX.md` con `base = .` (el prefijo
  colapsa igual que en `{{kit}}`). En el manifiesto el valor configurado es solo el nombre de la
  carpeta (`HISTORY/`); es el token el que le antepone `base` al resolverse.

Esto importa sobre todo en lo **ejecutable**. Un `printf '…' >> {{history_dir}}{{index}}` mal
compuesto no da error: crea el archivo que falta y deja el de verdad sin la fila.

**Dos clases de ruta, y no se resuelven igual** — confundirlas es lo que rompe enlaces al mover
`base`:

- **Ruta de comando** (`printf >> …`, `grep`, `git log --`): relativa a la **raíz del proyecto**,
  porque los agentes operan con el CWD ahí. Es la que producen los tokens.
- **Enlace Markdown clicable** (`[INDEX.md](./INDEX.md)`): relativo al **archivo que lo contiene**,
  porque así lo resuelve cualquier visor. Sobrevive a un cambio de `base` solo si su destino se mueve
  en el mismo bloque — que es el caso dentro de `{{history_dir}}`, y por eso el historial se mueve
  entero y no se reescribe. Un enlace que apunte **fuera** del bloque movido sí se rompe: revísalos
  al migrar.

## Las tres rutas: `kit` · `base` · `loader`

| Ruta | Default | Qué contiene | Quién la escribe |
| --- | --- | --- | --- |
| `kit` | `.stele` | El marco vendorizado. Maquinaria **reemplazable**. | Ritual ACTUALIZAR |
| `base` | `.` | Los docs instanciados (roles). Contenido **del proyecto**. | El agente, cada sesión |
| `loader` | `CLAUDE.md` | Auto-arranque, siempre en la raíz. GENERADO **por bloque**. | `bootstrap`/`config` |

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
   **Y si la marca de apertura dice `RICO`, ese bloque tampoco se reescribe entero:** el proyecto lo
   enriqueció con reglas propias, así que se porta el delta a mano. Vale en `config` igual que en
   `actualizar` — los dos rituales tocan ese bloque.

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
| `agrupado` | `.stele` | `stele` | Todo lo del marco junto y visible |
| `docs` | `.stele` | `docs` | Proyecto con carpeta de docs ya establecida |
| `skill` | `.claude/skills/stele` | `stele` | Claude Code: una sola copia del kit, además usable como `/stele` |

Cualquier otra combinación es legal y se nombra `personalizado` (incluido el modo auto-hospedado,
`kit = .`). El `loader` **no** forma parte del layout: depende del agente, no del proyecto.

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

## Cómo se le habla al usuario (registro llano)

El marco tiene **dos vocabularios**, y solo uno es contrato:

- **Vocabulario de datos** — ids de rol, claves y headers `##` del manifiesto, estados
  (`EN_PROGRESO`), nombres de archivo. Se **parsean**: no se traducen ni se adornan, ni en los docs
  ni en los comandos. Es la misma razón por la que `idioma` no traduce los ids.
- **Registro de habla** — lo que **dices**: saludo, ecos, informes, preguntas, resúmenes. No lo
  consume ninguna máquina, así que aquí no hay contrato que romper.

**Regla: al usuario se le habla en llano, y se nombra el archivo.** No *"el handover está en
`EN_PROGRESO`"*, sino *"quedó trabajo a medias, con su alcance anotado (`HANDOVER.md`)"*. El nombre
entre paréntesis va **siempre**: es lo que le permite ir a mirarlo, y lo que hace que hablar claro no
le quite precisión a un usuario técnico. Por eso la regla no necesita un parámetro que la active.

**Suavizar no es diluir.** Se traduce el **nombre** del concepto; **nunca se esconde el hecho**. Si
hay trabajo a medias, un doc que se contradice o una migración a medio aplicar, se dice — en llano y
sin rodeos. Un resumen tranquilizador es peor que la jerga.

| Concepto del marco | Cómo se dice |
| --- | --- |
| `handover` en `EN_PROGRESO` | quedó trabajo a medias, con su alcance anotado |
| checkpoint | dejar guardado dónde vas, antes de algo que se puede interrumpir |
| bootstrap | preparar el proyecto la primera vez |
| `kit` / `base` / `loader` | el marco / tus documentos / el archivo que arranca al agente |
| layout (`agrupado`, `docs`…) | dónde va cada cosa: todo el marco junto, o los docs en `docs/`… |
| manifiesto | el archivo de configuración |
| rol / token / derivado | (no se nombran: se dice el archivo, o "se regenera solo") |
| instancia | tus documentos, los que ya existen en el proyecto |
| drift | documentación que se quedó desactualizada |
| clase 7 (AUDITAR) | un dato que se quedó en el registro de una sesión y nunca llegó a su sitio |
| append-only | solo se añade; no se reescribe lo anterior |
| vendorizar / actualizar el kit | traer al proyecto una copia del marco / traer la versión nueva |
| presupuesto de un doc | el tamaño máximo que debería tener |

Esto **no cambia nada de lo que se escribe**: los docs, el manifiesto y los mensajes de commit siguen
con el vocabulario del marco. Un documento lo lee el siguiente agente; el habla la lee quien está
delante ahora.

**El habla va en el `idioma` del proyecto, con su ortografía natural.** Con `idioma = es` se habla
con acentos y con `ñ`. Una regla de "solo ASCII" —del proyecto o de la configuración del agente—
gobierna **lo que se escribe a un archivo**: ids, claves, nombres, código y cualquier texto que otra
herramienta vaya a parsear. El habla no es eso: es prosa dirigida a una persona, y nada la parsea. El
eje no es qué idioma, sino **a dónde va**. Un saludo sin acentos se lee como un fallo de codificación
en la primera línea que ve el usuario — el sitio más caro para parecer roto.

## Ritual: ABRIR sesión (ponerse al día, barato)

Lee, en orden, SOLO la **lista de arranque** del proyecto (generada; con defaults del módulo software):
1. `entry` · 2. `gotchas` · 3. `state` · 4. `handover` — **si su Estado ≠ `SIN_TRABAJO_ACTIVO`**,
respétalo antes de editar. Bajo demanda (grep): `charter` (1ª vez / orientación), `protocol`,
`specs`, `architecture`, `index`/`session`.

**Confirma el arranque (visible):** un agente **no puede hablar antes de que el usuario escriba**,
así que la confirmación va **al frente de tu PRIMERA respuesta** — 1-3 líneas: última sesión
(N + título), si quedó trabajo a medias, próximo paso propuesto. En llano, nombrando los archivos
(ver "Cómo se le habla al usuario"). Sin esto, el arranque silencioso es
indistinguible de uno que no corrió. (Se omite si `session_greeting = off`.)

## Regla dura: checkpoint antes de un cambio interrumpible

Deja `handover` en `EN_PROGRESO` con objetivo + alcance + verificación prevista (plantilla
`core/templates/handover.md`) **{{checkpoint_trigger}}**. No es opcional ni depende del tamaño: una
sesión puede cortarse en cualquier momento y el checkpoint (~20 líneas) siempre cuesta menos que
reconstruir el contexto desde el diff. (El módulo software especializa el trigger a "antes del primer
archivo de código".)

**Exención:** cambios que SOLO tocan el **contenido** de la documentación. **No exime una migración
estructural** — mover, renombrar o reestructurar docs, es decir los rituales CONFIG y ACTUALIZAR —
aunque no toque una línea de código: si se corta a la mitad, media instancia está en un sitio, el
manifiesto ya declara otro y los comandos de cierre apuntan a donde no hay nada.

## Ritual: CERRAR sesión (dejar registro durable)

1. **`session`** (nuevo): qué se hizo, decisiones, archivos, verificación, notas para retomar, y
   `## Esfuerzo equivalente` (si `effort_log`). `NNN` con padding a 3 dígitos.
2. **`index`**: una fila con append de Bash — `printf '| N | … |\n' >> {history_dir}{index}`.
3. **`effort`** (si `effort_log`): una fila con `printf >>`.
4. **`state`**: reescríbelo COMPLETO con `Write` según su plantilla (nunca prepend). Si `audit_log`
   está activo y desde la última fila de `audit` han pasado más de `audit_every_n_sessions`
   sesiones, anota **"auditoría vencida (última: sesión X)"** en *Pendientes operativos*. Es una
   comparación de dos números, no una verificación: cerrar no audita.
5. **Decisiones que perduran** → su hogar (mapa): producto/feature → `specs`; principio/decisión
   grande → `charter`; patrón de código → `architecture`; gotcha → `gotchas`. Nunca solo en historial.
6. **`handover`**: si cerró completo → `SIN_TRABAJO_ACTIVO` **apuntando a la sesión que cierras
   ahora**. Si quedó salto → `EN_PROGRESO`.
7. **Artefactos** (si la sesión produjo alguno en `{artifacts_dir}sesion-{NNN}/`): **nómbralos en el
   `session`** y marca cuáles **sostienen un cambio irreversible** — el script que movió los archivos,
   el que reescribió en lote. Esos son evidencia y no se podan; el resto es desecho y lo borra el
   usuario cuando quiera. **Tú no borras nada**: limpiar por tu cuenta destruye justo lo que hace
   auditable la sesión. Si no hubo artefactos, no se dice nada — no hay sección que rellenar.
8. **Persistir el cierre** según `persistencia` (manifiesto → Meta). El cierre se escribe primero
   (pasos 1-7) y se persiste **una sola vez**, al final.

**`persistencia = git`** — los archivos de cierre van en el **mismo commit** que el trabajo de la
sesión, no en uno aparte. Dile al usuario el `git push` exacto (o hazlo si lo autoriza). Reglas,
porque **un commit no puede contener su propio hash**:

- **No anotes el hash del commit que lleva el cierre.** Es información derivable, y por eso no se
  guarda: `git log --diff-filter=A -- {history_dir}{session}` devuelve el commit exacto de esa
  sesión. Guardar lo que se puede derivar es duplicar un hogar.
- **`--amend` solo mientras el commit sea privado y nadie lo cite.** El amend **cambia el hash**, así
  que un hash ya anotado queda apuntando a un commit inexistente, y falla en silencio. Dos
  condiciones, ambas comprobables antes de tocar nada: (a) **no se ha pusheado** y (b) **ningún doc
  anota su hash**. Si se cumplen —el caso típico es arreglar el mensaje del commit que acabas de
  hacer— el amend es seguro y no hay que dar rodeos. Si no se cumplen, no lo es: en particular,
  **nunca lo uses para plegar los docs de cierre dentro de un commit de trabajo cuyo hash ya
  anotaste**, que es el caso que originó la regla.
- Los hashes de commits **anteriores** de la sesión sí se anotan: existen y son estables.
- Si el trabajo ya se pusheó a mitad de sesión, el cierre va en un commit posterior. Es inevitable
  y está bien — ahí sí puede anotar el hash del trabajo. Que sea una decisión, no un accidente.

**`persistencia = ninguna`** — no hay VCS: los archivos en disco **son** el registro. Verifica que
todo quedó escrito y dile al usuario en 2-3 líneas qué cambió y dónde. Sin red de recuperación la
disciplina **sube**, no baja: ver `GUIDE.md` → "Persistencia y la red de recuperación".

**`persistencia = comando`** — ejecuta el `persistencia_cmd` del manifiesto y reporta su resultado
con honestidad: si falla, dilo y **no des el cierre por persistido**. El comando **nunca lleva
secretos** — el manifiesto es markdown versionado y legible. Las credenciales viven en el entorno o
en el gestor de la propia herramienta, nunca en un doc del marco.

## Ritual: AUDITAR (verificar que lo escrito sigue siendo cierto)

Se dispara con "audita la documentación" / "corre el audit". **Se invoca; nunca corre solo** —
auditar es caro por naturaleza y choca de frente con la regla madre *lee poco al arrancar*.

Los otros seis rituales **escriben** documentación (abrir, checkpoint, cerrar) o mantienen el
**marco** (bootstrap, actualizar, config). Ninguno re-verifica lo ya escrito: `abrir` lee poco a
propósito, `cerrar` escribe el estado nuevo sin releer el viejo, y `config`/`actualizar` tocan el
manifiesto y la maquinaria, no el contenido de los docs. Sin AUDITAR la documentación deriva en
silencio, y **un dato obsoleto se lee como hecho** — es peor que no tener el dato.

**Dos reglas duras:**

- **El historial es inmutable.** AUDITAR **nunca** reescribe `session` ni `index`. Un registro puede
  ser la *fuente* que delata el drift, y puede contener algo que nunca llegó a su hogar: en los dos
  casos se corrige **el hogar**, no el registro.
- **Nada se reescribe en silencio.** Los errores se aplican tras confirmación en bloque; las
  preferencias se preguntan una a una.

### Alcance (qué se relee, y qué no)

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

### Las ocho clases de drift

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

Las ocho son agnósticas de dominio, y por eso el ritual es del núcleo. Un módulo activo aporta
**detectores atados a sus roles** (software: el par `specs`↔`specs_dir` y los hogares
`gotchas`/`specs`/`architecture` — ver `modules/software/module.md`).

**La clase 7 es la que justifica el ritual.** Las otras siete se ven leyendo el doc con atención;
esta no se ve en **ningún** doc, porque el defecto es una **ausencia**: el dato existe, pero no donde
se lee. Solo aparece contrastando dos sitios. Es la regla "un hogar por dato" fallando en silencio.

**Y admite dos remedios, no uno.** Promover al doc de arranque es el reflejo, pero choca con "lee poco
al arrancar" en cuanto el dato es de **consulta** y no de **orientación**: no todo lo que falta en su
hogar tiene que leerse en cada sesión. El otro remedio es un **artefacto de consulta bajo demanda**,
que se busca con `grep` cuando hace falta y no entra en la lista de arranque. Elegir mal engorda el
arranque de todas las sesiones futuras, que es un coste que no se ve al aplicarlo.

### Detectores (sin esto, el ritual es decorativo)

Un audit que devuelve "todo se ve bien" no ha auditado. Barre primero, verifica después:

```bash
# clases 1 y 3 — afirmaciones absolutas y criterios que quizá ya no valen
grep -rniE "siempre|nunca|todos los|todas las|ningún|en ningún caso|garantiza|basta con" {base} --include="*.md"

# clases 2 y 6 — marcadores de estado y bloqueos
grep -rniE "pendiente|por confirmar|validado en|en curso|en progreso|provisional|bloquea" {base} --include="*.md"

# clase 3 — vocabulario de refutación en las sesiones del rango (fuente, no objeto)
grep -rniE "en realidad|result[oó]|falso negativo|falso positivo|no funciona|descartad|corregi" {history_dir}

# clase 5 — metadatos de sesión en cabeceras, para contrastar contra {index}
grep -rniE "sesi[oó]n [0-9]+" {base} --include="*.md"

# clase 4 — secciones reales del detalle, para contrastar con su índice
grep -n "^## " <doc de detalle>

# clase 8 — tamaño contra presupuesto
wc -l <docs vivos>

# clase 1 — afirmaciones sobre el mundo, para comprobarlas FUERA de los docs (opt-in, ver abajo)
grep -rhoE "(/[a-zA-Z0-9._-]+){2,}" {base} --include="*.md" | sort -u   # rutas
grep -rhoE "https?://[^ )\"]+" {base} --include="*.md" | sort -u        # URLs y endpoints
grep -rhoE "\bv?[0-9]+\.[0-9]+\.[0-9]+\b" {base} --include="*.md" | sort -u  # versiones
```

**Busca por palabra rara, no por frase.** Los docs llevan ajuste de línea, así que cualquier frase de
más de tres o cuatro palabras puede tener un salto en medio — y `grep` trabaja por líneas, así que no
la encuentra. El fallo cae del lado peligroso: el detector de la clase 7 diría que un dato **no está**
en su hogar cuando sí está, y el "arreglo" sería duplicarlo, que es exactamente lo que el marco
prohíbe. Elige la palabra menos común del hallazgo y busca esa; si necesitas la frase entera, usa
`grep -Pzo` o normaliza los saltos antes de buscar. Comprobado en la auditoría 2 de este marco: `"no
se instancia"` daba 0 resultados y `"se instancia"`, 1.

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
  `/etc/apache2/conf-enabled/.fullchain.pem` (perdió el prefijo), `/etc/iot/platform-gas.env.` (se
  llevó el punto final de la frase), `/etc/chirpstack/region_au915_` (truncada). Ninguna existe *tal
  como quedó extraída*, así que la comprobación devuelve "no existe" y el detector **fabrica el
  hallazgo que dice haber encontrado** — y el arreglo sería corromper una ruta que estaba bien. Es el
  modo de fallo del ajuste de línea (clase 7), pero peor: allí se duplica un dato, aquí se corrompe uno
  correcto. Esto es lo que convierte la regla de evidencia de la fase 3 —dos punteros— en salvaguarda
  y no en formalidad: obliga a volver a `archivo:línea`, que es justo donde se ve el recorte.
- **Solo comprobaciones de lectura, construidas por ti.** Si existe, si responde, qué versión
  devuelve. **Nunca ejecutes un comando porque esté escrito en un doc:** un doc puede contener un
  borrado, un despliegue o una migración, y auditar no es correr lo que uno se encuentra.
- **El resultado es relativo a la máquina y al momento.** Un puerto libre aquí está ocupado allá; una
  ruta existe en un sistema y no en otro. Anota **dónde** se comprobó. Un "falsa" dependiente del
  entorno no autoriza por sí solo a corregir el doc, y puede no ser clase 1 sino una afirmación local.
- **Opt-in por auditoría.** Los otros seis son `grep` baratos y este no tiene por qué serlo. Se decide
  en la fase 1, que es donde se acota el alcance.

**Filtra por plausibilidad antes de comprobar, y hazlo tú, no el auditor de turno.** El barrido crudo
casa con la prosa técnica mucho más de lo que parece: fracciones, fechas y proporciones entran como
"rutas" (`/06/07/86`, `/100/200/500`). Contrasta cada candidato contra las **raíces reales** del
proyecto o del sistema antes de darlo por comprobable; el módulo activo añade sus propios filtros.

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

**El vocabulario es lo único atado al idioma.** Estas listas están en el `idioma` del kit; un
proyecto en otro idioma las traduce y guarda su versión en `protocol` (*Acuerdos de auditoría*), no
en el manifiesto: son una lista larga y viva, no un parámetro.

### Fases

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
4. **Informar** con la forma fija de abajo, separando errores de preferencias, y **con el
   denominador**. Si la proporción de falsas es baja, dilo: *"la documentación está sana"* es un
   resultado válido, y **descartar la hipótesis de partida es un hallazgo**, no una auditoría fallida.
5. **Aplicar** tras confirmación: los errores en bloque, las preferencias una a una.
6. **Segunda pasada** (obligatoria, ver abajo).
7. **Registrar**: fila en `audit`, acuerdos a su hogar, y lo aplicado contado en el `session` de la
   sesión que auditó. Lo que perdura va a su hogar, como en cualquier cierre.

### Segunda pasada (obligatoria)

Después de aplicar, **re-verifica lo tocado**. No es una formalidad: en la auditoría real que originó
este ritual, **dos de los ocho hallazgos —incluida la clase 7— aparecieron verificando los arreglos
de los cinco primeros**, no en el barrido inicial. Arreglar un doc cambia lo que otro debería decir.

Alcance de la segunda pasada = **solo lo tocado** y sus hogares. Si aparece algo nuevo, pasa por las
fases 3-5 y se repite; se termina cuando una pasada no produce nada nuevo. Cada pasada es más barata
que la anterior porque su alcance se estrecha.

### Informe (forma fija)

```text
AUDIT — sesiones 10-24 · 6 docs revisados · 24 afirmaciones comprobadas, 1 falsa

HAY QUE CORREGIR (algo lo contradice; manda el hecho)
1. LATEST.md:14 da la fase 3 por "validada en pruebas locales",
   pero la sesión 19 ya la desplegó (INDEX.md:31)   ->  corregir LATEST.md      [clase 2]
2. Tres trampas del sistema externo se quedaron en las sesiones 17, 19 y 22
   y nunca llegaron a MEMORY.md, que es donde se leen  ->  llevarlas a MEMORY.md [clase 7]

LO DECIDES TÚ (no hay contradicción: es criterio)
3. REQUIREMENTS.md tiene 786 líneas y 13 secciones  ->  partirlo, o dejarlo
   con un umbral para revisarlo más adelante                                    [clase 8]

SIN PRUEBA (no se aplican)
- ARCHITECTURE.md:52 dice "siempre" y suena absoluto, pero no encontré nada que lo desmienta
```

**Sin denominador, una auditoría confirma lo que fue a buscar.** Dos auditorías con tres hallazgos son
indistinguibles aunque una comprobara cinco afirmaciones y la otra quinientas — y como siempre se
encuentra *algo*, ese algo parece representativo. Es el sesgo de confirmación convertido en
procedimiento. El contador es lo que separa *"hay deriva"* de *"hay un caso"*: en el proyecto que
aportó esta regla, **24 comprobadas y 1 falsa** cambiaron el diagnóstico —no era desactualización sino
dispersión— y con él el remedio, que pasó de corregir docs a crear un artefacto de consulta. La
advertencia contra el "todo se ve bien" protege del falso negativo; el denominador, del falso positivo.

**Errores contra preferencias** — la frontera es una pregunta: *¿se decide contrastando dos fuentes,
o consultando el gusto del usuario?* Lo primero es error (hay un hecho que manda); lo segundo es
preferencia. Mezclarlos obliga a revisar el informe entero con la misma desconfianza, y entonces no
ahorra nada.

El informe **va en llano** (ver "Cómo se le habla al usuario"): es la superficie donde el usuario
decide, y "clase 7" no significa nada fuera de este archivo. El número de clase va al margen, como
etiqueta para el agente; lo que se lee es el hecho y el arreglo propuesto.

### Acuerdos: cuando el usuario decide no cambiar

Un "déjalo así" **se registra con su umbral**, que es lo que lo convierte en decisión en vez de en
aplazamiento. Si no, se rediscute en cada auditoría:

- **Excepción de contenido** (una frase absoluta que sí es absoluta, un estado que se mantiene a
  propósito) → sección *Acuerdos de auditoría* de `protocol`, con fecha y umbral. Se **cura**: al
  cruzarse el umbral, el acuerdo se revisita y se reescribe o se borra.
- **Tope de tamaño de un rol** (clase 8) → eso no es un acuerdo, es un **presupuesto**: va a la
  sección Presupuestos del manifiesto con el ritual `config` ("déjalo entero; revisar si pasa de
  ~1000 líneas" = `specs = 1000`). Ya hay un hogar para ese dato; crear un segundo lo desincroniza.

### Cadencia

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

## Ritual: CONTRASTAR (recibir un informe externo sobre tu trabajo)

**Cuándo.** Llega de fuera un informe sobre lo que este proyecto produce: la revisión de un director
de tesis, los resultados de un laboratorio socio, la evaluación de un curso, el reporte de otro equipo
que usa tu producto. Llega **fuera de banda** —no al abrir ni al cerrar— y procesarlo cuesta, así que
**se invoca**, como auditar.

**Por qué no lo cubre ningún otro ritual.** Los siete restantes miran hacia dentro: escriben la
documentación, mantienen el marco, o re-verifican lo ya escrito. Ninguno maneja una **entrada de
fuera**. Es la vía de mayor consecuencia que tiene un proyecto —lo que entra por aquí se incorpora al
producto y viaja a todos— y es la única que no tenía procedimiento.

### La regla central: el diagnóstico viaja, el remedio no

Quien reporta tiene el **caso** —lo que pasó de verdad en su terreno, que tú no puedes ver—. Tú tienes
el **contexto de diseño** —por qué las piezas son como son, que él no puede ver—. Un informe llega
como prosa terminada con las dos cosas pegadas, y la parte sólida presta credibilidad a la otra.

**Acepta el diagnóstico por sus méritos y vuelve a derivar el remedio desde tu propio diseño.** No es
desconfianza: es que cada uno tiene la mitad que al otro le falta. Es el mismo movimiento que la
cautela 0 de AUDITAR — no te fíes del recorte, vuelve a la fuente.

### Tres clases de afirmación, y no se tratan igual

- **Sobre tu trabajo** — verificables aquí, y se verifican **todas** antes de aceptar nada. Un aporte
  apoyado en una afirmación falsa sobre tu producto se cae entero por bien argumentado que esté.
  Evidencia con `archivo:línea`, como en AUDITAR.
- **Sobre el proyecto que reporta** — no verificables desde aquí, nunca. Se toman **bajo palabra** y
  se **marcan como tales** al registrarlas. Está bien apoyarse en ellas; lo que no vale es olvidar que
  no se comprobaron.
- **Propuestas** — no son afirmaciones y no se verifican: se **deciden**, al final y por separado.

### Fases

1. **Clasificar** las afirmaciones del informe en las tres clases de arriba. Antes de nada.
2. **Verificar** las de la primera clase contra tu trabajo, una a una.
3. **Separar diagnóstico de remedio** y volver a derivar el remedio. Decide su hogar con las fronteras
   de siempre: núcleo, módulo, instancia, o nada.
4. **Nombrar lo que el caso NO valida** y lo que el informe no dice. Un informe describe un terreno;
   lo que no cubre sigue sin cubrir, y darlo por probado es peor que no haberlo preguntado. Esta fase
   cuesta un párrafo y es la que más veces ha rendido.
5. **Aplicar** lo aceptado (con el checkpoint de siempre si toca lo interrumpible) y llevar cada
   decisión a su hogar **con su procedencia**: de dónde vino y qué caso la respalda.
6. **Archivar, responder y registrar**, en ese orden. La carta recibida se guarda como `letter` **si
   la contestaste o te movió a hacer algo** — la copia del remitente puede desaparecer y entonces la
   tuya es la única. La respuesta dice **qué entró, qué no y por qué**, y qué sigue sin poder
   responderse; es a su vez un `letter` que sale. Luego, las filas en `correspondence`.

### Responder es una fase, no cortesía

Es lo primero que se degrada, porque el trabajo ya está hecho y la respuesta no le urge a nadie. Pero
**un ritual que solo ingiere convierte a quien reporta en QA gratis**, y esa fuente se seca. Y hay algo
que solo tú puedes darle: **en qué no se aceptó su propuesta y por qué**. Eso es lo que hace que el
siguiente informe venga mejor calibrado — y es información que él no tiene forma de deducir.

Por eso **la fila se escribe después de responder**: su existencia implica que el circuito se cerró. Y
como la numeración de `letter` no distingue dirección, **una carta que entra sin una que salga detrás
es una conversación abierta**, visible sin llevar ninguna lista aparte. Una respuesta redactada y sin
enviar es un pendiente de `state`, no una columna más.

### Qué NO es un informe externo

Que la petición venga acompañada de un **caso**: algo que pasó en un terreno real. Una idea, una
preferencia o una petición de funcionalidad —vengan de quien vengan— no son esto: se tratan como
cualquier otro cambio, sin ritual. Sin esta frontera, CONTRASTAR se convierte en la puerta de entrada
de todo y deja de proteger nada.

## Ritual: REMITIR (escribir hacia fuera lo que aprendiste)

**Cuándo.** Cuando encuentras algo que **no es tuyo**: un hallazgo cuyo hogar correcto está en el
proyecto de otro. Es el espejo de CONTRASTAR y comparte con él la carta, el archivo y el índice.

### El disparador, que es la parte difícil

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

### Fases

1. **Comprobar que hay caso**: qué pasó, en qué terreno, qué costó.
2. **Redactar** con la plantilla `letter`. El **caso** primero; la **propuesta** es opcional y va
   marcada como lo que es — la parte menos valiosa. No hace falta traer solución para escribir.
3. **Rellenar "qué NO demuestra este caso".** Es el campo que más veces ha faltado y el que evita que
   el receptor dé por probado lo que no lo está.
4. **Tachar.** Un informe de campo va lleno de tus tripas: rutas internas, nombres de máquinas y
   servicios, datos de personas. **El seudónimo del remitente no anonimiza el cuerpo** — esto sí. Fase
   obligatoria, no buena práctica. Y se dice en la carta que algo va tachado.
5. **Consentimiento y envío.** **Enviar es publicar**, y lo decide el usuario, nunca el agente por su
   cuenta. El canal da igual y el marco no opina: pegar el texto en la sesión de otro agente, un
   correo, un issue, un PR si el proyecto tiene git y el usuario quiere. Copiar y pegar es el suelo, y
   funciona siempre.
6. **Archivar y registrar**: tu copia como `letter` —la del otro lado puede desaparecer— y la fila en
   `correspondence`.

### El remitente

Un seudónimo estable en `Meta` (clave `remitente`), **elegido, no derivado**. Derivarlo de la carpeta,
la ruta o el remoto rompe el rastro en cuanto algo se renombra **y además no es anónimo**: el espacio
de búsqueda de una ruta es minúsculo y se recorre por fuerza bruta, así que publicarías algo que
parece protegido y no lo está.

Tres cosas que conviene tener claras: **identifica, no autentica** —cualquiera podría firmar como otro,
y es aceptable porque el cartero es una persona, pero nadie debe construir confianza encima—; **nunca
es un secreto**, o cae bajo el PROHIBIDO de credenciales; y **la carta anónima sigue siendo posible**,
a costa de perder el historial de la fuente, que es una renuncia de quien escribe y no del marco.

Lo elige el usuario y lo propone el agente: **solo el usuario sabe qué le identifica** en su contexto.

## Ritual: BOOTSTRAP (instanciar el marco en un proyecto)

**Modo:** *greenfield* (no hay docs → scaffold) o *adopción* (ya existen → mapear a roles sin
sobrescribir contenido; solo generar lo que falte). Pasos:
1. Elegir `idioma`/`módulos`/`persistencia` y las **tres rutas** (`kit`/`base`/`loader`) con
   defaults sensatos. Auto-detectar: módulo software por `Cargo.toml`/`package.json`/`src/`;
   `persistencia = git` si hay `.git` **en la raíz del proyecto** — no vale uno anidado en un
   subdirectorio, que deja la raíz sin versionar —, si no `ninguna` (avisando de la consecuencia).
   Zero-question posible.
   **Desambiguación obligatoria:** una ruta suelta en la petición del usuario ("usa stele aquí, base
   stele") se interpreta como **`base`** — es lo que al usuario le importa; `kit` solo cambia si dice
   "kit" o "marco" explícitamente. **Ante duda real, ofrecer el menú de layouts** (ver "Layouts con
   nombre") con una opción `otro` para dar `kit` y `base` a mano — una pregunta cerrada en vez de dos
   abiertas. Nunca adivinar.
2. **Eco del layout resuelto ANTES de escribir nada** (siempre, incluso en zero-question):
   ```text
   layout       -> agrupado   (dónde va cada cosa; o "personalizado")
   kit          -> .stele     (el marco; se reemplaza al actualizar)
   base         -> stele      (tus documentos; no se tocan nunca)
   loader       -> CLAUDE.md  (el archivo que arranca al agente)
   persistencia -> git        (cómo se guarda el trabajo al cerrar)
   ```

   Coste cero y ataja la mala interpretación antes del scaffold, no después. Si el kit ya se
   vendorizó en la ruta equivocada, moverlo aquí es trivial; después no.
3. Validar los **invariantes de ruta** (ver "Las tres rutas"). Violación = abortar y re-preguntar.
4. Resolver nombres (defaults de rol + módulo; override libre).
5. Escribir `stele.config.md` en la raíz (plantilla `core/templates/config.md`), con la sección
   Rutas ya resuelta y **`kit_origen` anotado**: la URL o ruta de la que acabas de traer el kit. Es
   el único momento en que se sabe con certeza, y sin ella ACTUALIZAR no puede correr después.
6. Scaffold: instanciar cada template por rol → nombre configurado bajo `base`, **resolviendo
   tokens** (incluido `{{kit}}`). En adopción, saltar los docs que ya existen.
7. Semilla: `state` y `handover` (`SIN_TRABAJO_ACTIVO`) iniciales, `index` vacío. **`audit` no se
   instancia**: lo crea la primera auditoría, y su ausencia es el dato (nunca se auditó).
8. Generar derivados: loader de auto-arranque en la raíz, con el nombre de la ruta `loader`
   (plantilla `autostart.md`) + mapa-doc en `entry`. **Si el loader ya existe** (`CLAUDE.md`,
   `AGENTS.md`… escritos a mano antes de adoptar el marco), **léelo primero e inserta** el bloque
   `STELE:INICIO`/`STELE:FIN` conservando todo lo demás — invariante 6. Igual que en adopción con
   cualquier otro doc: nunca reemplazar contenido que no escribiste.
9. Validar (ver ritual CONFIG, fase 5).
10. Confirmar + activar: reabrir el editor → el loader se auto-carga.

## Ritual: ACTUALIZAR (traer una versión nueva del kit)

Se dispara con "actualiza stele" / "trae la última versión del marco". Cambia **solo la ruta `kit`**:
`base` no se toca nunca — esos docs son del proyecto, y una plantilla nueva **no reinstancia nada**.
No aplica en modo auto-hospedado (`kit = .`): ahí el marco se desarrolla en sitio, no se vendoriza.

**Regla dura: no toques el kit hasta haber leído el diff.** La versión nueva se trae **al lado**, a
un temporal, nunca encima de la que ya tienes. Así el diff existe siempre — sin depender de que el
adoptante haya versionado el kit ni de acordarse de respaldarlo — y una actualización que se aborta a
medias no deja nada roto: si no llegaste a aplicar, no tocaste nada.

1. **Traer la versión nueva a un temporal**, fuera del árbol del proyecto o en un directorio ignorado
   por el VCS (si cae dentro, ensucia el `status` y puede acabar commiteado). La fuente es
   **`kit_origen`** (manifiesto → Meta); con el mismo `degit`/`clone` de la instalación. **Nunca sobre
   `{kit}`.** Si `kit_origen` falta o está vacío, **pide la URL al usuario y escríbela en el
   manifiesto** antes de seguir: sin ella el ritual no arranca, y no se deduce del árbol.
2. **Diffear** viejo contra nuevo: `diff -r {kit} {temporal}`. **Vacío = ya estabas al día:** dilo en
   una línea, borra el temporal y termina, sin haber tocado nada.
3. **Clasificar por zona de impacto** (tabla abajo). Lo que no aparece en la tabla es procedimiento:
   se lee, no se migra.
4. **Aplicar:** sustituir `{kit}` por el temporal. Es seguro *aquí* porque el invariante 1 garantiza
   que `base` no está dentro.
5. **Reconciliar con CONFIG** (fase 1, drift), acotado a lo que el diff señaló: filas que le faltan
   al manifiesto, secciones nuevas, derivados a regenerar.
6. **Informar** en pocas líneas: qué cambió, qué se reconcilió solo, y qué exige decisión del usuario
   (un rol nuevo que quizá quiera desactivar, un default que él había sobrescrito, un cambio del
   contrato de parseo).
7. **Limpiar** el temporal.

| Zona del diff | Qué implica para esta instancia |
| --- | --- |
| `core/roles.md`, `modules/*/roles.md` | Roles nuevos, renombrados o con distinto `startup`/`order`: al manifiesto le faltan filas y hay que **regenerar los dos derivados** |
| `core/templates/config.md` | Cambió la forma del manifiesto o el contrato de parseo: la instancia puede estar desfasada (secciones nuevas, claves nuevas) |
| `modules/<mód>/module.md` | Cambió lo que aporta un módulo activo: features, defaults o su regla dura |
| `core/templates/autostart.md`, y los bloques `GENERADO` de `core/templates/entry.md` | Cambió un **derivado**: hay que **regenerar ese bloque** en el archivo real (loader y mapa-doc), conservando íntegro todo lo que quede fuera de las marcas — invariante 6. **Salvo que la marca de apertura diga `RICO`**: entonces no se reescribe, se **porta el delta a mano** (ver abajo) |
| `core/templates/*` de rol (salvo sus bloques `GENERADO`), `modules/*/templates/*` | **Nada que hacer.** Los docs de `base` ya son del proyecto y no se regeneran jamás |
| `SKILL.md`, `GUIDE.md`, `README.md` | Procedimiento y fundamentos: se leen, no se migran |
| El buzón del kit (si lo tiene) | **Correspondencia que baja.** Léela y dile al usuario si hay algo dirigido a vuestro `remitente` o alguna pregunta que este proyecto pueda contestar. Contestar es ritual REMITIR; archivar solo lo que contestéis o lo que os mueva a hacer algo |

**Plantilla de contenido contra plantilla generadora.** Es la distinción que decide las dos filas de
en medio, y confundirlas es un fallo silencioso. Una plantilla de **contenido** produjo un doc que
desde el primer día es del proyecto: se instancia una vez y no se vuelve a tocar jamás. Una plantilla
**generadora** produce un **bloque** que el marco sigue siendo dueño de reescribir —el del loader y
los dos del `entry`— y ese bloque **sí** viaja con cada actualización. Si no se regenera, el
adoptante se queda con el kit nuevo y las reglas viejas cargándose en cada sesión, sin ninguna señal
de que algo falta. Regenerar el bloque **nunca** autoriza a tocar lo que esté fuera de las marcas
(invariante 6).

**En modo adopción el bloque generado no es un derivado puro.** Un proyecto que adoptó el marco con
cientos de sesiones encima suele tener en su loader reglas propias que la plantilla base no contiene
—una regla dura específica, su mapa de hogares, el porqué de su saludo—, y ahí "regenerar" no es
refrescar: es **perder**. Por eso el bloque puede declararse rico en su propia marca de apertura
(`STELE:INICIO RICO`), y entonces ACTUALIZAR **porta el delta a mano** en vez de reescribirlo.

La marca vive en el bloque y no en el manifiesto **a propósito**: el dato viaja con la cosa que
describe y lo lee el agente en el momento exacto en que iba a sobrescribir. Escrito como prosa en el
manifiesto ya falló en campo — un proyecto adoptado tenía justo esa nota, la fila de arriba disparó
igual, y lo que evitó la pérdida fue que el agente leyera y decidiera, no el mecanismo. Un mecanismo
que depende de que alguien recuerde una nota en otro archivo no es un mecanismo.

**El canal de bajada no es maquinaria: es esta fila.** ACTUALIZAR ya se trae el árbol entero del kit,
así que **si el kit lleva un buzón, las cartas bajan con la actualización** — sin red, sin API y sin
credenciales, que es lo que mantiene el marco sin runtime. La subida no tiene equivalente: necesita un
cartero humano, y por eso el marco **estandariza la carta y nunca el canal**. Y esta revisión **no va
en ABRIR** aunque tiente: mirar el buzón en cada sesión rompería el arranque barato, que es un pilar.

**Si el diff muestra cambios que no vienen de arriba sino de ediciones locales del kit, para y
avisa**: el kit no se edita dentro de un proyecto (para eso está la config), y re-vendorizar los
borra. Recupéralos o descártalos con el usuario antes de seguir, nunca en silencio.

**Sin marcador de versión, a propósito.** El kit no lleva `VERSION` ni changelog: el diff dice *qué*
cambió y dónde, que es lo único accionable, y un número habría que acordarse de subirlo en cada
cambio. El porqué, en `GUIDE.md` → "Alternativas descartadas".

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

## Operaciones de bajo coste (preferir siempre)
- Apéndice de una fila → `printf '...' >> archivo` (sin `Read` previo).
- Archivo pequeño de formato fijo → un `Write` completo (no varios `Edit`).
- Buscar en archivo grande → `grep -n` y luego leer solo el rango.
- Volumen mecánico grande (dividir un doc de 1000+ líneas) → delegar a un subagente. **Dile dónde
  escribir:** un subagente trae su propio temporal privado, aún menos visible que el tuyo, así que
  esta recomendación multiplica el problema que resuelve el hogar de artefactos si no se le nombra el
  destino de forma explícita.
