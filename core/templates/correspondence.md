# {{correspondence}} — Índice de correspondencia (append-only, OPCIONAL)

> Una fila por carta, **en las dos direcciones** (rituales CONTRASTAR y REMITIR,
> `{{kit}}/SKILL.md`). Este archivo **lo crea la primera carta**, no el bootstrap: su ausencia
> significa que este proyecto no ha intercambiado nada con fuera.
> Al terminar:
> `printf '| N | YYYY-MM-DD | dir | corresponsal | asunto | desenlace |\n' >> {{correspondence_dir}}{{correspondence}}`
>
> **Y si en algún momento dos corresponsales miden lo mismo y les sale distinto, este archivo gana una
> SECCIÓN —no una columna—** con su propia tabla: *qué se midió · su valor · el tuyo · causa*. La fila
> **nace** con el desacuerdo y se **cierra** cuando se sabe la causa; mientras la causa esté vacía está
> **abierta**, y esa es la que vale — es la única forma escrita de decir *aquí hay algo que ninguno de
> los dos entiende todavía*. **La regla que la hace útil: un desacuerdo sin causa no se resuelve
> escribiendo la causa más interesante.** Se deja abierto. Lo caro no es la fila vacía: es lo que uno
> escribe para no dejarla vacía.
>
> **Sección y no columna, por dos razones y la segunda es la de diseño:** las filas viejas no tienen
> desacuerdo que registrar, y sobre todo **un desacuerdo no es una carta** — vive *entre* dos, o entre
> una carta y una medición posterior, y a veces se cierra tres cartas después. **Y va ANTES de la tabla
> de cartas**, porque esa tabla se alimenta con `printf >>`: una sección al final rompería el append
> sin dar error, dejando la fila siguiente debajo de la sección y la tabla partida.
>
> **Una fila abierta no lleva fecha de caducidad: lleva CERRADOR.** Es decir, quién podría cerrarla. La
> diferencia importa porque una fila vieja sin cerrador se lee igual que una abandonada, y con él se
> lee bien en los dos casos: si el cerrador **existe y no ha contestado**, la fila está *esperando*; si
> el cerrador es **nadie** —porque ninguna de las dos partes puede producir el dato—, la fila no está
> rancia, **está completa**: *"ninguno de los dos puede cerrar esto"* es un resultado, no una tarea.
>
> **Y llevar quién la abrió y contra qué corpus NO es el remedio que hay que mantener**, porque se
> escribe **una vez, al nacer, y no se toca nunca**. Lo que se pudre es lo que hay que actualizar. La
> distinción, que sirve fuera de aquí: **un contador expira, una medición no.** *Abierta* es un
> contador de nada; *abierta desde tal día, contra tal corpus, la cierra tal parte* es una medición, y
> las mediciones no caducan — se quedan siendo verdad de su día.
>
> `Dir` = `->` sale · `<-` entra. Es lo que permite ver el intercambio como una conversación.
> `Corresponsal` = con quién. Si escribes tú, a quién; si recibes, el **remitente** que venga en la
> carta. Guardarlo no es burocracia: una fuente que ya acertó antes se pesa distinto que la primera
> carta de una desconocida, y ese dato solo existe si se anota.
> `Asunto` = una línea; es el mismo que encabeza la carta.
> `Desenlace` = qué salió de ahí. En una carta recibida, **qué se aceptó y qué se rechazó con su
> razón** — eso es lo que este índice guarda y ningún otro doc: un descarte razonado evita volver a
> discutirlo, y le dice a quien escribió cómo calibrar la próxima. El detalle de lo aceptado no se
> copia aquí: vive en el hogar que corrigió, con su procedencia.
>
> **En una carta que sale, el desenlace empieza por su estado: `redactada`, `publicada` o
> `entregada`.** El agente escribe la fila al redactar y mueve el estado a `publicada`; **solo el
> usuario mueve el estado a `entregada`**, porque el cartero es él: un agente no puede comprobar que
> una carta salió. Una fila `redactada` es una conversación que no ha salido, y verla es lo que evita
> que alguien tenga que preguntar si ya se contestó.
>
> **`publicada` solo aplica a las cartas que afirman cambios en algo que al destinatario le llega**, y
> significa que su identificador de publicación ya está **dentro** de la carta, comprobado contra el
> sitio donde el otro va a mirar. Una fila `entregada` que nunca pasó por `publicada`, en una carta de
> esas, es una deuda **visible aquí** en vez de un descubrimiento una carta después. Las cartas que no
> afirman cambios van de `redactada` a `entregada` directamente. **Las filas viejas no se reescriben**
> para añadirles el estado.
>
> **El acuse de recibo no necesita mecanismo.** La numeración es única para las dos direcciones, así
> que una carta que sale y **no tiene detrás una que entra** es una conversación abierta, y se ve de
> un vistazo. No hay columna de "¿contestaron?" que mantener.
>
> **Qué se archiva:** lo que contestaste o lo que te movió a hacer algo. Un aviso general que leíste y
> seguiste adelante, no. No es cuestión de frecuencia —eso no se puede decidir cuando llega la primera
> carta— sino de relevancia, igual que con los artefactos de una sesión.
>
> **Se archiva la carta, no el sobre.** Lo que rodea a una carta al llegar —*"esto viene de tal
> proyecto"*, *"esto lo dijo su agente"*, *"me lo pasaron por ahí"*— no forma parte de ella y **no se
> comprobó**: es una afirmación de la segunda clase (`{{kit}}/core/rituals/contrastar.md`). Si entra al
> archivo, entra **marcada como no verificada**; si no vale la pena marcarla, no entra. Lo que nunca
> puede pasar es que una atribución del sobre se lea, meses después, como si viniera firmada — y menos
> que alguien construya encima una conclusión de diseño. Ya ocurrió, y acabó publicado en una carta.
>
> (Feature `correspondence_log`: apagarlo si el proyecto no intercambia nada con fuera. Apagarlo deja
> a los dos rituales sin memoria: cada carta se evalúa como si fuera la primera, y lo ya rechazado
> vuelve.)

| # | Fecha | Dir | Corresponsal | Asunto | Desenlace |
| --- | --- | --- | --- | --- | --- |
