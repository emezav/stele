# {{gotchas}} — Trampas y convenciones no evidentes al trabajar aquí

> **Hogar único** de lo que no es obvio **al trabajar en este proyecto**: trampas del entorno y de las
> herramientas, de publicar, de verificar, de dónde guardar las cosas, y las convenciones que un
> agente debe respetar y no puede deducir leyendo.
>
> **Es el complemento del historial, y por eso se lee en cada arranque y el historial no.** El
> historial es episódico, crece y es inmutable: guarda *qué pasó*. Esto guarda *qué no es obvio*. Una
> lección que solo vive en un acta está enterrada en un archivo que nadie vuelve a abrir.
>
> Se edita incrementalmente pero se **cura**: una entrada resuelta u obsoleta se **borra** (su rastro
> queda en el historial).
>
> **NO lleva** — y las dos últimas son las que de verdad se cuelan:
>
> - contexto de negocio o decisiones de producto → `{{specs}}`
> - principios y decisiones grandes → `{{charter}}`
> - proceso y reglas de sesión → `{{entry}}`
> - **el trabajo en curso, su alcance o lo que falta por entregar** → `{{handover}}`
> - **dónde estamos y el próximo paso** → `{{state}}`
>
> Para eso, apunta.
>
> **Por qué esas dos, y no es una advertencia teórica.** `{{handover}}` y `{{state}}` **se podan**;
> este archivo **se carga en cada sesión y nadie lo cura si no sabe que hay algo que curar**. Una
> tarea pendiente escrita aquí sobrevive a su sesión y sigue leyéndose como cierta. Caso de campo:
> un bootstrap anotó aquí *"antes de cerrar debe existir `hello.py`"*; el archivo se creó y se
> verificó ese mismo día, la entrada se quedó, y **la sesión siguiente se ofreció a rehacer el
> trabajo ya terminado**. Contenido efímero en el sitio más permanente es la peor combinación que
> admite este marco.

---

<!-- ARRANCA VACÍO Y ESO ESTÁ BIEN.

     Un proyecto nuevo NO TIENE TRAMPAS TODAVÍA: se descubren trabajando. Si al instanciar no tienes
     ninguna, **borra los ejemplos de abajo y deja el archivo con su cabecera y nada más**. Un
     `{{gotchas}}` vacío el primer día es correcto y significa exactamente lo que parece.

     No lo rellenes con lo que sí tienes a mano —el objetivo del proyecto, la tarea de hoy, lo que
     falta por entregar—: nada de eso es una trampa, y todo tiene su hogar en la lista de arriba.
     Se observó dos veces en campo, las dos porque el archivo pedía contenido y el agente lo dio.

     Las secciones de abajo son EJEMPLOS DE FORMA, no un esqueleto que haya que llenar. Añade una
     sección cuando tengas al menos una entrada real que meter en ella. -->

## Entorno y herramientas

- **`la herramienta`, `en esta máquina o este entorno`** — `qué hace distinto de lo esperado` →
  `cómo comprobarlo o qué usar en su lugar`.
- Un detector que devuelve **cero** merece su propia entrada si el cero puede ser falso: anota **con
  qué comando** se obtuvo y **con qué control** se comprobó que el comando funciona.

## Al publicar o hacer durable el trabajo

- **`el paso que se salta`** — `qué se pierde si se salta` → `qué hacer`.

## Convenciones no evidentes

- **`la regla`** — `qué pasa si no se respeta / por qué no se deduce leyendo`.

## Seguridad

- Reglas no negociables del proyecto: nunca escribir secretos en un doc, qué no sale de aquí, qué
  hace falta validar antes de confiar en un dato externo.

<!-- ARRANCA VACÍO Y ESO ESTÁ BIEN, y esta nota existe porque no estaba escrito.

     Un proyecto recién instalado **no tiene trampas**: se descubren trabajando. Un archivo con la
     cabecera y las secciones vacías el primer día es CORRECTO y significa exactamente lo que parece.
     Añade una sección cuando tengas al menos una entrada real que meter en ella — y bórrala cuando
     se quede sin ninguna.

     ASÍ SE VE UNA ENTRADA DE VERDAD, para que sepas cuándo tienes una y cuándo no:

       ## Entorno

       - **`npm test` pasa en local y falla en CI**, y la diferencia es la versión de Node: CI usa la
         del `.nvmrc` y local la del sistema. Comprobar con `node -v` antes de dar un verde por bueno.

     Lo que la hace entrada: **pasó**, tiene un coste, y no se deduce leyendo el repo. Una regla que
     te acabas de inventar no es una trampa; una que te mordió, sí.

     Redacción tomada de un agente de campo que resolvió esto solo, en un bootstrap real: la escribió
     bien sin que ninguna plantilla se lo dijera, que es justo como un hueco se queda abierto. -->
