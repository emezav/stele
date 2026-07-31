# MODULAR.md — Rediseño modular + configurable del marco (EN DISEÑO)

> **Estado: análisis/diseño, NO implementado.** Este documento captura las decisiones tomadas
> para evolucionar `.stele/` de un kit fijo a un marco **modular + configurable**. Cuando se
> implemente, su contenido se reparte a `GUIDE.md`/`SKILL.md`/`templates/` y este archivo se retira
> o se convierte en changelog. No confundir con el marco YA vigente en la raíz del repo.

## Objetivo

Que el marco sirva **más allá de software** (preparar materiales, planear actividades,
investigación) sin perder los aspectos de desarrollo. Para eso: un **núcleo agnóstico** de
documentación/continuidad + **módulos** que aporten disciplinas específicas (el primero: software),
y una **capa de configuración** que adapte nombres y parámetros a las preferencias de quien lo usa.

## Decisiones tomadas

1. **Separar ROL de NOMBRE.** El marco se define sobre **ids de rol estables** (agnósticos de
   dominio e idioma); una **config** enlaza rol → nombre/ruta concreta. Rituales, punteros y el
   "un hogar por dato" se expresan en roles.
2. **Modelo de 3 capas:** núcleo (roles + rituales + principios) · módulos (paquetes de
   roles+disciplinas con defaults) · config (binding + toggles + parámetros, por proyecto).
3. **Config = fuente única, en markdown** (`stele.config.md` en la raíz). Todo lo accionable en
   **tablas** (fila clave→valor) para parseo sin ambigüedad; la prosa solo explica.
4. **Profundidad N2:** nombres + toggles (módulos/features) + presupuestos + wording de rituales +
   **idioma**. (N3 = roles/módulos custom del usuario → futuro, no ahora.)
5. **Indirección: resolver en bootstrap** (los docs instanciados llevan **nombres concretos y
   legibles**), y **config-mode es el único renombrador sancionado** (usa el manifiesto como mapa
   rol→nombre; hace `git mv` + barrido de referencias + regenera los derivados). Se descartó la
   referencia por rol en vivo (`[state]`) porque markdown no tiene indirección nativa y el marco es
   agente-primero.
6. **Idioma como parámetro** (`idioma`): en N2 dirige qué variante de template se scaffoldea y el
   wording de los prompts de ritual — no traducción runtime del contenido de proyecto. Los headers
   `##` del manifiesto y los ids de rol/feature son **tokens fijos** (no se traducen) → el parseo
   nunca depende del idioma.
7. **Artefactos derivados (generados desde el manifiesto, no editados a mano):** el **auto-arranque**
   (hoy `CLAUDE.md`) y el **mapa de documentación** (la tabla "necesito X → tal archivo").

## Rituales

`abrir` · `checkpoint` · `cerrar` · **`config`** (nuevo 4º verbo: leer manifiesto → proponer/recoger
cambios → aplicar consistente → regenerar derivados → validar sin referencias colgadas).

## Vocabulario de roles

| Rol (id) | Qué es | Nombre default | Origen |
|---|---|---|---|
| entry        | cómo trabajar / punto de entrada | AGENTS.md | núcleo |
| charter      | por qué: norte, principios, restricciones, decisiones grandes, glosario | DESIGN.md | núcleo |
| protocol     | formatos/reglas de doc | PROTOCOL.md | núcleo |
| state        | dónde estamos (se sobrescribe) | LATEST.md | núcleo |
| handover     | trabajo en curso (checkpoint) | HANDOVER.md | núcleo |
| index        | índice de sesiones | INDEX.md | núcleo |
| history_dir  | carpeta de historial | HISTORY/ | núcleo |
| session      | registro por sesión | sesion-{NNN}-{YYYY-MM-DD}.md | núcleo |
| specs        | detalle/decisiones por unidad | REQUIREMENTS.md | software |
| architecture | estructura + patrones | ARCHITECTURE.md | software |
| gotchas      | trampas de código | MEMORY.md | software |
| effort       | log de esfuerzo | ESFUERZO.md | software |

## Forma del manifiesto (`stele.config.md`)

Secciones fijas (`##`): **Meta** (idioma, módulos) · **Nombres** (rol→archivo, col. Origen; `—` =
desactivado) · **Features** (toggles) · **Presupuestos** (rol→máx líneas) · **Wording de rituales**.
Contrato de parseo: headers `##` canónicos y fijos; en cada tabla col1=clave, col2=valor, filas extra
se ignoran; `—`=desactivado; fila ausente=default del rol/feature; al aplicar, config-mode reescribe
la tabla **completa** (Write) y regenera auto-arranque + mapa-doc.

## Mapa de documentación derivado

No se escribe a mano: se **genera** desde metadatos de rol + el binding de la config. Cada rol
declara `startup` (obligatorio|on-demand), `order` y `triggers` (frases "necesito…" que enrutan a
ese hogar). Produce **dos artefactos** desde la misma fuente: (1) la **lista de lectura de arranque**
(roles `obligatorio` ordenados por `order`, nombre resuelto por config) y (2) la **tabla de
enrutamiento** `trigger → nombre-configurado`. Que `gotchas` sea obligatorio-de-arranque lo aporta el
**módulo software**; núcleo-solo no lo incluye. Regla: roles activos = núcleo + módulos activos −
los `—`. Renombrar/activar/desactivar módulo → ambos artefactos se regeneran, sin ediciones manuales
ni referencias colgadas.

## Ritual `config` (4º verbo, único renombrador sancionado)

Fases: **leer+reconciliar** (contrasta manifiesto vs archivos reales; detecta/ofrece arreglar
drift) · **clasificar** el cambio por radio de impacto (renombrar / toggle módulo / toggle feature /
presupuesto / wording / idioma / base) · **previsualizar** (dry-run + confirmar; renombrar toca
varios archivos) · **aplicar** en orden seguro y **acotado a los docs del marco** (`git mv` →
reescribir tabla completa del manifiesto → barrido de referencias por el mapa viejo→nuevo, **nunca
código de producto** → regenerar derivados) · **validar** (`grep` viejo=0; cada nombre resuelve a un
archivo; ningún rol activo apunta a faltante). Reglas fijas: desactivar módulo **no** borra sus docs
(quedan huérfanos preservados + aviso); colisión de nombre aborta; cambiar patrón `session` afecta
solo sesiones futuras (el historial pasado es inmutable). Seguro **porque** el marco se auto-documenta.

## Ubicación de los docs: parámetro `base`

Un solo parámetro captura raíz/subdir × visible/oculto (la visibilidad va implícita en la ruta):
`base = .` (raíz, **default**) · `docs/` (subdir visible) · `.notas/` (subdir oculto). Todo lo demás
es relativo a `base` (`{base}/HISTORY/`, `{base}/LATEST.md`…). Default `.` por: convención
`AGENTS.md`/`CLAUDE.md` en raíz, auto-arranque sin indirección, y = layout actual de este repo (no
migrar para validar). **Dos anclas fijas en la raíz** (no siguen a `base`): el **loader de
auto-arranque** (`CLAUDE.md`/`AGENTS.md` mínimo, apunta a `base`) y el **manifiesto**
(`stele.config.md`). Se añade a la tabla Meta: `base`.

## Estructura del kit y migración

Separa maquinaria (`.stele/`) de docs instanciados (`base`). Kit: `README.md` · `GUIDE.md`
(fundamentos agnósticos) · `SKILL.md` (rituales) · `core/{roles.md, templates/}` · `modules/software/
{module.md, roles.md, conventions.md, templates/}`. **Templates nombrados por ROL** (`entry.md`, no
`AGENTS.md`); el nombre de salida vive en `roles.md`/config. `roles.md` es la fuente del mapa
derivado. Migración del kit plano actual: `templates/AGENTS→core/templates/entry`, `DESIGN→charter`,
`PROTOCOL→protocol`, `HISTORY/LATEST→state`, `HISTORY/HANDOVER→handover`, `HISTORY/INDEX→index`,
`HISTORY/sesion-NNN→session`, `CLAUDE→autostart`; `ARCHITECTURE`/`MEMORY→gotchas`/`HISTORY/ESFUERZO→
effort` al módulo software; extraer lo software de `GUIDE.md`; crear `core/roles.md`, `modules/
software/{module,roles,conventions}.md`, `core/templates/config.md`, `specs.md`; añadir `config` a
`SKILL.md`.

## Bootstrap (nacer: instanciar un proyecto desde el kit)

**Dos modos:** *greenfield* (no hay docs → scaffold desde templates) y *adopción* (ya existen docs,
como ESTE repo → mapea a roles, escribe config apuntando a ellos, **no sobreescribe contenido**, solo
genera lo que falte). 8 pasos: (1) elegir `idioma`/`módulos`/`base` con defaults sensatos y
**auto-detección** de módulo (Cargo.toml/package.json/src → software) — zero-question posible, o una
confirmación de los 3 de alto impacto; (2) resolver nombres (defaults de rol+módulo, override libre);
(3) escribir el manifiesto en la raíz; (4) scaffold por rol → nombre configurado bajo `base`,
**resolviendo tokens** (aquí se materializa la decisión #5: cross-refs de rol → nombres concretos →
docs legibles; en adopción se salta para los ya existentes); (5) semilla de estado (state/handover
`SIN_TRABAJO_ACTIVO`, index vacío); (6) generar derivados (loader de auto-arranque en raíz + mapa-doc
en `entry`); (7) validar; (8) confirmar + activar (reabrir editor → auto-carga, sin pasos manuales).
Progressive disclosure: lo fino (nombres/presupuestos/wording) se deja para `config`.

## Ciclo de vida completo

**bootstrap** (nacer) → **abrir / checkpoint / cerrar** (operar) → **config** (adaptar). Tres capas
(núcleo / módulos / config); todo lo derivado (auto-arranque + mapa-doc) sale del manifiesto.

## Pendiente (orden de trabajo)

- [x] Pilares/rituales agnósticos + tabla núcleo-vs-módulo-software
- [x] Las 6 decisiones de arriba
- [x] Forma concreta del manifiesto markdown
- [x] Mapa de documentación derivado (metadatos de rol + 2 artefactos)
- [x] Ritual `config` paso a paso
- [x] Parámetro `base` (ubicación de docs) + estructura del kit + migración
- [x] Bootstrap paso a paso
- [x] **Diseño completo**
- [x] **Implementado (s242):** kit reestructurado a `core/`+`modules/software/` (git mv); `roles.md`
      de núcleo y software; `module.md`+`conventions.md`; `config.md` + `autostart.md` con tokens;
      todos los templates por rol con tokens `{{rol}}`; `specs.md` nuevo; GUIDE agnóstico + 3 capas;
      SKILL con bootstrap/config + regla del mapa derivado; README modular. Validado (roles↔templates,
      sin refs colgadas). **Dogfood:** `stele.config.md` en la raíz (adopción, base=., software) —
      los 12 roles resuelven a archivos reales.
- [ ] Pendiente: commit + push; y (futuro) reescribir este MODULAR.md como changelog o retirarlo.
