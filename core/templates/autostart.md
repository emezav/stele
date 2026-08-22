# Auto-arranque de stele ({{loader}})

<!-- PLANTILLA de PUERTA. Bootstrap/`config` la resuelven a nombres concretos.

     Se escribe UNA COPIA POR PUERTA, en la raíz del proyecto, con cada nombre de la ruta `loader`.
     Una puerta es un ADAPTADOR, no un documento: su único contenido es QUÉ LEER Y EN QUÉ ORDEN,
     dicho en la forma que su harness entiende. Las reglas viven una sola vez, en {{entry}}.

     REGLA DURA — si el archivo YA EXISTE, se MODIFICA, no se crea de cero: su contenido es del
     usuario y se conserva íntegro. Todo lo que produce el marco va entre las marcas
     STELE:INICIO / STELE:FIN de abajo, y **solo eso** se reescribe al regenerar. Sobrescribir el
     archivo entero ya destruyó el `CLAUDE.md` de un proyecto real, y con varias puertas esa regla
     tiene ahora varias superficies donde fallar: compruébala EN CADA UNA. -->

<!-- STELE:INICIO — bloque GENERADO y PROTEGIDO. Fuera de estas marcas el marco no toca nada; dentro
     tampoco se reescribe entero: ACTUALIZAR y CONFIG **portan el delta a mano** y conservan lo que
     este bloque diga de más. Es el default porque la divergencia nace en el bootstrap.
     `STELE:INICIO RICO` (marca antigua) significa lo mismo — si la llevas, déjala.
     Para autorizar la reescritura entera: `STELE:INICIO LIMPIO`, y solo tras comprobar con un diff
     que este bloque es la plantilla con los tokens resueltos y nada más. Detalle en ACTUALIZAR. -->

Este proyecto usa el marco **stele**, que vive en `{{kit}}/`.

**Antes de responder cualquier otra cosa, lee estos archivos en este orden.** Cómo se trabaja aquí lo
dice `{{entry}}`; este archivo solo te dice **qué abrir**.

<!-- BOOTSTRAP: emitir la lista de abajo Y el bloque de imports, los dos, con un renglón por cada rol
     `startup: obligatorio` ordenado por `order`, ruta resuelta {base}/[<valor de history_dir>]{nombre}
     — relativa a la raíz, donde vive esta puerta. El valor de history_dir ya trae su `/` final.
     Ejemplo con defaults (base = .): -->

1. `AGENTS.md` — cómo trabajar aquí: proceso, reglas de sesión, mapa de dónde vive cada cosa.
2. `memory.md` — trampas y convenciones no evidentes al trabajar en este proyecto.
3. `history/latest.md` — dónde estamos y el próximo paso.
4. `history/handover.md` — si su `Estado` no es `SIN_TRABAJO_ACTIVO`, respeta su alcance antes de
   editar nada.

@AGENTS.md
@memory.md
@history/latest.md
@history/handover.md

<!-- LA LISTA EN PROSA NO ES REDUNDANTE Y NO SE QUITA.

     Las marcas `@` solo las ejecutan algunos harness. Se observó en campo a un agente leer esta
     puerta, NO actuar sobre las marcas, y traer los cuatro archivos porque la prosa los enunciaba:
     sin esa lista habría arrancado sin estado, sin memoria y sin handover, saludando igual. La prosa
     es el contrato de la puerta; los imports son el atajo de quien pueda usarlos. -->

<!-- STELE:FIN -->
