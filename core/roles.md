# core/roles.md — Roles del núcleo (fuente del mapa derivado)

> **Fuente de verdad de los roles agnósticos** del marco. De aquí se generan los dos artefactos
> derivados (lista de arranque + tabla de enrutamiento) combinando estos metadatos con el binding
> `rol → nombre` del manifiesto (`stele.config.md`). Los ids de rol son **estables y no se
> traducen**; los nombres son defaults que la config puede reescribir.

## Resolución de ruta

`ruta = {base}/[<valor del contenedor> si la ubicación nombra uno]{nombre}`, donde el valor del
contenedor es el nombre de carpeta que da el manifiesto (`history/`, `correspondencia/`…). Ubicación
`base` = directo bajo `base`; `history` o `correspondence_dir` = bajo esa carpeta. Un rol con nombre
`—` en la config está **desactivado** (no aparece en arranque ni en el mapa).

**No confundir el valor con el token.** El manifiesto guarda el nombre de la carpeta; el token
`{{history_dir}}` que usan las plantillas resuelve a la ruta completa desde la raíz, **con `base` ya
delante**. Por eso `{{history_dir}}{{index}}` es una ruta ejecutable y `{base}/{{history_dir}}…`
duplicaría `base`. Ver `core/reference/rutas-y-tokens.md` → "Convención de tokens".

**Ningún rol vive en el kit.** `base` (docs instanciados) y `kit` (el marco) son rutas
independientes del manifiesto: los roles se resuelven siempre bajo `base`; `{{kit}}` solo aparece
como referencia *hacia* el marco dentro de las plantillas. El `loader` es la tercera ruta y no es
un rol: se genera en la raíz. Ver `guide.md` → "Las tres rutas" — **el porqué del modelo**, que es lo
que hace falta aquí. Si lo que buscas son los **invariantes que hay que validar**, esos están en
`core/reference/rutas-y-tokens.md`, que es la referencia operativa.

## Roles

| Rol | Nombre default | Ubicación | startup | order | Propósito |
| --- | --- | --- | --- | --- | --- |
| entry | guia-agente.md | base | obligatorio | 10 | Cómo trabajar: proceso, estructura, convenciones, arranque. Punto de entrada único. |
| gotchas | memory.md | base | obligatorio | 20 | Trampas y convenciones no evidentes **al trabajar en el proyecto**: del entorno, de las herramientas, de publicar, de verificar. Hogar único; se **cura**. |
| state | latest.md | history | obligatorio | 30 | Dónde estamos y el próximo paso. Se **sobrescribe**; acotado. |
| handover | handover.md | history | obligatorio* | 40 | Checkpoint de trabajo en curso. *Se lee al abrir solo si su Estado ≠ `SIN_TRABAJO_ACTIVO`. |
| charter | design.md | base | on-demand | — | Por qué el proyecto es así: norte, principios, restricciones, decisiones grandes, glosario. |
| manual | manual.md | base | on-demand | — | **Para la persona, no para el agente**: qué se espera de ella, qué avisos verá y qué significan. Escrito en el vocabulario del proyecto. Opcional (feature `manual_doc`). |
| protocol | protocol.md | base | on-demand | — | Formatos y convenciones de documentación entre sesiones. |
| index | index.md | history | on-demand | — | Índice append-only de sesiones. Se lee con grep. |
| session | sesion-{NNN}-{YYYY-MM-DD}.md | history | on-demand | — | Registro por sesión. Inmutable; se lee con grep. |
| audit | audit.md | history | on-demand | — | Log append-only de auditorías de documentación. Opcional (feature `audit_log`). |
| correspondence | cartas.md | correspondence_dir | on-demand | — | Índice append-only del intercambio con el exterior, **en las dos direcciones**, con lo aceptado y **lo rechazado y por qué**. Opcional (feature `correspondence_log`). |
| letter | carta-{NNN}-{YYYY-MM-DD}.md | correspondence_dir | on-demand | — | Una carta, enviada o recibida. Numeración **única para ambas direcciones**: leer 1..N es leer la conversación. Inmutable. |
| correspondence_dir | correspondencia/ | base | contenedor | — | Carpeta del intercambio con el exterior. No es un doc. |
| history_dir | history/ | base | contenedor | — | Carpeta que agrupa state/handover/index/session. No es un doc. |
| artifacts_dir | artefactos/ | base | contenedor | — | Hogar de los **artefactos** que una sesión produce y no son documentación: scripts de un solo uso, extracciones, volcados intermedios. Subdirectorio por sesión. **No se instancia en bootstrap**: lo crea el primer artefacto. |

El patrón de numeración de `session` (`{NNN}`) vive **solo aquí**: es parte de su nombre, no un
parámetro aparte. Cambiarlo afecta únicamente a sesiones futuras (el historial es inmutable).

## Convención de nombres: minúscula por defecto

**MAYÚSCULA solo donde la impone algo externo.** Los defaults de arriba están en minúscula porque la
mayúscula solo señala mientras es rara: cuando la lleva todo, deja de distinguir y solo queda el
volumen. Reservada así, **vuelve a significar algo — "este nombre no lo elegimos nosotros"**.

Los que la conservan, y por qué:

| Nombre | Quién lo impone |
| --- | --- |
| `README.md`, `LICENSE` | Convención universal y las plataformas de código |
| `CLAUDE.md` (u otro `loader`) | La herramienta lo busca por ese nombre exacto |
| `AGENTS.md`, `CLAUDE.md` (las **puertas**) | Cada harness auto-carga el suyo de la raíz, con ese nombre exacto |
| `SKILL.md` | El layout `skill`: al vendorizar el kit en `.claude/skills/…`, Claude Code busca ese nombre |

**`SKILL.md` es el que más fácil se cuela en un barrido de minúsculas**, porque no lo impone la
plataforma sino un layout — y bajarlo rompe ese layout **en todo adoptante sin dar error en ningún
sitio**.

Y una consecuencia que ya mordió: **`base` no se llama como el kit.** Ver la tabla de layouts en
`{{kit}}/core/reference/rutas-y-tokens.md` — un nombre que es subcadena de otro no solo confunde a las personas, hace que una
sustitución textual corrompa en silencio.

## Triggers (enrutamiento: "necesito… → hogar")

| Rol | Necesito… |
| --- | --- |
| entry | cómo trabajar, proceso, reglas operativas, arranque |
| gotchas | una trampa no evidente de trabajar aquí: del entorno, de una herramienta, de publicar, de verificar |
| charter | por qué: principios, decisiones grandes, restricciones, glosario |
| manual | qué se espera de mí, qué significa este aviso, qué hace falta que yo haga |
| protocol | formatos/reglas de documentación y registro |
| state | dónde estamos, próximo paso inmediato, retomar contexto |
| handover | trabajo a medias, checkpoint de un salto en curso |
| index | qué pasó y cuándo (índice de sesiones) |
| session | el detalle de una sesión concreta |
| audit | cuándo se auditó la documentación, qué salió y qué se decidió no cambiar |
| correspondence | qué se intercambió con fuera, qué se aceptó, qué se rechazó y por qué |
| letter | el texto de una carta concreta, enviada o recibida |
| artifacts_dir | dónde poner un script de un solo uso, una extracción o un volcado intermedio |

## Notas

- **`startup` + `order`** alimentan la *lista de lectura de arranque* (roles `obligatorio` ordenados
  por `order`, nombre resuelto por la config). Los `on-demand` van a la nota "lee lo demás con grep".
- **`triggers`** alimenta la *tabla de enrutamiento* del mapa de documentación.
- Los módulos activos aportan más roles (ver `modules/<mód>/roles.md`); se fusionan con estos por
  `order`. Que `architecture` exista, por ejemplo, lo aporta el módulo `producto`.
- **`gotchas` está en el núcleo, y no siempre lo estuvo.** Vivió en el módulo de producto (entonces llamado `software`) descrito como
  *"trampas al leer el código"*, y de ahí salían dos agujeros: un proyecto no-software no tenía **hogar
  para sus trampas**, y uno que nacía en carpeta vacía tampoco, porque la detección del módulo corría
  cuando la evidencia todavía no existía. Bajó al núcleo en la sesión 58 por tres razones:
  - **Es el complemento de `history`, y los dos son continuidad, no objeto del trabajo.** El historial
    es episódico, crece y es inmutable: guarda *qué pasó*. `gotchas` es destilado y se cura: guarda
    *qué no es obvio*. Sin el segundo, una lección aprendida solo existe dentro de un acta que nadie
    vuelve a abrir — y por eso `gotchas` se carga en cada arranque y el historial no.
  - **Lo que contiene es proceso, no producto.** Trampas del entorno, de una herramienta, de publicar,
    de verificar, de dónde guardar. Nada de eso depende de que el producto sea código.
  - **ABRIR ya lo daba por hecho.** Un ritual del núcleo llevaba `gotchas` en su lista de arranque
    mientras el rol vivía en un módulo opcional: sin él, apuntaba a un rol inexistente.
- **Varios roles nacen del uso y no del scaffold**, y por la misma razón: una carpeta o un log vacíos
  en cada adopción son peso muerto, y su **ausencia es el dato**.
  - **`audit`** lo crea la primera auditoría que corre; que no exista significa que el proyecto nunca
    se ha auditado.
  - **`correspondence`, `letter` y `correspondence_dir`** los crea la primera carta, en cualquiera de
    las dos direcciones; que no existan significa que este proyecto no ha intercambiado nada con
    fuera.
  - **`artifacts_dir`** lo crea el primer artefacto; que no exista significa que ninguna sesión ha
    necesitado producir nada fuera de la documentación.
- **`manual` es el único rol cuyo lector no es un agente**, y de ahí salen sus dos particularidades.
  Es `on-demand` **para el agente** —no entra en la lista de arranque, que se lee entera cada sesión—
  pero para la persona es lo contrario: es lo primero, y a veces lo único. Y **no se redacta desde la
  plantilla sino desde el proyecto**: la plantilla dice qué preguntas contestar, no con qué palabras.
  Un `manual` que suene al marco en vez de al proyecto ha fallado aunque esté completo.
- **La correspondencia tiene la misma forma que el historial**, y por eso no hay nada nuevo que
  aprender: un **índice** (`correspondence`, como `index`), un **archivo por pieza** (`letter`, como
  `session`, inmutable y numerado) y su **carpeta** (`correspondence_dir`, como `history_dir`). La
  única diferencia real es que la numeración de `letter` **no distingue dirección**: una carta
  enviada y su respuesta son consecutivas, así que el hilo es el orden.
- **Los tres contenedores no se comportan igual en el enrutamiento.** `history_dir` y
  `correspondence_dir` **agrupan roles** y no tienen trigger propio: nadie pregunta dónde va
  `history/`, se pregunta por `state`, por `index` o por `letter`. `artifacts_dir` **sí** lo tiene,
  porque él mismo es el destino: no contiene roles sino archivos sueltos, y "dónde pongo este script"
  es exactamente una pregunta de enrutamiento.
