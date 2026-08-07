# Auto-arranque de la stele ({{loader}})

<!-- PLANTILLA. Bootstrap/`config` la resuelven a nombres concretos. Se escribe SIEMPRE en la raíz
     del proyecto, con el nombre de la ruta `loader`.

     REGLA DURA — si el archivo YA EXISTE, se MODIFICA, no se crea de cero: su contenido es del
     usuario y se conserva íntegro. Todo lo que produce el marco va entre las marcas
     STELE:INICIO / STELE:FIN de abajo, y **solo eso** se reescribe al regenerar. Sobrescribir el
     archivo entero ya destruyó el `CLAUDE.md` de un proyecto real. -->

<!-- STELE:INICIO — bloque GENERADO. No editar a mano: se reescribe entero en `config` y al
     actualizar. Lo que esté FUERA de estas marcas es del proyecto y el marco no lo toca.

     MODO ADOPCIÓN: si este bloque se enriqueció con reglas propias del proyecto que la plantilla
     base no contiene, cambia la marca de apertura a `STELE:INICIO RICO`. Entonces ni ACTUALIZAR ni
     CONFIG lo reescriben entero: portan solo el delta del kit nuevo, a mano. Sin la marca, un
     proyecto adoptado pierde sus reglas la primera vez que alguien regenere. -->

Este proyecto usa el marco **stele** (`{{kit}}/`). El agente carga este archivo al iniciar cada
sesión, así que el ritual de apertura se ejecuta **automáticamente**: el contexto mínimo viene
importado abajo. No hace falta pedir "lee {{entry}}".

**Reglas de sesión (resumen — detalle en `{{kit}}/SKILL.md`):**

- **En tu PRIMERA respuesta de la sesión, empieza con 1-3 líneas de orientación** que confirmen el
  arranque: última sesión (N + título), si quedó trabajo a medias (`{{handover}}`), y próximo paso
  propuesto — sea cual sea el primer mensaje del usuario. (No puedes emitir un mensaje antes de que
  el usuario escriba; por eso el saludo va AL FRENTE de tu primera respuesta. Es la señal visible de
  que la stele se activó.) *(Se omite si `session_greeting = off`.)*
- **Al usuario se le habla en llano, y se nombra el archivo entre paréntesis** — vale para el saludo,
  los informes y los resúmenes, no para lo que escribes en los docs. Tabla de equivalencias en
  `{{kit}}/SKILL.md` → "Cómo se le habla al usuario". Hablar claro nunca es suavizar el hecho.
- **El habla va en el idioma del proyecto, con su ortografía natural** (en español, con acentos). Una
  regla de "solo ASCII" gobierna lo que se escribe a un archivo, no la prosa que lees tú.
- Si `{{handover}}` (abajo) está en `EN_PROGRESO`, respeta su alcance antes de editar.
- **{{checkpoint_trigger}}**, deja `{{handover}}` en `EN_PROGRESO` con objetivo + alcance (regla
  dura; exención: editar el **contenido** de un doc — mover o renombrar docs **no** exime).
- **Al cerrar**, sigue el checklist de cierre de `{{protocol}}` (sesión-NNN, {{index}}, [{{effort}}],
  reescribir {{state}}, decisiones a su hogar, refrescar {{handover}}, y el **aviso de auditoría** si
  toca).
- **Un hogar por dato:** consulta el *mapa de documentación* en `{{entry}}` (se genera del manifiesto).
- Lee lo demás **bajo demanda con `grep`**; no abras archivos grandes completos.

---

## Contexto de arranque (auto-importado — GENERADO desde la lista de arranque)

<!-- BOOTSTRAP: emitir un @import por cada rol `startup: obligatorio`, ordenado por `order`, con la
     ruta resuelta {base}/[<valor de history_dir>]{nombre} — relativa a la raíz, donde vive este
     loader. El valor de history_dir ya trae su `/` final; no añadir otro.
     Ejemplo con defaults (base = .): -->

@AGENTS.md
@memory.md
@history/latest.md
@history/handover.md

<!-- STELE:FIN -->
