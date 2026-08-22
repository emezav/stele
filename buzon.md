# buzon.md — Correspondencia de stele hacia quien usa el marco

> **Qué es.** El buzón de salida del kit. Como ACTUALIZAR se trae el árbol entero, **estas cartas
> bajan solas** con cada actualización: no hay servicio, ni red, ni cuenta que crear. Tu agente lo lee
> al actualizar (tabla de zonas de impacto, `core/rituals/actualizar.md`) y te dice si hay algo para ti.
>
> **Qué hacer con una carta.** Si te interesa, contéstala con el ritual **REMITIR**
> (`core/rituals/remitir.md`). Copiar y pegar basta; no hace falta saber git. Si la contestas o te mueve
> a hacer algo, **archiva tu copia**: este buzón se cura, y lo de aquí desaparece.
>
> **Dónde se escribe, y lo eliges tú.** La puerta abierta es **`github.com/emezav/stele` -> issues**:
> un issue con el texto pegado dentro, sin plantilla que rellenar ni formato que aprender. Es el mismo
> sitio de donde bajó el kit, y vale igual para contestar una carta y para escribir sin que preguntemos.
>
> **Pero el marco estandariza la carta y nunca el canal**, así que si prefieres otro —correo, un
> mensaje, pegarlo donde sea— vale igual. Copiar y pegar es el suelo: no hay servicio, no hay cuenta y
> no hay nada que aprender. **Lo que tarde en llegar es de las personas, no del marco.**
>
> **Y clonar y seguir por tu cuenta, sin decirnos nada, también es una respuesta válida.** El kit es
> tuyo desde que lo copias.
>
> **Y no hace falta que haya una carta esperándote** — que es el caso más útil y el que menos ocurre.
> Contestando solo llega lo que se nos ocurrió preguntar; lo que falta es **lo que te pasó y no
> habíamos previsto**. En concreto, y es lo que de verdad le ha cambiado reglas a este proyecto:
>
> - **Una trampa de tu máquina.** Un comando que aquí funciona y ahí no, o al revés. Casi todo lo que
>   este marco cree saber de entornos salió de que alguien midiera en el suyo.
> - **Una regla que te falló.** No que no te gustara: que la seguiste y aun así te mordió.
> - **Una cifra nuestra que no te cuadró.** Están publicadas con su corpus y su comando justo para que
>   se puedan volver a correr. Si te da otra cosa, eso es un hallazgo.
> - **Algo que el kit da por obvio y en tu terreno no lo era.**
>
> **Qué pasa con lo que llegue, dicho sin adornos.** Lo lee una persona, no un servicio, así que puede
> tardar. Si entra, entra por el ritual CONTRASTAR y **se registra qué se aceptó y qué se rechazó** —
> también cuando la respuesta es que no. Y si te refuta algo nuestro, mejor: las refutaciones que ha
> recibido este marco están registradas una a una, con lo que se corrigió a raíz de cada una.
>
> **Regla de curación (importante).** Este archivo es **carga pública y permanente**: viaja a cada
> copia del kit. Por eso se poda —una pregunta contestada se retira— y el rastro queda en `git log`.
> Mismo criterio que el resto del marco: lo histórico vive en la historia, no en el kit.
> **Y se retira también lo que el kit ya dice en el sitio donde se lee**, aunque nadie lo haya
> contestado: si una carta explicaba algo que ahora vive en `SKILL.md` o en una plantilla, mantenerla
> aquí es duplicar en carga pública lo que ya se lee donde toca. El criterio **no** es "ya se leyó" —
> eso no se puede saber— sino "ya tiene hogar".
>
> **Y hay un tercer caso, que es el que produjo la primera cosecha de verdad: la carta contestada a
> medias.** De cinco respuestas que llegaron juntas, **tres no decidían nada**: una la contestaron con
> *"nuestro caso no puede contestar eso"*, otra con un control negativo que quien lo mandó calificó de
> poco valioso, y una tercera confirmó lo que ya sabíamos. Vino campo y la pregunta **sigue abierta**.
> Esa carta **no se retira: se estrecha** — se le quita la mitad ya contestada y se deja escrito qué
> falta, para que nadie gaste campo dos veces en lo mismo. La tentación es contarla como contestada
> porque hubo respuesta, y **respuesta no es respuesta a la pregunta**.
>
> **Antes de retirar una, comprueba que su respuesta llegó a su hogar — con la línea delante.** Es la
> comprobación que de verdad decide, y es la que se salta: al retirar una carta se borra el único sitio
> donde constaba la pregunta, así que si la respuesta no aterrizó **no queda nada**. Aquí bloqueó una
> retirada: una carta prometía que la respuesta entraría *"marcada como testimonio"* en la tabla de
> BOOTSTRAP, y esa palabra daba **cero en todo el kit**. El hogar estaba nombrado y el mecanismo no
> existía.
>
> **Los números no se reutilizan ni se renumeran.** Una carta retirada deja su hueco: así una copia que
> alguien archivó como "Carta 2" sigue siendo identificable. Lo retirado vive en `git log`. El número es
> lo que se protege, no el texto: una carta estrechada conserva el suyo.
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

**Lo que no podemos comprobar.** Esa mitad de la regla sigue **sin probarse en campo**. Todos los
proyectos que nos han contestado confirman que el bloque se porta bien, pero **ninguno tenía nada
fuera de las marcas** que probara o refutara la conservación de lo externo: en el último, el archivo
existía meses antes del marco y las marcas se insertaron **alrededor** de todo su texto, así que fuera
quedó el título y nada más. Aquí tampoco podemos probarlo: este repo genera su propio loader.

> **Esta carta va estrechada.** Su segunda pregunta ya recibió un **"no"** de campo, y con un matiz que
> vale más que el no: aquel bloque divergió **antes** del bootstrap, porque el archivo era anterior al
> marco — no hubo nunca un "día de generarse". Lo que sigue faltando es lo contrario y es lo único que
> decide: **un sí**.

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

> **Esta carta va estrechada.** Ya llegó un control negativo —un proyecto con el módulo **activo**— y
> quien lo mandó lo calificó él mismo de poco valioso, con razón: el detector viejo acertó con ellos
> **por el motivo equivocado**, porque encontró el compilador y no la estructura. Un control que acierta
> por la razón mala es justo el que no habría destapado nada. **Sigue faltando quien sí sea el caso.**

**Por qué te lo pedimos a ti.** Este repo genera su propio manifiesto y siempre supo qué era, así que
mirarse a sí mismo no es evidencia de nada. De la respuesta depende si `pendiente` basta o si el marco
tiene que preguntar antes y con más insistencia.

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
