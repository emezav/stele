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

## Regla dura: checkpoint antes del primer archivo de código

Antes de tocar el primer archivo de **código** de la sesión, `handover` debe quedar en
`EN_PROGRESO` con objetivo + alcance + verificación prevista. No depende del tamaño estimado del
cambio: una sesión puede cortarse en cualquier momento y el checkpoint (~20 líneas) siempre cuesta
menos que reconstruir el contexto desde el diff. **Exención:** cambios que SOLO tocan documentación.

Esta regla vive en el módulo (no en el núcleo) porque presupone la noción de "archivo de código".
El núcleo agnóstico usa un `checkpoint_trigger` genérico configurable (`stele.config.md` →
Wording, default *antes de un cambio interrumpible*); este módulo lo especializa a "antes del primer
archivo de código". Un proyecto de software sin código todavía —o cuyo producto no es código— debe
reescribirlo con el ritual `config`, o la exención de documentación deja la regla muerta.

## Defaults que aporta al manifiesto

```text
módulos: [software]
features:  effort_log = on
nombres:   gotchas=MEMORY.md  specs=REQUIREMENTS.md  specs_dir=temas/
           architecture=ARCHITECTURE.md  effort=ESFUERZO.md
wording:   checkpoint.trigger = antes de la primera edición de código
```

## Al desactivarlo

`config` retira sus filas del arranque y del mapa, pero **no borra** los docs `gotchas`/`specs`/
`architecture`/`effort` (quedan huérfanos preservados + aviso). El usuario decide archivarlos.
El `checkpoint_trigger` vuelve al default genérico del núcleo.
