<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

## Ritual: CONTRASTAR (recibir un informe externo sobre tu trabajo)

**Cuándo.** Llega de fuera un informe sobre lo que este proyecto produce: la revisión de un director
de tesis, los resultados de un laboratorio socio, la evaluación de un curso, el reporte de otro equipo
que usa tu producto. Llega **fuera de banda** —no al abrir ni al cerrar— y procesarlo cuesta, así que
**se invoca**, como auditar.

**Por qué no lo cubre ningún otro ritual.** Los siete anteriores miran hacia dentro: escriben la
documentación, mantienen el marco, o re-verifican lo ya escrito. El octavo, REMITIR, sí mira hacia
fuera —pero es la **salida**—. Ninguno maneja una **entrada de fuera**. Es la vía de mayor
consecuencia que tiene un proyecto —lo que entra por aquí se incorpora al
producto y viaja a todos— y es la única que no tenía procedimiento.

### La regla central: el diagnóstico viaja, el remedio no

Quien reporta tiene el **caso** —lo que pasó de verdad en su terreno, que tú no puedes ver—. Tú tienes
el **contexto de diseño** —por qué las piezas son como son, que él no puede ver—. Un informe llega
como prosa terminada con las dos cosas pegadas, y la parte sólida presta credibilidad a la otra.

**Acepta el diagnóstico por sus méritos y vuelve a derivar el remedio desde tu propio diseño.** No es
desconfianza: es que cada uno tiene la mitad que al otro le falta. Es el mismo movimiento que la
cautela 0 de AUDITAR — no te fíes del recorte, vuelve a la fuente.

**Y hay una razón por debajo, más concreta que *cada uno tiene su mitad*: el medio decide qué remedios
existen.** Un mismo diagnóstico —*este dato tiene dos hogares*— admite remedios distintos según de qué
esté hecho tu producto. Si es **código**, puedes generar un hogar desde el otro. Si es **prosa**, no
puedes generar nada, pero **puedes borrar**. Y si el sitio duplicado es la portada que lee todo el que
llega, no puedes borrar sin empeorar el producto, y lo que queda es **detectar**: una prueba que impide
*publicar* la divergencia, asumiendo que los dos hogares siguen ahí.

Caso de campo, con los dos lados a la vez: un proyecto de código y otro de documentación recibieron el
mismo diagnóstico y salieron con remedios opuestos —**borrar** y **detectar**—, y los dos correctos.
Lo que lo hace instructivo es que **ninguno de los dos podía haber elegido el del otro.** Por eso el
remedio no viaja: no es que sea menos fiable que el diagnóstico, es que **está atado a un material que
quien reporta no tiene**.

### Tres clases de afirmación, y no se tratan igual

- **Sobre tu trabajo** — verificables aquí, y se verifican **todas** antes de aceptar nada. Un aporte
  apoyado en una afirmación falsa sobre tu producto se cae entero por bien argumentado que esté.
  Evidencia con `archivo:línea`, como en AUDITAR.
- **Sobre el proyecto que reporta** — no verificables desde aquí, nunca. Se toman **bajo palabra** y
  se **marcan como tales** al registrarlas. Está bien apoyarse en ellas; lo que no vale es olvidar que
  no se comprobaron.
- **Propuestas** — no son afirmaciones y no se verifican: se **deciden**, al final y por separado.

**Y clasifica también lo que dice quien trae la carta.** Un informe casi nunca llega desnudo: llega
con un marco alrededor —*"esto viene de tal proyecto"*, *"esto salió del razonamiento del agente"*,
*"me lo pasaron sin contexto"*—, y **ese marco es una afirmación más**, de la segunda clase: habla de
un terreno que no puedes ver. Tómala bajo palabra si quieres, pero **márcala**, y sobre todo no
construyas una conclusión de diseño encima sin decir en qué se apoya.

Ocurrió: se dio por bueno que una observación venía del razonamiento interno de un agente ajeno —lo
dijo el cartero, de buena fe— y se levantó sobre eso una hipótesis. Al comprobarlo, la observación
estaba **commiteada en un archivo** del otro proyecto: visible desde cualquier sitio. La hipótesis
sobrevivió por otras razones, pero **su ejemplo la contradecía**.

**Y esto alcanza al archivo, no solo a la sesión en que llega: se archiva la carta, no el sobre.**
Marcarla te protege hoy; el archivo es donde el dato se releerá dentro de meses, ya sin nadie que
recuerde de dónde salió. Una atribución que venga en el sobre entra **marcada como no verificada**, o
no entra — nunca puede acabar leyéndose como si viniera firmada.

### Fases

0. **¿Ya llegó esta carta?** Antes de leerla con atención, búscala en tu archivo. **Con cartero humano
   el reenvío es normal** —se pega dos veces, se pega una vieja creyéndola nueva, se reenvía tras una
   interrupción— y no detectarlo sale caro: reprocesas, **reaplicas hallazgos ya aplicados**, escribes
   una fila duplicada y, en el peor caso, "descubres" lo mismo dos veces y lo registras como nuevo.
   Detectarlo cuesta un `grep`.
   **Busca por un token distintivo —un número raro, un comando literal—, nunca por el número de
   carta.** Cada proyecto numera **su propio** archivo, así que la "carta 5" de quien escribe puede ser
   tu carta 8: buscar por número da un falso negativo y te hace reprocesarla entera. Y por token y no
   por frase, por lo de siempre: los docs llevan ajuste de línea y una oración puede partirse en dos.
   **Si aparece, diffea antes de decidir.** Idéntica = ya procesada: di dónde está archivada y qué
   salió de ella, y para. **Distinta = es una revisión**, y entonces lo que importa es el **delta** —
   procesarla entera de nuevo es tan malo como ignorarla.
   **Y ya que estás en el archivo, comprueba el estado: si esta carta responde a una tuya, la tuya
   tiene que figurar como `entregada`.** **Y ese estado se lee de la COLUMNA, no se busca como palabra
   en la fila:** partir por `|`, tomar el desenlace y anclar al principio, porque la regla dice que el
   desenlace *empieza* por el estado. Una fila lleva prosa, y su prosa menciona estados de otras
   cartas — un `grep` de la palabra sobre la fila entera devuelve el estado equivocado sin avisar. Pasó
   en los dos sentidos en el mismo proyecto: una entrante contada como `redactada` porque su resumen
   decía *"llegó marcada como redactada"*, y después una saliente contada como entregada porque el suyo
   mencionaba *"la carta ya entregada"*. **El aviso de la primera estaba escrito dos líneas más arriba
   y no evitó la segunda, porque describía el caso y no el remedio.**
   Si dice `redactada`, uno de los dos registros está mal —
   resuélvelo antes de seguir. Los dos estados **crean** la contradicción, pero no la miran solas: la
   primera vez que ocurrió la vio el usuario preguntando, y la segunda pasó desapercibida **en el
   mismo material que se estaba archivando**. Una señal que nadie comprueba no es una señal.
1. **Clasificar** las afirmaciones del informe en las tres clases de arriba.
2. **Verificar** las de la primera clase contra tu trabajo, una a una.
3. **Separar diagnóstico de remedio** y volver a derivar el remedio. Decide su hogar con las fronteras
   de siempre: núcleo, módulo, instancia, o nada.
4. **Nombrar lo que el caso NO valida** y lo que el informe no dice. Un informe describe un terreno;
   lo que no cubre sigue sin cubrir, y darlo por probado es peor que no haberlo preguntado. Esta fase
   cuesta un párrafo y es la que más veces ha rendido.
5. **Aplicar** lo aceptado (con el checkpoint de siempre si toca lo interrumpible) y llevar cada
   decisión a su hogar **con su procedencia**: de dónde vino y qué caso la respalda.

   **Y aplicar no siempre es hoy: el día del hallazgo es el peor día para tocar el sitio donde vive.**
   Formulación de un corresponsal, con dos casos y uno de cada lado. Aquí: se midió que un ritual
   costaba 1,44 sesiones y se decidió **no** escribirle tres reglas más esa misma tarde. Allí:
   encontraron un agujero en un tipo del núcleo de su herramienta y **no lo cambiaron el mismo día**.
   Los dos aplazaron **con el diagnóstico escrito y publicado**, que es lo que separa esto de dejarlo
   para nunca.
   **La razón no es prudencia genérica: es que el hallazgo llega con su prisa puesta**, y el sitio
   donde vive suele ser el que más cuesta deshacer. Lo que **no** se aplaza es escribirlo — el
   diagnóstico viaja hoy aunque el remedio espere, y si no se escribe, esperar es perderlo.
6. **Archivar, responder y registrar**, en ese orden. La carta recibida se guarda como `letter` **si
   la contestaste o te movió a hacer algo** — la copia del remitente puede desaparecer y entonces la
   tuya es la única. La respuesta dice **qué entró, qué no y por qué**, y qué sigue sin poder
   responderse; es a su vez un `letter` que sale. Luego, las filas en `correspondence`.

### Responder es una fase, no cortesía

Es lo primero que se degrada, porque el trabajo ya está hecho y la respuesta no le urge a nadie. Pero
**un ritual que solo ingiere convierte a quien reporta en QA gratis**, y esa fuente se seca. Y hay algo
que solo tú puedes darle: **en qué no se aceptó su propuesta y por qué**. Eso es lo que hace que el
siguiente informe venga mejor calibrado — y es información que él no tiene forma de deducir.

**Y tiene un gemelo oscuro, que solo aparece cuando el intercambio ya lleva tiempo: responder de
MÁS.** Pasadas unas cuantas cartas, devolver un hallazgo deja de ser un aporte y se vuelve una
**obligación social** — el otro te dio algo, y no dar nada se siente como no haber trabajado. Ahí es
donde se **fabrica un hallazgo simétrico**: se busca en tu terreno el equivalente de lo que él
encontró en el suyo, y lo que sale es ruido con forma de reciprocidad. Es peor que el silencio,
porque viene avalado por todo lo que sí era bueno en la misma carta.

**"No aplica" es una respuesta completa, y decirlo vale más que inventar el espejo.** Le dice al otro
algo real —que su hallazgo es específico de su terreno, cosa que él no puede saber— y protege lo único
que hace útil el canal: que un hallazgo signifique algo. La frontera de REMITIR —*si no hay caso, no
hay carta*— es la misma regla; lo que cambia aquí es de dónde viene la presión: no de tener una idea
suelta, sino de **deberle algo a alguien**.

Lo formuló un corresponsal ejerciéndolo, que es la única forma en que esto se puede decir con
autoridad: escribió *"preferimos decir «no aplica» a inventar un hallazgo simétrico"* **en la misma
carta en la que podría haberlo inventado**.

Por eso **la fila se escribe después de responder**: su existencia implica que el circuito se cerró. Y
como la numeración de `letter` no distingue dirección, **una carta que entra sin una que salga detrás
es una conversación abierta**, visible sin llevar ninguna lista aparte. Una respuesta redactada y sin
enviar es un pendiente de `state` **que apunta aquí** — no una columna más, y tampoco el estado copiado
en dos sitios: el índice dice cuál y en qué estado, y el `state` solo dice que hay que mirarlo. Copiarlo
crea la lista aparte que la frase anterior acaba de declarar innecesaria, y **esa copia es la que se
queda vieja**: caso de campo, cuatro sesiones anunciando una carta que ya había sido contestada.

### Qué NO es un informe externo

Que la petición venga acompañada de un **caso**: algo que pasó en un terreno real. Una idea, una
preferencia o una petición de funcionalidad —vengan de quien vengan— no son esto: se tratan como
cualquier otro cambio, sin ritual. Sin esta frontera, CONTRASTAR se convierte en la puerta de entrada
de todo y deja de proteger nada.

