# Sesión `N` — `YYYY-MM-DD`

Resumen de 1-3 líneas: qué se hizo y por qué importa.

## Qué se hizo

- ADAPTAR: bullets de lo realizado, con contexto suficiente para retomar.

## Decisiones

- ADAPTAR: decisiones tomadas y su porqué. **Si perduran, llevarlas también a su hogar**
  (`{{charter}}` / `{{specs}}` / `{{architecture}}` / `{{gotchas}}`) — aquí queda el registro, no el hogar.

## Archivos tocados

- `ruta/archivo` — qué cambió.

<!-- No anotes aquí el hash del commit que incluye ESTE archivo: un commit no puede contener su
     propio hash, y se recupera con `git log --diff-filter=A -- <este archivo>`. Los hashes de
     commits anteriores de la sesión sí van. Ver {{protocol}}. -->

## Verificación

- ADAPTAR: qué se probó y con qué resultado (tests, build, E2E, verificación en vivo). Ser
  honesto: si algo quedó sin probar o falló, decirlo.

## Tropiezos

- ADAPTAR: **qué salió mal en el camino** — lo que se intentó y no estaba, lo que costó tres intentos,
  el detector propio que falló, la conclusión que hubo que retirar. **No los defectos del producto: los
  del trabajo de esta sesión.**
- **Va aquí y no en *Qué se hizo* porque es la única dimensión que ningún artefacto reconstruye.** El
  árbol final, el diff y el commit dicen **qué** quedó hecho; **ninguno dice qué costó**. A posteriori
  no se distingue *"no estaba en la lista y era obvio"* de *"no estaba en la lista y costó una hora de
  tanteo"*. Si no se escribe al cerrar, **se pierde entero**.
- **El nombre de la sección es fijo a propósito, y esa es la mitad que se subestima.** Una sección que
  cada acta titula distinto —*"un fallo propio"*, *"dos defectos de contador"*, *"lo que el detector
  enseñó"*— **no se puede barrer, ni contar, ni auditar**: existe y es invisible.
- **Convergencia medida, y es la razón de que esté en la plantilla.** Dos proyectos que usan este marco
  la inventaron **por separado y sin acordarlo**, y ninguno la tenía escrita — uno se la atribuía al
  kit. Uno la titulaba de forma **estable**; el otro contó **17 de sus últimas 20 actas** con un título
  distinto cada vez.
  **Ese 17 salió de un patrón con las palabras que a su autor se le ocurrieron** (*fallo*, *tropiezo*,
  *detector*…), así que es **un suelo y no un número**: nadie lo contó a mano.
- **Y el nombre estable tampoco basta, que es la mitad contraintuitiva.** En el proyecto que sí lo tenía
  estable, su primer barrido dio **6** y el número real era **11** — contado a mano, porque **ningún
  patrón las encontraba**. No fallaba la estabilidad del título: fallaba que **ningún documento
  declaraba que la sección existía**, así que no había por dónde saber qué buscar.
  > **Un nombre estable que nadie ha escrito es tan invisible como uno variable.** Por eso la sección va
  > en la plantilla y no solo en la costumbre: lo que la hace contable no es el nombre, es **estar
  > declarada en un sitio donde alguien pueda leerla**.
- **Di QUIÉN lo cazó, y es el campo que más rinde.** Un comando, un control con su valor esperado, el
  usuario, un corresponsal — o **nadie, hasta que fue tarde**. Una línea por tropiezo.
- **Y cuenta también lo que encontró el CIERRE, no solo lo que rompió el trabajo.** Es el defecto que
  este campo tuvo en sus dos primeras semanas de uso, en dos proyectos a la vez: un acento cazado por el
  barrido, una fila descuadrada por el control de columnas, **se corrigen en el momento y no llegan a
  esta sección**, porque uno los vive como parte del cierre y no como un tropiezo del trabajo. Con esa
  omisión, el campo mide **la mitad que uno recuerda** y deja fuera justo la que contestaría si las
  comprobaciones sirven.
- **Por qué ese campo y no otro:** es lo único que permite contestar, con el tiempo, *qué impide de
  verdad un error* frente a *qué solo lo explica después*. Sin él, un proyecto puede tener cien
  tropiezos registrados y **seguir sin saber si sus reglas sirven**.
- **Caso de campo, y nace de un fracaso.** Se intentó clasificar quién había cazado los tropiezos de las
  doce últimas actas de un proyecto **que sí los registraba**: el clasificador dejó **cinco de ocho sin
  clasificar**, porque las secciones contaban el error y **no decían quién lo encontró**. La pregunta
  llegó de un corresponsal, no había datos para contestarla, y este campo salió de ahí.
- **Si no hubo ninguno, dilo en una línea.** *"Sin tropiezos"* es información: distingue una sesión
  limpia de una en la que nadie miró.

## Notas para retomar

- ADAPTAR: lo que la siguiente sesión debe saber (gotchas del entorno, pasos pendientes, palancas).
- **Aquí NO se copia un estado que vive en otro documento** — el de una carta, el de un proceso, el de
  un pendiente con su propio hogar. Un `session` **no se reescribe nunca**, así que un estado escrito
  aquí queda congelado en el instante de cerrar y a partir de ahí se lee como actual siendo un fósil.
  Se nombra el hogar y se apunta: *"queda la carta N sin contestar — mira su fila"*, no *"la carta N
  está sin contestar"*. Ver `{{kit}}/core/rituals/remitir.md` → las doce cabeceras: mismo defecto, otro
  artefacto, y ahí se decidió que **una omisión en una plantilla no se defiende sola**.
- **Y el peor de todos, aparte porque no es uno más de la lista: el CONTADOR.** *"Séptima sesión
  seguida sin desplegar"*, *"lleva once esperando"*. Un contador no solo caduca como cualquier estado:
  **su falsedad CRECE con el tiempo**, y se sigue leyendo como urgencia vigente mucho después de que
  el trabajo esté hecho. Es además **el que más se escribe**, porque al cerrar es el dato más expresivo
  que uno tiene a mano — y **un contador dentro de un registro inmutable es un contador que no se
  actualizará nunca**. Aporte de campo: un proyecto miró sus tres últimas actas, encontró tres frases
  de estado con tres desenlaces distintos, y la que **falsa al día siguiente** era justo el contador.
  Si el dato importa, va donde se pueda podar; aquí va la fecha y el hecho: *"al cerrar esta sesión
  llevaba siete sin desplegar"*, que no caduca porque **dice cuándo se contó**.

## Esfuerzo equivalente (OPCIONAL)

| Funcionalidad | {{effort_unit}} |
| --- | --- |
| `funcionalidad` | `X-Y` |
| **Total** | **`X-Y`** |
