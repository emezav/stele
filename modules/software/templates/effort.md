# {{effort}} — Esfuerzo humano equivalente por sesión (append-only, OPCIONAL)

> Una fila por sesión. `{{effort_unit}}` = rango que le tomaría a UN ingeniero senior hacer el
> mismo trabajo con calidad de producción (investigación + implementación + validación + docs). El
> detalle por funcionalidad va en `## Esfuerzo equivalente` del `{{session}}`.
> Al cerrar: `printf '| N | YYYY-MM-DD | X-Y | funcionalidades |\n' >> {{history_dir}}{{effort}}`
>
> (Feature `effort_log`: quitar este archivo y apagar el toggle si el proyecto no lleva la serie.)

| Sesión | Fecha | {{effort_unit}} | Funcionalidades clave |
|--------|-------|-------------|----------------------|
| 1 | <YYYY-MM-DD> | <X-Y> | <funcionalidades> |
