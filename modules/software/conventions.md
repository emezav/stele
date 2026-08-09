# modules/software/conventions.md — Convenciones de desarrollo

> Disciplinas específicas de software que el módulo aporta. No son parte del núcleo agnóstico.
> Adáptalas al proyecto en `{{entry}}` (sección Convenciones) y en `{{architecture}}`; este archivo
> es la referencia de qué debe quedar cubierto.

## Git

> Esta sección presupone `persistencia = git` en el manifiesto. Con `ninguna` o `comando`, el
> cierre se rige por el paso 7 de CERRAR (`{{kit}}/core/rituals/cerrar.md`) y estas reglas no aplican.

- **Ramas:** `feature/<nombre>`, `fix/<descripción>` (o la convención del repo). No trabajar en la
  rama por defecto sin acordarlo.
- **Commits/push:** commitear o pushear solo cuando el usuario lo pida o confirme. Al preparar un
  commit, indicar el `git push` exacto. En monorepos con submódulos: commitear cada submódulo antes
  de actualizar su puntero en el repo padre.
- **No revertir trabajo ajeno** (regla del núcleo, ver `{{entry}}`): solo hunks propios de la sesión.

## Verificación (antes de cerrar / persistir)

- Compilar/typecheck + linter limpios en lo tocado; correr los tests afectados.
- Reportar el resultado con honestidad en `{{session}}` → Verificación: si algo quedó sin probar o
  falló, decirlo. No afirmar "hecho y verificado" sin evidencia.
- E2E/validación en vivo cuando aplique (deploy, integración): validar en local antes de stagear.

## Checkpoint antes del primer archivo de código

Regla dura del módulo (ver `module.md`): `{{handover}}` en `EN_PROGRESO` **{{checkpoint_trigger}}**.
Exención: cambios que SOLO tocan el **contenido** de la documentación; una migración estructural
(mover o renombrar docs) **no** está exenta.

## Dónde va cada decisión de código

- Contrato/feature/modelo de datos → `{{specs}}`.
- Patrón reutilizable del producto → `{{architecture}}` (uno por producto).
- Trampa no evidente al leer el código → `{{gotchas}}`.
- Principio/apuesta grande transversal → `{{charter}}`.
- Nunca dejar una decisión de código solo en el historial ni en memoria privada del agente.

## Seguridad (mínimos al escribir código)

- Nunca exponer secretos; `.env` no se commitea (solo plantillas sin secretos).
- **Ninguna credencial en un doc del marco** (regla del núcleo, ver `{{entry}}`): se nombra la
  herramienta y de dónde toma sus credenciales, nunca el valor.
- Validar toda entrada en el servidor; CORS acotado al origen esperado.
