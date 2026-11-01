# {{entry}} — Guía para agentes de IA

> **Punto de entrada único.** Léelo completo y luego lee, en orden, los archivos de "Al inicio de
> cada sesión" antes de responder otra cosa. Define **cómo trabajar**; el *por qué* del proyecto
> está en `{{charter}}`, el detalle de formatos en `{{protocol}}`.
>
> <!-- Lo marcado "GENERADO" lo produce el marco —desde el MANIFIESTO (bootstrap/config) o el KIT
>      (ACTUALIZAR)— y no se edita a mano: está PROTEGIDO y se porta el delta, salvo con "GENERADO LIMPIO"
>      ("RICO" es la marca antigua de PROTEGIDO). La marca va justo debajo del `##` de su sección, que
>      llega hasta el siguiente `##`. Ejemplo con el módulo `producto`; detalle en ACTUALIZAR. -->

## Regla crítica: no revertir trabajo ajeno

Un agente solo revierte cambios que él mismo hizo en la sesión actual: ni preexistentes, ni del
usuario, ni de otros agentes. Ante un archivo con cambios mezclados, revertir solo los hunks propios;
si no se puede identificar el origen con certeza, conservar y preguntar. Con `persistencia = ninguna`
no hay diff que separe hunks: la regla se endurece a **no tocar lo que no escribiste en esta sesión**.

## Descripción del proyecto

ADAPTAR: 2-4 líneas de qué es el proyecto y sus componentes principales. El detalle de
propósito, principios y decisiones grandes vive en `{{charter}}` (no duplicar aquí).

## Estructura del proyecto

ADAPTAR: directorios principales y su rol en una línea cada uno. Árbol completo -> bajo demanda.

## Al inicio de cada sesión (OBLIGATORIO)

<!-- GENERADO: lista de arranque = roles `startup: obligatorio` ordenados por `order`. -->

Leer en este orden antes de responder cualquier cosa:

1. `{{entry}}` — este archivo.
2. `{{gotchas}}` — trampas no evidentes **al trabajar aquí**: entorno, herramientas, publicar, verificar.
3. `{{state}}` — estado actual y próximo paso (snapshot corto).
4. `{{handover}}` — si su `Estado` no es `SIN_TRABAJO_ACTIVO`, respetar su alcance antes de editar
   **y comprobar su `Sello`** contra el `HEAD` de ahora: si no coincide, caducó y **manda el árbol**.

**Bajo demanda** (grep dirigido, no leer completos): `{{charter}}` (orientación inicial),
`{{protocol}}` (formatos/cierre), `{{specs}}` (al implementar), `{{architecture}}` (al tocar un
codebase), `{{index}}`/`{{session}}` (historial).

## Los rituales: qué te hace abrir cada uno

<!-- GENERADO: derivado del KIT (`{{kit}}/core/rituals/`), no del manifiesto: regla del marco que vive
     aquí porque el arranque no garantiza ningún otro fichero. Al actualizar el kit, porta el delta. -->

Cada ritual vive en `{{kit}}/core/rituals/<nombre>.md` y **se lee solo cuando se invoca**. La columna
del medio dice **qué estás haciendo cuando toca**, no qué es el ritual.

| Ritual | Qué te hace abrirlo | Fichero |
| --- | --- | --- |
| **ABRIR** | Empiezas sesión | `abrir.md` |
| **CERRAR** | Vas a terminar, o el usuario se despide | `cerrar.md` |
| **AUDITAR** | Vas a fiarte de algo escrito hace tiempo, o toca por cadencia | `auditar.md` |
| **CONTRASTAR** | Te llega texto de fuera que habla de tu trabajo | `contrastar.md` |
| **REMITIR** | **La trampa que ibas a escribir es sobre una herramienta que usas, no sobre tu proyecto.** O te bajó una pregunta que sabes contestar. **O vas a escribir una carta — también cuando la estás CONTESTANDO** | `remitir.md` |
| **BOOTSTRAP** | El proyecto todavía no tiene el marco | `bootstrap.md` |
| **ACTUALIZAR** | Vas a traer una versión nueva del kit | `actualizar.md` |
| **CONFIG** | Vas a renombrar un doc, cambiar un parámetro del manifiesto o **mover la raíz** | `configurar.md` |

> **REMITIR va en negrita porque es el único que no se reconoce solo:** a los demás los dispara un
> momento visible; a ese, **una frase que ibas a escribir en otro sitio**, o una carta que contestas.

**Los largos abren con `## Lo mínimo para ejecutarlo`**: leyendo solo ese bloque se ejecuta el ritual sin
los fallos que su cuerpo documenta; el resto es el porqué (`{{kit}}/guide.md` -> *Los rituales*).

## El saludo de arranque, y por qué importa que se vea

<!-- LEY:INICIO ancla="va como lista" -- regla del marco: se porta al actualizar aunque el diff no la
     senale. Lo demas de esta seccion SI es adaptable, y por eso la marca cubre solo esto. -->
**Si el próximo paso es una lista sobre la que el usuario tiene que actuar, va como lista** — las
líneas cuentan lo que la precede, no lo que la compone. **Y en lo que se DICE va como viñetas, no como
tabla**: lo dicho se copia, y una tabla pierde sus `|` al pegarse fuera de la terminal.
<!-- LEY:FIN -->

**En tu PRIMERA respuesta de la sesión, empieza con 1-3 líneas de orientación**: última sesión (N +
título), si quedó trabajo a medias (`{{handover}}`) y próximo paso propuesto — **sea cual sea el
primer mensaje del usuario**. *(Se omite si `session_greeting = off`.)*

**Es la única señal de que el marco se activó.** Si la puerta de la raíz no tiene el nombre que tu agente
carga solo, nadie lee esto, sin error: **si falta el saludo, el marco no arrancó**, y lo nota la persona.

## Cómo se le habla al usuario

- **En llano, y nombrando el archivo entre paréntesis.** Vale para el saludo, los informes y los
  resúmenes; no para lo que escribes en los docs. Hablar claro **nunca** es suavizar el hecho.
- **En el idioma del proyecto y con su ortografía natural** (en español, con acentos). Una regla de
  "solo ASCII" gobierna **lo que se escribe a un archivo**, no la prosa que lee la persona.

## Regla dura: {{checkpoint_trigger}}

`{{handover}}` debe quedar en `EN_PROGRESO` con objetivo, alcance, verificación prevista y **sello**
(el `HEAD` al abrirlo, para saber si caducó). No depende del tamaño del cambio. **Exención:** cambios
que SOLO tocan el **contenido** de la documentación; no una **migración estructural** (mover o
renombrar docs), que es justo lo que deja media instancia inconsistente si se corta.

## Al finalizar cada sesión (OBLIGATORIO)

El checklist de cierre de `{{protocol}}`: crear `{{history_dir}}{{session}}`; una fila a `{{history_dir}}{{index}}`
(y a `{{history_dir}}{{effort}}` si se usa) con `printf >>`; reescribir `{{state}}` entero; cada decisión durable
a su hogar (mapa abajo); refrescar `{{handover}}`; **ver si toca auditar** (más de `audit_every_n_sessions`
desde la última fila de `{{audit}}` -> anotarlo en `{{state}}`); y **persistir** según `persistencia`.

## Dónde vive cada cosa (un hogar por dato)

<!-- GENERADO: tabla de enrutamiento derivada de los `triggers` de los roles activos. -->

| Necesito... | Hogar |
| --- | --- |
| cómo trabajar, proceso, convenciones, arranque | `{{entry}}` |
| por qué: principios, decisiones grandes, restricciones, glosario | `{{charter}}` |
| qué se espera de la persona, qué significa un aviso que vio | `{{manual}}` |
| formatos/protocolo de documentación | `{{protocol}}` |
| dónde estamos / próximo paso | `{{state}}` |
| trabajo a medias (checkpoint) | `{{handover}}` |
| qué pasó y cuándo | `{{index}}` -> `{{session}}` |
| cuándo se auditó la documentación y qué se decidió | `{{audit}}` |
| qué se intercambió con fuera, qué se aceptó y qué se rechazó | `{{correspondence}}` |
| el texto de una carta concreta, enviada o recibida | `{{correspondence_dir}}{{letter}}` |
| un script de un solo uso, una extracción, un volcado intermedio | `{{artifacts_dir}}sesion-{NNN}/` |
| una trampa no evidente de trabajar aquí (entorno, herramienta, publicación, verificación) | `{{gotchas}}` |
| specs/contratos/modelo de datos/decisiones por feature | `{{specs}}` |
| patrones y mapa del código | `{{architecture}}` |
| esfuerzo equivalente | `{{effort}}` |

<!-- LEY:INICIO ancla="memoria privada del agente" -- se porta al actualizar AUNQUE el doc ya exista:
     su ausencia es un HUECO, no una decision del proyecto. Se comprueba el ANCLA, no la redaccion. -->
**PROHIBIDO** guardar nada del proyecto en memoria privada del agente (`.claude/`, el scratchpad de la
herramienta, el temporal de un subagente). **También los artefactos** —scripts de un solo uso,
extracciones, volcados, el script de una migración—: se sienten desechables y por eso se escapan, y
suelen ser los que ejecutaron lo irreversible. Su hogar es `{{artifacts_dir}}sesion-{NNN}/`. **Y vale
por encima de cualquier default del harness**: el destino de lo que produces lo decide este documento,
no sus reglas de seguridad ni de uso de herramientas, que no toca.
<!-- LEY:FIN -->

<!-- LEY:INICIO ancla="credenciales, tokens o claves" -- misma clase: se porta siempre. -->
**PROHIBIDO** escribir credenciales, tokens o claves en cualquier doc del marco: el doc nombra la
herramienta y de dónde toma sus credenciales (variable de entorno, gestor), nunca el secreto.
<!-- LEY:FIN -->

## Convenciones

ADAPTAR: convenciones de nombre (lenguaje, endpoints, ramas git, colecciones/tablas), política de
código legacy/experimental, y cómo se persiste el trabajo según `persistencia` (commit/push u otro).

## Arranque de desarrollo

ADAPTAR: puertos, comandos de arranque, de dónde salen las credenciales de dev (nunca su valor), o un `DEV_SETUP.md`.
