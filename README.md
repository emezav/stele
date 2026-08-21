# stele — Marco modular de documentación y continuidad para agentes

Sistema portátil para que **cualquier agente de IA** (Claude Code u otro) trabaje sobre un proyecto
con **memoria persistente entre sesiones** y **coste de tokens acotado**. Abstrae el marco que
evolucionó en un proyecto real a lo largo de 240+ sesiones. **Modular y configurable:** un núcleo
agnóstico de dominio (sirve para software y para trabajo no-software: materiales, planeación,
investigación) + módulos que añaden disciplinas específicas + una capa de config.

> **El nombre.** Una *estela* (gr. στήλη) es la piedra erguida e **inscrita** que deja un registro
> perdurable para quien viene después. Es justo lo que hace este marco entre sesiones: inscribir el
> estado y el porqué para que el siguiente agente retome el rumbo sin reconstruirlo.

**"Acotado" con un número, porque una promesa sin cifra no se puede comprobar.** Medido contra
`4256a96` con `tiktoken`/`o200k_base` sobre `git cat-file`:

| Lo que pagas | Cuánto | Cuándo se paga |
| --- | --- | --- |
| El **set de arranque** — layout vendorizado, el default | **~3 200** | **cada sesión** |
| El **enrutador** — solo en layout `skill` | 5 714 | al invocar `/stele` |
| **Auditar** (`auditar.md` + `verificar.md`, se leen juntos) | 39 221 | cada ~10 sesiones |
| El kit entero | ~125 000 | **nunca de una vez** |

**Lo único que se paga siempre es el set de arranque**: el loader más los documentos que importa
—`entry`, `gotchas`, `state` y `handover`—. `SKILL.md` cuesta **cero** en el layout vendorizado, y no
por suerte: la puerta **no lo nombra**.

**Ese set crece con TU proyecto, no con el kit.** El kit es de tamaño fijo; lo que engorda son tus
propios documentos. En la instancia con la que se desarrolla este marco eran **~10 400 tras 57
sesiones** y **16 013 tras 108**, repartidos así al cerrar la sesión 108:

```text
gotchas    9 562   60%   trampas del proyecto      <- el que crece
entry      3 291   21%   como se trabaja aqui
state      2 314   14%   donde estamos
handover     341    2%   trabajo a medias
puerta       505    3%   generada
```

**Por eso el marco le pone presupuesto a cada uno de ellos**, y el que domina es el que acumula. Los
topes y qué hacer cuando se cruzan están en `guide.md`.

## 🤖 ¿Eres un agente de IA? Instala stele por tu cuenta

Si tu usuario te dio la URL de este repo y te pidió algo como *"usa stele aquí"* / *"instala stele"*,
haz esto sin pedir más instrucciones:

1. **Vendoriza el kit** en la raíz del proyecto, en `.stele/` (elige la que funcione en tu entorno):

   ```bash
   git clone --depth 1 https://github.com/emezav/stele .stele && rm -rf .stele/.git
   # alternativa con Node: npx degit emezav/stele .stele
   ```

   `.stele/` es el default de la ruta **`kit`**, y es **independiente** de dónde vayan los docs del
   proyecto (ruta `base`). Si el usuario nombró una carpeta sin más ("usa stele aquí, base stele"),
   eso es `base`, no `kit`: no muevas el kit salvo que diga "kit" o "marco". Si **sí** nombró dónde
   va el kit, clona directamente ahí — no clones en `.stele/` para mover después.
2. **Lee `.stele/SKILL.md`** y ejecuta el **ritual BOOTSTRAP** al que enruta: detecta *greenfield* vs
   *adopción* (si el repo ya tiene docs, mapéalos sin sobrescribir), elige `idioma`/`módulos` y las
   tres rutas `kit`/`base`/`loader` con defaults sensatos (el módulo de producto **no se olfatea**: se
   resuelve por criterio — y *"todavía no"* es una respuesta válida — y se declara en el eco), **hace eco del layout
   resuelto antes de escribir**, escribe
   `stele.config.md` en la raíz, instancia las plantillas y **genera** las puertas de auto-arranque
   (`CLAUDE.md`) + el mapa de documentación.
3. **Confirma** al usuario con el saludo de arranque (1-3 líneas) y explícale que a partir de la
   próxima apertura del editor el marco se activa solo.

Si lo que te piden es *"actualiza stele"*, el ritual es **otro**: ACTUALIZAR, al que enruta `.stele/SKILL.md`.
No hay archivo de versión que consultar — lo que manda es el **diff** entre el kit que ya tienes y el
nuevo. Por eso la copia nueva se trae **a un temporal, al lado**, y no encima: primero se lee el
diff, y solo después se sustituye el kit.

No improvises la estructura: la fuente de verdad de rituales es `.stele/SKILL.md`, y las fronteras
que necesitas para instalar —las tres rutas, sus invariantes y la convención de tokens— están en
`.stele/core/reference/rutas-y-tokens.md`. Esos dos, antes de escribir nada.

**`guide.md` NO hace falta para instalar, y es de los caros.** Es el *por qué* del diseño, para quien
decide adoptarlo o quiere entender las fronteras; **pesa más que la instalación entera**
—`bootstrap.md` + `rutas-y-tokens.md`, que son 6 224 tokens fijos—, así que leerlo en el
bootstrap **multiplica por más de dos y medio** lo que hay que leer. Léelo cuando quieras entender el
marco, no para montarlo.

## Arquitectura (tres capas)

- **Núcleo** (`core/`) — roles + rituales + principios, agnósticos. Se define sobre **ids de rol
  estables**, no sobre nombres de archivo. Roles en `core/roles.md`; plantillas en `core/templates/`.
- **Módulos** (`modules/<nombre>/`) — paquetes de roles + disciplinas. Incluido: `producto`
  (añade `specs`/`architecture`/`effort` + convenciones + la regla del checkpoint antes del primer
  archivo de código). Un proyecto sin producto con estructura no lo activa. `módulos = software`, su
  nombre anterior, sigue siendo válido para siempre.
- **Config** (`stele.config.md`, en la raíz del proyecto destino) — **fuente única** que enlaza
  `rol → nombre`, activa módulos, fija toggles/presupuestos/wording/idioma, declara las **tres
  rutas** — `kit` (dónde vive el marco, default `.stele`), `base` (dónde viven tus docs, default
  `.`) y `loader` (**la lista de puertas** de auto-arranque — `CLAUDE.md` para Claude Code,
  `AGENTS.md` para Codex y la mayoría, y la tuya si tu harness lee otra) — y la **persistencia** (`git` ·
  `ninguna` · `comando`: cómo se vuelve durable el trabajo al cerrar). De aquí se **generan** el
  auto-arranque y el mapa de documentación.

## Qué contiene esta carpeta

- **`SKILL.md`** — hoja operativa y **enrutador**: qué ritual existe, cuándo se invoca y dónde vive;
  más lo que hace falta *antes* de elegir ritual (rutas, cómo se le habla al usuario, precedencia
  frente al harness, la regla dura del checkpoint). Léelo primero. **Los rituales no están aquí**:
  cada uno vive en `core/rituals/` y se lee solo cuando se invoca — es lo que mantiene acotado el
  coste del arranque.
- **`core/rituals/`** — un archivo por ritual: `abrir`, `cerrar`, `auditar`, `contrastar`, `remitir`,
  `bootstrap`, `actualizar`, `configurar`.
- **`core/reference/`** — referencia que se lee bajo demanda: `rutas-y-tokens.md` (las tres rutas,
  los layouts con nombre y la convención de tokens de las plantillas).
- **`guide.md`** — el *por qué*: pilares, arquitectura de capas, roles y fronteras, presupuestos.
  Referencia; se lee una vez.
- **`core/`** — `roles.md` (roles del núcleo, fuente del mapa derivado) + `templates/` (plantillas
  por rol: `entry`, `gotchas`, `charter`, `protocol`, `state`, `handover`, `index`, `session`,
  `audit`, `correspondence`, `letter`, `manual`, `autostart`, `config`).
- **`modules/producto/`** — `module.md` (manifiesto), `roles.md`, `conventions.md` y `templates/`
  (`specs`, `architecture`, `effort`).
- **`buzon.md`** — correspondencia de stele hacia quien usa el marco. **Baja sola** con cada
  actualización (no hay servicio ni cuenta: el kit se copia, y las cartas viajan con él). Contiene
  preguntas que solo pueden responder proyectos reales. Se contesta con el ritual REMITIR, y
  **el canal de vuelta son los issues de este mismo repo** — copiar y pegar basta.

**Y no hace falta esperar a que preguntemos.** Si adoptas el marco y algo te falla —una trampa de tu
máquina, una regla que seguiste y aun así te mordió, una de nuestras cifras que no te cuadra al
re-correrla—, **eso es lo que a este proyecto le cambia reglas de verdad**, y hasta hoy no lo pedía en
ningún sitio. Los detalles de qué es útil mandar están en `buzon.md`; el sitio son los **issues**.

> **Por qué importa más de lo que parece.** Este marco verifica su propia documentación con detectores
> que él mismo escribe, y su ritual de auditoría lo dice sin rodeos: *un corresponsal es el único
> detector que no comparte tu alcance*. Todo lo demás que corre aquí —los barridos, las pasadas, los
> controles— lo escribió quien también escribió los documentos.

Las plantillas se escriben **por rol** con tokens `{{rol}}`; bootstrap/`config` los resuelven a los
nombres del manifiesto (los docs instanciados quedan con nombres concretos y legibles).

## Instalar en un proyecto (vendorizado)

El marco es **markdown puro** (sin runtime): se instala **vendorizando** una copia del kit dentro
del proyecto, en la ruta `kit` (default `.stele/`). La copia es la fuente para el agente; para
actualizar se sustituye entera con el ritual **ACTUALIZAR** (que primero saca el diff y luego
reconcilia tu instancia) — por eso **tus docs (`base`) nunca pueden vivir dentro del kit**.

1. Trae el kit a `<proyecto>/.stele/` (elige uno):

   ```bash
   # opción a — degit (sin historia git, recomendado)
   npx degit emezav/stele .stele
   # opción b — clonar y copiar
   git clone --depth 1 git@github.com:emezav/stele.git /tmp/stele && mkdir -p .stele && cp -r /tmp/stele/. .stele/ && rm -rf .stele/.git
   ```

2. **No tienes que pedir nada.** El agente lee `.stele/SKILL.md`, ejecuta el ritual BOOTSTRAP y te lo
   deja listo: detecta si el proyecto empieza de cero o ya tiene docs, resuelve `idioma`, `módulos` y
   las tres rutas con defaults sensatos, **te enseña el resultado antes de escribir nada**, escribe
   `stele.config.md` en la raíz, instancia las plantillas bajo `base` y genera las puertas de
   auto-arranque + el mapa-doc. Si tu agente se queda a medias, dile que **continúe la instalación**.
3. Las **puertas** (ruta `loader`) son la activación automática, y **sus nombres los imponen los
   agentes, no el marco**: `CLAUDE.md` para Claude Code, `AGENTS.md` para Codex y la mayoría. Se
   escriben **todas**, para que el proyecto abra con cualquiera de ellos y equivocarse con una no deje
   el marco mudo. El agente las
   carga al iniciar la sesión, hace `@`-import del set de arranque y saluda con 1-3 líneas (señal de
   que arrancó). Cámbialo si tu agente espera otro nombre (`AGENTS.md`, etc.).
4. (Opcional, Claude Code) para que `/stele` recuerde los rituales bajo demanda, vendoriza el kit
   directamente en `.claude/skills/stele` y pon `kit = .claude/skills/stele`: una sola copia, que se
   actualiza en un solo sitio. Si ya lo tienes en `.stele/`, `cp -r .stele .claude/skills/stele`
   también funciona, pero deja dos copias que hay que mantener sincronizadas a mano.

Para cambiar nombres, activar/desactivar módulos o mover cualquiera de las tres rutas después:
ritual **CONFIG** (no editar los derivados a mano).

**Layouts con nombre:** hay cuatro atajos para las combinaciones habituales de `kit` + `base` —
`default`, `agrupado`, `docs` y `skill`. Puedes pedir uno por su nombre ("bootstrapea con layout
agrupado"), y bootstrap te dice cuál resolvió antes de escribir nada. La tabla con los valores de
cada uno está en `core/reference/rutas-y-tokens.md`; no son un parámetro que se guarde, solo una
forma corta de nombrar dos rutas.

> Detalle de rituales y plantillas: `SKILL.md`. Fundamentos, capas y fronteras: `guide.md`.
> El historial de diseño del marco vive en `git log`, no en el kit: todo lo que se vendoriza es lo
> que un agente necesita para trabajar.

## Cómo se corrigieron las cifras de arriba

**Todas estuvieron mal publicadas en este mismo archivo**, y van aquí porque son la única evidencia
honesta de que el método funciona: un marco que promete detectar documentación que envejece debería
poder enseñar la suya envejeciendo. Ninguna la encontró un lector — las encontró el ritual de
auditoría del propio kit.

**1 · La cifra que nombraba mal a su sujeto.** Decía que auditar cuesta *"~12 400"*. Auditar lee
`auditar.md` **y** `verificar.md`, así que el número real es casi el triple. No estaba solo
caducada: **nombraba un fichero donde la operación usa dos**, y contra eso no protege ningún sello
—certifica que la cifra sigue viva, no que sea la cifra de lo que dices—.

**2 · El presupuesto que acotaba lo que no crecía.** Este archivo decía que los documentos que
crecen son `state` y `handover`, *"por eso el manifiesto les pone presupuesto"*. Son el 17% del
total. Se había acotado **lo que se sobrescribe** —y por construcción no crece— dejando libre **lo
que se acumula**, que es el 60%. El fallo no estaba en las cifras: **la frase que justificaba el
presupuesto nombraba mal a su sujeto**, y mientras siguiera en pie nadie iba a buscar el tope que
faltaba.

**3 · El superlativo.** Este archivo llamaba a `guide.md` *"el archivo más caro del kit"*, con
*"~7 000 tokens"*. Medido en `aac632b`: es el **tercero** —`verificar.md` 22 245 y `auditar.md`
16 314 van delante— y pesa **10 466**, un +50% sobre lo publicado. Las dos afirmaciones fueron
ciertas el día que se escribieron; después se partió otro fichero en dos y **el mismo cambio hecho
para mejorar el marco las volvió falsas**, sin que nadie tocara esa línea. Un superlativo no lleva
cifra, así que ningún barrido de números lo ve — y sin embargo afirma lo mismo que un contador:
**una posición en una lista que sigue moviéndose**.

**Y su corrección tropezó con lo mismo que corregía:** el primer borrador decía *"son 10 466
tokens"* y **la propia edición que lo escribía hizo crecer `guide.md` 108 tokens**. Una cifra exacta
sobre un fichero que el mismo cambio está tocando **nace falsa**. Por eso arriba se sella lo que no
se mueve —los 6 224 de la instalación— y el resto se deja en la tabla de `guide.md`, que lleva su
corpus.

**4 · El reparto que nació caducado.** El bloque de tokens de arriba se escribió a mitad de una
sesión y **esa misma sesión podó uno de los ficheros que medía, una hora después**. Además `state`
y `handover` fluctúan **dentro** de una sesión: un checkpoint abierto multiplica el `handover` por
cinco. Por eso ese bloque lleva el instante en que se midió, y por eso lo que se afirma en prosa es
la **proporción** y no el porcentaje exacto.

> **La regla que sale de las cuatro, y que este proyecto paga cada vez que la olvida:** una cifra
> sobre tu propio corpus es una **foto**. Si la publicas sin decir de cuándo es y contra qué se
> midió, no envejece mal: envejece **en silencio**, que es peor.
