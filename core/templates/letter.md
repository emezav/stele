# Carta {{NNN}} — {asunto en una línea}

> Plantilla del rol `letter`. Un archivo por carta en `{{correspondence_dir}}`, numerado sin
> distinguir dirección. **Inmutable**: una carta enviada no se reescribe; si cambias de opinión, se
> escribe otra. Se lee con `grep`, no de corrido.

**De:** {remitente — el de tu manifiesto, o `anónimo`}
**Para:** {destinatario}
**Fecha:** {YYYY-MM-DD}
**Dirección:** {sale | entra}
**Responde a:** {carta y pregunta concretas, o `—` si abre tema}

> **`Responde a` no es opcional.** Las cartas **se cruzan**: quien te escribe puede no tener tu última,
> y tú puedes contestar algo que ya cambió. Lo que hace que cruzarse no rompa nada es que cada carta
> **diga a qué contesta**, sin depender de que las dos partes compartan estado. Y como cada proyecto
> numera **su propio** archivo, un número suelto no basta al cruzar de archivo: nombra también el
> asunto o la pregunta. Con contacto en tiempo real esto no se nota; en cuanto haya días de latencia,
> es lo único que sostiene el hilo.

## El caso

Qué hiciste, qué pasó y qué costó, en tu terreno. **Es la carga útil de la carta**: lo único que quien
la recibe no puede conseguir por ningún otro medio. Concreto y con números si los hay — "24 de 940
candidatos" dice más que "muchos falsos positivos".

## Lo que afirmo sobre tu trabajo

Separado a propósito de lo de arriba. Son dos clases de afirmación que se comprueban distinto: **esto**
el receptor puede verificarlo contra su propio material; **lo de arriba** no, y lo tomará bajo palabra.
Separarlas tú le ahorra la primera fase entera y hace la carta más honesta.

## Qué NO demuestra este caso

Dónde termina tu evidencia. Un caso describe un terreno; lo que no cubre sigue sin cubrir. Este campo
existe porque **es el que más veces ha faltado**, y sin él el receptor tiene que deducirlo o —peor—
dar por probado lo que no lo está.

## Propuesta (opcional, y la parte menos valiosa)

Si tienes una idea de qué hacer, dila. Pero se sabe, y está medido, que **el diagnóstico viaja y el
remedio no**: quien recibe tiene el contexto de diseño que tú no ves, igual que tú tienes el caso que
él no ve. Así que **no hace falta traer solución para que la carta valga**. Si no la tienes, borra esta
sección y manda la carta igual.

## Qué va tachado

Si redactaste algo —rutas internas, nombres de servicio, datos de terceros—, dilo aquí. Cambia cómo se
lee: no es lo mismo *"no dan el dato"* que *"el dato va tachado"*. Lo primero invita a preguntar; lo
segundo dice que no.

<!-- ANTES DE ENVIAR (ritual REMITIR, `{{kit}}/SKILL.md`):
     1. Tachado: relee buscando rutas internas, nombres de máquinas y servicios, datos de personas.
        El seudónimo del remitente NO anonimiza el cuerpo.
     2. Consentimiento: enviar es publicar. Lo decide el usuario, nunca el agente por su cuenta.
     3. Archivo: guarda tu copia aquí. La copia del otro lado puede desaparecer (los buzones se
        curan), y entonces esta es la única.
     4. Índice: la fila en `{{correspondence}}`, después de enviar. -->
