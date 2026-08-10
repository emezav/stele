<!-- Ritual del kit stele. Se lee BAJO DEMANDA: `SKILL.md` enruta y no repite este contenido.
     Si cambias una regla de aquí, comprueba si `SKILL.md` la resume en su tabla de rituales. -->

# Ritual: ACTUALIZAR (traer una versión nueva del kit)

Se dispara con "actualiza stele" / "trae la última versión del marco". Cambia **solo la ruta `kit`**:
`base` no se toca nunca — esos docs son del proyecto, y una plantilla nueva **no reinstancia nada**.
No aplica en modo auto-hospedado (`kit = .`): ahí el marco se desarrolla en sitio, no se vendoriza.

**Regla dura: no toques el kit hasta haber leído el diff.** La versión nueva se trae **al lado**, a
un temporal, nunca encima de la que ya tienes. Así el diff existe siempre — sin depender de que el
adoptante haya versionado el kit ni de acordarse de respaldarlo — y una actualización que se aborta a
medias no deja nada roto: si no llegaste a aplicar, no tocaste nada.

1. **Traer la versión nueva a un temporal**, fuera del árbol del proyecto o en un directorio ignorado
   por el VCS (si cae dentro, ensucia el `status` y puede acabar commiteado). La fuente es
   **`kit_origen`** (manifiesto → Meta); con el mismo `degit`/`clone` de la instalación. **Nunca sobre
   `{kit}`.** Si `kit_origen` falta o está vacío, **pide la URL al usuario y escríbela en el
   manifiesto** antes de seguir: sin ella el ritual no arranca, y no se deduce del árbol.
2. **Diffear** viejo contra nuevo: `diff -r {kit} {temporal}`. **Un diff vacío tiene dos causas y se
   leen igual:** que estés al día de verdad, o que **el origen no haya publicado lo que dice tener** —
   dicho por quien lo vivió, *"un clone de un kit sin publicar no da error: devuelve un kit"*.
   Antes de dar por bueno el primero, mira un dato observable del origen —la fecha de su último cambio,
   el identificador de su última entrega— y compáralo con lo que esperabas encontrar. Si cuadra: dilo en
   una línea, borra el temporal y termina sin haber tocado nada. Si no, **el problema está aguas arriba**
   y lo que toca es avisar, no actualizar.
3. **Clasificar por zona de impacto** (tabla abajo). Lo que no aparece en la tabla es procedimiento:
   se lee, no se migra. **Y lee entero todo archivo que el diff marque como NUEVO** (`Only in
   <temporal>:`) antes de aplicar, esté o no en la tabla: un archivo que no existía no puede tener
   fila, porque la fila que lo describiría viaja en el mismo kit que lo trae. Ver abajo.
   **Si el diff muestra un archivo del kit RENOMBRADO o ELIMINADO, busca en tus propios docs los
   enlaces al nombre viejo.** La tabla te dice qué hacer con el contenido del kit, no con las
   referencias que tú tengas hacia él, y esas se quedan colgando en silencio. `grep` del nombre viejo
   en `{base}` = 0 antes de dar la actualización por buena.
4. **Aplicar:** sustituir `{kit}` por el temporal. Es seguro *aquí* porque el invariante 1 garantiza
   que `base` no está dentro. **A partir de esta línea el procedimiento vigente es el del kit nuevo**,
   no el que traes en contexto: si el diff tocó `SKILL.md`, relee lo que gobierna los pasos que te
   quedan — el informe del paso 6 incluido.
5. **Reconciliar con CONFIG** (fase 1, drift), acotado a lo que el diff señaló: filas que le faltan
   al manifiesto, secciones nuevas, derivados a regenerar.
6. **Informar** en pocas líneas: qué cambió, qué se reconcilió solo, y qué exige decisión del usuario
   (un rol nuevo que quizá quiera desactivar, un default que él había sobrescrito, un cambio del
   contrato de parseo). **Y si el diff tocó las puertas o el bloque `GENERADO` del `entry`, pídele que
   reabra el editor y te salude** — el delta a esos bloques se porta **a mano** y a **cada** puerta, así
   que es donde una copia puede quedarse atrás sin que nada lo diga: *si no te contesta con la última
   sesión y el próximo paso, el delta no llegó a la puerta que lee tu agente.*
7. **Limpiar** el temporal.

**Lo que acabas de instalar gobierna lo que te queda por hacer, y es el paso que más se salta.** Un
agente ejecuta ACTUALIZAR con el procedimiento que cargó al abrir la sesión, así que entre el paso 4 y
el 7 sigue operando de memoria con el kit que acaba de sustituir. El caso visible es el **informe**: es
el primer mensaje en que el adoptante ve la versión nueva y el único que todavía obedece a la anterior.
Si esta actualización trae una regla de **habla** —cómo se le dice un número a la persona, en qué
registro se le explica un rol nuevo—, esa regla ya rige aquí, y **ningún paso la recuerda** porque no
es una regla de procedimiento. Caso de campo: un informe correcto en todo lo demás soltó *"ya vamos en
9 sesiones sin auditar"* —contador desnudo, sin consecuencia y sin qué se espera de la persona— en la
misma actualización que traía la regla que lo prohíbe.

Es la misma ley que ya conoce la tabla de zonas —*una regla que gobierna el actualizar no gobierna la
actualización que la entrega*— aplicada un nivel más adentro: no al **qué migrar**, sino al
**procedimiento en curso**. Una regla nueva llega tarde por construcción, y lo único que la rescata es
releerla en el paso 4.

| Zona del diff | Qué implica para esta instancia |
| --- | --- |
| `core/roles.md`, `modules/*/roles.md` | Roles nuevos, renombrados o con distinto `startup`/`order`: al manifiesto le faltan filas y hay que **regenerar los dos derivados**. Y si el rol nuevo es un **doc**, su archivo **no existe en esta instancia**: ofrécelo al usuario e instáncialo si acepta — regenerar los derivados deja el mapa apuntando a algo que no está |
| `core/templates/config.md` | Cambió la forma del manifiesto o el contrato de parseo: la instancia puede estar desfasada (secciones nuevas, claves nuevas) |
| `modules/<mód>/module.md` | Cambió lo que aporta un módulo activo: features, defaults o su regla dura |
| Un directorio de `modules/` **cambió de nombre** | El valor de `módulos` en tu manifiesto **sigue siendo válido**: los nombres viejos son alias permanentes y nadie tiene que migrar nada. Si quieres el nombre nuevo, es un cambio de una celda. Lo que **sí** hay que revisar son las referencias por ruta (`modules/<viejo>/…`) que tus propios docs hayan escrito |
| `core/templates/autostart.md`, y los bloques `GENERADO` de `core/templates/entry.md` | Cambió un **derivado**: **porta el delta a mano** al archivo real — **y `autostart` se instancia una vez POR PUERTA**, así que el delta va a **todas**, no solo a la primera que encuentres. Dos puertas que digan cosas distintas es el único modo en que varias puertas hacen daño. Se conserva íntegro lo que quede fuera de las marcas —invariante 6— **y también lo de dentro**, que está protegido por default (ver abajo). Solo se regenera el bloque entero si su marca dice `LIMPIO` |
| `core/templates/*` de rol (salvo sus bloques `GENERADO`), `modules/*/templates/*` | **Nada que hacer** para los docs que ya existen: son del proyecto y no se regeneran jamás. **Tres excepciones, y las tres abajo.** Dos son por AUSENCIA: un **rol nuevo** (no hay doc que respetar) y una **sección nueva** dentro de una plantilla de rol que sí existe (el doc está, el trozo no). La tercera no: una **comprobación corregida** —un comando que el kit manda correr y que estaba mal—, donde el adoptante no tiene una ausencia sino **una copia rota que pasa en verde**. Las tres **se ofrecen**, no se aplican en silencio |
| `SKILL.md`, `guide.md`, `README.md` | Procedimiento y fundamentos: se leen, no se migran |
| `core/rituals/*`, `core/reference/*` | **Los rituales ya no viven en `SKILL.md`, y las rutas y tokens tampoco.** Se leen, no se migran — pero **tus propios docs pueden enlazar al nombre viejo**: si alguno cita `SKILL.md -> CERRAR` o similar, ahora apunta a un enrutador y no al contenido. **Barre por el nombre viejo, no por la forma del puntero** — el patrón `SKILL.md -> RITUAL` no encuentra ni la prosa que describe qué contiene `SKILL.md` ni las citas a secciones suyas que se movieron, y las dos existían: `grep -rn "SKILL\.md" <base> --include="*.md"` y revisa cada acierto a mano. Es más ruidoso y es el único que los caza; aquí el patrón estrecho dejó tres referencias colgadas en el propio kit. |
| El **bloque de detectores** de `core/rituals/auditar.md` | Solo importa si tu proyecto **no** está en el `idioma` del kit y por tanto tiene detectores **derivados** en la sección *Detectores de auditoría* de `protocol`. Si el kit añadió, quitó o cambió un detector, el tuyo no se entera: **es un derivado que no se regenera**, como el loader. Porta el delta a mano — y si el detector nuevo es **gramatical** y no léxico, no lo traduzcas: derívalo, y guárdalo con su control positivo. Un detector que no existe en tu sección **no da error: da cero**, y ese cero se lee como corpus limpio |
| El buzón del kit (si lo tiene) | **Correspondencia que baja.** Léela y dile al usuario si hay algo dirigido al `remitente` de este proyecto o alguna pregunta que pueda contestar. Contestar es ritual REMITIR; archivar solo lo que se conteste o lo que mueva a hacer algo. **Baja aunque `correspondence_log` esté en `off`** —viaja dentro del kit—, y entonces se puede leer pero no contestar: hace falta activar el rol y elegir `remitente`, y las dos cosas las decide la persona. No ofrezcas REMITIR sin decirlo |

**Un rol nuevo es el único caso donde una plantilla de contenido sí llega a quien ya adoptó.** La fila
de las plantillas de rol dice que los docs de `base` no se regeneran jamás, y es cierto **para los que
existen**: son del proyecto. Pero un rol que no existía no tiene doc que respetar, así que ahí no hay
conflicto — hay una ausencia. Sin esta excepción, una capacidad nueva del marco solo la reciben los
proyectos que empiecen después, y el que la necesitaba lleva sesiones sin saber que existe. **Se
ofrece, no se crea en silencio:** el usuario puede no quererla.

**Y una SECCIÓN nueva dentro de una plantilla de rol que ya existe cae en el mismo hueco, pero pasa
desapercibida.** El doc del adoptante existe, así que la fila dice *"nada que hacer"* y se cumple al
pie de la letra — mientras la sección **no aparece nunca**. Es la misma ausencia que justifica la
excepción de arriba, en una escala en la que nadie la busca: no falta un documento, falta un trozo de
uno que sí está, y desde fuera se ve idéntico a un doc que el proyecto decidió no usar. **Al ver un
`##` nuevo en una plantilla de rol, ofrécelo igual que un rol nuevo**, con el mismo criterio: se
ofrece, no se inserta en silencio.

**Y la tercera excepción es de otra familia, porque no hay nada ausente: hay algo presente y roto.**
Cuando el kit **corrige un comando que manda correr**, el adoptante conserva el suyo y **sigue dando
verde** — que es peor que no tenerlo, porque un hueco se nota y un verde falso no. Las dos anteriores
se detectan mirando qué falta; esta no se detecta mirando nada, porque el doc del adoptante está
completo, bien formado y **contesta lo que se le pregunta**. **Al ver que cambió un comando dentro de
una plantilla de rol, mira si el viejo podía fallar** — si no podía, el adoptante lleva un certificador.

Caso propio, y es lo que abrió esta fila: el control positivo de no-ASCII de la plantilla de `protocol`
solo exigía **salida**, y con un `>= 1` pasaba en verde tanto con el detector sano como con el detector
midiendo caracteres donde el patrón supone bytes. La corrección es escribirle **el número esperado**;
lo que no arreglaba nada era añadir una advertencia al lado.

Caso propio: se añadió `## Detectores de auditoría` a la plantilla de `protocol` —el hogar de los
detectores derivados de un proyecto que no está en el `idioma` del kit— y **solo lo habrían recibido
los proyectos bootstrapeados después.** El adoptante al que le hacía falta era, exactamente, el que ya
llevaba sesiones trabajando en otro idioma.

**Y su toggle no se escribe hasta que conteste.** Un `off` puesto por precaución y un `off` elegido son
el mismo texto en el manifiesto, y no son la misma decisión: la próxima actualización ya no ve un rol
nuevo, no vuelve a ofrecerlo, y la capacidad se pierde **en silencio** — que es exactamente lo que esta
excepción existe para evitar. Si la sesión acaba sin respuesta, la pregunta va a los pendientes de
`state`, que es el hogar de lo que queda abierto y se relee en cada arranque. Preguntar dos veces
cuesta una línea; una oferta que nadie llegó a declinar y que ya no se repite no la recupera nadie.

Caso de campo: un agente dejó dos roles nuevos en `off` *"para no cambiarte el comportamiento sin que
lo pidas"* y preguntó **en el mismo mensaje**, con el manifiesto ya escrito. Ahí acabó bien —el usuario
los activó—, así que lo que se documenta no es una pérdida observada sino el modo de fallo que ese
orden habilita: con un "ahora no", o sin respuesta, el manifiesto habría quedado idéntico a si nunca se
hubiera ofrecido nada.

**Plantilla de contenido contra plantilla generadora.** Es la distinción que decide las dos filas de
plantillas —la del loader con sus bloques `GENERADO`, y la de las plantillas de rol—, y confundirlas
es un fallo silencioso. Una plantilla de **contenido** produjo un doc que desde el primer día es del
proyecto: se instancia una vez y no se vuelve a tocar jamás. Una plantilla **generadora** produce un
**bloque** que el marco sigue siendo dueño de reescribir —el del loader y los dos del `entry`— y ese
bloque **sí** viaja con cada actualización. Si no se regenera, el adoptante se queda con el kit nuevo
y las reglas viejas cargándose en cada sesión, sin ninguna señal de que algo falta. Regenerar el
bloque **nunca** autoriza a tocar lo que esté fuera de las marcas (invariante 6).

**Y hay un límite conocido, sin remedio, que conviene saber: la discrepancia entre plantilla e instancia
es un detector gratis que se apaga al converger.** Cuando el kit corrige una plantilla de **contenido**,
el adoptante no la recibe —su doc ya es suyo— y durante un tiempo su copia y el texto del kit **dicen
cosas distintas**. Esa discrepancia es justo lo que hace visible el problema: aparece al comparar, y
alguien lo arregla a mano. Pero en cuanto lo arregla, los dos textos coinciden **por caminos distintos**
y la señal desaparece — para esa regla y para todas las que ya convergieron. Dicho crudo: **el adoptante
más al día es el que menos aviso tiene.** Lo aporta un proyecto en campo, no trae remedio, y se escribe
para que nadie lo descubra creyendo que es un fallo suyo.

**Observado en vivo poco después, por los dos lados a la vez:** una instancia arregló su copia a mano y
el kit arregló su plantilla por su cuenta; **los dos textos acabaron diciendo lo mismo por caminos
distintos y ninguno avisó al otro**. El resultado era correcto y la señal se apagó igual. **La
convergencia y el silencio llegan juntos** — no es un fallo que se pueda arreglar, es lo que significa
converger.

**El bloque generado casi nunca es un derivado puro, así que el default lo protege.** Un proyecto que
adoptó el marco con cientos de sesiones encima suele tener en su loader reglas propias que la plantilla
base no contiene —una regla dura específica, su mapa de hogares, el porqué de su saludo—, y ahí
"regenerar" no es refrescar: es **perder**. Por eso ACTUALIZAR y CONFIG **portan el delta a mano**: se
aplica lo que cambió en el kit y se conserva lo que el bloque diga de más.

**Esto es un default invertido, y lo invirtió la evidencia (2026-08-08).** Antes se reescribía entero
salvo que alguien hubiera escrito `STELE:INICIO RICO`. Dos razones lo tumbaron:

- **La salvaguarda dependía del paso que falla.** Quien tiene que escribir la marca es el bootstrap, o
  sea el mismo paso que reescribe el bloque; y se observó a un agente instanciar el loader condensado
  al 19% **borrando de paso la instrucción de marcar `RICO`**. Una protección que se pierde con lo que
  protege no es una protección.
- **El caso para el que el default estaba calibrado no se ha observado nunca.** De los bloques
  generados que se han podido mirar, ninguno salió idéntico a la plantilla: uno divergió por reglas
  propias del manifiesto el día mismo de adoptar (abajo), y dos corridas del mismo bootstrap, el mismo
  día y con el mismo agente, salieron al 19% y al 104% de la plantilla — **en direcciones opuestas**.
  El default protegía el caso raro y exponía el común.

**Y hay un criterio que decide sin depender del caso: el default protege lo que no tiene otra copia.**
Lo del kit siempre se puede recuperar — está en el kit, y el diff sigue ahí para portarlo mañana. Lo
que el adoptante escribió en su bloque solo existe ahí. Cuando una de las dos pérdidas es reversible y
la otra no, el default va del lado de la irreversible.

**Las marcas, entonces:**

| Marca de apertura | Qué hacen ACTUALIZAR y CONFIG |
| --- | --- |
| `STELE:INICIO` (pelada) | **Portar el delta a mano.** Es el default |
| `STELE:INICIO RICO` | Lo mismo. Marca antigua, ya redundante: **se acepta siempre y no se retira** — hay copias en campo y quitarla no gana nada |
| `STELE:INICIO LIMPIO` | Regenerar el bloque entero. **Solo tras comprobarlo con un diff** contra la plantilla con los tokens resueltos: es una afirmación verificable, no una impresión |

**Para CONFIG el delta es siempre una sustitución**, nunca contenido nuevo: cambian nombres de rol o
rutas resueltas. Sustituirlos en su sitio conservando el resto es a la vez lo correcto y lo más barato
— y era ya lo que hacía falta, porque regenerar entero también se llevaba las reglas del adoptante.

**Trampa de este cambio, y es recursiva:** al proteger el bloque, el cambio **no puede llegar por
regeneración** a quien ya adoptó — su loader dirá la regla vieja para siempre. Llega solo por esta
tabla de zonas, portado a mano. Es el caso general de la nota de arriba: un cambio en una plantilla
generadora no alcanza al adoptante salvo que ACTUALIZAR se lo diga.

**Y hay una variante donde el default no es una comodidad, sino la única protección que existe.** El
invariante 6 conserva lo que queda **fuera** de las marcas — pero un proyecto que adoptó puede haber
acabado con su texto escrito a mano **dentro** de ellas, si al insertar el bloque se rodeó lo que ya
había en vez de añadirlo aparte. Ahí el invariante 6 no protege nada, porque **no hay nada fuera**: todo
lo propio está en la zona que la regla autorizaba a reescribir entera. Caso real y documentado en campo,
con el loader escrito a mano meses antes de la adopción.

Así que al adoptar sobre un loader existente hay **más de dos situaciones**: generado de cero,
contenido propio fuera de las marcas (lo cubre el invariante 6), y **contenido propio encerrado
dentro** (lo cubre el default de portar el delta, y **nada más**). La tercera es la más peligrosa y la
menos evidente, porque desde fuera se ve igual que la primera — **y es exactamente por eso que no puede
depender de que alguien la reconozca y escriba una marca.** Reconocerla es el paso difícil; era el que
la regla vieja daba por hecho.

**Y "generado de cero" no es "nada que proteger", que es como estuvo escrito aquí y era falso.** Lo
corrigió un proyecto adoptante el mismo día de adoptar: su loader lo generó el bootstrap, sin una
línea escrita a mano, y **quedó marcado `RICO` esa misma jornada** porque el bloque generado se apartó
de la plantilla en reglas propias suyas —el idioma del habla frente al de los documentos, su
convención de escritura y el bloque redactado en otro idioma que la plantilla—. **La divergencia nació
en el bootstrap.** Sin la marca, la primera regeneración habría puesto al agente a contestar en otro
idioma del que se le pregunta.

**Ese caso es el que acabó invirtiendo el default, y tardó de más en hacerlo.** Cuando un adoptante
tiene que marcar el bloque el día que lo genera, lo que el dato dice no es *"acuérdate de marcar"*
sino *"la marca describe el caso normal"*. Se leyó como un aviso durante varias sesiones antes de
leerse como lo que era: la refutación del default.

Así que el eje no es el **origen** del contenido —preexistente contra generado— sino la **divergencia
respecto de la plantilla**, venga de donde venga. La plantilla siempre lo dijo bien (*"si este bloque
se enriqueció con reglas propias que la plantilla base no contiene"*); **fue esta prosa la que tradujo
esa condición a una historia sobre archivos anteriores a la adopción**, y una vez contada así, el
adoptante que generó de cero se da por excluido y no vuelve a mirar. Es la clase de error que no
contradice a la regla: la estrecha.

La marca vive en el bloque y no en el manifiesto **a propósito**: el dato viaja con la cosa que
describe y lo lee el agente en el momento exacto en que iba a sobrescribir.

**Lo que la marca no hace:** decir *qué* falta. Marca un bloque como no reescribible, pero portar el
delta sigue exigiendo comparar la plantilla nueva contra el bloque a mano. Convierte una regeneración
automática en una **comparación manual** — que es lo correcto, pero cuesta, y conviene no venderla como
gratis. Escrito como prosa en el manifiesto ya falló en campo — un proyecto adoptado tenía justo esa
nota, la fila del loader disparó igual, y lo que evitó la pérdida fue que el agente leyera y decidiera,
no el mecanismo. Un mecanismo que depende de que alguien recuerde una nota en otro archivo no es un
mecanismo.

**Una regla que gobierna el actualizar no gobierna la actualización que la entrega.** Quien actualiza
sigue el ritual del kit **viejo**: cuando clasifica el diff, el kit nuevo todavía no está aplicado. Así
que **cualquier fila que se añada a esta tabla es invisible durante la actualización que la introduce**,
y solo empieza a servir a partir de la siguiente. No es un defecto de una fila concreta: es una
propiedad del mecanismo, y vale para todas las que hay aquí.

Por eso la fase 3 manda **leer todo archivo nuevo del diff**, esté o no en la tabla. Esa regla también
llegó tarde una vez —no hay forma de evitarlo, nada puede gobernar su propia entrega—, pero al ser
**genérica** solo llega tarde **esa** vez: después cubre cualquier pieza futura sin necesidad de una
fila por cada una. Una fila por feature, en cambio, llega tarde siempre.

Detectado en campo: un proyecto recibió el buzón y su agente lo leyó, pero **no por la fila** —que no
existía en su kit— sino porque `diff -rq` imprimió `Only in <temporal>: buzon.md` y la fase de
clasificar le llevó a abrirlo. **Funcionó porque el diff obliga a mirar**, que es más robusto que
cualquier fila: no depende de que el destinatario ya tenga la versión que se lo dice.

**El canal de bajada no es maquinaria: es esta fila.** ACTUALIZAR ya se trae el árbol entero del kit,
así que **si el kit lleva un buzón, las cartas bajan con la actualización** — sin red, sin API y sin
credenciales, que es lo que mantiene el marco sin runtime. La subida no tiene equivalente: necesita un
cartero humano, y por eso el marco **estandariza la carta y nunca el canal**. Y esta revisión **no va
en ABRIR** aunque tiente: mirar el buzón en cada sesión rompería el arranque barato, que es un pilar.

**Si el diff muestra cambios que no vienen de arriba sino de ediciones locales del kit, para y
avisa**: el kit no se edita dentro de un proyecto (para eso está la config), y re-vendorizar los
borra. Recupéralos o descártalos con el usuario antes de seguir, nunca en silencio.

**Sin marcador de versión, a propósito.** El kit no lleva `VERSION` ni changelog: el diff dice *qué*
cambió y dónde, que es lo único accionable, y un número habría que acordarse de subirlo en cada
cambio. El porqué, en `guide.md` → "Alternativas descartadas".

**Pero el diff tiene un punto ciego, y son los renombrados.** Un directorio que cambia de nombre
aparece como N borrados y N añadidos: el vínculo semántico —*esto se llamaba así*— **no está en el
diff**. Por eso los renombrados llevan su fila en la tabla de zonas de arriba, que es lo único que se
lee justo cuando el cambio aterriza.

**Y por eso el kit no rompe nombres en campo: los deja como alias permanentes.** El valor viejo de un
manifiesto sigue siendo válido indefinidamente y nadie tiene que leer nada para no romperse. Los dos
precedentes vivos son `RICO` (sesión 57) y `módulos = software` (sesión 58).

**El aviso NO va al buzón, y la razón es su propia regla escrita.** El buzón **se poda** —una entrada
resuelta se retira— y solo contiene *el ahora*: un adoptante que se salta tres actualizaciones recibe
el mismo buzón que uno que se salta una, sin lo que había en medio. Un changelog sirve porque lo lees
**entero desde donde estás**, y aquí no hay ni "desde dónde" (no hay versión) ni "entre" (se podó).

> **Un aviso cuya omisión rompe algo no va en un archivo que se poda.** El buzón lleva el *porqué*,
> dirigido a la persona; la instrucción va donde está parado el lector en el momento que importa. Y si
> la instrucción hace falta para no romperse, el diseño está mal: **arréglalo con un alias, no con un
> aviso.**
