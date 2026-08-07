# modules/software/module.md — Manifiesto del módulo `software`

> Un **módulo** empaqueta roles + disciplinas para un tipo de trabajo. Este añade lo específico de
> desarrollo de software sobre el núcleo agnóstico. Se activa con `módulos: [software]` en
> `stele.config.md`. El núcleo no depende de él; desactivarlo devuelve un marco agnóstico limpio.

## Qué aporta

| Aporta | Detalle |
| --- | --- |
| **Roles** | `gotchas`, `specs`, `specs_dir`, `architecture`, `effort` (ver `roles.md`). `gotchas` es **obligatorio de arranque** (order 20). |
| **Templates** | `templates/{gotchas,specs,architecture,effort}.md` |
| **Features** | enciende `effort_log` (opcional; el rol `effort` depende de este toggle) |
| **Convenciones** | git/test/deploy + reglas de código (ver `conventions.md`) |
| **Regla dura** | *HANDOVER en `EN_PROGRESO` antes de editar el PRIMER archivo de código* (ver abajo) |
| **Detectores** | Lo que el ritual AUDITAR puede comprobar gracias a estos roles (ver abajo) |

## Regla dura: checkpoint antes del primer archivo de código

Antes de tocar el primer archivo de **código** de la sesión, `handover` debe quedar en
`EN_PROGRESO` con objetivo + alcance + verificación prevista. No depende del tamaño estimado del
cambio: una sesión puede cortarse en cualquier momento y el checkpoint (~20 líneas) siempre cuesta
menos que reconstruir el contexto desde el diff. **Exención:** cambios que SOLO tocan el **contenido**
de la documentación — no una **migración estructural** (mover o renombrar docs: rituales CONFIG y
ACTUALIZAR), que es interrumpible aunque no toque código.

Esta regla vive en el módulo (no en el núcleo) porque presupone la noción de "archivo de código".
El núcleo agnóstico usa un `checkpoint_trigger` genérico configurable (`stele.config.md` →
Wording, default *antes de un cambio interrumpible*); este módulo lo especializa a "antes del primer
archivo de código". Un proyecto de software sin código todavía —o cuyo producto no es código— debe
reescribirlo con el ritual `config`, o la exención de documentación deja la regla muerta.

## Qué aporta al ritual AUDITAR

Las ocho clases de drift son del núcleo (`SKILL.md` → AUDITAR): son propiedades de cualquier
documentación, no de software. Lo que aporta este módulo son **detectores concretos**, porque una
detección necesita saber *qué doc contradice a cuál*, y eso depende de los roles activos:

| Clase | Detector que habilitan estos roles |
| --- | --- |
| 4 — índice desincronizado | Par `specs` ↔ `specs_dir`: cada tema de `specs_dir` tiene entrada en `specs`, y cada entrada apunta a un archivo que existe. Al revés también: una sección de `specs` que ya superó ~50 líneas debería estar extraída |
| 7 — hallazgo sin hogar | Los tres hogares de promoción del módulo: trampa técnica → `gotchas`; decisión por feature → `specs`; patrón o mapa del producto → `architecture`. Por cada sesión del rango, sus *Decisiones* deben tener eco en uno de los tres |
| 8 — crecimiento sin revisión | Los topes del módulo: sección de `gotchas` por subsistema ~150-200 líneas; tema de `specs` ~600-800 |
| 2 y 6 — estado obsoleto y bloqueo | `specs` es donde viven las fases y las preguntas abiertas por feature, y por eso es el doc que más rápido caduca cuando el producto avanza |
| 1 — afirmación caducada | Patrones comprobables propios de software, para el detector de entorno del núcleo: puertos, unidades de servicio (`\b[a-z0-9_-]+\.service\b`), nombres de contenedor e imágenes. **Solo si el proyecto opera servicios en red** (ver abajo). Valen las cuatro cautelas del núcleo, empezando por que un candidato extraído es un **recorte** de la afirmación |

### Números contra formas: por qué el filtro de puertos es distinto

**Una ruta tiene forma; un número solo tiene contexto.** Por eso el filtro de plausibilidad de rutas
—contrastar contra raíces reales— descarta el 80% sin equivocarse, y el equivalente para puertos **no
existe**: ningún rango numérico distingue un puerto de un año, de un recuento de líneas, de una norma
(`8601`) o de un identificador. La documentación técnica está llena de números en el rango de puertos.

Así que **el filtro de verdad es el ancla léxica del propio barrido**, no el rango:

```bash
# el ancla es lo que filtra: dos puntos PEGADOS a los digitos, o la palabra delante
grep -rhoE "\b(puerto |port |:)[0-9]{2,5}\b" {base} --include="*.md"
```

Con dos avisos, porque el ancla tampoco es gratis:

- **`:` pegado a dígitos casa con `archivo:línea`**, que la documentación técnica usa a todas horas
  (`SKILL.md:401`), y también con marcas de tiempo (`10:36`), fragmentos IPv6 e identificadores de
  dispositivo. Descarta esos antes de comprobar nada: si el token de la izquierda parece un archivo,
  no es un puerto.
- **El rango `<1024` sigue sirviendo, pero como segundo filtro, no como el principal.** Documentarlo
  al revés —que es lo que hacía este módulo— vende como filtro lo que apenas descarta nada.

### Cuándo NO aplica este detector

Los patrones de puertos, servicios y contenedores son de **proyectos que operan servicios en red**, no
de "software" en general. Una librería, un compilador o un paquete de análisis no tienen nada que
comprobar aquí y solo arrastrarían falsos. Es más estrecho que el módulo, y por eso va condicionado en
vez de en un módulo aparte: **un módulo es un paquete de roles**, y esto no aporta ninguno.

El hogar de promoción es lo que hace detectable la clase 7 —la más valiosa y la más invisible— y es
justo lo que un proyecto sin este módulo no tiene: sin `gotchas` ni `specs`, un hallazgo de sesión
solo puede promoverse a `entry` o a `charter`, y el detector se queda en eso.

## Cuando el producto no es código

Los roles de este módulo (`gotchas`, `specs`, `architecture`) describen el **objeto del trabajo**;
los del núcleo describen el **proceso de continuidad**. Por eso viven aquí. El criterio para activar
el módulo es que el proyecto tenga un **producto con estructura y decisiones por feature** — no que
haya un compilador. Un kit de documentación, un corpus curado o una colección de plantillas encajan
igual que un codebase: `architecture` mapea la estructura de ese producto, `specs` sus decisiones y
`gotchas` las trampas al editarlo.

Lo que sí hay que ajustar en ese caso es el `checkpoint_trigger` (ver arriba): si el producto **es**
documentación, la exención por "solo contenido de documentación" eximiría al proyecto entero y
dejaría la regla dura muerta. Especialízalo a lo que hace las veces de código.

**Escribir código no es lo que decide activar el módulo.** Un proyecto de documentación pura genera
scripts en cuanto hay volumen: inventariar, emparejar, comparar dos fuentes, mover veinte archivos.
Eso es **herramienta para manipular el proyecto**, no producto, y su hogar es `artifacts_dir` — no
convierte el proyecto en software ni pide estos roles. El criterio sigue siendo el de arriba: un
producto con estructura y decisiones por feature.

## Defaults que aporta al manifiesto

```text
módulos: [software]
features:  effort_log = on
nombres:   gotchas=memory.md  specs=requirements.md  specs_dir=temas/
           architecture=architecture.md  effort=esfuerzo.md
wording:   checkpoint.trigger = antes de la primera edición de código
```

## Al desactivarlo

`config` retira sus filas del arranque y del mapa, pero **no borra** los docs `gotchas`/`specs`/
`architecture`/`effort` (quedan huérfanos preservados + aviso). El usuario decide archivarlos.
El `checkpoint_trigger` vuelve al default genérico del núcleo.
