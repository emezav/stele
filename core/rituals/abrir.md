<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

# Ritual: ABRIR sesión (ponerse al día, barato)

Lee, en orden, SOLO la **lista de arranque** del proyecto (generada; con defaults del módulo de producto):

1. `entry` - 2. `gotchas` - 3. `state` - 4. `handover` — **si su Estado != `SIN_TRABAJO_ACTIVO`**,
respétalo **y compruébalo** antes de editar (abajo). Bajo demanda (grep): `charter` (1.a vez /
orientación), `protocol`, `specs`, `architecture`, `index`/`session`.

**El `handover` es el único doc del set que puede estar caducado, y hay que comprobarlo.** Se escribe
**antes** de la primera edición y no se vuelve a tocar hasta el cierre: en esa ventana su contenido es
**pasado presentado en presente**, y si la sesión murió ahí, miente. El `state` no tiene el problema
—se reescribe al cerrar, así que está al día o no existe.

Cuesta un comando, con VCS o sin él: **recalcula la huella de los ficheros de su `Sello`** y compárala.
Lo que no case lo tocó alguien después de escribirlo: **manda el disco, no el documento** — mira qué
cambió en esos ficheros y dilo al usuario en el saludo, en vez de proponerle trabajo ya hecho. **Un
commit ajeno que no toca el alcance no lo caduca:** con dos sesiones en el mismo proyecto el `HEAD` se
mueve sin que tu trabajo haya avanzado, y por eso el `HEAD`, si lo hay, es atajo y no decisor.

```sh
# el sello: al abrir el checkpoint se anota la salida; al retomar se corre igual y se compara
for f in README.md ruta/que/aun/no/existe.md; do   # los ficheros del alcance, por nombre
  if [ -f "$f" ]; then printf '%s  %s\n' "$(tr -d '\r' < "$f" | sha256sum | cut -c1-12)" "$f"
  else printf 'ausente       %s\n' "$f"; fi
done
```

Si ese `handover` trae un sello viejo —solo un `HEAD`, o ninguno—, la comprobación equivalente es mirar
si hay cambios posteriores al último cierre **en su alcance**; ponle el sello por contenido la próxima
vez que abras un checkpoint.

**El síntoma, para reconocerlo sin comando:** si el `handover` te propone construir algo que **ya
está construido**, caducó. Caso de campo, y de manual: un agente arrancó por una puerta del marco y
propuso *"convertir los dos archivos de la raíz en puertas delgadas"* — leyendo el proyecto **a través
de las dos puertas delgadas ya construidas**. El sello que lo destapaba estaba escrito en el propio
documento; nadie lo había mandado mirar. **Un doc que afirma en vez de comprobar falla en silencio, y
el saludo sale igual de convincente.**

**Sin VCS esto aplica igual**, y es lo que ganó el sello al dejar de ser un commit: la huella se
recalcula sobre los ficheros, no sobre una historia. Antes, sin VCS el `handover` no se podía
contrastar contra nada y lo sustituía una descripción libre de *qué se observa en disco*, que nadie
podía comprobar.

**Y si el `handover` declara un CIERRE MÍNIMO, el saludo lo dice.** El campo trae un contador —*"MINIMO
2 de 3"*— y la lista de lo que falta. **Va en el saludo porque es el único sitio donde la deuda del
cierre se ve sin buscarla**, y porque al tercero el siguiente cierre tiene que ser completo: si eso no
se anuncia al abrir, se descubre al cerrar, que es cuando ya no hay tiempo. Está en CERRAR -> *El cierre
mínimo*.

**Confirma el arranque (visible):** un agente **no puede hablar antes de que el usuario escriba**,
así que la confirmación va **al frente de tu PRIMERA respuesta** — 1-3 líneas: última sesión
(N + título), si quedó trabajo a medias, próximo paso propuesto. En llano, nombrando los archivos
(ver "Cómo se le habla al usuario"). Sin esto, el arranque silencioso es
indistinguible de uno que no corrió. (Se omite si `session_greeting = off`.)

**Y si el próximo paso es PLURAL, va como lista, y las líneas cuentan lo que la precede — no lo que la
compone.** El contrato dice *próximo paso* en singular y muchas veces no lo es: cuatro decisiones que
bloquean son cuatro renglones que el usuario va a leer para actuar, no prosa que tú resumes. Es
*cuando el usuario tenga que actuar sobre una lista, dásela como lista* (`{{kit}}/SKILL.md`).

**En el saludo la lista va como VIÑETAS, no como tabla**, y no es estilo: el saludo **se dice**, y lo
dicho se copia. Una tabla pierde sus `|` al pegarse fuera de la terminal y llega como texto corrido —
medido en campo. La tabla se reserva para lo que se escribe a un archivo.

> **Medido en campo, dos veces:** los dos agentes que ejecutaron esto expandieron el saludo **por el
> mismo motivo** —el próximo paso era una lista de cuatro— y **los dos acertaron sin que el ritual se
> lo dijera**. El número no era el problema: un saludo de tres líneas con las tres piezas **cabe**. Lo
> que faltaba era esta línea.

## Y si el proyecto tiene correspondencia, dila en el saludo

**Solo si `correspondence_dir` está activo.** Un proyecto sin corresponsales se salta esta sección
entera.

**Lo que hay que decir son dos listas, y las dos las tiene ya el `handover`:**

| Lista | De dónde sale |
| --- | --- |
| **Sin entregar** — cartas escritas que el usuario aún no ha hecho llegar | la tabla *Lo que espera al usuario* del `handover` |
| **Esperando respuesta** — corresponsales cuya última carta del hilo es nuestra | el índice de correspondencia, con un comando |

**No cuesta contexto**, que es lo que hace que quepa: el `handover` ya está cargado, y la segunda lista
sale de un comando cuya **salida** son unas pocas líneas aunque el índice pese cientos de kilobytes.

```bash
# ultima carta de cada hilo; si la direccion es la de salida, esperamos respuesta
awk -F'|' '/^\| *[0-9]+ *\|/{n=$2;d=$4;c=$5;gsub(/ /,"",n);gsub(/ /,"",d);
  gsub(/^ +| +$/,"",c); if(d=="->"||d=="<-"){ult[c]=d; num[c]=n; f[c]=$3}}
  END{for(k in ult) if(ult[k]=="->") printf "  %-32s nuestra %s (%s)\n", k, num[k], f[k]}' "$INDICE"
```

**Y la regla que decide cómo se dice: enumera, no diagnostiques.**

> **"Esperando" no es "tardan".** Los días desde que salió la nuestra **no miden al corresponsal**:
> miden el canal, que casi siempre es una persona que copia y pega, y que ninguno de los dos lados
> puede observar.

**El caso es propio y reciente.** Un proyecto midió el silencio de un corresponsal —seis días contra
un máximo histórico de uno—, lo escribió en su estado **como el único dato que no encajaba**, y sacó de
ahí una conclusión sobre el otro proyecto. La carta llegó fechada **seis días antes**: existía desde el
primer día. **La medida era correcta y la inferencia falsa**, y lo que la delató fue que aquella carta
llevaba **fecha de redacción** y las propias no.

**De ahí sale la otra mitad, barata:** que las cartas propias lleven **fecha de redacción** además de la
de entrega. Sin ella, *"días desde que salió"* y *"días que llevan sin contestar"* son indistinguibles,
y el segundo es el que uno escribe sin darse cuenta.

## Y si otra sesión trabaja a la vez: titular y satélite

**El marco admite dos sesiones independientes en el mismo proyecto** —dos agentes, dos ventanas, que
coordina el usuario desde fuera—, con una condición: **una es la titular y las demás, satélites**. La
titular lleva el `handover`, el `state`, las actas y la numeración; el satélite no toca ninguno, porque
dos escritores en un documento que se reescribe entero se pisan sin error.

**Cómo sabes que eres satélite:** te lo dice el usuario, o al abrir encuentras el `handover` en
`EN_PROGRESO`. **El sello no dice si ese trabajo está vivo**: casa si la otra sesión aún no editó y no
casa si ya editó, igual que el de una sesión que murió sin cerrar. Eso solo lo sabe el usuario:
**pregunta** antes de seguir, y no retomes ese trabajo hasta que te conteste.

**Siendo satélite:**

- **Tu checkpoint es una nota propia**, `{history_dir}satelite-YYYY-MM-DD-<tema>.md`, creada **antes de
  tu primera edición** con la forma del `handover`: objetivo, alcance y sello. Cumple la regla dura sin
  tocar el `handover` de la titular, y un fichero con nombre propio no choca con nada.
- **Saluda declarándote satélite**, y no propongas el próximo paso del proyecto: es de la titular.
- **No tienes número de sesión, y cierras completando la nota** (CERRAR, *Siendo satélite*).

**Siendo titular, el saludo lista las notas sin recoger**, igual que la correspondencia. Lo que queda en
el historial con ese prefijo es exactamente lo pendiente, porque al recogerlas se mueven:

```bash
HIST=ruta/del/historial/   # {history_dir}
ls "$HIST"satelite-*.md 2>/dev/null   # cada una: trabajo de un satelite que tu acta aun no recoge
```

**Lo que esto no arregla, y se dice:** el saludo es una foto. Si la otra sesión mueve algo después de
que saludes, el saludo no lo sabe; lo sabrá el sello, al recalcularlo.
