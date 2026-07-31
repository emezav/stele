# {{charter}} — Decisiones y principios generales del proyecto

> **Por qué `<PROYECTO>` es como es**, a gran escala. Estable, se lee para orientarse y rara vez
> cambia. Frontera: aquí van *principios y apuestas grandes transversales*; las *specs y
> decisiones por feature* van en `{{specs}}`; los *patrones de código* en `{{architecture}}`;
> las *trampas de código* en `{{gotchas}}`. Tope objetivo ~200 líneas: si una decisión crece, se
> extrae a su tema de `{{specs}}` y aquí queda 1-3 líneas + link.

## Norte
ADAPTAR: una frase de qué logra el proyecto y para quién. El "éxito" en una línea.

## Principios de diseño (no negociables)
ADAPTAR: 4-8 tenets durables que guían decisiones. Ejemplos de forma:
- **<Principio>** — qué implica en la práctica y por qué. (p. ej. "Seguridad primero: validar
  toda entrada en el servidor, nunca confiar en el cliente".)
- **Un hogar por dato** — cada hecho vive en un solo documento; los demás apuntan.

## Restricciones y no-negociables
ADAPTAR: límites que acotan el espacio de solución: normativos/compliance, presupuesto,
"no usar `<X>`" (servicios de pago, dependencias vetadas), plataformas objetivo, rendimiento
mínimo, privacidad. Un agente debe poder descartar opciones leyendo esto.

## Decisiones estructurales (ADR-lite)
> Decisiones grandes y transversales. Formato corto y fechado; el detalle va a `{{specs}}`.

### <YYYY-MM-DD> — <Título de la decisión>
- **Contexto:** el problema en 1-2 líneas.
- **Decisión:** qué se eligió.
- **Alternativas descartadas:** X (por qué no), Y (por qué no).
- **Consecuencias / detalle:** link al tema de `{{specs}}` si aplica.

(Repetir por decisión. Las que dejan de ser relevantes se podan; su rastro queda en el historial.)

## Glosario
ADAPTAR: términos del dominio con una definición corta, para vocabulario compartido entre
agentes y humanos. Incluir los identificadores canónicos y qué NO confundir con qué.

## Mapa de documentación
Dónde vive cada tipo de información: ver `{{entry}}` → "Dónde vive cada cosa" (no duplicar aquí).
