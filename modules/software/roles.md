# modules/software/roles.md — Roles del módulo `software`

> Roles que este módulo **añade** al núcleo cuando está activo (`módulos: [… software …]` en el
> manifiesto). Mismo formato y semántica que `core/roles.md`; se fusionan por `order`.

## Roles

| Rol | Nombre default | Ubicación | startup | order | Propósito |
| --- | --- | --- | --- | --- | --- |
| gotchas | MEMORY.md | base | obligatorio | 20 | Trampas y convenciones no evidentes al leer el código. Hogar único; se cura. |
| specs | REQUIREMENTS.md | base | on-demand | — | Qué hace el producto: specs, contratos, modelo de datos, decisiones por feature. |
| architecture | ARCHITECTURE.md | base† | on-demand | — | Cómo está organizado el código: mapa de módulos + patrones reutilizables. |
| effort | ESFUERZO.md | history | on-demand | — | Log append-only de esfuerzo-equivalente por sesión. Opcional (feature `effort_log`). |
| specs_dir | temas/ | base | contenedor | — | Carpeta de los temas extraídos de `specs`. No es un doc. |

† `architecture` es **uno por codebase**: en monorepos el nombre se prefija por área (`<área>/ARCHITECTURE.md`).

`specs_dir` se resuelve relativo a `base` y alimenta el token `{{specs_dir}}` de la plantilla
`specs`: cuando una decisión supera ~50 líneas se extrae a `{base}/{specs_dir}/<TEMA>.md`.

## Triggers (enrutamiento)

| Rol | Necesito… |
| --- | --- |
| gotchas | convención/gotcha técnico antes de escribir código |
| specs | specs, contratos de API, modelo de datos, decisiones de producto por feature |
| architecture | estructura + patrones reutilizables de un codebase |
| effort | esfuerzo-equivalente por funcionalidad |
