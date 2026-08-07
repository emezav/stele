# {{entry}} — Guía para agentes de IA

> **Punto de entrada único.** Léelo completo y luego lee, en orden, los archivos de "Al inicio de
> cada sesión" antes de responder otra cosa. Define **cómo trabajar**; el *por qué* del proyecto
> está en `{{charter}}`, el detalle de formatos en `{{protocol}}`.
>
> <!-- Las secciones marcadas "GENERADO" las produce bootstrap/`config` desde el manifiesto
>      (`stele.config.md`). No editarlas a mano. El ejemplo mostrado usa el módulo `software`.
>      MODO ADOPCIÓN: una sección enriquecida con contenido propio que la plantilla base no tiene se
>      marca "GENERADO RICO"; ACTUALIZAR entonces porta el delta a mano en vez de reescribirla. -->

## Regla crítica: no revertir trabajo ajeno

Un agente solo revierte cambios que él mismo hizo en la sesión actual. No revertir cambios
preexistentes, del usuario ni de otros agentes. Ante un archivo con cambios mezclados, revertir
solo los hunks propios; si no se puede identificar el origen con certeza, conservar y preguntar.

Con `persistencia = ninguna` no hay diff que permita identificar hunks: la regla se endurece a
**no tocar lo que no escribiste en esta sesión** y preguntar ante la duda.

## Descripción del proyecto

ADAPTAR: 2-4 líneas de qué es el proyecto y sus componentes principales. El detalle de
propósito, principios y decisiones grandes vive en `{{charter}}` (no duplicar aquí).

## Estructura del proyecto

ADAPTAR: directorios principales y su rol en una línea cada uno. Árbol completo → bajo demanda.

## Al inicio de cada sesión (OBLIGATORIO)

<!-- GENERADO: lista de arranque = roles `startup: obligatorio` ordenados por `order`. -->

Leer en este orden antes de responder cualquier cosa:

1. `{{entry}}` — este archivo.
2. `{{gotchas}}` — gotchas y convenciones técnicas no evidentes en el código.
3. `{{state}}` — estado actual y próximo paso (snapshot corto).
4. `{{handover}}` — si su `Estado` no es `SIN_TRABAJO_ACTIVO`, respetar su alcance antes de editar.

**Bajo demanda** (grep dirigido, no leer completos): `{{charter}}` (orientación inicial),
`{{protocol}}` (formatos/cierre), `{{specs}}` (al implementar), `{{architecture}}` (al tocar un
codebase), `{{index}}`/`{{session}}` (historial).

## Regla dura: {{checkpoint_trigger}}

`{{handover}}` debe quedar en `EN_PROGRESO` con objetivo, alcance y verificación prevista.
No depende del tamaño estimado del cambio. **Exención:** cambios que SOLO tocan el **contenido** de
la documentación. No exime una **migración estructural** (mover o renombrar docs: rituales CONFIG y
ACTUALIZAR), aunque no toque código: es justo lo que deja media instancia inconsistente si se corta.

## Al finalizar cada sesión (OBLIGATORIO)

Seguir el checklist de cierre de `{{protocol}}`. En resumen: crear `{{history_dir}}{{session}}`;
append de una fila a `{{history_dir}}{{index}}` (y `{{history_dir}}{{effort}}` si se usa) con
`printf >>`; reescribir `{{state}}` completo; llevar
toda decisión durable a su hogar (mapa abajo), nunca solo en el historial; refrescar `{{handover}}`
(→ `SIN_TRABAJO_ACTIVO` apuntando a la sesión que cierras ahora, o `EN_PROGRESO`); **comprobar si
toca auditar** (si pasaron más de `audit_every_n_sessions` desde la última fila de `{{audit}}`,
anotarlo en los pendientes de `{{state}}`); y **persistir** según `persistencia`.

## Dónde vive cada cosa (un hogar por dato)

<!-- GENERADO: tabla de enrutamiento derivada de los `triggers` de los roles activos. -->

| Necesito… | Hogar |
| --- | --- |
| cómo trabajar, proceso, convenciones, arranque | `{{entry}}` |
| por qué: principios, decisiones grandes, restricciones, glosario | `{{charter}}` |
| formatos/protocolo de documentación | `{{protocol}}` |
| dónde estamos / próximo paso | `{{state}}` |
| trabajo a medias (checkpoint) | `{{handover}}` |
| qué pasó y cuándo | `{{index}}` → `{{session}}` |
| cuándo se auditó la documentación y qué se decidió | `{{audit}}` |
| trampas de código | `{{gotchas}}` |
| specs/contratos/modelo de datos/decisiones por feature | `{{specs}}` |
| patrones y mapa del código | `{{architecture}}` |
| esfuerzo equivalente | `{{effort}}` |

**PROHIBIDO** guardar contenido del proyecto en memoria privada del agente (`.claude/` etc.):
todo va a los docs del proyecto, visibles para cualquier agente y humano.

**PROHIBIDO** escribir credenciales, tokens o claves en cualquier doc del marco. Si el proyecto
necesita acceso a un servicio, el doc nombra la herramienta y de dónde toma sus credenciales
(variable de entorno, gestor de credenciales), nunca el secreto.

## Convenciones

ADAPTAR: convenciones de nombre (lenguaje, endpoints, ramas git, colecciones/tablas),
política de código legacy/experimental si aplica, y cómo se persiste el trabajo (según
`persistencia`: reglas de commit/push, o el procedimiento que aplique).

## Arranque de desarrollo

ADAPTAR: puertos, comandos de arranque, cómo se obtienen las credenciales de dev (nunca los
valores) o puntero a un `DEV_SETUP.md`.
