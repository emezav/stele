# {{effort}} — Esfuerzo humano equivalente por sesión (append-only, OPCIONAL)

> Una fila por sesión. `{{effort_unit}}` = rango que le tomaría a UN ingeniero senior hacer el
> mismo trabajo con calidad de producción (investigación + implementación + validación + docs). El
> detalle por funcionalidad va en `## Esfuerzo equivalente` del `{{session}}`.
> Al cerrar: `printf '| N | YYYY-MM-DD | X-Y | funcionalidades |\n' >> {{history_dir}}{{effort}}`
>
> (Feature `effort_log`: quitar este archivo y apagar el toggle si el proyecto no lleva la serie.)

<!-- ARRANCA CON LA TABLA VACÍA Y ESO ESTÁ BIEN: la serie empieza en el primer cierre.
     Cero filas el primer día es correcto, no es un fallo de instanciación.

     ASÍ SE VE LA PRIMERA FILA, cuando la haya:

       | 1 | 2026-03-14 | 3-5 | scaffold del proyecto y primer endpoint con sus pruebas |

     El rango es una AUTOESTIMACIÓN, no una medida: cuánto habría costado el mismo trabajo con
     calidad de producción. Se escribe con `printf >>`, sin leer el archivo entero. -->

| Sesión | Fecha | {{effort_unit}} | Funcionalidades clave |
| --- | --- | --- | --- |
