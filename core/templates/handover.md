# {{handover}}

## Estado

SIN_TRABAJO_ACTIVO

## Última sesión cerrada

Sesión `N` (`YYYY-MM-DD`) — título. Ver el archivo de esa sesión en `{{history_dir}}`.
No hay trabajo a medias.

<!-- RECIÉN INSTALADO: aquí no va una sesión inventada. La forma es
     "**Ninguna.** El marco se instaló el `YYYY-MM-DD` y todavía no se ha cerrado ninguna
     sesión, así que no hay acta que citar en `{{history_dir}}`. No hay trabajo a medias."
     Se sustituye por la forma normal al cerrar la primera. -->

## Lo que espera al usuario

<!-- Lo que NADIE puede hacer salvo el usuario: entregar algo, decidir algo, aprobar algo.
     Va en tabla porque se ejecuta de una pasada; en prosa hay que reconstruirla.
     Se lee en el saludo de la sesión siguiente (ver el ritual ABRIR). Si no hay nada,
     se deja la sección con "Nada pendiente" -- borrarla la vuelve invisible. -->

| Qué | Detalle |
| --- | --- |
| **...** | ... |

<!-- =========================================================================
     Cuando arranca un cambio interrumpible, SOBRESCRIBIR con la forma EN_PROGRESO:

## Estado
EN_PROGRESO

## Sello
El commit de `HEAD` al abrir este checkpoint (si `persistencia = git`), y si el árbol estaba limpio.
Debajo, la instrucción de compararlo: **si no coincide con el `HEAD` de ahora, este documento describe
un pasado y manda el árbol.** Cuesta una línea y es lo único que distingue un checkpoint vigente de
uno caducado — ver el ritual ABRIR. Sin VCS no hay sello: en su lugar, di **qué se observa en disco**
para saber por dónde ibas (p. ej. "el paso 2 deja los dos archivos a la vez").

## Salto actual
Objetivo en una frase + decisiones ya tomadas que el siguiente agente debe respetar.

## Alcance permitido / No tocar
- permitido: <archivo/dir>
- no tocar: <archivo/dir fuera de alcance>

## Trampas de este salto
- Lo que sabes que puede salir mal en LO QUE VAS A HACER, no trampas generales del proyecto — esas
  viven en su hogar. Aquí van las que dispararían en las próximas horas.
- Es el sitio donde una advertencia llega a tiempo: un doc que se lee al arrancar informa; esto
  detiene. (Ver `{{kit}}/SKILL.md` → la regla dura del checkpoint.)

<!-- SI EL SALTO NO CABE, SE APUNTA -- NO SE ENGORDA ESTE DOCUMENTO. El detalle va a
     `{{artifacts_dir}}sesion-NNN/checkpoint.md` y aqui queda el puntero mas las tres lineas que
     hacen falta para no romper nada al retomar.
     EL CRITERIO NO ES CUANTO CABE: ES QUIEN LO PAGA. Esto se lee en CADA sesion, tambien cuando no
     hay trabajo a medias; el artefacto lo paga solo quien retome el salto. Y un handover que no
     cabe casi nunca pide mas sitio: pide ver que lleva VARIOS saltos en un doc disenado para UNO.
     El porque entero, en {{kit}}/SKILL.md -> regla dura del checkpoint. -->

## Estado intermedio
- **Tampoco aquí se copia un estado que vive en otro documento.** Su hogar lo dice y este apunta:
  *"hay una carta sin entregar — mírala en su índice"*, no *"la carta N está publicada"*. Un
  `handover` se reescribe a cada checkpoint, así que la copia vive menos que en un `session` — pero
  vive **justo el tramo en que alguien la lee para retomar**. Se decidió NO poner este aviso cuando la
  regla entró en las otras dos plantillas, con la razón de que **no había caso**; lo hubo dos sesiones
  después, en este mismo documento, y lo encontró la segunda pasada de una auditoría.
- Qué quedó a medias (p. ej. "X hecho, Y pendiente -> inconsistente hasta Y").
- Qué está sin persistir (sin commitear, si `persistencia = git`).

## Pendiente inmediato (en orden)
- Paso 1 concreto para retomar...

## Si fui interrumpido
**Empieza comprobando el sello**, no leyendo esta lista: si no coincide, parte de lo de arriba ya está
hecho y este texto no sabe cuánto. Retomar desde: ...   No repetir: ...

Marca aparte **lo destructivo y lo que no se repite** (un borrado, una copia de evidencia que la
segunda vez sobrescribiría la buena). Es lo que hace daño cuando alguien retoma con el documento
caducado en la mano.
========================================================================= -->
