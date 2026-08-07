# {{correspondence}} — Índice de correspondencia (append-only, OPCIONAL)

> Una fila por carta, **en las dos direcciones** (rituales CONTRASTAR y REMITIR,
> `{{kit}}/SKILL.md`). Este archivo **lo crea la primera carta**, no el bootstrap: su ausencia
> significa que este proyecto no ha intercambiado nada con fuera.
> Al terminar:
> `printf '| N | YYYY-MM-DD | dir | corresponsal | asunto | desenlace |\n' >> {{correspondence_dir}}{{correspondence}}`
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
> **En una carta que sale, el desenlace empieza por su estado: `redactada` o `entregada`.** El agente
> escribe la fila al redactar y **solo el usuario mueve el estado a entregada**, porque el cartero es
> él: un agente no puede comprobar que una carta salió. Una fila `redactada` es una conversación que
> no ha salido, y verla es lo que evita que alguien tenga que preguntar si ya se contestó.
>
> **El acuse de recibo no necesita mecanismo.** La numeración es única para las dos direcciones, así
> que una carta que sale y **no tiene detrás una que entra** es una conversación abierta, y se ve de
> un vistazo. No hay columna de "¿contestaron?" que mantener.
>
> **Qué se archiva:** lo que contestaste o lo que te movió a hacer algo. Un aviso general que leíste y
> seguiste adelante, no. No es cuestión de frecuencia —eso no se puede decidir cuando llega la primera
> carta— sino de relevancia, igual que con los artefactos de una sesión.
>
> (Feature `correspondence_log`: apagarlo si el proyecto no intercambia nada con fuera. Apagarlo deja
> a los dos rituales sin memoria: cada carta se evalúa como si fuera la primera, y lo ya rechazado
> vuelve.)

| # | Fecha | Dir | Corresponsal | Asunto | Desenlace |
| --- | --- | --- | --- | --- | --- |
