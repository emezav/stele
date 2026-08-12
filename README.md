# stele — Marco modular de documentación y continuidad para agentes

Sistema portátil para que **cualquier agente de IA** (Claude Code u otro) trabaje sobre un proyecto
con **memoria persistente entre sesiones** y **coste de tokens acotado**. Abstrae el marco que
evolucionó en un proyecto real a lo largo de 240+ sesiones. **Modular y configurable:** un núcleo
agnóstico de dominio (sirve para software y para trabajo no-software: materiales, planeación,
investigación) + módulos que añaden disciplinas específicas + una capa de config.

**"Acotado" con un número, porque una promesa sin cifra no se puede comprobar.** Y con sus
condiciones al lado, porque **lo que se paga cada sesión depende del layout**:

- **Layout vendorizado (`.stele/` + loader, el default): lo que se paga siempre es el *set de
  arranque*, no el kit.** Son el loader y los cuatro docs que importa: **~3 900 tokens** en un
  proyecto recién bootstrapeado. `SKILL.md` cuesta **cero** — medido en seis sesiones reales de un
  agente que no es Claude Code, ninguna lo abrió.
- **Layout `skill` (`.claude/skills/stele`):** invocar `/stele` carga el enrutador entero,
  **~4 500 tokens**. Antes de partirlo por ritual eran **~36 200**; los rituales caros ya no viajan
  con él — auditar cuesta sus ~12 400 solo la sesión que audita. El reparto está en `guide.md`.

**El set de arranque crece con el proyecto, y ese es el número que hay que vigilar.** Las ~3 900 son
el suelo de un proyecto nuevo; en la instancia con la que se desarrolla este marco eran **~10 400 tras
57 sesiones** y son **16 660 tras 108** (medido el 2026-08-12). El kit no crece —es de tamaño fijo—;
crecen los docs de la instancia.

**Y este párrafo decía que los que crecían eran `latest` y `handover`, *"por eso el manifiesto les pone
presupuesto"*. Era falso, y el presupuesto salió de ahí.** Repartido ese mismo arranque de 16 660:

```text
gotchas   10 690   64%   sin tope total hasta hoy
entry      3 291   20%   sin tope hasta hoy
state      1 964   12%   con tope desde el principio
handover     210    1%   con tope desde el principio
puerta       505    3%   generada
```

**Los dos que llevaban presupuesto son el 13% del total.** Se acotó lo que era fácil de acotar —dos
docs de formato fijo, que se sobrescriben y por eso no crecen— y quedó libre justo el que crece por
acumulación. No fue un descuido de cifras: **la frase que justificaba el presupuesto nombraba mal a su
sujeto**, y mientras siguiera en pie nadie iba a buscar el tope que faltaba. Corregido: `gotchas` y
`entry` tienen tope en `guide.md`, y su tabla marca cuáles importa la puerta.

> **El nombre.** Una *estela* (gr. στήλη) es la piedra erguida e **inscrita** que deja un registro
> perdurable para quien viene después. Es justo lo que hace este marco entre sesiones: inscribir el
> estado y el porqué para que el siguiente agente retome el rumbo sin reconstruirlo.

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

**`guide.md` NO hace falta para instalar, y es el archivo más caro del kit.** Es el *por qué* del
diseño, para quien decide adoptarlo o quiere entender las fronteras; leerlo en el bootstrap costaba
~7 000 tokens, casi el 20% de la instalación, medido en campo. Léelo cuando quieras entender el
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
  preguntas que solo pueden responder proyectos reales. Se contesta con el ritual REMITIR, por el
  canal que prefieras — copiar y pegar basta.

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

2. Pide al agente **"bootstrapea la stele"** (ritual BOOTSTRAP, al que enruta `.stele/SKILL.md`): detecta
   greenfield vs adopción, elige `idioma`/`módulos` y las tres rutas (con defaults), te muestra el
   layout resuelto, escribe `stele.config.md` en la raíz, instancia las plantillas bajo `base`, y
   genera las puertas de auto-arranque + el mapa-doc.
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
