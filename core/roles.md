# core/roles.md — Roles del núcleo (fuente del mapa derivado)

> **Fuente de verdad de los roles agnósticos** del marco. De aquí se generan los dos artefactos
> derivados (lista de arranque + tabla de enrutamiento) combinando estos metadatos con el binding
> `rol → nombre` del manifiesto (`stele.config.md`). Los ids de rol son **estables y no se
> traducen**; los nombres son defaults que la config puede reescribir.

## Resolución de ruta

`ruta = {base}/[<valor de history_dir> si ubicación=history]{nombre}`, donde `<valor de history_dir>`
es el nombre de carpeta del manifiesto (`HISTORY/`). Un rol con nombre `—` en la config está
**desactivado** (no aparece en arranque ni en el mapa).

**No confundir el valor con el token.** El manifiesto guarda el nombre de la carpeta; el token
`{{history_dir}}` que usan las plantillas resuelve a la ruta completa desde la raíz, **con `base` ya
delante**. Por eso `{{history_dir}}{{index}}` es una ruta ejecutable y `{base}/{{history_dir}}…`
duplicaría `base`. Ver `SKILL.md` → "Convención de tokens".

**Ningún rol vive en el kit.** `base` (docs instanciados) y `kit` (el marco) son rutas
independientes del manifiesto: los roles se resuelven siempre bajo `base`; `{{kit}}` solo aparece
como referencia *hacia* el marco dentro de las plantillas. El `loader` es la tercera ruta y no es
un rol: se genera en la raíz. Ver `GUIDE.md` → "Las tres rutas".

## Roles

| Rol | Nombre default | Ubicación | startup | order | Propósito |
| --- | --- | --- | --- | --- | --- |
| entry | AGENTS.md | base | obligatorio | 10 | Cómo trabajar: proceso, estructura, convenciones, arranque. Punto de entrada único. |
| state | LATEST.md | history | obligatorio | 30 | Dónde estamos y el próximo paso. Se **sobrescribe**; acotado. |
| handover | HANDOVER.md | history | obligatorio* | 40 | Checkpoint de trabajo en curso. *Se lee al abrir solo si su Estado ≠ `SIN_TRABAJO_ACTIVO`. |
| charter | DESIGN.md | base | on-demand | — | Por qué el proyecto es así: norte, principios, restricciones, decisiones grandes, glosario. |
| protocol | PROTOCOL.md | base | on-demand | — | Formatos y convenciones de documentación entre sesiones. |
| index | INDEX.md | history | on-demand | — | Índice append-only de sesiones. Se lee con grep. |
| session | sesion-{NNN}-{YYYY-MM-DD}.md | history | on-demand | — | Registro por sesión. Inmutable; se lee con grep. |
| history_dir | HISTORY/ | base | contenedor | — | Carpeta que agrupa state/handover/index/session. No es un doc. |

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

## Notas

- **`startup` + `order`** alimentan la *lista de lectura de arranque* (roles `obligatorio` ordenados
  por `order`, nombre resuelto por la config). Los `on-demand` van a la nota "lee lo demás con grep".
- **`triggers`** alimenta la *tabla de enrutamiento* del mapa de documentación.
- Los módulos activos aportan más roles (ver `modules/<mód>/roles.md`); se fusionan con estos por
  `order`. Que `gotchas` sea obligatorio-de-arranque, por ejemplo, lo aporta el módulo `software`.
