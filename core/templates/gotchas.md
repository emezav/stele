# {{gotchas}} — Gotchas técnicos no evidentes en el código

> **Hogar único** de las trampas y convenciones no evidentes al leer el código (las que un agente
> debe respetar al escribir). Lectura obligatoria al inicio de sesión, tras `{{entry}}`.
> NO lleva: contexto de negocio / decisiones de producto (→ `{{specs}}`), principios (→
> `{{charter}}`), proceso (→ `{{entry}}`). Para eso, apunta.
>
> Se edita incrementalmente pero se **cura**: una entrada resuelta u obsoleta se **borra** (su
> rastro queda en el historial). Si una sección de subsistema supera ~150-200 líneas, extraerla a
> un tema de `{{specs}}` con un resumen de 1-3 líneas + link aquí.

---

## Stack y versiones (gotchas críticos)
ADAPTAR: versiones que importan y las trampas asociadas (una API que cambió, una palabra
reservada nueva, un flag obligatorio). Ejemplo de forma:
- **<dependencia> v<X>** — <la trampa concreta> → <qué hacer en su lugar>.

## <Subsistema A> (p. ej. backend)
ADAPTAR: convenciones no obvias y errores fáciles de cometer. Cada entrada: el síntoma o la
regla, y el porqué en pocas palabras. Ejemplos de forma:
- **<Regla>** — <qué pasa si no se respeta / cómo hacerlo bien>.

## <Subsistema B> (p. ej. frontend)
ADAPTAR.

## Seguridad
ADAPTAR: reglas no negociables al escribir código (nunca exponer secretos, validar en servidor,
`.env` no se commitea, CORS acotado, …).
