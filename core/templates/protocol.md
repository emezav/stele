# {{protocol}} — Protocolo de documentación entre sesiones

> **Cómo** se documenta (formatos, convenciones de edición, topes de tamaño) para que ningún
> archivo de estado crezca sin límite y para minimizar tokens/roundtrips de cualquier agente.
> Los rituales condensados están en `{{kit}}/SKILL.md`; el *por qué*, en `{{kit}}/GUIDE.md`.
> Los nombres de archivo abajo son los del manifiesto (`stele.config.md`).

## Principios
1. Los archivos de **estado** no crecen: se **sobrescriben** (formato fijo + tope).
2. El **historial** vive en archivos por sesión y **no se reabre** (se referencia por link).
3. Los **apéndices de una línea** usan `printf '...' >> archivo`, no `Read`+`Edit`.
4. Ninguna sección de un doc fuente-de-verdad supera **~150-200 líneas**; si crece, se extrae a
   un tema de `{{specs}}` + link.
5. Nada de esto vive en memoria privada del agente; todo en el repo.
6. **Un hogar por dato** (mapa en `{{kit}}/SKILL.md` y en `{{entry}}`).

## Archivos y su rol
Ver `{{kit}}/GUIDE.md` → "Roles y fronteras". Aquí solo los **formatos**.

### `{{state}}` — formato fijo, se SOBREESCRIBE (~{{budget:state}} líneas)
```markdown
# Estado actual
> Última sesión: Sesión N (YYYY-MM-DD) — ver {{session}}
> Índice completo: {{index}}

## Dónde estamos
- (3-8 bullets del estado REAL, no histórico)
## Próximo paso inmediato
- (lo que haría la siguiente sesión; reemplaza, no acumula)
## Pendientes operativos
- Push pendiente / procesos en background / decisiones abiertas
## Referencias
- {{specs}} §X — … / {{charter}} § … / tema de {{specs}}
```
Al cerrar: reescribir COMPLETO con `Write`. Nunca `Edit` para prepend/rename de "anterior".

### `{{index}}` — tabla append-only
`| Sesión | Fecha | Resumen | Archivo |`. Al cerrar:
`printf '| N | YYYY-MM-DD | resumen | <session> |\n' >> {{history_dir}}{{index}}`.

### `{{effort}}` — tabla append-only (OPCIONAL, feature `effort_log`)
`| Sesión | Fecha | {{effort_unit}} | Funcionalidades clave |`. Estimar lo que le tomaría a UN
ingeniero senior el mismo trabajo con calidad de producción (investigación + implementación +
validación + docs), en rango. Detalle por funcionalidad en el `{{session}}`.

### `{{session}}` — uno por sesión
Detalle completo: qué se hizo, decisiones, archivos tocados, verificación, notas para retomar,
y `## Esfuerzo equivalente` (si se usa). `NNN` con padding a 3 dígitos. No se reabre; se lee con grep.

### `{{handover}}` — checkpoint de trabajo en curso (~{{budget:handover}} líneas)
Estados: `SIN_TRABAJO_ACTIVO` | `EN_PROGRESO` | `COMPLETADO`. **Regla dura:** {{checkpoint_trigger}},
`EN_PROGRESO` con objetivo/alcance/verificación. Al cerrar, siempre refrescar el puntero a la
sesión que se cierra AHORA. Plantilla en `{{kit}}/core/templates/handover.md`.

### `{{specs}}` (+ temas)
Índice de decisiones: cada entrada ≤3 líneas + link cuando el detalle supera ~50 líneas. Un tema
por feature de diseño extenso; si supera ~600-800 líneas, dividir en sub-temas.

### `{{gotchas}}`
Hogar único de gotchas de código. Se edita incrementalmente pero se **cura** (se poda lo
obsoleto). Una sección de subsistema que supera ~150-200 líneas se extrae a un tema de `{{specs}}`.

## Checklist de inicio / cierre
Condensados en `{{kit}}/SKILL.md` (rituales ABRIR / CERRAR). Este archivo es la referencia de
formato cuando haya dudas.

## Operaciones de bajo coste (preferir)
- Apéndice de fila → `printf '...' >> archivo`.
- Archivo pequeño de formato fijo → un `Write`.
- Buscar en archivo grande → `grep -n` + lectura por rango.
- Si vas a EDITAR, léelo con el tool `Read` (no `cat`/`sed`) o el `Edit` se bloquea.
- Volumen mecánico grande → delegar a un subagente.
