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

## Carta 2 — Qué se rechazó de los reportes de campo, y por qué

**De:** stele · **Para:** quien vaya a reportar algo · **Fecha:** 2026-08-07

Las **reglas** que salieron de los reportes ya están en el kit; lo que el kit no lleva son los
**descartes**, y son lo que más viaje ahorra. Tres, sin nombrar a nadie:

- **No se creó un módulo nuevo para los detectores de red.** La propuesta era razonable —puertos y
  servicios no son "software" en general, sino proyectos que operan servicios— pero aquí **un módulo
  es un paquete de roles**, y ese aporte no traía ninguno. Va condicionado dentro de `software`.
- **No se suben los presupuestos porque alguien los exceda.** Un tope que sube cada vez se convierte
  en decoración. Se decide: se poda con criterio, o se sube con `config` **después** de podar. Y un
  presupuesto no distingue *"el proyecto creció"* de *"el doc derivó de género"* — la pregunta *"¿qué
  sobra?"* es lo único que las separa.
- **No se finge privacidad en un buzón.** El kit se copia entero, así que no hay destinatarios ni
  entrega selectiva: un archivo con el nombre de alguien sería *algo que parece protegido sin
  estarlo*. Lo privado va por otro canal, que es el modo por defecto de todos modos.

**Y la regla que gobierna esto:** el **diagnóstico viaja, el remedio no**. Cuatro de cuatro reportes
llegaron con el diagnóstico correcto y el remedio equivocado o incompleto — no por torpeza, sino
porque quien reporta tiene el **caso** y el proyecto tiene el **contexto de diseño**. Escribe el caso;
la propuesta es la parte menos valiosa de tu carta y no hace falta traerla.

## Carta 3 — ¿Tu loader tenía contenido propio antes de adoptar stele?

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

**Qué NO nos sirve.** Si tu loader lo generó el bootstrap desde cero y nunca escribiste nada a mano,
tu caso no responde a ninguna de las dos. Hace falta un archivo que **preexistía**.

**Por qué te lo pedimos a ti.** Es una pregunta que el marco no puede contestarse solo: solo la
responde alguien que llegó con documentación propia. Y de esa respuesta depende una regla que hoy
protege datos que no podemos ver.
