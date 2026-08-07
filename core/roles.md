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
duplicaría `base`. Ver `SKILL.md` → "Convención de tokens".

**Ningún rol vive en el kit.** `base` (docs instanciados) y `kit` (el marco) son rutas
independientes del manifiesto: los roles se resuelven siempre bajo `base`; `{{kit}}` solo aparece
como referencia *hacia* el marco dentro de las plantillas. El `loader` es la tercera ruta y no es
un rol: se genera en la raíz. Ver `guide.md` → "Las tres rutas".

## Roles

| Rol | Nombre default | Ubicación | startup | order | Propósito |
| --- | --- | --- | --- | --- | --- |
| entry | AGENTS.md | base | obligatorio | 10 | Cómo trabajar: proceso, estructura, convenciones, arranque. Punto de entrada único. |
| state | latest.md | history | obligatorio | 30 | Dónde estamos y el próximo paso. Se **sobrescribe**; acotado. |
| handover | handover.md | history | obligatorio* | 40 | Checkpoint de trabajo en curso. *Se lee al abrir solo si su Estado ≠ `SIN_TRABAJO_ACTIVO`. |
| charter | design.md | base | on-demand | — | Por qué el proyecto es así: norte, principios, restricciones, decisiones grandes, glosario. |
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

## Triggers (enrutamiento: "necesito… → hogar")

| Rol | Necesito… |
| --- | --- |
| entry | cómo trabajar, proceso, reglas operativas, arranque |
| charter | por qué: principios, decisiones grandes, restricciones, glosario |
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
  `order`. Que `gotchas` sea obligatorio-de-arranque, por ejemplo, lo aporta el módulo `software`.
- **Varios roles nacen del uso y no del scaffold**, y por la misma razón: una carpeta o un log vacíos
  en cada adopción son peso muerto, y su **ausencia es el dato**.
  - **`audit`** lo crea la primera auditoría que corre; que no exista significa que el proyecto nunca
    se ha auditado.
  - **`correspondence`, `letter` y `correspondence_dir`** los crea la primera carta, en cualquiera de
    las dos direcciones; que no existan significa que este proyecto no ha intercambiado nada con
    fuera.
  - **`artifacts_dir`** lo crea el primer artefacto; que no exista significa que ninguna sesión ha
    necesitado producir nada fuera de la documentación.
- **La correspondencia tiene la misma forma que el historial**, y por eso no hay nada nuevo que
  aprender: un **índice** (`correspondence`, como `index`), un **archivo por pieza** (`letter`, como
  `session`, inmutable y numerado) y su **carpeta** (`correspondence_dir`, como `history_dir`). La
  única diferencia real es que la numeración de `letter` **no distingue dirección**: una carta
  enviada y su respuesta son consecutivas, así que el hilo es el orden.
- **Los dos contenedores se comportan distinto en el enrutamiento.** `history_dir` agrupa cuatro roles
  y no tiene trigger propio: nadie pregunta dónde va `history/`, se pregunta por `state` o por
  `index`. `artifacts_dir` **sí** lo tiene, porque él mismo es el destino: no contiene roles, contiene
  archivos sueltos, y "dónde pongo este script" es exactamente una pregunta de enrutamiento.
