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
