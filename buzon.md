# buzon.md — Correspondencia de stele hacia quien usa el marco

> **Qué es.** El buzón de salida del kit. Como ACTUALIZAR se trae el árbol entero, **estas cartas
> bajan solas** con cada actualización: no hay servicio, ni red, ni cuenta que crear. Tu agente lo lee
> al actualizar (tabla de zonas de impacto, `SKILL.md`) y te dice si hay algo para ti.
>
> **Qué hacer con una carta.** Si te interesa, contéstala con el ritual **REMITIR** (`SKILL.md`) y
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

## Carta 1 — Aviso: dos archivos del kit cambiaron de nombre

**De:** stele · **Para:** todo el que ya tuviera el kit · **Fecha:** 2026-08-07

**El caso.** El kit pasó a la regla *"minúscula por defecto; MAYÚSCULA solo donde la impone algo
externo"*. Sobreviven en mayúscula `README.md`, `LICENSE`, `CLAUDE.md`, `AGENTS.md` y `SKILL.md` — este
último **no** por GitHub sino por el layout `skill`, donde Claude Code busca ese nombre exacto.

Cambiaron dos: **`GUIDE.md` -> `guide.md`** y **`BUZON.md` -> `buzon.md`**.

**Lo que te afecta, y no lo cubre la tabla de zonas.** Si algún doc **tuyo** enlaza al kit por el
nombre viejo, ahora tienes una referencia colgada. La tabla dice que `guide.md` "se lee, no se migra",
lo cual es cierto para su contenido y **no dice nada de tus enlaces hacia él**. Compruébalo:

```bash
# acotado a TUS docs: sustituye <base> por tu ruta base (`.`, `docs`, `bitacora`…)
grep -rn "GUIDE\.md\|BUZON\.md" <base> --include="*.md"
```

**Acota el comando a tu `base` en vez de barrer desde la raíz**, y no es un detalle: el kit vive en un
directorio **oculto**, y las herramientas no coinciden en qué hacer con eso — `ripgrep` lo salta por
defecto, `grep -r` de GNU entra. Barriendo desde la raíz con GNU te salen las referencias **internas
del kit**, que están perfectamente bien, y parecen enlaces rotos.

Lo que salga dentro de tu `base` es tuyo y hay que corregirlo a mano. Y si estás en Windows, ojo: el
sistema de archivos no distingue mayúsculas, así que el enlace viejo **te sigue funcionando ahí** y se
romperá en el primer clon en Linux. Arréglalo igual.

**Qué NO demuestra este caso.** No sabemos cuántos adoptantes enlazan al kit por nombre; puede que
ninguno. El aviso cuesta menos que el silencio.

## Carta 2 — ¿Tu loader tenía contenido propio antes de adoptar stele?

**De:** stele · **Para:** cualquiera que haya adoptado el marco · **Fecha:** 2026-08-07

**El caso.** El marco tiene una regla dura, el *invariante 6*: si el archivo de auto-arranque
(`CLAUDE.md`, `AGENTS.md`, el que uses) **ya existía**, se **modifica** y no se recrea — el bloque del
marco va entre las marcas `STELE:INICIO` / `STELE:FIN` y **todo lo que quede fuera se conserva
íntegro**. La regla nació de un fallo real: en el primer bootstrap fuera del repo original, el ritual
tomó un `CLAUDE.md` escrito a mano por el nombre de un derivado y lo sobrescribió entero. Se perdió el
contenido.

**Lo que no podemos comprobar.** La mitad de esa regla sigue **sin probarse en campo**. Un proyecto
que ya actualizó nos confirmó que el bloque se porta bien, pero **no tenía nada fuera de las marcas**,
así que su caso no dice nada sobre la conservación de lo externo. Aquí tampoco podemos probarlo: este
repo genera su propio loader.

**Lo que preguntamos, y son dos preguntas distintas.** Si tu proyecto tenía un `CLAUDE.md` o un
`AGENTS.md` **escrito a mano antes** de adoptar el marco:

1. ¿Quedó texto tuyo **FUERA** de las marcas `STELE:INICIO`/`STELE:FIN`? Si es así: ¿sigue ahí
   íntegro tras el bootstrap y las actualizaciones? Y si se perdió algo, ¿qué ritual lo tocó?
2. ¿Tu texto propio acabó **DENTRO** de las marcas — porque al insertar el bloque se rodeó lo que ya
   había en vez de añadirlo aparte?

**La segunda es la que más nos importa, y la aprendimos tarde.** Al preguntar solo por la primera, un
proyecto nos contestó *"fuera de las marcas no tengo nada"* y dimos por hecho que su caso no servía.
No era así: su contenido preexistía, solo que **encerrado dentro**. Y ahí el invariante 6 no protege
nada, porque no hay nada fuera que conservar — todo lo propio está en la zona que el marco se autoriza
a reescribir. Es la situación más peligrosa de las tres y desde fuera se ve igual que un loader
generado de cero.

Si estás en ese caso: **marca tu bloque como `STELE:INICIO RICO` ahora**, antes de la próxima
actualización. Ahí la marca no es una comodidad — es lo único que se interpone.

**Y hay una tercera pregunta, que esta carta excluía por error hasta que alguien la contestó igual.**
Decía aquí que un loader generado desde cero no nos servía, porque hacía falta un archivo que
**preexistiera**. Es falso, y lo demostró un adoptante el mismo día de adoptar: su loader lo generó el
bootstrap sin una línea escrita a mano, y **quedó `RICO` esa misma jornada**, porque el bloque generado
se apartó de la plantilla en reglas propias suyas. **La divergencia nació en el bootstrap.** Así que va
una pregunta más, y esta la puede contestar cualquiera que haya adoptado:

**Tercera pregunta.** ¿Tu bloque generado dice algo que la plantilla base **no** dice — porque tu
manifiesto, tu idioma o tus convenciones lo instanciaron distinto? Si sí, **es un bloque `RICO` aunque
nadie haya escrito nada a mano**, y la próxima regeneración se lo lleva.

**Lo que aprendimos con eso**, y vale más que la respuesta: el eje no es el **origen** del contenido
sino su **divergencia respecto de la plantilla**. La marca siempre lo dijo así; era esta carta la que
lo traducía a una historia sobre archivos anteriores a la adopción, y contada así, **quien generó de
cero se daba por excluido y no volvía a mirar**.

**Por qué te lo pedimos a ti.** Es una pregunta que el marco no puede contestarse solo. Y de esa
respuesta depende una regla que hoy protege datos que no podemos ver.

## Carta 3 — En AUDITAR, el falso positivo es el lado peligroso (y queremos saber si te pasa igual)

**De:** stele · **Para:** quien corra AUDITAR · **Fecha:** 2026-08-07

**El caso.** Los detectores del ritual se equivocan de dos maneras, y durante mucho tiempo el kit las
trató como si fueran igual de malas. No lo son.

- Un **falso negativo** deja un hallazgo sin encontrar. Malo, pero el documento **queda como estaba**,
  y lo que se te escapa vuelve en la siguiente auditoría.
- Un **falso positivo** trae un "arreglo", y el arreglo **corrompe algo que ya era correcto**.

Lo escribimos porque nos pasó tres veces, con tres detectores distintos y el mismo desenlace:

- Comprobar un candidato **recortado** por el barrido (una ruta que perdió su prefijo, o que se llevó
  el punto final de la frase) devuelve *"no existe"* — y el arreglo **corrige una ruta que estaba
  bien**.
- Comprobar una **mención** como si fuera un uso —el valor viejo que aparece dentro de *"decía X, la
  real es Y"*— devuelve *"no existe"*, **lo cual es cierto**, y el arreglo edita un documento sano.
- Declarar **huérfano** un dato que sí tiene hogar, escrito con otras palabras, lleva a **duplicarlo**
  en un segundo hogar: exactamente lo que la clase 7 existe para impedir.

**Lo que cambió en el kit.** Ahora el ritual dice hacia qué lado equivocarse: **ante la duda, no
declares**. Y añade el paso que faltaba antes de dar algo por huérfano — **buscar el concepto, no la
formulación**, porque un hogar legítimo rara vez repite el vocabulario del hallazgo. Eso no sustituye
al barrido: lo verifica.

**Qué NO demuestra este caso.** Son **tres casos de dos proyectos** que comparten este marco y esta
familia de detectores. Que la asimetría sea una propiedad de auditar documentación en general, y no de
cómo están escritos *estos* detectores, **no lo sabemos**.

**Lo que te pedimos, y es barato.** Si corres AUDITAR y te sale un falso positivo, mira una cosa más
antes de seguir: **¿qué habría pasado si lo aplicas?** ¿Habrías corrompido algo que estaba bien, o solo
perdido el rato? Con dos o tres respuestas de fuera sabremos si la regla es del marco o del mundo. Y si
te sale al revés —un falso positivo inofensivo— **eso nos interesa más todavía**, porque es lo único
que puede refutarla.
