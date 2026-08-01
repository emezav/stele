# stele — Marco modular de documentación y continuidad para agentes

Sistema portátil para que **cualquier agente de IA** (Claude Code u otro) trabaje sobre un proyecto
con **memoria persistente entre sesiones** y **coste de tokens acotado**. Abstrae el marco que
evolucionó en un proyecto real a lo largo de 240+ sesiones. **Modular y configurable:** un núcleo
agnóstico de dominio (sirve para software y para trabajo no-software: materiales, planeación,
investigación) + módulos que añaden disciplinas específicas + una capa de config.

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
2. **Lee `.stele/SKILL.md`** y ejecuta el **ritual BOOTSTRAP** que describe: detecta *greenfield* vs
   *adopción* (si el repo ya tiene docs, mapéalos sin sobrescribir), elige `idioma`/`módulos` y las
   tres rutas `kit`/`base`/`loader` con defaults sensatos (auto-detecta el módulo `software` por
   `Cargo.toml`/`package.json`/`src/`), **hace eco del layout resuelto antes de escribir**, escribe
   `stele.config.md` en la raíz, instancia las plantillas y **genera** el loader de auto-arranque
   (`CLAUDE.md`) + el mapa de documentación.
3. **Confirma** al usuario con el saludo de arranque (1-3 líneas) y explícale que a partir de la
   próxima apertura del editor el marco se activa solo.

No improvises la estructura: la fuente de verdad de rituales es `.stele/SKILL.md`, y el *por qué* y
las fronteras están en `.stele/GUIDE.md`. Léelos antes de escribir nada.

## Arquitectura (tres capas)

- **Núcleo** (`core/`) — roles + rituales + principios, agnósticos. Se define sobre **ids de rol
  estables**, no sobre nombres de archivo. Roles en `core/roles.md`; plantillas en `core/templates/`.
- **Módulos** (`modules/<nombre>/`) — paquetes de roles + disciplinas. Incluido: `software`
  (añade `specs`/`architecture`/`gotchas`/`effort` + convenciones + la regla del checkpoint antes
  del primer archivo de código). Un proyecto no-software no lo activa.
- **Config** (`stele.config.md`, en la raíz del proyecto destino) — **fuente única** que enlaza
  `rol → nombre`, activa módulos, fija toggles/presupuestos/wording/idioma, declara las **tres
  rutas** — `kit` (dónde vive el marco, default `.stele`), `base` (dónde viven tus docs, default
  `.`) y `loader` (el auto-arranque, default `CLAUDE.md`) — y la **persistencia** (`git` ·
  `ninguna` · `comando`: cómo se vuelve durable el trabajo al cerrar). De aquí se **generan** el
  auto-arranque y el mapa de documentación.

## Qué contiene esta carpeta

- **`SKILL.md`** — hoja operativa: rituales *bootstrap · abrir · checkpoint · cerrar · config*, el
  mapa de documentación (cómo se genera) y la convención de tokens. Léelo primero.
- **`GUIDE.md`** — el *por qué*: pilares, arquitectura de capas, roles y fronteras, presupuestos.
  Referencia; se lee una vez.
- **`core/`** — `roles.md` (roles del núcleo, fuente del mapa derivado) + `templates/` (plantillas
  por rol: `entry`, `charter`, `protocol`, `state`, `handover`, `index`, `session`, `autostart`,
  `config`).
- **`modules/software/`** — `module.md` (manifiesto), `roles.md`, `conventions.md` y `templates/`
  (`specs`, `architecture`, `gotchas`, `effort`).

Las plantillas se escriben **por rol** con tokens `{{rol}}`; bootstrap/`config` los resuelven a los
nombres del manifiesto (los docs instanciados quedan con nombres concretos y legibles).

## Instalar en un proyecto (vendorizado)

El marco es **markdown puro** (sin runtime): se instala **vendorizando** una copia del kit dentro
del proyecto, en la ruta `kit` (default `.stele/`). La copia es la fuente para el agente; para
actualizar, se re-vendoriza — por eso **tus docs (`base`) nunca pueden vivir dentro del kit**.

1. Trae el kit a `<proyecto>/.stele/` (elige uno):

   ```bash
   # opción a — degit (sin historia git, recomendado)
   npx degit emezav/stele .stele
   # opción b — clonar y copiar
   git clone --depth 1 git@github.com:emezav/stele.git /tmp/stele && mkdir -p .stele && cp -r /tmp/stele/. .stele/ && rm -rf .stele/.git
   ```

2. Pide al agente **"bootstrapea la stele"** (ritual BOOTSTRAP en `.stele/SKILL.md`): detecta
   greenfield vs adopción, elige `idioma`/`módulos` y las tres rutas (con defaults), te muestra el
   layout resuelto, escribe `stele.config.md` en la raíz, instancia las plantillas bajo `base`, y
   genera el loader de auto-arranque + el mapa-doc.
3. El **loader** (ruta `loader`, por defecto `CLAUDE.md`) es la activación automática: el agente lo
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
cada uno está en `SKILL.md` → "Layouts con nombre"; no son un parámetro que se guarde, solo una
forma corta de nombrar dos rutas.

> Detalle de rituales y plantillas: `SKILL.md`. Fundamentos, capas y fronteras: `GUIDE.md`.
> El historial de diseño del marco vive en `git log`, no en el kit: todo lo que se vendoriza es lo
> que un agente necesita para trabajar.
