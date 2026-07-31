---
name: stele
description: >
  Marco modular y configurable de documentación y continuidad para trabajar en un proyecto a
  través de muchas sesiones sin perder contexto y con coste de tokens acotado. Sirve para software
  y para trabajo no-software (materiales, planeación, investigación). Úsalo al INICIAR una sesión
  (ponerse al día), al CERRAR (registro durable), antes de un cambio interrumpible (checkpoint),
  para INICIALIZAR el marco en un proyecto (bootstrap), o para ADAPTARLO (config: nombres, módulos,
  parámetros). Núcleo agnóstico + módulos (software) + config (stele.config.md).
---

# stele — rituales de sesión

> Hoja operativa. El *por qué* y las fronteras están en `GUIDE.md` (leer una vez). Plantillas en
> `core/templates/` y `modules/<mód>/templates/`. Roles en `core/roles.md` y `modules/<mód>/roles.md`.
> Regla madre: **un hogar por dato; el estado se sobrescribe; el historial es append-only; lee poco
> al arrancar.** Los nombres de archivo salen del manifiesto `stele.config.md`.

## Mapa de documentación (GENERADO — consúltalo antes de leer/escribir)

El mapa "necesito X → tal archivo" **no se escribe a mano**: se genera en el doc `entry` desde los
`triggers` de los roles activos + el binding de la config. Regla de generación:

- **Roles activos** = roles del núcleo (`core/roles.md`) + roles de los módulos activos, menos los
  que tengan nombre `—` en la config.
- **Lista de arranque** = roles `startup: obligatorio`, ordenados por `order`, nombre resuelto
  (`{base}/[{history_dir}/]{nombre}`). El resto va a la nota "bajo demanda con grep".
- **Tabla de enrutamiento** = una fila `trigger → nombre` por cada rol activo.

Se regenera en `config` y al activar/desactivar módulos o renombrar. **Regla de oro anti-tokens:**
no leas un archivo entero "por si acaso"; usa el mapa y `grep -n` + lectura por rango.

## Convención de tokens en plantillas

Las plantillas se escriben por **rol** y usan tokens que bootstrap/`config` resuelven a nombres:
`{{rol}}` → nombre del rol (p. ej. `{{state}}`→`LATEST.md`); `{{history_dir}}` → carpeta de
historial; `{{budget:rol}}` → tope de líneas; `{{effort_unit}}`, `{{checkpoint_trigger}}`,
`{{session_greeting}}` → valores de Features/Wording; `{{specs_dir}}` → carpeta de temas de `specs`.
Los bloques marcados `<!-- GENERADO -->` los produce el marco, no se editan a mano.

## Ritual: ABRIR sesión (ponerse al día, barato)

Lee, en orden, SOLO la **lista de arranque** del proyecto (generada; con defaults del módulo software):
1. `entry` · 2. `gotchas` · 3. `state` · 4. `handover` — **si su Estado ≠ `SIN_TRABAJO_ACTIVO`**,
respétalo antes de editar. Bajo demanda (grep): `charter` (1ª vez / orientación), `protocol`,
`specs`, `architecture`, `index`/`session`.

**Confirma el arranque (visible):** un agente **no puede hablar antes de que el usuario escriba**,
así que la confirmación va **al frente de tu PRIMERA respuesta** — 1-3 líneas: última sesión
(N + título), estado del `handover`, próximo paso propuesto. Sin esto, el arranque silencioso es
indistinguible de uno que no corrió. (Se omite si `session_greeting = off`.)

## Regla dura: checkpoint antes de un cambio interrumpible

Deja `handover` en `EN_PROGRESO` con objetivo + alcance + verificación prevista (plantilla
`core/templates/handover.md`) **{{checkpoint_trigger}}**. No es opcional ni depende del tamaño: una
sesión puede cortarse en cualquier momento y el checkpoint (~20 líneas) siempre cuesta menos que
reconstruir el contexto desde el diff. Exención: cambios que SOLO tocan documentación. (El módulo
software especializa el trigger a "antes del primer archivo de código".)

## Ritual: CERRAR sesión (dejar registro durable)

1. **`session`** (nuevo): qué se hizo, decisiones, archivos, verificación, notas para retomar, y
   `## Esfuerzo equivalente` (si `effort_log`). `NNN` con padding a 3 dígitos.
2. **`index`**: una fila con append de Bash — `printf '| N | … |\n' >> {history_dir}{index}`.
3. **`effort`** (si `effort_log`): una fila con `printf >>`.
4. **`state`**: reescríbelo COMPLETO con `Write` según su plantilla (nunca prepend).
5. **Decisiones que perduran** → su hogar (mapa): producto/feature → `specs`; principio/decisión
   grande → `charter`; patrón de código → `architecture`; gotcha → `gotchas`. Nunca solo en historial.
6. **`handover`**: si cerró completo → `SIN_TRABAJO_ACTIVO` **apuntando a la sesión que cierras
   ahora**. Si quedó salto → `EN_PROGRESO`.
7. Si dejaste un commit: dile al usuario el `git push` exacto (o hazlo si lo autoriza).

## Ritual: BOOTSTRAP (instanciar el marco en un proyecto)

**Modo:** *greenfield* (no hay docs → scaffold) o *adopción* (ya existen → mapear a roles sin
sobrescribir contenido; solo generar lo que falte). Pasos:
1. Elegir `idioma`/`módulos`/`base` (defaults sensatos; auto-detectar módulo software por
   `Cargo.toml`/`package.json`/`src/`). Zero-question posible; si no, confirmar los 3 de golpe.
2. Resolver nombres (defaults de rol + módulo; override libre).
3. Escribir `stele.config.md` en la raíz (plantilla `core/templates/config.md`).
4. Scaffold: instanciar cada template por rol → nombre configurado bajo `base`, **resolviendo
   tokens**. En adopción, saltar los docs que ya existen.
5. Semilla: `state` y `handover` (`SIN_TRABAJO_ACTIVO`) iniciales, `index` vacío.
6. Generar derivados: loader de auto-arranque en la raíz (plantilla `autostart.md`) + mapa-doc en `entry`.
7. Validar (ver ritual CONFIG, fase 5).
8. Confirmar + activar: reabrir el editor → el loader se auto-carga.

## Ritual: CONFIG (adaptar nombres/parámetros — único renombrador sancionado)

1. **Leer + reconciliar** `stele.config.md` contra los archivos reales; reportar/arreglar drift.
2. **Clasificar** el cambio por radio de impacto: renombrar / toggle módulo / toggle feature /
   presupuesto / wording / idioma / `base`.
3. **Previsualizar** (dry-run) y confirmar (renombrar toca varios archivos).
4. **Aplicar**, acotado a los **docs del marco** (nunca código de producto): `git mv` → reescribir
   la tabla del manifiesto **completa** → barrido de referencias por el mapa viejo→nuevo →
   regenerar derivados (auto-arranque + mapa-doc).
5. **Validar**: `grep` del nombre viejo = 0; cada nombre resuelve a un archivo; ningún rol activo
   apunta a faltante.

Reglas fijas: desactivar un módulo **no** borra sus docs (huérfanos preservados + aviso); colisión
de nombre aborta; cambiar el patrón `session` afecta solo sesiones futuras (el historial es inmutable).

## Operaciones de bajo coste (preferir siempre)
- Apéndice de una fila → `printf '...' >> archivo` (sin `Read` previo).
- Archivo pequeño de formato fijo → un `Write` completo (no varios `Edit`).
- Buscar en archivo grande → `grep -n` y luego leer solo el rango.
- Volumen mecánico grande (dividir un doc de 1000+ líneas) → delegar a un subagente.
