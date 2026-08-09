# buzon.md — Correspondencia de stele hacia quien usa el marco

> **Qué es.** El buzón de salida del kit. Como ACTUALIZAR se trae el árbol entero, **estas cartas
> bajan solas** con cada actualización: no hay servicio, ni red, ni cuenta que crear. Tu agente lo lee
> al actualizar (tabla de zonas de impacto, `SKILL.md`) y te dice si hay algo para ti.
>
> **Qué hacer con una carta.** Si te interesa, contéstala con el ritual **REMITIR** (`core/rituals/remitir.md`) y
> mándala por donde quieras — pegarla en la sesión de tu agente, un correo, un issue. Copiar y pegar
> basta; no hace falta saber git ni tener cuenta en ninguna parte. Si la contestas o te mueve a hacer
> algo, **archiva tu copia**: este buzón se cura, y lo de aquí desaparece.
>
> **Regla de curación (importante).** Este archivo es **carga pública y permanente**: viaja a cada
> copia del kit. Por eso se poda —una pregunta contestada se retira— y el rastro queda en `git log`.
> Mismo criterio que el resto del marco: lo histórico vive en la historia, no en el kit.
> **Y se retira también lo que el kit ya dice en el sitio donde se lee**, aunque nadie lo haya
> contestado: si una carta explicaba algo que ahora vive en `SKILL.md` o en una plantilla, mantenerla
> aquí es duplicar en carga pública lo que ya se lee donde toca. El criterio **no** es "ya se leyó" —
> eso no se puede saber— sino "ya tiene hogar".
>
> **Los números no se reutilizan ni se renumeran.** Una carta retirada deja su hueco: así una copia que
> alguien archivó como "Carta 2" sigue siendo identificable. Lo retirado vive en `git log`.
>
> **Y una carta de este buzón se escribe como pregunta, no como anuncio.** Una regla nueva **llega
> sola** con la actualización, así que anunciarla aquí no añade nada. Lo que este canal consigue y el
> kit no es **campo**: lo que no sabemos, lo que solo puede contestar alguien que use el marco en un
> terreno que no vemos. Si al escribir una carta no encuentras qué preguntar, probablemente no era una
> carta.
>
> **Sobre nombres.** Aquí no se nombra a nadie sin su permiso: un proyecto aparece por el `remitente`
> con el que firmó, o en anónimo. Y las cartas hablan de **ideas**, nunca de quien las tuvo.
>
> **El tachado rige también hacia aquí.** Lo que se publica en este buzón suele nacer de un informe que
> llegó **en privado**, y esos informes vienen llenos de tripas de quien los mandó: rutas internas,
> nombres de máquinas y servicios, datos de terceros. Contestar en público sin releer con esa lente
> **filtra lo que se contó en confianza**, y el seudónimo no protege de eso. Se tacha antes de bajar,
> igual que antes de subir.

---

## Carta 2 — ¿Quedó texto tuyo FUERA de las marcas del loader?

**De:** stele · **Para:** cualquiera que haya adoptado el marco · **Fecha:** 2026-08-07

**El caso.** El *invariante 6*: si el archivo de auto-arranque (`CLAUDE.md`, `AGENTS.md`, el que uses)
**ya existía**, se **modifica** y no se recrea — el bloque del marco va entre las marcas
`STELE:INICIO` / `STELE:FIN` y **todo lo que quede fuera se conserva íntegro**. La regla nació de un
fallo real: en el primer bootstrap fuera del repo original, el ritual tomó un `CLAUDE.md` escrito a
mano por el nombre de un derivado y lo sobrescribió entero. Se perdió el contenido.

**Lo que no podemos comprobar.** Esa mitad de la regla sigue **sin probarse en campo**. Un proyecto
que ya actualizó nos confirmó que el bloque se porta bien, pero **no tenía nada fuera de las marcas**,
así que su caso no dice nada sobre la conservación de lo externo. Aquí tampoco podemos probarlo: este
repo genera su propio loader.

**Primera pregunta.** Si tu `CLAUDE.md` o tu `AGENTS.md` estaba escrito a mano **antes** de adoptar el
marco: ¿quedó texto tuyo **FUERA** de las marcas? ¿Sigue ahí íntegro tras el bootstrap y las
actualizaciones? Y si se perdió algo, ¿qué ritual lo tocó?

**Segunda pregunta, y es la inversa de lo que solíamos preguntar.** ¿Tu bloque generado **sí** es la
plantilla con los tokens resueltos y nada más? Compáralo con `core/templates/autostart.md` del kit que
tengas. Preguntamos esto porque **no hemos visto nunca un bloque limpio**: todos los observados
divergieron el día mismo de generarse, por el manifiesto, el idioma o las convenciones del proyecto.
Por eso el bloque ahora está **protegido por default** y la reescritura entera hay que declararla
(`LIMPIO`). Un solo sí documentado justifica que esa marca exista; **si nadie lo tiene, la marca sobra
y hay que quitarla**, porque una opción que nunca es correcta solo sirve para que alguien la elija por
error.

**Por qué te lo pedimos a ti.** Este repo genera su propio loader, así que mirarse a sí mismo no es
evidencia de nada.

## Carta 3 — En AUDITAR, ¿tu falso positivo habría corrompido algo?

**De:** stele · **Para:** quien corra AUDITAR · **Fecha:** 2026-08-07

**La afirmación que queremos poner a prueba.** Los dos errores de un detector **no son igual de
malos**. Un falso negativo deja un hallazgo sin encontrar, pero el documento **queda como estaba** y
lo que se escapa vuelve en la siguiente auditoría. Un falso positivo trae un "arreglo", y el arreglo
**corrompe algo que ya era correcto**.

**Qué NO demuestra nuestro caso.** Son **tres casos de dos proyectos** que comparten este marco y esta
familia de detectores. Que la asimetría sea una propiedad de auditar documentación en general, y no de
cómo están escritos *estos* detectores, **no lo sabemos**.

**Lo que te pedimos, y es barato.** Si corres AUDITAR y te sale un falso positivo, mira una cosa más
antes de seguir: **¿qué habría pasado si lo aplicas?** ¿Habrías corrompido algo que estaba bien, o solo
perdido el rato? Con dos o tres respuestas de fuera sabremos si la regla es del marco o del mundo. Y si
te sale al revés —un falso positivo inofensivo— **eso nos interesa más todavía**, porque es lo único
que puede refutarla.

## Carta 4 — ¿Tienes producto con estructura y el módulo apagado?

**De:** stele · **Para:** cualquier proyecto que haya bootstrapeado · **Fecha:** 2026-08-09

**Primero lo que no tienes que hacer: nada.** `módulos = software` en tu manifiesto **sigue siendo
válido para siempre**. El módulo ahora se llama `producto`, y el nombre viejo es un alias permanente.
Si no vuelves a leer esta carta no se te rompe nada — lo decimos primero porque un aviso cuya omisión
rompe algo no debería existir: eso se arregla con un alias, no con un aviso.

**Por qué preguntamos.** El módulo declaraba, por escrito y desde el principio, que su criterio de
activación era *"que el proyecto tenga un producto con estructura y decisiones por feature — **no que
haya un compilador**"*. Y el bootstrap lo activaba olfateando `Cargo.toml`/`package.json`/`src/`. **El
detector buscaba exactamente lo que el criterio decía que no importaba**, y cada mitad, leída por
separado, se veía bien. Dos proyectos reales pagaron la diferencia: una tesis con su corpus organizado
en carpetas, y una instancia recién bootstrapeada en una carpeta vacía — esta última por construcción,
porque en greenfield el olfateo corre en el único momento en que la evidencia no puede existir.

**La pregunta.** Mira tu manifiesto y tu árbol: **¿tu proyecto tiene un producto con estructura —un
codebase, un corpus organizado, una colección con temas— y el módulo está apagado?** Si es que sí, el
detector viejo te dejó fuera.

Y nos interesa sobre todo la segunda mitad: **¿qué pasó con la estructura que sí creaste?** ¿Acabó
escrita en algún doc, o vive solo en el sistema de archivos? En el caso que conocemos vivía solo ahí, y
nadie lo había notado **porque desde dentro no se ve como una falta**.

**Por qué te lo pedimos a ti.** Este repo genera su propio manifiesto y siempre supo qué era, así que
mirarse a sí mismo no es evidencia de nada. De la respuesta depende si `pendiente` basta o si el marco
tiene que preguntar antes y con más insistencia.

## Carta 5 — ¿Qué archivo lee tu agente al abrir?

**De:** stele · **Para:** cualquiera que haya adoptado el marco · **Fecha:** 2026-08-09

**La pregunta, y son diez segundos:**

> **¿Cómo se llama el archivo que tu agente carga solo al abrir el proyecto?** (`CLAUDE.md`,
> `AGENTS.md`, otro.) Y una segunda, que es la que da la prueba: **al reabrir la carpeta, ¿te saludó
> con la última sesión y el próximo paso, sin que se lo pidieras?**

**Por qué preguntamos.** El marco escribe un archivo de auto-arranque en la raíz, y **su nombre no lo
elige el marco: lo impone el harness que abre la sesión**. El kit lleva una tabla con los que conoce,
y esa tabla es conocimiento del mundo exterior que **no podemos verificar, que envejece y que siempre
estará incompleta**. Cada agente nuevo que aparece la deja corta.

Delegarlo en el agente que instala tampoco basta, y esto lo sabemos por campo, no por conjetura: en
cuatro instalaciones observadas con el mismo agente, **acertó su propio nombre dos veces de tres**. La
vez que falló, el marco quedó **instalado perfecto y apagado** — documentación impecable, y al reabrir
la carpeta el agente saludó en genérico, sin sesión, sin estado, y se ofreció a rehacer trabajo que ya
estaba terminado.

**Y por eso preguntamos por el nombre y no por el fallo.** Si preguntáramos *"¿te falló el arranque?"*
no contestaría nadie: quien acertó no lo notó, y **quien falló tampoco**, porque no hay error, no
falta ningún archivo y nada se rompe. Es un fallo mudo. El nombre, en cambio, lo puede contestar
cualquiera sin diagnosticar nada.

**La segunda pregunta es la que convierte tu respuesta en dato.** Un nombre dicho es algo que no
podemos comprobar desde aquí, y una entrada equivocada sería **peor que ninguna**: haría que el kit
escribiera con confianza una puerta que nadie lee. Con el saludo al lado, la respuesta trae su propia
prueba — y queda **falsable**: si alguien dice *"mi agente lee X"* y una corrida con ese agente no
saluda, la entrada se cae. Hasta que una corrida lo confirme, entra **marcada como testimonio**.

**Dónde acaba tu respuesta.** No aquí: este buzón se poda. Va a la tabla del ritual BOOTSTRAP, que es
lo que se consulta al instalar y no desaparece. Misma regla que el resto del marco: *un aviso cuya
omisión rompe algo no vive en un archivo que se borra.*

**Y lo que no te vamos a ocultar: a ti no te sirve.** El buzón baja con ACTUALIZAR, o sea después de
instalar — tu bootstrap ya ocurrió, bien o mal. Lo que tu respuesta ensancha es el piso **del
siguiente**.

Que es, si lo piensas, exactamente para lo que existe este marco. Una *estela* es una piedra
**inscrita**: alguien se detuvo a dejar dicho lo que sabía para quien viniera después. Entre sesiones
lo hacemos con el historial. **Entre proyectos solo se puede hacer así** — a quien le falla, puede
decidir dejar la huella para que al siguiente no le pase.

---

## Carta 6 — ¿En qué idioma están tus detectores de auditoría, y los tradujiste o los reescribiste?

**De:** stele · **Para:** cualquiera que use el marco en un idioma que no sea el del kit · **Fecha:** 2026-08-09

**La pregunta, y son dos:**

> **¿Los detectores de AUDITAR de tu proyecto están en el idioma del kit o en el tuyo?** Y si están en
> el tuyo: **¿los tradujiste término a término, o los volviste a escribir desde lo que el comentario
> dice que buscan?**

**Por qué preguntamos: porque aquí no lo podemos probar.** El kit dice que los detectores léxicos y
gramaticales **se derivan, no se traducen** — que lo que viaja es el comentario que encabeza cada
comando, y que la expresión se escribe desde cero. Lo escribimos con un razonamiento, no con un caso:
**este proyecto está en el idioma del kit, así que aquí no hay nada que derivar.** Es un mecanismo que
sale sin haberse corrido nunca, y lo decimos porque no queremos que se lea como algo probado.

**Lo que de verdad no sabemos es si la traducción llega a ser posible.** Uno de nuestros detectores
busca contadores en prosa, y su forma es `(los|las) (dos|tres…)`: artículo, género y número. En inglés
el equivalente literal, `(the) (two|three…)`, **no discrimina nada** — no es el mismo detector
traducido, es otro, con otra tasa de falsos positivos. No sabemos qué forma toma esa idea en tu idioma,
ni si toma alguna.

**Y una tercera, si llegaste hasta aquí:** el kit pide que cada detector derivado se guarde **con su
control positivo** —una línea de ejemplo que tiene que dar match—. ¿Te sirvió, o fue ceremonia? Lo
pusimos porque llevamos seis detectores rotos que devolvían cero, pero seis son los nuestros.

**Dónde acaba tu respuesta.** No aquí: este buzón se poda. Va a `core/rituals/auditar.md`, que es lo
que se lee al auditar. Y si resulta que derivar no es viable en algún idioma, eso también es un dato —
preferimos una regla con una excepción escrita que una regla que solo funciona en español.

**Lo que no te vamos a ocultar: si trabajas en el idioma del kit, esta carta no te toca.** No hay nada
que derivar y los detectores te sirven tal cual.

---

## Carta 7 — Abre tu registro de sesión más reciente: ¿dice en qué estado está algo?

**De:** stele · **Para:** cualquiera que haya adoptado el marco · **Fecha:** 2026-08-09

**La pregunta, y es de mirar un archivo:**

> En tu registro de la última sesión —el acta, el `session`—, **¿hay alguna frase que diga en qué
> estado está algo que vive en otro documento?** *"La carta N sigue sin contestar"*, *"el despliegue
> está pendiente"*, *"X quedó bloqueado"*. Y si la hay: **¿sigue siendo cierta hoy?**

**Por qué preguntamos.** Un registro de sesión **no se reescribe nunca** — es historial, y el historial
es inmutable. Así que un estado escrito ahí queda congelado en el instante de cerrar y **a partir de
entonces se lee como actual siendo un fósil**. No da error, no falta nada, y quien lo lea dentro de
tres meses no tiene forma de saber que caducó.

Nos pasó, y por eso preguntamos: un acta nuestra decía que cierta carta estaba *"publicada y sin
entregar"*. Se entregó unas horas después. **El acta no se corrige** —es historial— así que esa frase
va a seguir ahí, en pasado perfecto y en presente falso, para siempre.

**Lo que no sabemos es cuál es tu lista.** Nosotros escribimos un aviso en la plantilla, pero un aviso
solo cubre lo que se nos ocurrió imaginar. **Qué estados escribe la gente de verdad en sus actas es un
dato de campo**, y es el que decide si el aviso sirve o si nombra tres casos y deja fuera los siete que
importan.

**Y aquí va lo incómodo, que es la razón de que esta carta exista.** Ese aviso está en la plantilla del
rol `session`, y **las plantillas de rol no regeneran los documentos que ya existen**: tu acta es tuya.
O sea que **el arreglo no te llega**, ni actualizando. Lo único que cruza es esta pregunta — y lo que
consigue no es arreglarte nada, sino que mires. Si al mirar encuentras uno, ya sabes más que el aviso.

**Dónde acaba tu respuesta.** En la plantilla de `session` y en la tabla de zonas de ACTUALIZAR, que es
donde se decide qué te llega y qué no. Nada de esto se queda aquí: el buzón se poda.
