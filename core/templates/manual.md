# {{manual}} — Qué es esto y qué se espera de ti

> Plantilla del rol `manual`. **Es el único doc del marco cuyo lector no es un agente**, y eso cambia
> todo lo demás: no se rellena, **se redacta**, con el proyecto delante y en sus palabras.
>
> **Lo que esta plantilla fija son las preguntas, no la redacción.** Cópiale la estructura y **no le
> copies el tono**: los ejemplos de abajo hablan de un proyecto que no es el tuyo. Si al terminar el
> texto suena al marco en vez de sonar al proyecto, está mal aunque esté completo.
>
> **Regla de oro: aquí no se nombra el marco.** Nada de `rol`, `token`, `handover`, `clase 7`,
> `manifiesto`, `instancia` ni nombres de ritual. Se habla de *empezar*, *terminar*, *lo que quedó a
> medias*, *el archivo donde está X*. La tabla de equivalencias está en `{{kit}}/SKILL.md` → *Cómo se
> le habla al usuario*, y aquí se aplica **entera**.
>
> **Cómo crece, y es lo que lo hace útil.** Este doc **no se escribe completo el primer día**: nace
> corto y **crece con las preguntas que la persona hace de verdad**. Cuando pregunte algo que este
> archivo no contesta, la respuesta se escribe aquí antes de seguir. Un manual redactado de
> imaginación contesta lo que creímos que iban a preguntar, y esa lista casi nunca es la real. La
> señal de que va bien no es su tamaño: es que las preguntas dejen de repetirse.
>
> **Presupuesto de hecho: si no se lee de una sentada, no se lee.** Cuando crezca de más, lo que sobra
> casi siempre es explicación del marco que se coló, no respuestas.
>
> (Feature `manual_doc`: apágalo si quien usa este proyecto es la misma persona que lo configuró y
> conoce el marco. Apagarlo no rompe nada — solo deja al proyecto sin puerta de entrada para alguien
> que llegue de fuera, o para el propio autor dentro de un año.)

---

## Qué estamos haciendo aquí

Una o dos frases, **del proyecto, no del marco**. Qué se está construyendo o escribiendo y para qué.
Sale de lo que ya está escrito en el doc de *por qué* del proyecto; no lo inventes ni lo adornes.

> Ejemplo (proyecto de tesis): *"Aquí se escribe el anteproyecto de investigación. Cada capítulo tiene
> su archivo, y hay un registro de qué se avanzó cada día para no perder el hilo entre semanas."*

## Cómo trabajamos

<!-- ESTA SECCIÓN ES LA EXCEPCIÓN DE LA PLANTILLA, y conviene saber por qué.

     Las demás fijan preguntas y tú redactas: "qué estamos haciendo aquí" TIENE que salir de tu
     proyecto. Pero **el ciclo de trabajo es idéntico en todos los proyectos que usan este marco**, y
     hacer que cada agente lo redacte de cero solo produce versiones distintas de lo mismo — algunas
     con jerga, que es justo lo que este documento no puede tener.

     Así que aquí abajo hay TEXTO, no instrucciones. Se copia y se rellenan los [corchetes].

     LOS CORCHETES SON HUECOS, NO EJEMPLOS: si dejas uno sin sustituir, se nota.
     Y no lo reescribas "para que suene mejor": lo que suena a este marco en esta sección es
     correcto, porque el mecanismo es de este marco. Lo que tiene que sonar a tu proyecto es la
     última frase, la de qué se hace aquí de verdad. -->

El trabajo va **por sesiones**. Una sesión es simplemente un rato de trabajo: te sientas, le dices al
[asistente/agente] qué quieres hacer, se hace, y al terminar **se lo dices**. Eso es todo lo que tiene
de ceremonia.

Lo que pasa al terminar es lo único que conviene entender, porque es lo que hace que esto sirva: se
**deja escrito qué se avanzó y qué sigue** (`[archivo de estado]`). La próxima vez —mañana o dentro de
un mes— lo lee antes de saludarte y retoma donde quedaste, **sin que tengas que explicarle nada otra
vez**.

No es burocracia y no es para llevarte la cuenta. Es la diferencia entre volver y decir *"seguimos con
lo de ayer"* y tener que reconstruir de memoria qué se estaba haciendo y por qué.

Si una sesión se corta a la mitad —se acaba el día, se cierra el portátil— **no se pierde**: queda
anotado dónde iba (`[archivo de trabajo a medias]`) y al volver se retoma desde ahí.

En este proyecto, una sesión típica es [qué se hace de verdad aquí: escribir una sección, revisar
resultados, corregir una parte del texto].

<!-- SI EL PROYECTO NO LLEVA COPIA DE SEGURIDAD, DILO AQUÍ Y NO MÁS ABAJO. Es lo que más le importa
     a alguien que no conoce la herramienta, y llega tarde en cualquier otro sitio. Un agente de campo
     lo puso por su cuenta y acertó:

       "Este proyecto no está bajo control de versiones. No hay historial de cambios ni forma de
        deshacer un borrado: los archivos en disco son todo el registro que existe. Si quieres red de
        seguridad, dilo y se monta — es un cambio pequeño y se hace en cualquier momento."

     Si el proyecto SÍ lleva control de versiones, esta nota no aplica y no se escribe. -->

## Qué se espera de ti

**Esta sección es la razón de que el doc exista, y la respuesta honesta suele ser: muy poco.** Dilo
así, con esas palabras si hace falta.

Lo que sí: decir cuándo empiezas y cuándo terminas, y contestar cuando el agente pregunte algo que
solo tú sabes. Lo que **no**:

- **no** hace falta leer los documentos internos del proyecto para trabajar;
- **no** hace falta recordar nada entre sesiones — para eso está el registro;
- **no** hay cuota, ni límite de sesiones, ni nada que se acumule en tu contra.

## Qué vas a ver, y qué significa

Los avisos que **este** proyecto emite, con su nombre en llano y —lo que más falta— **qué te pide cada
uno**, aunque la respuesta sea *"nada"*. Un aviso sin consecuencia escrita se lee como una advertencia.

| Vas a ver algo como… | Significa | Qué tienes que hacer |
| --- | --- | --- |
| "quedó trabajo a medias" | la sesión anterior se cortó y dejó anotado dónde iba | nada; el agente lo retoma solo |
| "van N sesiones desde la última revisión" | cada tanto conviene repasar que la documentación siga siendo cierta | nada; se pide cuando quieras, y cerrar sesiones siempre suma |

Añade una fila por cada aviso que la persona haya visto o vaya a ver, y **quítalas cuando dejen de
aparecer**. La tercera columna es obligatoria: es la que faltaba el día que un usuario leyó un
contador y entendió que no debía seguir trabajando.

## Si algo no está aquí

Pregúntale al agente. Y si la respuesta no estaba en este archivo, **el agente la escribe aquí antes
de seguir** — así la siguiente persona ya no tiene que preguntar.
