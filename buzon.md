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
