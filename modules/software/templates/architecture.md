# {{architecture}} — <área/producto>

> Mapa de módulos y **patrones reutilizables** de este producto (normalmente un codebase). Objetivo:
> que ningún agente tenga que releer el fuente para redescubrir un patrón ya entendido. Se **lee** al
> inicio de cualquier sesión que toque esta área y se **actualiza** al cerrar si se descubre un patrón
> nuevo. Uno por producto (monorepo: uno por paquete relevante).
>
> Frontera: aquí va *cómo está organizado el producto* (estructura + patrones). Las *trampas
> puntuales* van en `{{gotchas}}`; las *decisiones de producto* en `{{specs}}`.

## Stack
ADAPTAR: lenguaje, framework, librerías clave, cómo se construye/corre.

## Estructura de módulos
ADAPTAR: directorios/paquetes y su responsabilidad, en una línea cada uno.

## Patrones reutilizables ⭐
ADAPTAR: los patrones que se repiten y conviene conocer antes de escribir. Por patrón:
- **<Nombre del patrón>** — qué resuelve, dónde está la implementación de referencia
  (`ruta/archivo.ext`), y cómo usarlo. (p. ej. capa de acceso a datos, manejo de auth,
  componente de tabla estándar, proxy/BFF, manejo de errores.)

## Flujos clave
ADAPTAR: 1-3 flujos de punta a punta que un agente nuevo debe entender (p. ej. "petición →
proxy → API → DB", "evento en tiempo real", "build/deploy").
