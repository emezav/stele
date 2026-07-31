# {{index}} — Índice de sesiones (append-only)

> Una fila por sesión. Al cerrar, agregar con Bash (sin leer el archivo completo):
> `printf '| N | YYYY-MM-DD | resumen | <session> |\n' >> {{history_dir}}{{index}}`

| Sesión | Fecha | Resumen | Archivo |
|--------|-------|---------|---------|
| 1 | <YYYY-MM-DD> | <resumen de una línea> | <session-001> |
