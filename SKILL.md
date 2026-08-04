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

## Convención de tokens en plantillas

Las plantillas se escriben por **rol** y usan tokens que bootstrap/`config` resuelven a nombres:
`{{rol}}` → nombre del rol (p. ej. `{{state}}`→`LATEST.md`); `{{history_dir}}` y `{{specs_dir}}` →
**rutas** de los roles contenedores; `{{budget:rol}}` → tope de líneas; `{{effort_unit}}` y
`{{checkpoint_trigger}}` → valores de Features/Wording; `{{kit}}` y `{{loader}}` → rutas
(sección Rutas). Los toggles como `session_greeting` **se consultan, no se interpolan**: no hay
token para ellos.
Los bloques marcados `<!-- GENERADO -->` los produce el marco, no se editan a mano.

**Toda ruta interpolada es relativa a la raíz del proyecto**, nunca al doc que la contiene: los
agentes operan con el CWD en la raíz y es lo que hace `grep`, así que el valor no depende de dónde
quedó cada archivo. De ahí dos reglas de composición:

- `{{kit}}` se escribe **sin `/` final** y se usa como `{{kit}}/SKILL.md`. **Con `kit = .` el prefijo
  colapsa**: `{{kit}}/SKILL.md` → `SKILL.md`, no `./SKILL.md`.
- Los **contenedores** (`{{history_dir}}`, `{{specs_dir}}`) resuelven **con `base` delante y con `/`
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
7. **Persistir el cierre** según `persistencia` (manifiesto → Meta). El cierre se escribe primero
   (pasos 1-6) y se persiste **una sola vez**, al final.

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

**El vocabulario es lo único atado al idioma.** Estas listas están en el `idioma` del kit; un
proyecto en otro idioma las traduce y guarda su versión en `protocol` (*Acuerdos de auditoría*), no
en el manifiesto: son una lista larga y viva, no un parámetro.

### Fases

1. **Delimitar** el alcance y decirlo en una línea *antes* de leer nada: rango de sesiones y cuántos
   docs entran. Si sale caro, el momento de acotar es ese, no después.
2. **Barrer** con los detectores. Lo que sale es un **candidato**, no un hallazgo.
3. **Verificar** cada candidato. Aquí se va el grueso del coste. Un hallazgo entra al informe **solo
   con evidencia**: dos punteros que se contradicen (`archivo:línea` de la afirmación + el
   `archivo:línea`, comando o hecho que la desmiente). Sin evidencia es **sospecha** — va aparte y no
   se aplica.
4. **Informar** con la forma fija de abajo, separando errores de preferencias.
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
AUDIT — sesiones 10-24 · 6 docs revisados

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

**Coste de referencia:** la auditoría manual que originó el ritual costó ~1-1,5 horas-ingeniero sobre
~15 docs, con 8 hallazgos (7 errores, 1 preferencia) y la mayor parte del tiempo en **verificar**, no
en encontrar. Si tu barrido produce cincuenta candidatos, el problema es el barrido: acota el alcance
antes de ponerte a verificarlos.

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
| `core/templates/autostart.md`, y los bloques `GENERADO` de `core/templates/entry.md` | Cambió un **derivado**: hay que **regenerar ese bloque** en el archivo real (loader y mapa-doc), conservando íntegro todo lo que quede fuera de las marcas — invariante 6 |
| `core/templates/*` de rol (salvo sus bloques `GENERADO`), `modules/*/templates/*` | **Nada que hacer.** Los docs de `base` ya son del proyecto y no se regeneran jamás |
| `SKILL.md`, `GUIDE.md`, `README.md` | Procedimiento y fundamentos: se leen, no se migran |

**Plantilla de contenido contra plantilla generadora.** Es la distinción que decide las dos filas de
en medio, y confundirlas es un fallo silencioso. Una plantilla de **contenido** produjo un doc que
desde el primer día es del proyecto: se instancia una vez y no se vuelve a tocar jamás. Una plantilla
**generadora** produce un **bloque** que el marco sigue siendo dueño de reescribir —el del loader y
los dos del `entry`— y ese bloque **sí** viaja con cada actualización. Si no se regenera, el
adoptante se queda con el kit nuevo y las reglas viejas cargándose en cada sesión, sin ninguna señal
de que algo falta. Regenerar el bloque **nunca** autoriza a tocar lo que esté fuera de las marcas
(invariante 6).

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
- Volumen mecánico grande (dividir un doc de 1000+ líneas) → delegar a un subagente.
