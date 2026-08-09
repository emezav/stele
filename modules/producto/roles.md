# modules/producto/roles.md — Roles del módulo `producto`

> Roles que este módulo **añade** al núcleo cuando está activo (`módulos: [… producto …]` en el
> manifiesto). Mismo formato y semántica que `core/roles.md`; se fusionan por `order`.

## Roles

| Rol | Nombre default | Ubicación | startup | order | Propósito |
| --- | --- | --- | --- | --- | --- |
| specs | requirements.md | base | on-demand | — | Qué hace el producto: specs, contratos, modelo de datos, decisiones por feature. |
| architecture | architecture.md | base† | on-demand | — | Cómo está organizado el producto (normalmente el codebase): mapa de módulos + patrones reutilizables. |
| effort | esfuerzo.md | history | on-demand | — | Log append-only de esfuerzo-equivalente por sesión. Opcional (feature `effort_log`). |
| specs_dir | temas/ | base | contenedor | — | Carpeta de los temas extraídos de `specs`. No es un doc. |

† `architecture` es **uno por producto** (normalmente un codebase): en monorepos el nombre se prefija
por área (`<área>/architecture.md`). Si el producto no es código, mapea la estructura de ese producto
— ver `module.md` → "Cuando el producto no es código".

`specs_dir` es un rol contenedor: el manifiesto guarda el nombre de la carpeta (`temas/`) y el token
`{{specs_dir}}` de la plantilla `specs` resuelve a la ruta completa desde la raíz, con `base` ya
delante y con `/` final. Cuando una decisión supera ~50 líneas se extrae a `{{specs_dir}}<TEMA>.md`
(sin barra intermedia). Ver `SKILL.md` → "Convención de tokens".

## Triggers (enrutamiento)

| Rol | Necesito… |
| --- | --- |
| specs | specs, contratos de API, modelo de datos, decisiones de producto por feature |
| architecture | estructura + patrones reutilizables del producto (normalmente el codebase) |
| effort | esfuerzo-equivalente por funcionalidad |
