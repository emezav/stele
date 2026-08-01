# Auto-arranque de la stele ({{loader}})

<!-- PLANTILLA. Bootstrap/`config` la resuelven a nombres concretos y regeneran el bloque
     auto-importado desde la lista de arranque del manifiesto. NO editar el bloque a mano.
     Se escribe SIEMPRE en la raíz del proyecto, con el nombre de la ruta `loader`. -->

Este proyecto usa el marco **stele** (`{{kit}}/`). El agente carga este archivo al iniciar cada
sesión, así que el ritual de apertura se ejecuta **automáticamente**: el contexto mínimo viene
importado abajo. No hace falta pedir "lee {{entry}}".

**Reglas de sesión (resumen — detalle en `{{kit}}/SKILL.md`):**

- **En tu PRIMERA respuesta de la sesión, empieza con 1-3 líneas de orientación** que confirmen el
  arranque: última sesión (N + título), estado del `{{handover}}`, y próximo paso propuesto — sea
  cual sea el primer mensaje del usuario. (No puedes emitir un mensaje antes de que el usuario
  escriba; por eso el saludo va AL FRENTE de tu primera respuesta. Es la señal visible de que la
  stele se activó.) *(Se omite si `session_greeting = off`.)*
- Si `{{handover}}` (abajo) está en `EN_PROGRESO`, respeta su alcance antes de editar.
- **{{checkpoint_trigger}}**, deja `{{handover}}` en `EN_PROGRESO` con objetivo + alcance (regla
  dura; exención: cambios solo de documentación).
- **Al cerrar**, sigue el checklist de cierre de `{{protocol}}` (sesión-NNN, {{index}}, [{{effort}}],
  reescribir {{state}}, decisiones a su hogar, refrescar {{handover}}).
- **Un hogar por dato:** consulta el *mapa de documentación* en `{{entry}}` (se genera del manifiesto).
- Lee lo demás **bajo demanda con `grep`**; no abras archivos grandes completos.

---

## Contexto de arranque (auto-importado — GENERADO desde la lista de arranque)

<!-- BOOTSTRAP: emitir un @import por cada rol `startup: obligatorio`, ordenado por `order`, con la
     ruta resuelta {base}/[{history_dir}/]{nombre} — relativa a la raíz, donde vive este loader.
     Ejemplo con defaults (base = .): -->

@AGENTS.md
@MEMORY.md
@HISTORY/LATEST.md
@HISTORY/HANDOVER.md
