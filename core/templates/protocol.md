# {{protocol}} — Protocolo de documentación entre sesiones

> **Cómo** se documenta (formatos, convenciones de edición, topes de tamaño) para que ningún
> archivo de estado crezca sin límite y para minimizar tokens/roundtrips de cualquier agente.
> Los rituales condensados están en `{{kit}}/SKILL.md`; el *por qué*, en `{{kit}}/guide.md`.
> Los nombres de archivo abajo son los del manifiesto (`stele.config.md`).

## Principios

<!-- LEY:INICIO ancla="por encima de cualquier default" -- misma clase: se porta siempre. -->

1. Los archivos de **estado** no crecen: se **sobrescriben** (formato fijo + tope).
2. El **historial** vive en archivos por sesión y **no se reabre** (se referencia por link).
3. Los **apéndices de una línea** usan `printf '...' >> archivo`, no `Read`+`Edit`.
4. Ninguna sección de un doc fuente-de-verdad supera **~150-200 líneas**; si crece, se extrae a
   un tema de `{{specs}}` + link.
5. Nada de esto vive en memoria privada del agente; todo en el proyecto. **Tampoco los artefactos**
   —scripts de un solo uso, extracciones, volcados—: su hogar es `{{artifacts_dir}}sesion-{NNN}/`, y
   esa regla vale por encima de cualquier default del harness (`{{kit}}/SKILL.md` → Precedencia).
6. **Un hogar por dato** (mapa en `{{kit}}/SKILL.md` y en `{{entry}}`).

<!-- LEY:FIN -->

## Rutas: comando contra enlace

Dos clases, y no se resuelven igual:

- **Ruta de comando** (`printf '...' >> …`, `grep`, `git log --`): siempre **desde la raíz del
  proyecto**, porque ahí opera el agente. Incluye `{{history_dir}}` delante del nombre del archivo.
- **Enlace Markdown clicable** (`[{{index}}](./{{index}})`): relativo **al archivo que lo contiene**,
  que es como lo resuelve cualquier visor.

El `printf >>` del cierre es el que más vigilar: si la ruta está mal, **no da error** — crea el
archivo que falta y el bueno se queda sin la fila. Si hay más de una copia del comando en los docs,
todas deben decir lo mismo; mejor aún, que solo una lo deletree y las demás lo nombren en prosa.

**Y contar las columnas de la fila no basta: cuenta separadores, no valida celdas.** Una fila con las
columnas **permutadas** —la descripción donde van las horas, las horas donde va la descripción— tiene
el número correcto de `|` y **pasa el control en verde**. Ocurrió dos veces seguidas en el log de
esfuerzo de este mismo proyecto, y lo destapó una **suma**, no el control: las dos filas quedaron fuera
del total sin que nada avisara.

```bash
# columnas: necesario, no suficiente
tail -n 1 "$FILA" | awk -F'|' '{print NF-2}'          # tiene que dar las de la cabecera
# y ANCLA una celda por su FORMA, la que tenga forma reconocible:
tail -n 1 "$FILA" | awk -F'|' '{print ($4 ~ /^ *[0-9.,-]+ *$/) ? "OK" : "PERMUTADA"}'
```

> **Un control de forma que solo cuenta piezas no distingue una fila bien puesta de una barajada.**
> Basta con anclar **una** celda de formato reconocible —una cifra, una fecha, una flecha— para que la
> permutación deje de pasar.

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

**Siempre que se toque** —al cerrar y también a mitad de sesión— se reescribe COMPLETO con `Write`.
Nunca `Edit`: una sustitución cuyo ancla ya no existe **no falla, no hace nada**, y eso se ve igual que
"ya estaba bien". Y nunca prepend ni rename de "anterior".
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

Detalle completo: qué se hizo, decisiones, archivos tocados, verificación, **tropiezos**, notas para
retomar, y `## Esfuerzo equivalente` (si se usa). `NNN` con padding a 3 dígitos. No se reabre; se lee
con grep.

**La fecha se mide, no se recuerda** — `date '+%Y-%m-%d %z'`, o `Get-Date -Format 'yyyy-MM-dd K'`. La
que inyecta el harness suele ser **UTC** y adelanta un día durante la última franja horaria local; y
`git log -1` **no da hoy, da el día del último commit**. Si el shell no corre en la máquina del usuario
las dos señales fallan a la vez y coinciden, así que **se pregunta**. Detalle y sonda de zona en
CERRAR → *De dónde sale la fecha*. Vale para toda fecha estampada, no solo la del acta.

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

**Excepción: una carta publicada en un buzón propio no necesita copia aquí.** Ese archivo está
versionado, así que el historial del control de versiones conserva lo que la curación retire — la copia
ya existe. Solo lo que sale por un canal que no deja rastro (chat, correo, un mensaje pegado) necesita
archivarse aparte.

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
`EN_PROGRESO` con objetivo/alcance/verificación **y sello**. Al cerrar, siempre refrescar el puntero a
la sesión que se cierra AHORA. Plantilla en `{{kit}}/core/templates/handover.md`.

**El sello, y por qué es el cuarto y no un adorno.** Es el `HEAD` al abrir el checkpoint (con VCS) más
la instrucción de compararlo. Existe porque este es **el único doc del set de arranque que puede estar
caducado**: se escribe *antes* de la primera edición y no se vuelve a tocar hasta el cierre, así que en
esa ventana afirma en presente algo que ya es pasado. **Cuando el sello y el `HEAD` discrepan, manda el
árbol** — el `handover` no compite con el repositorio, lo indexa. La comprobación es del ritual ABRIR;
aquí vive el formato.

### `{{specs}}` (+ temas)

Una sección por decisión o feature, **autocontenida**. Se lee con `grep` y por rango, nunca entera:
lo que importa es que cada sección sea corta por sí misma, no el tamaño del archivo.

**Umbral de extracción: ~50 líneas.** Por debajo, la decisión vive aquí. Por encima, se mueve a
`{{specs_dir}}<TEMA>.md` y aquí queda un resumen de 2-3 líneas + link; si un tema supera ~600-800
líneas, se divide en sub-temas. `{{specs_dir}}` se crea **cuando la primera decisión cruce el
umbral**, no en el scaffold: una carpeta vacía no documenta nada.

### `{{gotchas}}`

Hogar único de las trampas de **trabajar en este proyecto**: del entorno, de las herramientas, de
publicar, de verificar, de dónde guardar. Se edita incrementalmente pero se **cura** — una entrada
resuelta u obsoleta se **borra**, y su rastro queda en el historial.

**Es el complemento de `{{index}}`, y por eso se lee en cada arranque y el historial no.** El
historial es episódico, crece y es inmutable: guarda *qué pasó*. Este guarda *qué no es obvio*. Una
lección que solo vive en un acta está enterrada en un archivo que nadie vuelve a abrir.

**No la guardes en `{{handover}}`**: ese doc se poda por diseño, y ahí la trampa muere sin aviso.
Caso real de campo.

## Acuerdos de auditoría

Lo que el ritual AUDITAR señaló y el usuario decidió **no** cambiar. Se registra aquí para no
rediscutirlo en cada auditoría, y **siempre con umbral** — eso es lo que lo hace una decisión y no un
aplazamiento. Sección **curada**: al cruzarse el umbral, el acuerdo se revisita y se reescribe o se
borra.

| Fecha | Doc | Acuerdo | Umbral de revisión |
| --- | --- | --- | --- |

Un **tope de tamaño** de un rol no va aquí: es un **presupuesto** y su hogar es el manifiesto
(sección Presupuestos, con el ritual `config`). Aquí van las excepciones de **contenido**.

## Detectores de auditoría

**Solo si este proyecto no está en el `idioma` del kit.** Si coincide, esta sección se queda vacía y no
hay nada que hacer: los detectores de `{{kit}}/core/rituals/auditar.md` ya sirven tal cual.

Los detectores **léxicos** y **gramaticales** de AUDITAR están en el idioma del kit. Un proyecto en
otro idioma **los deriva** —no los traduce término a término, que produce detectores malos— y guarda
los suyos aquí. Es una lista larga y viva, no un parámetro: por eso no va al manifiesto. Y no va en
*Acuerdos de auditoría*, que guarda decisiones con umbral y esto no lo es.

| Clases | Comando | Control positivo (tiene que dar match) |
| --- | --- | --- |

**El control positivo no es opcional.** Una regex recién escrita y nunca ejercida es la mejor fuente
de ceros falsos que hay, y aquí un cero falso se lee como *"corpus limpio"*. Un detector sin su línea
de ejemplo no se guarda.

**Un término entra con el hallazgo que lo justificó, no porque suene a que podría.** Cada término que
se añade lo paga cada auditoría futura en ruido; si nunca cazó nada, no está ganado.

**Si cambia `idioma` (ritual `config`), esta sección queda caducada entera** y la siguiente auditoría
la vuelve a derivar. No se traduce lo que había.

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
# CONTROL POSITIVO, en la misma tanda, CON SU NUMERO ESPERADO: tiene que dar 2
printf 'a\303\251b\n' | LC_ALL=C grep -o '[^ -~]' | wc -l    # 2 = una acentuada, dos bytes
```

**El control lleva un número, y no un "algo salió", porque un `>= 1` no puede fallar.** El acentuado
del control es **un** carácter y **dos** bytes: bajo `LC_ALL=C` el detector reporta **2** y bajo un
locale UTF-8 reporta **1**. Un control que solo exigiera salida pasaría en verde **en los dos mundos**,
así que certificaría igual el detector sano y el detector que está casando caracteres donde tú crees
que cuenta bytes. Con el 2 escrito al lado, el mismo comando comprueba dos cosas: que el detector
dispara **y** que estás en la semántica que el patrón supone. Ver `{{kit}}/core/reference/verificar.md` →
*"Un número sin expectativa no es información: es decoración"*.

**Sin salida = limpio SOLO si el control positivo dio su número.** Un detector roto no da error: **da
silencio, que es exactamente lo que esperabas ver.** Va en la misma tanda y no como advertencia aparte,
porque una advertencia hay que leerla desde dentro de la vía que ya elegiste — así la vía segura cuesta
lo mismo. Casos reales de este silencio: un `grep -P` que aborta por *locale* y escribe el error a
**stderr**, un `grep` en minúscula contra un encabezado en mayúscula, y un `grep -o` con el patrón mal
escrito. Los tres "corpus limpios" eran ceros de un comando que no llegó a mirar.

El control usa `printf` con escapes **octales** y **no contiene** ningún byte no-ASCII: así el propio
ejemplo sobrevive a un barrido que sustituya esos caracteres. Octal y no hexadecimal porque `\303\251`
es POSIX y `\xc3\xa9` es una extensión — un control que no corre en la shell de quien lo copia devuelve
silencio, que es el fallo que este control existe para descartar.

Con `persistencia = git`, si el mensaje ya está commiteado pero **no pusheado y
sin hash citado en ningún doc**, `--amend` lo arregla (ver reglas de `--amend` en CERRAR).

## Topes del set de arranque: se comprueban, no se recuerdan

**Obligatorio en el cierre, igual que el barrido de arriba.** Un tope que nadie mide no es un tope, es
una intención — y a diferencia del texto, este no falla al escribirlo: falla **creciendo**, un poco por
sesión, sin que ninguna sesión lo note.

**El bloque lee las cifras del manifiesto y no lleva las suyas.** Si las copiara sería un segundo hogar,
y el segundo hogar es el que se queda viejo justo cuando alguien sube un tope con el ritual `config`:

```bash
sed -n '/^## Presupuestos/,/^## [^P]/p' stele.config.md | grep '^| [a-z]' |
while IFS='|' read -r _ rol tope _; do
  rol=$(echo "$rol" | tr -d ' '); tope=$(echo "$tope" | tr -dc 0-9)
  case "$rol" in
    entry) f={{entry}} ;;    gotchas)  f={{gotchas}} ;;
    state) f={{state}} ;;    handover) f={{handover}} ;;
    *) continue ;;
  esac
  n=$(wc -l < "$f")
  [ "$n" -le "$tope" ] && e=OK || e="PASADO por $((n-tope))"
  printf '  %-9s %4s / %-4s %s\n' "$rol" "$n" "$tope" "$e"
done
printf '  CONTROL   6 / 5    %s   (esperado PASADO)\n' "$([ 6 -le 5 ] && echo OK || echo PASADO)"
printf '  CONTROL   4 / 5    %s   (esperado OK)\n'     "$([ 4 -le 5 ] && echo OK || echo PASADO)"
```

**El `case` filtra a propósito, y por eso lleva su `continue`.** La sección *Presupuestos* puede tener
filas que no son docs de tu proyecto —el kit se pone tope a sí mismo ahí— y una fila cuyo rol no
reconoces no es un error: es una fila que no te toca. Sin el `continue`, `wc -l` va contra un fichero
que no existe y el cierre se para en algo que no era un problema.

**Los dos controles van dentro del bloque y con su valor esperado.** Uno solo no separa: un detector
que siempre dijera `PASADO` pasaría el control positivo, y uno que siempre dijera `OK` pasaría el
negativo. Hacen falta los dos porque lo que se comprueba no es que el comando hable, es que
**discrimine**. Es la misma exigencia que el `2` del control de no-ASCII —*un número sin expectativa no
es información: es decoración*, `{{kit}}/core/reference/verificar.md`— aplicada a un detector que
responde con una palabra en vez de con una cifra.

> **Por qué existe, medido en el repo del propio marco antes de escribirlo:** de los cuatro topes del
> arranque, **dos estaban rotos y nadie lo había mirado nunca**. El único que se cumplía no era el
> mejor puesto: era **el único que alguien tecleaba a mano cada sesión**. Lo que hacía cumplirse a ese
> tope no era el tope, era el hábito de mirarlo.

**Y no lo pongas detrás de una tubería si vas a mirar su código de salida** — con `| tee` el código que
ves es el de `tee`. Es la ley *un comando que aborta detrás de una tubería es un cero*, aplicada al
bloque que la cita.

**Lo que este detector NO acota son los tokens**, y conviene saberlo antes de fiarse: cuenta líneas
porque las líneas son lo que el manifiesto presupuesta, pero lo que cuesta cargar el arranque son
tokens, y una línea de prosa envuelta a 100 columnas no vale lo que una de tabla. Un doc puede estar
conforme en las cuatro filas y seguir siendo caro. Para el precio real hay que medirlo con un
tokenizador, y eso no es un paso de cierre: es una medición, con su fecha (ver *una cifra sobre tu
corpus es una FOTO*).

## El respaldo declarado: se comprueba que alguien contestó, no que exista

**Va en el cierre, colgado de un paso que ya se da**, por la misma razón que los topes: un recordatorio
*a la vista* no se corre y una comprobación *en el camino* sí. Solo aplica si `base` está excluido del
control de versiones (BOOTSTRAP, paso 7).

```sh
grep -c '^# RESPALDO:' .gitignore      # esperado: 1
grep -c '^# RESPALDO:' /dev/null       # CONTROL NEGATIVO: 0
```

**Lo que comprueba y lo que no, dicho para que nadie se confíe.** No comprueba que el respaldo
**exista** —eso no se puede saber desde el texto, y ningún detector lo sabrá—: comprueba que **alguien
contestó la pregunta**. Es exactamente lo que la convención pide, y es menos de lo que parece.

**Y el control negativo no sobra aunque parezca tonto.** Sin él, un `1` no distingue *"la línea está"*
de *"el patrón casa con cualquier cosa"*, y este detector es de los que caen en eso: si alguien escribe
el patrón sin el ancla `^`, empieza a contar la palabra dentro de la prosa de otros comentarios.

> **Por qué el valor va enumerado y no en prosa** — el caso está en BOOTSTRAP, paso 7: un `.gitignore`
> decía *"su respaldo es aparte del de este repo"*, que **no declara nada y suena a decisión tomada**, y
> nadie lo miró en 39 sesiones. Este detector **habría dado 0 ahí**, que es precisamente su oficio: la
> prosa tranquilizadora no casa con `^# RESPALDO:`.

**Aporte de un adoptante**, que lo propuso después de aplicar la convención a un fichero real.

**Sin VCS este bloque no aplica y el hueco es real.** Con `persistencia = ninguna` no hay `.gitignore`
donde poner la declaración. El sitio que ha funcionado en campo es el **`Sello` del `handover`**, que
sin VCS ya dice *qué se observa en disco*: ahí cabe **qué existe y qué no tiene copia**. No está
resuelto como convención — se dice para que quien llegue a ese caso sepa que es un hueco conocido y no
un olvido suyo.

## Los bloques de LEY del kit están en tus documentos, o falta uno

**Una plantilla de rol mezcla dos clases de prosa**: la que tu proyecto puede haber reescrito
legítimamente —y que por eso no se pisa al actualizar— y la que es **ley del marco** y tiene que
llegar aunque el documento ya exista. La segunda va marcada `LEY:INICIO` … `LEY:FIN`.

**Y esta comprobación es la única que caza el hueco en un proyecto que YA lo tiene**, porque no mira el
diff de una actualización: mira el estado de hoy.

```bash
# cada marca declara el ANCLA que hay que encontrar. Se comprueba el ancla y NO el texto,
# porque una instancia puede condensar la regla con otras palabras y seguir teniendola:
# comparar la linea entera da FALTA sobre reglas que si estan.
# Y el doc se APLANA antes de buscar, porque plantilla e instancia envuelven distinto.
grep -o 'LEY:INICIO ancla="[^"]*"' "$KIT/core/templates/entry.md" |
  sed 's/.*ancla="//; s/"$//' |
  while IFS= read -r ancla; do
    tr '\n' ' ' < "$ENTRY" | tr -s ' ' | grep -qF "$ancla" || echo "FALTA en $ENTRY: $ancla"
  done
# CONTROL POSITIVO: borra el bloque de una copia del doc y tiene que decir FALTA
# CONTROL NEGATIVO: sobre el doc intacto no puede decir nada
```

> **Su ausencia es un hueco, no una decisión.** Si el bloque no está, no es que tu proyecto lo haya
> descartado: es que **nunca llegó** — y el diff de la actualización no lo señaló, porque el fichero
> ya existía.

**Y hay un coste que conviene saber antes de portarlo:** estos bloques **no caben gratis en un `entry`
con el presupuesto ajustado**. Un adoptante que portó el `PROHIBIDO` de los artefactos pasó de **237 a
248 líneas contra un tope de 240** que acababa de subir de 200. **Portar una ley puede obligar a subir
un presupuesto**, y es mejor saberlo al portarla que al medir los topes.

## Lo que se escapó fuera del proyecto: se enumera, no se pregunta

**El control hermano del anterior.** Aquel comprueba *lo que debía estar dentro y no está*; este,
**lo que está fuera y debería estar dentro** — y es el único de los dos que **devuelve el fichero**.

**No hace falta creerle nada al agente.** El harness abre un directorio por sesión bajo una raíz
temporal, esa raíz está fuera del proyecto pero **no fuera de la máquina**, y el cierre es un comando
que corre en la máquina:

```bash
# RAICES es configuracion del proyecto, no un supuesto del kit: cada harness usa la suya
for raiz in $RAICES; do
  [ -d "$raiz" ] && find "$raiz" -type f -newermt "$DESDE" -printf '%TY-%Tm-%Td %8s %p
'
done
# CONTROL POSITIVO: escribe un fichero ahi a proposito y tiene que aparecer
```

> **La señal indirecta detecta que falta algo; la directa devuelve el fichero.** En el caso que la
> aportó, ese fichero —un clon del kit olvidado en el temporal— **resolvió una pregunta que las dos
> partes habían dado por indecidible** y que estaba escrita bajo *"qué NO demuestra"* en las dos
> cartas.

**Y tiene dos límites, los dos medidos:**

- **La raíz depende del harness.** Un proyecto que no conozca la convención de su agente no puede
  escribir el `find`, y ahí la señal indirecta es la que hay. **Por eso `RAICES` es configuración.**
- **Una raíz COMPARTIDA no se puede enumerar sin criterio de pertenencia.** Corrido aquí, el
  scratchpad por proyecto estaba **limpio** y la fuga real estaba en `/tmp`… que tiene **5 934**
  ficheros de otros programas. **Enumerarla sin filtro se trae basura ajena**: hace falta acotar por
  fecha, por nombre o por contenido, y decir cuál se usó.

**El caso de campo es propio y va entero:** 16 ficheros de este proyecto —borradores de cartas y
trozos de prosa que acabaron en el kit— vivían en `/tmp` desde hacía nueve sesiones. **Ninguna
comprobación los miraba**, porque la única raíz que teníamos en la cabeza era la que el harness
declara.

## Párrafos partidos: la prosa que se rompe al insertar algo en medio

**Editar un documento largo insertando bloques parte párrafos, y el linter no lo ve.** El resultado es
una frase que empieza en minúscula colgando debajo de una cita o de un bloque de código, y **la mitad
de arriba se queda dentro de la cita afirmando algo que no le toca**.

```bash
# prosa en MINUSCULA que sigue a algo que NO es prosa: blanco, cita, tabla o cierre de bloque
awk '/^```/ { code=!code; prev="CODE"; next }
     NR>1 && !code && (prev=="" || prev ~ /^>/ || prev ~ /^\|/ || prev=="CODE") &&
     $0 ~ /^[a-z]/ && $0 !~ /^(https?:|www\.)/ { print FILENAME ":" NR }
     { prev=$0 }' "$DOCS"/*.md
# CONTROL POSITIVO: una cita seguida de una linea en minuscula -> 1
# CONTROL NEGATIVO: prosa sana con parrafos en negrita          -> 0
```

**Dos avisos sobre el patrón, los dos aprendidos fallando:**

- **La clase de caracteres no puede incluir `*` ni `` ` ``** — casi todo párrafo abre en negrita, y el
  detector devuelve el fichero entero.
- **Y la versión que solo mira «tras línea en blanco» NO caza el caso que la produjo**, porque el
  huérfano suele quedar **pegado** a la cita que lo partió. Hay que mirar también qué había antes.

**En su primera corrida sobre este kit encontró dos**, y el segundo era de contenido y no de forma:
media frase se había quedado **dentro de una cita**, que pasaba así a afirmar algo ajeno. **Llevaba
once sesiones ahí** — y esa cifra costó dos correcciones: la primera dijo *cinco* apoyándose en un
`git log -S`, que **data el texto y no el defecto** (ver *Un instrumento que data el TEXTO…*).

> **Un párrafo partido no rompe nada que un linter mire**: la sintaxis es válida, los bloques cierran
> y las tablas cuadran. **Lo único que se rompe es la frase**, y eso solo lo ve quien la lea entera —
> que es justo lo que nadie hace en un documento que solo se parchea.

## Cada comprobación del cierre anota cuándo disparó por última vez

**Un bloque de comprobaciones no se degrada de golpe: se degrada por una.** Y el problema es que una
comprobación que lleva sesiones sin decir nada **se ve igual** en los dos casos posibles — que el
proyecto esté limpio, o que nadie esté leyendo su salida.

> **Una comprobación que lleva N sesiones sin disparar no está probada inútil, pero sí está probada
> NO-OBSERVADA.** Es *un cero no vale sin control positivo* aplicado al **tiempo** en vez de al comando.

**Cuesta una columna**, al lado del valor esperado:

```text
comprobacion            esperado   ultimo disparo
  no-ASCII de la fila      0         sesion N   (disparo: una letra acentuada)
  columnas de la fila      6 y 6     sesion N-6 (disparo: cuatro filas con una columna de mas)
  topes del arranque       4 OK      sesion N-10
  respaldo declarado       1         nunca -- anadida en la sesion N-12
```

**Qué hacer con un `nunca` o con un número viejo**, y no es quitarla: **es correrla contra un caso
fabricado**. Si no dispara ni con el caso plantado, está rota; si dispara, está viva y el proyecto
estaba limpio. **Las dos respuestas valen; lo que no vale es no saberlo.**

**Aporte de un adoptante**, que lo propuso tras medir su propio bloque y encontrar que de sus
comprobaciones **ninguna figuraba** como origen de un tropiezo registrado — no porque no dispararan,
sino porque sus disparos **no llegaban a la sección donde se cuentan**.

## Checklist de inicio / cierre

Condensados en `{{kit}}/core/rituals/` (un archivo por ritual). Este archivo es la referencia de
formato cuando haya dudas.

## Operaciones de bajo coste (preferir)

- Apéndice de fila → `printf '...' >> archivo`.
- Archivo pequeño de formato fijo → un `Write`.
- Buscar en archivo grande → `grep -n` + lectura por rango.
- Si vas a EDITAR, léelo con el tool `Read` (no `cat`/`sed`) o el `Edit` se bloquea.
- Volumen mecánico grande → delegar a un subagente.
