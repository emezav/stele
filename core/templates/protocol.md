# {{protocol}} — Protocolo de documentación entre sesiones

> **Cómo** se documenta (formatos, convenciones de edición, topes de tamaño) para que ningún
> archivo de estado crezca sin límite y para minimizar tokens/roundtrips de cualquier agente.
> Los rituales condensados están en `{{kit}}/SKILL.md`; el *por qué*, en `{{kit}}/guide.md`.
> Los nombres de archivo abajo son los del manifiesto (`stele.config.md`).

## Principios

1. Los archivos de **estado** no crecen: se **sobrescriben** (formato fijo + tope).
2. El **historial** vive en archivos por sesión y **no se reabre** (se referencia por link).
3. Los **apéndices de una línea** usan `printf '...' >> archivo`, no `Read`+`Edit`.
4. Ninguna sección de un doc fuente-de-verdad supera **~150-200 líneas**; si crece, se extrae a
   un tema de `{{specs}}` + link.
5. Nada de esto vive en memoria privada del agente; todo en el proyecto. **Tampoco los artefactos**
   —scripts de un solo uso, extracciones, volcados—: su hogar es `{{artifacts_dir}}sesion-{NNN}/`, y
   esa regla vale por encima de cualquier default del harness (`{{kit}}/SKILL.md` → Precedencia).
6. **Un hogar por dato** (mapa en `{{kit}}/SKILL.md` y en `{{entry}}`).

## Rutas: comando contra enlace

Dos clases, y no se resuelven igual:

- **Ruta de comando** (`printf '...' >> …`, `grep`, `git log --`): siempre **desde la raíz del
  proyecto**, porque ahí opera el agente. Incluye `{{history_dir}}` delante del nombre del archivo.
- **Enlace Markdown clicable** (`[{{index}}](./{{index}})`): relativo **al archivo que lo contiene**,
  que es como lo resuelve cualquier visor.

El `printf >>` del cierre es el que más vigilar: si la ruta está mal, **no da error** — crea el
archivo que falta y el bueno se queda sin la fila. Si hay más de una copia del comando en los docs,
todas deben decir lo mismo; mejor aún, que solo una lo deletree y las demás lo nombren en prosa.

Al mover `base`, los enlaces relativos sobreviven si su destino viaja en el mismo bloque (es el caso
dentro de `{{history_dir}}`); los que apuntan fuera del bloque se rompen y hay que revisarlos.

## Archivos y su rol

Ver `{{kit}}/guide.md` → "Roles y fronteras". Aquí solo los **formatos**.

### `{{state}}` — formato fijo, se SOBREESCRIBE (~{{budget:state}} líneas)

```markdown
# Estado actual
> Última sesión: Sesión N (YYYY-MM-DD) — ver {{session}}
> Índice completo: {{index}}

## Dónde estamos
- (3-8 bullets del estado REAL, no histórico)
## Próximo paso inmediato
- (lo que haría la siguiente sesión; reemplaza, no acumula)
## Pendientes operativos
- Procesos en background / decisiones abiertas / trabajo sin persistir
## Referencias
- {{specs}} §X — … / {{charter}} § … / tema de {{specs}}
```

Al cerrar: reescribir COMPLETO con `Write`. Nunca `Edit` para prepend/rename de "anterior".
En *Pendientes operativos* no anotes "push pendiente" por el commit que lleva este mismo cierre:
se escribe antes de commitear y quedaría obsoleto al minuto siguiente.
**Este archivo condiciona a la sesión siguiente**, porque lo lee al arrancar: si lo que viene es una
prueba de comportamiento, no escribas aquí los criterios con los que la vas a evaluar — sería darle
la respuesta.

### `{{index}}` — tabla append-only

`| Sesión | Fecha | Resumen | Archivo |`. Al cerrar:
`printf '| N | YYYY-MM-DD | resumen | <session> |\n' >> {{history_dir}}{{index}}`.

### `{{effort}}` — tabla append-only (OPCIONAL, feature `effort_log`)

`| Sesión | Fecha | {{effort_unit}} | Funcionalidades clave |`. Estimar lo que le tomaría a UN
ingeniero senior el mismo trabajo con calidad de producción (investigación + implementación +
validación + docs), en rango. Detalle por funcionalidad en el `{{session}}`.

### `{{audit}}` — tabla append-only (OPCIONAL, feature `audit_log`)

`| Audit | Fecha | Sesiones | Alcance | Comprobadas | Hallazgos | Desenlace |`. Una fila por auditoría
(ritual AUDITAR). **Lo crea la primera auditoría, no el bootstrap.** `Sesiones` es el rango cubierto:
**cobertura temporal, no alcance** — dice qué quedó fuera, y quien acota el trabajo es el conjunto de
documentos de `Alcance`. `Comprobadas` es el **denominador** —cuántas afirmaciones
sobre el mundo se resolvieron y cuántas salieron falsas—, sin el cual dos auditorías con los mismos
hallazgos son indistinguibles; `—` si no se registró. Los hallazgos no se copian aquí: el detalle vive
en el `{{session}}` de la sesión que auditó, y lo que perdura, en el hogar que corrigió.

### `{{session}}` — uno por sesión

Detalle completo: qué se hizo, decisiones, archivos tocados, verificación, notas para retomar,
y `## Esfuerzo equivalente` (si se usa). `NNN` con padding a 3 dígitos. No se reabre; se lee con grep.

**No lleva su propio hash de commit** (con `persistencia = git`). El cierre viaja en el mismo commit
que el trabajo, y un commit no puede contener su propio hash. Para recuperarlo:
`git log --diff-filter=A -- {{history_dir}}<archivo de sesión>`.

### `{{correspondence}}` + `{{letter}}` — la correspondencia (OPCIONAL, `correspondence_log`)

**Misma forma que el historial**, así que no hay nada nuevo que aprender: un índice
(`{{correspondence}}`, como `{{index}}`), un archivo por carta (`{{letter}}`, como `{{session}}`:
inmutable, numerado, se lee con grep) y su carpeta (`{{correspondence_dir}}`). Lo crea la primera
carta, no el bootstrap.

Índice: `| # | Fecha | Dir | Corresponsal | Asunto | Desenlace |`, con `Dir` = `->` sale · `<-` entra.
En una carta recibida, `Desenlace` dice **qué se aceptó y qué se rechazó con su razón** — eso es lo
único que guarda este índice y no guarda ningún otro doc; el detalle de lo aceptado vive en el hogar
que corrigió, con su procedencia.

**La numeración es única para las dos direcciones**, y de ahí sale el acuse de recibo sin mecanismo:
una carta que sale sin otra detrás es una conversación abierta. Leer 1..N es leer la conversación.

**Se archiva lo que contestaste o lo que te movió a hacer algo**, no todo lo que llega: es relevancia,
no frecuencia —cuando llega la primera carta no puedes saber si habrá más—. Misma regla que los
artefactos de una sesión. Y guarda **tu copia de lo que envías**: los buzones se curan, así que la
copia del otro lado puede desaparecer.

### `{{artifacts_dir}}` — artefactos por sesión (se crea con el primero, no en bootstrap)

Un subdirectorio por sesión: `{{artifacts_dir}}sesion-{NNN}/`. Dentro va lo que la sesión produjo y
**no es documentación**: scripts de un solo uso, extracciones de un binario, volcados intermedios,
comparaciones de dos fuentes. **No** va aquí nada que sea contenido del proyecto ni ninguna decisión:
eso tiene su hogar en el mapa, y duplicarlo aquí lo desincroniza.

**Dos clases, y el `{{session}}` dice cuál es cuál.** Un artefacto que **sostiene un cambio
irreversible** —el script que movió los archivos, el que reescribió en lote— es **evidencia**: es la
única reconstrucción del *cómo* cuando el historial solo guarda el *qué*, y con `persistencia = ninguna`
es la única que hay. Ése no se poda. El resto es desecho y se borra cuando el usuario quiera.

**El agente nunca limpia por su cuenta.** Borrar es decisión del usuario, siempre. Un agente
"ordenando" al cerrar destruye justo lo que hacía auditable la sesión.

### `{{handover}}` — checkpoint de trabajo en curso (~{{budget:handover}} líneas)

Estados: `SIN_TRABAJO_ACTIVO` | `EN_PROGRESO` | `COMPLETADO`. **Regla dura:** {{checkpoint_trigger}},
`EN_PROGRESO` con objetivo/alcance/verificación. Al cerrar, siempre refrescar el puntero a la
sesión que se cierra AHORA. Plantilla en `{{kit}}/core/templates/handover.md`.

### `{{specs}}` (+ temas)

Una sección por decisión o feature, **autocontenida**. Se lee con `grep` y por rango, nunca entera:
lo que importa es que cada sección sea corta por sí misma, no el tamaño del archivo.

**Umbral de extracción: ~50 líneas.** Por debajo, la decisión vive aquí. Por encima, se mueve a
`{{specs_dir}}<TEMA>.md` y aquí queda un resumen de 2-3 líneas + link; si un tema supera ~600-800
líneas, se divide en sub-temas. `{{specs_dir}}` se crea **cuando la primera decisión cruce el
umbral**, no en el scaffold: una carpeta vacía no documenta nada.

### `{{gotchas}}`

Hogar único de gotchas de código. Se edita incrementalmente pero se **cura** (se poda lo
obsoleto). Una sección de subsistema que supera ~150-200 líneas se extrae a un tema de `{{specs}}`.

## Acuerdos de auditoría

Lo que el ritual AUDITAR señaló y el usuario decidió **no** cambiar. Se registra aquí para no
rediscutirlo en cada auditoría, y **siempre con umbral** — eso es lo que lo hace una decisión y no un
aplazamiento. Sección **curada**: al cruzarse el umbral, el acuerdo se revisita y se reescribe o se
borra.

| Fecha | Doc | Acuerdo | Umbral de revisión |
| --- | --- | --- | --- |

Un **tope de tamaño** de un rol no va aquí: es un **presupuesto** y su hogar es el manifiesto
(sección Presupuestos, con el ritual `config`). Aquí van las excepciones de **contenido**.

Si el proyecto no está en el idioma del kit, su versión traducida del **vocabulario de detectores**
(las listas de `grep` de `{{kit}}/SKILL.md` → AUDITAR) también vive aquí: es una lista larga y viva,
no un parámetro del manifiesto.

## Convenciones de texto (y dónde se escapan)

Si este proyecto restringe qué se puede escribir a un archivo —solo ASCII, terminología fija, lo que
sea—, la regla vive en `{{entry}}` → Convenciones. Lo que va aquí es **cómo se comprueba**, porque una
convención de texto no falla donde está escrita sino donde se escribe.

**Los dos sitios calientes son las filas append-only y el mensaje de commit.** Son los únicos momentos
del cierre en que se redacta **prosa narrativa hacia un archivo**; en el resto se escriben rutas e
identificadores, donde el error salta solo. Ahí es donde se cuela el registro con el que se le habla al
usuario.

**Se comprueba con un comando, no releyendo**, y como paso del cierre. Ejemplo para una regla de
solo-ASCII, sobre lo que acabas de escribir:

```bash
# la fila que acabas de añadir
tail -n 1 {{history_dir}}{{index}} | LC_ALL=C grep -n '[^ -~]'
# el mensaje del commit, antes de pushear
git log -1 --format=%B | LC_ALL=C grep -n '[^ -~]'
```

Sin salida = limpio. Con `persistencia = git`, si el mensaje ya está commiteado pero **no pusheado y
sin hash citado en ningún doc**, `--amend` lo arregla (ver reglas de `--amend` en CERRAR).

## Checklist de inicio / cierre

Condensados en `{{kit}}/SKILL.md` (rituales ABRIR / CERRAR). Este archivo es la referencia de
formato cuando haya dudas.

## Operaciones de bajo coste (preferir)

- Apéndice de fila → `printf '...' >> archivo`.
- Archivo pequeño de formato fijo → un `Write`.
- Buscar en archivo grande → `grep -n` + lectura por rango.
- Si vas a EDITAR, léelo con el tool `Read` (no `cat`/`sed`) o el `Edit` se bloquea.
- Volumen mecánico grande → delegar a un subagente.
