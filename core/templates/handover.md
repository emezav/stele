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

## Cierre

COMPLETO

<!-- Si el cierre fue MINIMO (ver CERRAR -> "El cierre minimo"), aqui va el contador y la deuda:
"MINIMO 2 de 3" y debajo "falta: acta de la NNN, fila de index, fila de esfuerzo, state".
Al tercero, el siguiente cierre va COMPLETO. Al cerrar completo vuelve a COMPLETO y se dice de
que sesiones se completo el acta -- o que no se va a completar, que tambien es una decision. -->

## Lo que espera al usuario

<!-- Lo que NADIE puede hacer salvo el usuario: entregar algo, decidir algo, aprobar algo.
Va en tabla porque se ejecuta de una pasada; en prosa hay que reconstruirla.
Se lee en el saludo de la sesión siguiente (ver el ritual ABRIR). Si no hay nada,
se deja la sección con "Nada pendiente" -- borrarla la vuelve invisible. -->

| Qué | Detalle |
| --- | --- |
| **...** | ... |

<!-- SU TOPE SE MIDE SOBRE LA FORMA INSTANCIADA, NO SOBRE ESTE FICHERO. Lo de abajo es andamio y
     no viaja: el fichero puede pasar del tope del rol sin que la instancia lo pase. Comprobado el
     2026-10-04: fichero 98 lineas, instanciada 26 de un tope de 40.
     EL AVISO ESTA AQUI Y NO SOLO EN AUDITAR porque alli llega quien audita, y a esto llega quien
     TOCA la plantilla. Le paso a quien escribio este comentario: leyo "98 de 90" y su primer
     impulso fue podar, con la regla publicada en AUDITAR paso 9 y aplicada por la auditoria 19
     dos semanas antes. El remedio estaba donde se descubre el defecto, no donde se comete. -->

## Forma EN_PROGRESO — ANDAMIO, se borra al instanciar

**Va citada y no comentada, y no es estilo.** Un comentario de 33 líneas es donde alguien escribe una
nota interior, y HTML no anida: el cierre de dentro cierra el de fuera y lo que sigue sale renderizado
como si fuera estado real. Le pasó a este fichero 28 días, en el documento que se lee en CADA arranque,
y lo vio el primero que instaló: desde un repo auto-hospedado es invisible, porque la plantilla solo se
renderiza al instalar. Una valla no tiene ese modo de fallo. **Y este párrafo no puede ir comentado:**
explicar la trampa exige escribir la secuencia que cierra un comentario, que dentro de uno lo cierra.

Cuando arranca un cambio interrumpible, **sobrescribir el documento entero** con esta forma:

```markdown
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

## Estado intermedio
- **Tampoco aquí se copia un estado que vive en otro documento.** Su hogar lo dice y este apunta:
  *"hay una carta sin entregar — mírala en su índice"*, no *"la carta N está publicada"*. Un
  `handover` se reescribe a cada checkpoint, así que la copia vive menos que en un `session` — pero
  vive **justo el tramo en que alguien la lee para retomar**.
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
```

<!-- SI EL SALTO NO CABE, SE APUNTA -- NO SE ENGORDA ESTE DOCUMENTO. El detalle va a
`{{artifacts_dir}}sesion-NNN/checkpoint.md` y aqui queda el puntero mas las tres lineas que
hacen falta para no romper nada al retomar. EL CRITERIO NO ES CUANTO CABE: ES QUIEN LO PAGA.
Esto se lee en CADA sesion, tambien cuando no hay trabajo a medias; el artefacto lo paga solo
quien retome el salto. Y un handover que no cabe casi nunca pide mas sitio: pide ver que lleva
VARIOS saltos en un doc disenado para UNO.
El porque entero, en {{kit}}/SKILL.md -> regla dura del checkpoint. -->
