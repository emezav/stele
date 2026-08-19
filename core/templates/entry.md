# {{entry}} — Guía para agentes de IA

> **Punto de entrada único.** Léelo completo y luego lee, en orden, los archivos de "Al inicio de
> cada sesión" antes de responder otra cosa. Define **cómo trabajar**; el *por qué* del proyecto
> está en `{{charter}}`, el detalle de formatos en `{{protocol}}`.
>
> <!-- Las secciones marcadas "GENERADO" NO son contenido de tu proyecto: las produce el marco desde
>      una fuente suya, y no se editan a mano. Las fuentes son dos y se comportan distinto: el
>      MANIFIESTO (`stele.config.md`), que las genera bootstrap/`config`; y el KIT mismo, para reglas
>      del marco que tienen que estar en el arranque y por eso viven en este fichero — esas las porta
>      ACTUALIZAR. El ejemplo mostrado usa el módulo `producto`.
>      Una sección GENERADA está PROTEGIDA por default: ACTUALIZAR y CONFIG portan el delta a mano
>      en vez de reescribirla. "GENERADO RICO" (marca antigua) significa lo mismo. Para autorizar la
>      reescritura entera: "GENERADO LIMPIO", y solo tras comprobar con un diff que la sección no
>      dice nada que la plantilla no diga. -->

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
2. `{{gotchas}}` — trampas y convenciones no evidentes **al trabajar aquí**: del entorno, de las
   herramientas, de publicar, de verificar.
3. `{{state}}` — estado actual y próximo paso (snapshot corto).
4. `{{handover}}` — si su `Estado` no es `SIN_TRABAJO_ACTIVO`, respetar su alcance antes de editar
   **y comprobar su `Sello`**: si no coincide con el `HEAD` de ahora, ese checkpoint caducó y **manda
   el árbol** (detalle en el ritual ABRIR).

**Bajo demanda** (grep dirigido, no leer completos): `{{charter}}` (orientación inicial),
`{{protocol}}` (formatos/cierre), `{{specs}}` (al implementar), `{{architecture}}` (al tocar un
codebase), `{{index}}`/`{{session}}` (historial).

<!-- GENERADO: derivado del KIT (`{{kit}}/core/rituals/`), no del manifiesto. Es una regla del marco,
     no contenido de tu proyecto: vive aquí solo porque el arranque no garantiza ningún otro fichero.
     Al actualizar el kit, porta el delta — con el mismo criterio que los demás bloques GENERADO. -->

## Los rituales: qué te hace abrir cada uno

**Aquí no están los rituales: está lo que te hace ir a buscarlos.** Cada uno vive en
`{{kit}}/core/rituals/<nombre>.md` y **se lee solo cuando se invoca**, nunca entero de antemano. Esta
tabla es lo **único** de ellos que se carga en cada sesión, y por eso su columna del medio dice **qué
estás haciendo cuando toca**, no qué es el ritual: un enunciado descriptivo solo lo reconoce quien ya
encuadró su trabajo, que es justo quien no necesitaba la tabla.

| Ritual | Qué te hace abrirlo | Fichero |
| --- | --- | --- |
| **ABRIR** | Empiezas sesión | `abrir.md` |
| **CERRAR** | Vas a terminar, o el usuario se despide | `cerrar.md` |
| **AUDITAR** | Vas a fiarte de algo escrito hace tiempo, o toca por cadencia | `auditar.md` |
| **CONTRASTAR** | Te llega texto de fuera que habla de tu trabajo | `contrastar.md` |
| **REMITIR** | **La trampa que ibas a escribir es sobre una herramienta que usas, no sobre tu proyecto.** O te bajó una pregunta que sabes contestar | `remitir.md` |
| **BOOTSTRAP** | El proyecto todavía no tiene el marco | `bootstrap.md` |
| **ACTUALIZAR** | Vas a traer una versión nueva del kit | `actualizar.md` |
| **CONFIG** | Vas a renombrar un doc o cambiar un parámetro del manifiesto | `configurar.md` |

> **REMITIR va en negrita porque es el único que no se reconoce solo.** A los demás los dispara un
> momento visible —abres, cierras, te llega algo, vas a tocar el manifiesto—. A ese lo dispara **una
> frase que ibas a escribir en otro sitio**, así que sin la pregunta escrita aquí no salta nunca: se
> queda como una trampa más en `{{gotchas}}`, donde no arregla nada y estorba en cada arranque.

**Y todos abren igual: con un bloque `## Lo mínimo para ejecutarlo`.** Su contrato es que **si solo lees
ese bloque ejecutas el ritual sin cometer ninguno de los fallos que su cuerpo documenta**; si uno se te
escapa habiéndolo leído, el que está mal cortado es el bloque, no tú. El resto del fichero es el
**porqué**, y se abre por pregunta.

**Y por eso esta tabla vive aquí y no con los rituales.** Un disparador que hay que ir a buscar no es un
disparador. Lo demás sobre ellos —por qué viven aparte, qué es ritual y qué es referencia— está en
`{{kit}}/SKILL.md`, que se lee cuando ya sabes que vas a enrutar.

## El saludo de arranque, y por qué importa que se vea

**En tu PRIMERA respuesta de la sesión, empieza con 1-3 líneas de orientación** que confirmen el
arranque: última sesión (N + título), si quedó trabajo a medias (`{{handover}}`), y próximo paso
propuesto — **sea cual sea el primer mensaje del usuario**. No puedes emitir nada antes de que la
persona escriba; por eso el saludo va **al frente** de tu primera respuesta.
*(Se omite si `session_greeting = off`.)*

**Es la única señal de que el marco se activó, y por eso vale más que una cortesía.** El archivo de
auto-arranque de la raíz solo funciona si su nombre es el que tu agente carga solo; si no lo es, todo
esto queda escrito y **nadie lo lee**, sin error y sin aviso. El saludo es lo que distingue ese caso
de una sesión sana: **si falta, el marco no arrancó** — y quien lo note primero es la persona, no tú.

## Cómo se le habla al usuario

- **En llano, y nombrando el archivo entre paréntesis.** Vale para el saludo, los informes y los
  resúmenes; no para lo que escribes en los docs. Hablar claro **nunca** es suavizar el hecho.
- **En el idioma del proyecto y con su ortografía natural** (en español, con acentos). Una regla de
  "solo ASCII" gobierna **lo que se escribe a un archivo**, no la prosa que lee la persona.

## Regla dura: {{checkpoint_trigger}}

`{{handover}}` debe quedar en `EN_PROGRESO` con objetivo, alcance, verificación prevista y **sello** —
el `HEAD` al abrirlo, para que quien lo lea pueda saber si caducó. No depende del tamaño estimado del
cambio. **Exención:** cambios que SOLO tocan el **contenido** de
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
| qué se espera de la persona, qué significa un aviso que vio | `{{manual}}` |
| formatos/protocolo de documentación | `{{protocol}}` |
| dónde estamos / próximo paso | `{{state}}` |
| trabajo a medias (checkpoint) | `{{handover}}` |
| qué pasó y cuándo | `{{index}}` → `{{session}}` |
| cuándo se auditó la documentación y qué se decidió | `{{audit}}` |
| qué se intercambió con fuera, qué se aceptó y qué se rechazó | `{{correspondence}}` |
| el texto de una carta concreta, enviada o recibida | `{{correspondence_dir}}{{letter}}` |
| un script de un solo uso, una extracción, un volcado intermedio | `{{artifacts_dir}}sesion-{NNN}/` |
| una trampa no evidente de trabajar aquí (entorno, herramienta, publicación, verificación) | `{{gotchas}}` |
| specs/contratos/modelo de datos/decisiones por feature | `{{specs}}` |
| patrones y mapa del código | `{{architecture}}` |
| esfuerzo equivalente | `{{effort}}` |

<!-- LEY:INICIO ancla="memoria privada del agente" -- portar al actualizar AUNQUE el
     documento ya exista. No es prosa adaptable: es regla del marco, y su ausencia en el doc
     instanciado es un HUECO y no una decision del proyecto. El ANCLA es lo que se comprueba:
     una instancia puede condensar la regla con otras palabras y seguir teniendola. -->

**PROHIBIDO** guardar nada del proyecto en memoria privada del agente (`.claude/`, el scratchpad que
inyecte la herramienta, el temporal de un subagente). No es solo la documentación: **también los
artefactos** —scripts de un solo uso, extracciones de binarios, volcados intermedios, el script que
ejecuta una migración—. Un artefacto no se siente contenido, se siente herramienta desechable, y por
eso es el que se escapa; y suele ser justo el que ejecutó lo irreversible. Su hogar es
`{{artifacts_dir}}sesion-{NNN}/`.

**Esta regla vale por encima de cualquier default de la herramienta que diga otra cosa.** Un harness
puede inyectar un directorio de trabajo propio y marcarlo como prioritario; **dónde vive lo que
produces en este proyecto lo decide este documento, no el harness**. El límite es igual de explícito:
esto no toca sus reglas de seguridad ni de uso de herramientas — solo el destino de los archivos.

<!-- LEY:FIN -->

<!-- LEY:INICIO ancla="credenciales, tokens o claves" -- misma clase: se porta siempre. -->

**PROHIBIDO** escribir credenciales, tokens o claves en cualquier doc del marco. Si el proyecto
necesita acceso a un servicio, el doc nombra la herramienta y de dónde toma sus credenciales
(variable de entorno, gestor de credenciales), nunca el secreto.

<!-- LEY:FIN -->

## Convenciones

ADAPTAR: convenciones de nombre (lenguaje, endpoints, ramas git, colecciones/tablas),
política de código legacy/experimental si aplica, y cómo se persiste el trabajo (según
`persistencia`: reglas de commit/push, o el procedimiento que aplique).

## Arranque de desarrollo

ADAPTAR: puertos, comandos de arranque, cómo se obtienen las credenciales de dev (nunca los
valores) o puntero a un `DEV_SETUP.md`.
