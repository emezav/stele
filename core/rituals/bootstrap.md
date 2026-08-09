<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

## Ritual: BOOTSTRAP (instanciar el marco en un proyecto)

**Modo:** *greenfield* (no hay docs → scaffold) o *adopción* (ya existen → mapear a roles sin
sobrescribir contenido; solo generar lo que falte). Pasos:
1. Elegir `idioma`/`módulos`/`persistencia` y las **tres rutas** (`kit`/`base`/`loader`) con
   defaults sensatos. Auto-detectar: módulo software por `Cargo.toml`/`package.json`/`src/`;
   `persistencia = git` si hay `.git` **en la raíz del proyecto** — no vale uno anidado en un
   subdirectorio, que deja la raíz sin versionar —, si no `ninguna` (avisando de la consecuencia).
   Zero-question posible.
   **Desambiguación obligatoria:** una ruta suelta en la petición del usuario ("usa stele aquí, base
   stele") se interpreta como **`base`** — es lo que al usuario le importa; `kit` solo cambia si dice
   "kit" o "marco" explícitamente. **Ante duda real, ofrecer el menú de layouts** (ver "Layouts con
   nombre") con una opción `otro` para dar `kit` y `base` a mano — una pregunta cerrada en vez de dos
   abiertas. Nunca adivinar.
   **Si no detectas código, recomienda `agrupado`** (y dilo, no lo impongas). Con el default
   `base = .` los docs de rol caen sueltos en la raíz del proyecto, y a quien no programa eso le
   parece un desorden ajeno: no distingue lo suyo de lo del marco, y acaba sin tocar archivos que son
   **suyos** y que debería editar cada sesión. Con `agrupado` todo queda bajo `bitacora/` y la raíz
   sigue siendo del usuario. No hace falta prefijar nada: **una carpeta con nombre ya dice de quién es
   lo que hay dentro** — y por eso no se llama como el marco (ver la tabla de layouts).
2. **Eco del layout resuelto ANTES de escribir nada** (siempre, incluso en zero-question):
   ```text
   layout       -> agrupado   (dónde va cada cosa; o "personalizado")
   kit          -> .stele     (el marco; se reemplaza al actualizar)
   base         -> bitacora   (tus documentos; no se tocan nunca)
   loader       -> CLAUDE.md  (el archivo que arranca al agente)
   persistencia -> git        (cómo se guarda el trabajo al cerrar)
   ```

   Coste cero y ataja la mala interpretación antes del scaffold, no después. Si el kit ya se
   vendorizó en la ruta equivocada, moverlo aquí es trivial; después no.
3. Validar los **invariantes de ruta** (ver "Las tres rutas"). Violación = abortar y re-preguntar.
4. Resolver nombres (defaults de rol + módulo; override libre).
5. Escribir `stele.config.md` en la raíz (plantilla `core/templates/config.md`), con la sección
   Rutas ya resuelta y **`kit_origen` anotado**: la URL o ruta de la que acabas de traer el kit. Es
   el único momento en que se sabe con certeza, y sin ella ACTUALIZAR no puede correr después.
6. Scaffold: instanciar cada template por rol → nombre configurado bajo `base`, **resolviendo
   tokens** (incluido `{{kit}}`). En adopción, saltar los docs que ya existen.
7. Semilla: `state` y `handover` (`SIN_TRABAJO_ACTIVO`) iniciales, `index` vacío. **`audit` no se
   instancia**: lo crea la primera auditoría, y su ausencia es el dato (nunca se auditó).
8. Generar derivados: loader de auto-arranque en la raíz, con el nombre de la ruta `loader`
   (plantilla `autostart.md`) + mapa-doc en `entry`. **Si el loader ya existe** (`CLAUDE.md`,
   `AGENTS.md`… escritos a mano antes de adoptar el marco), **léelo primero e inserta** el bloque
   `STELE:INICIO`/`STELE:FIN` conservando todo lo demás — invariante 6. Igual que en adopción con
   cualquier otro doc: nunca reemplazar contenido que no escribiste.
   **Emite la marca pelada, siempre.** El bloque queda protegido por default y tú no tienes que
   decidir nada: no escribas `LIMPIO` — esa marca la escribe quien haya comprobado con un diff que el
   bloque no dice nada que la plantilla no diga, y aquí acabas de instanciarlo, no de compararlo.
   **Y no toques los comentarios `STELE:*` al instanciar.** Llevan las instrucciones que gobiernan la
   próxima actualización; condensarlos "para ahorrar" ya borró una de ellas en campo. Lo que sí se
   quita es el comentario `PLANTILLA` de la cabecera, que habla del kit y no del proyecto.
9. Validar (ver ritual CONFIG, fase 5).
10. Confirmar + activar: reabrir el editor → el loader se auto-carga.

