# {{audit}} — Auditorías de documentación (append-only, OPCIONAL)

> Una fila por auditoría (ritual AUDITAR, `{{kit}}/SKILL.md`). Este archivo **lo crea la primera
> auditoría**, no el bootstrap: su ausencia significa que el proyecto nunca se ha auditado.
> Al terminar:
> `printf '| N | YYYY-MM-DD | A-B | alcance | C (F falsas) | E/P | desenlace |\n' >> {{history_dir}}{{audit}}`
>
> `Sesiones` = rango cubierto (desde la última auditoría hasta la última cerrada). Es el dato que
> **acota el alcance de la próxima**: sin él, auditar es releerlo todo cada vez.
> `Comprobadas` = cuántas **afirmaciones sobre el mundo** se extrajeron y resolvieron, y cuántas
> resultaron falsas. Es el **denominador**: sin él, dos auditorías con los mismos hallazgos son
> indistinguibles aunque una comprobara cinco afirmaciones y la otra quinientas. Las reglas no cuentan
> (`{{kit}}/SKILL.md` → *separa la afirmación de la regla*). `—` si no se registró.
> `Hallazgos` = `errores/preferencias` — errores son contradicciones verificables; preferencias son
> juicios que decide el usuario. El detalle no se copia aquí: vive en el `{{session}}` de la sesión
> que auditó, y lo que perdura, en su hogar.
>
> (Feature `audit_log`: apagar el toggle si el proyecto no lleva la serie. Apagarlo deja al ritual
> sin memoria: cada auditoría vuelve a ser completa.)

| Audit | Fecha | Sesiones | Alcance | Comprobadas | Hallazgos | Desenlace |
| --- | --- | --- | --- | --- | --- | --- |
