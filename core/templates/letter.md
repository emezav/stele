# Carta {{NNN}} — {asunto en una línea}

> Plantilla del rol `letter`. Un archivo por carta en `{{correspondence_dir}}`, numerado sin
> distinguir dirección. Se lee con `grep`, no de corrido.
>
> **La inmutabilidad empieza en la entrega, no en la escritura.** Una carta **entregada** o
> **recibida** no se reescribe jamás; si cambias de opinión, se escribe otra. Un borrador que aún no
> ha salido **sí** se revisa — y si mientras espera pasa algo que el destinatario debería saber, entra
> aquí en vez de en la siguiente.

**De:** {remitente — el de tu manifiesto, o `anónimo`}
**Para:** {destinatario}
**Fecha:** {YYYY-MM-DD}
**Dirección:** {sale | entra}
**Responde a:** {carta y pregunta concretas, o `—` si abre tema}

> **Aquí NO va el estado (`redactada`/`entregada`).** Vive en la fila de `{{correspondence}}` y en
> ningún otro sitio. El estado cambia **después** de entregar y lo entregado no se reescribe, así que
> un estado escrito aquí queda congelado en el instante de redactar y a partir de ahí se lee como
> actual siendo un fósil. Caso de campo: doce cartas entregadas diciendo `redactada` en su cabecera,
> con el índice dándolas por entregadas — el campo no lo pedía esta plantilla, lo añadió quien escribió
> la primera y lo copiaron las once siguientes.
>
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

**Y hay una tercera, que aparece sobre todo al responder: lo que afirmas sobre tu propio trabajo.**
*"Entró tu hallazgo"*, *"está escrito así"*, *"lo cambiamos"*. No es el caso —tuyo y no verificable por
él— ni una afirmación sobre su material: es una **acción tuya**, de la clase que hay que comprobar antes
de escribirla, y que él **sí puede contrastar si tiene acceso a lo que tocaste**. Si el artefacto le
llega —un kit que se distribuye, un repo que puede leer—, **dile dónde mirar y cuándo obtenerlo**: la
afirmación deja de valer lo que valga tu palabra, y lo que no cuadre es un hallazgo para él. Si no le
llega, **dilo**: queda bajo palabra, igual que el caso.

## Qué NO demuestra este caso

Dónde termina tu evidencia. Un caso describe un terreno; lo que no cubre sigue sin cubrir. Este campo
existe porque **es el que más veces ha faltado**, y sin él el receptor tiene que deducirlo o —peor—
dar por probado lo que no lo está.

**Si el caso es sobre comportamiento de un agente, di en qué entorno ocurrió** — herramienta, versión,
superficie, el modelo si lo sabes, y **los ajustes que hayas cambiado**. *"El agente hizo X"* no es
reproducible ni comparable sin eso: dos proyectos pueden diferir en cosas que ninguno de los dos ve, y
entonces las observaciones no se comparan entre sí sino entre dos montajes distintos. Sin el entorno,
una observación de comportamiento es una anécdota bien contada.

**Los ajustes son la parte que más se olvida, y la que más engaña.** Herramienta y versión se
recuerdan porque se nombran a diario; un interruptor que alguien encendió una vez se vuelve invisible
en cuanto deja de sorprender. Di **cuál es y de qué alcance** —global o de este proyecto—, porque un
ajuste de proyecto suele vivir en un archivo **no versionado**: entonces *"el mismo proyecto"* tampoco
es una variable controlada, y dos máquinas del mismo repo se comportan distinto sin que nada en el
repo lo explique. Caso de campo: dos proyectos llevaban tres cartas atribuyendo una diferencia de
comportamiento al sistema operativo; era un interruptor de proyecto, sin versionar, que uno tenía
encendido y el otro ni sabía que existía.

**Y hay un remedio más barato que ver lo que hace un agente: que el agente escriba lo que observa
donde queda.** Un caso real: una observación valiosa sobre el marco viajó porque su autor la dejó
**commiteada en un archivo** de su proyecto, no porque nadie le leyera el razonamiento. Lo que hace
comunicable una observación no es la transparencia del proceso, es que **el resultado aterrice en algo
durable** — que es, exactamente, el disparador de REMITIR.

## Propuesta (opcional, y la parte menos valiosa)

Si tienes una idea de qué hacer, dila. Pero se sabe, y está medido, que **el diagnóstico viaja y el
remedio no**: quien recibe tiene el contexto de diseño que tú no ves, igual que tú tienes el caso que
él no ve. Así que **no hace falta traer solución para que la carta valga**. Si no la tienes, borra esta
sección y manda la carta igual.

## Qué va tachado

Si redactaste algo —rutas internas, nombres de servicio, datos de terceros—, dilo aquí. Cambia cómo se
lee: no es lo mismo *"no dan el dato"* que *"el dato va tachado"*. Lo primero invita a preguntar; lo
segundo dice que no.

<!-- ANTES DE ENVIAR (ritual REMITIR, `{{kit}}/core/rituals/remitir.md`):
     1. Tachado: relee buscando rutas internas, nombres de máquinas y servicios, datos de personas.
        El seudónimo del remitente NO anonimiza el cuerpo.
     2. Consentimiento: enviar es publicar. Lo decide el usuario, nunca el agente por su cuenta.
     3. Sincronía: si la carta afirma cambios en algo que al destinatario le llega, dile CUÁNDO
        obtenerlo (antes de leerla, o mientras). El orden es parte de la carta, no logística.
     4. Archivo: guarda tu copia aquí. La copia del otro lado puede desaparecer (los buzones se
        curan), y entonces esta es la única.
     5. Índice: la fila en `{{correspondence}}`, después de enviar. -->
