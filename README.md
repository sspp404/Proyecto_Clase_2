# Agente de análisis de padrón regional

Entrega 2 — Creación de Agentes de IA · MADE 2026 · UCEMA

---

## Qué construí

Un agente que lee el chat de WhatsApp de un equipo de trabajo territorial —texto
y notas de voz— y lo cruza contra el padrón de socios de una asociación civil
para decir, fila por fila, qué ficha cambia de estado y con qué cita textual se
respalda. Además marca inconsistencias, sostiene entre sesiones los casos que
quedaron sin resolver, y genera el estado de la región en PNG y los listados de
llamados en PDF.

Es para el coordinador de una región, que hoy hace ese cruce a mano y pierde en
el camino la mitad de lo que el equipo dijo en audios. El contrato está escrito
para ser **replicable**: cambiando seis parámetros corre en cualquier región.

> **Caso anonimizado.** La organización, las listas, las regiones, los nombres,
> las razones sociales y los teléfonos de este repositorio son ficticios. Ver
> [Nota de anonimización](#nota-de-anonimización) al final.

---

## Cómo se lo pedí

Los prompts van textuales y en orden, con los reemplazos de la tabla de
anonimización aplicados (por ejemplo, donde decía el código real de la región
ahora dice `región`).

### Los pedidos, en orden

**1.**
> necesito crear un agente que analice historial de chats y audios de otros
> grupos y el html de otros grupos. es decir instrucciones universales que sea
> replicables en las otras regiones.

**2.**
> carpeta de chat hay una sola, la de secundarios no va

**3.**
> dame un descargable para compartir la habilidad

**4.**
> quiero que una vez que este instalado el prompt, el usuario obtenga la
> actualizacion periodica cuando lo solicite solamente con escribir OK

### Las cuatro iteraciones

Cada una toca **una sola pieza** de las seis. Esa fue la regla de trabajo: si
tocaba dos, no sabía cuál había mejorado el resultado.

---

**Iteración 1 — el mandato estaba disperso**

| | |
|---|---|
| **Antes** | Las cinco tareas del agente estaban repartidas entre los pasos del flujo. |
| **Qué falló** | Hacía las dos primeras —cambios de estado, inconsistencias— y se olvidaba de las otras tres. Los casos abiertos y los entregables sólo salían si se los pedía explícitamente, y en una corrida larga eso no pasa nunca. |
| **Pieza tocada** | **TAREA.** Las cinco subieron al encabezado del contrato como mandato explícito, con el detalle abajo en secciones propias. |
| **Después** | Las cinco salen siempre. Además quedó escrito qué pedido dispara cuáles: "analizá el chat" devuelve 1, 2 y 3 en el mensaje; "estado" o "listados" devuelven la 4 o la 5 como archivo. |

---

**Iteración 2 — la memoria entre sesiones**

La que más cambió el resultado.

| | |
|---|---|
| **Antes** | El contrato decía *"escribí la pregunta concreta"* y nada más. |
| **Qué falló** | Cada sesión arrancaba de cero. Un caso de dos fichas con el mismo apellido se volvió a discutir tres veces en dos semanas, y en la segunda vuelta se resolvió distinto que en la primera — sin que nadie se diera cuenta de que ya se había discutido. |
| **Pieza tocada** | **CONTEXTO.** Un archivo `casos-abiertos-R<letra>.md` que se lee al empezar cada corrida y se reescribe al terminar, con formato fijo: fichas involucradas, estado actual, qué falta, a quién preguntarle, y la pregunta ya redactada para copiar y pegar. Los casos cerrados **no se borran**: quedan con fecha y con quién los cerró. |
| **Después** | Cero repreguntas. El caso homónimo de la corrida 3 se abre una vez y sigue abierto con la misma redacción hasta que alguien lo conteste. |

---

**Iteración 3 — el parámetro que no existía**

| | |
|---|---|
| **Antes** | La tabla de parámetros pedía una "carpeta de chats secundarios", y había una regla de precedencia entre chats. |
| **Qué falló** | No existe tal cosa: hay una sola carpeta por región. El agente pedía un dato inexistente y se frenaba en el paso 1, esperando una respuesta imposible. |
| **Pieza tocada** | **CONTEXTO.** Se eliminó la fila y la regla de precedencia. Todo el texto pasó de plural a singular —"el chat", "las notas de voz de ese chat"— incluida la línea de descripción que decide cuándo se activa la habilidad. |
| **Después** | Arranca sin preguntar de más. De paso se agregó al paso 1 una advertencia concreta: el nombre de la carpeta viene con acentos descompuestos, así que hay que resolverlo con `glob` y no con `cd` directo. |

---

**Iteración 4 — el disparador `OK`**

| | |
|---|---|
| **Antes** | Cada actualización requería reescribir el pedido completo: qué archivos, desde cuándo, qué devolver. |
| **Qué falló** | Al agregar *"con OK corré todo"* apareció una contradicción adentro del propio contrato: decía "preguntá los parámetros si no los tenés" y a la vez "un OK no pregunta nada". El agente se quedaba trabado en cuál de las dos reglas ganaba. |
| **Pieza tocada** | **RESTRICCIONES.** Los parámetros se preguntan una sola vez, en la primera corrida, y quedan en un `region.json`. Se agregó un punto de control por región con el último mensaje y el último audio analizados, para que cada `OK` arranque donde terminó el anterior. |
| **Después** | `OK` dispara la corrida completa sin preguntar nada. Probado contra material real: leyó 2.355 mensajes y detectó 485 audios; en la segunda corrida devolvió 0 mensajes nuevos y 0 audios nuevos, como corresponde. |

---

## Qué funciona

**Las tres corridas están en `corridas/`**, con la entrada, el JSON de salida y el
informe legible. Las tres devuelven el mismo esquema con todas las claves
presentes, que era el punto: poder comparar un corte contra el anterior sin leer
prosa.

| | Corrida 1 | Corrida 2 | Corrida 3 |
|---|---|---|---|
| Tipo | primera, lee todo | incremental | incremental |
| User prompt | forma larga | `OK` | `OK` |
| Cambios de estado | 4 | 6 | 1 |
| Inconsistencias | 2 | 1 | 2 |
| Confirmaciones sin cambio | 1 | 0 | 1 |
| Casos abiertos | 2 | 1 | 3 |
| Casos cerrados | 0 | 1 | 1 |

**Lo que se probó y anduvo:**

- **El JSON sale siempre completo.** Incluso cuando una sección está vacía, la
  clave está y va con `[]`. Sin eso el diff entre cortes se rompía.
- **La abstención funciona.** En la corrida 3 el chat decía *"Alsina nos vota, ya
  está confirmado"* y en el padrón hay dos Alsina. El agente no eligió: dejó las
  dos sin clasificar y abrió el caso con la pregunta redactada. Es la restricción
  que más veces salvó el padrón.
- **Los informantes que se contradicen no se promedian.** Corrida 3, ficha
  Ferreyra: dos versiones del mismo día, cinco horas de diferencia. La ficha
  queda como está y las dos versiones van al informe con fecha y autor.
- **El universo se recalcula solo.** Cuando una ficha pasa a fallecido, renuncia,
  no vota o en blanco, los tres objetivos bajan y el informe lo dice
  explícitamente. Entre la corrida 1 y la 2 el objetivo IDEAL bajó de 104 a 103
  por dos fichas que salieron.
- **`OK` como user prompt completo.** Dos letras disparan la corrida entera.

**Cómo se usa:** se instala la habilidad, se define la región una sola vez, y a
partir de ahí se escribe `OK` cada vez que se quiere el corte actualizado.

---

## Qué falta o qué falló

- **La transcripción es el cuello de botella.** 485 audios tardan más que todo el
  resto junto. Probé bajar el modelo de Whisper y los apellidos empiezan a salir
  mal, que es exactamente lo que no se puede fallar acá. Sin resolver.
- **Los apodos siguen siendo manuales.** El equipo nombra gente por sobrenombre y
  el padrón tiene nombres legales. La técnica que destraba —buscar el nombre del
  chat en la columna de firmantes— está escrita en el contrato pero no
  automatizada; sigue dependiendo de que alguien la aplique.
- **El PNG se rompía en silencio.** Si el HTML se genera desde un f-string de
  Python y las llaves del CSS no van dobles, el navegador descarta los estilos sin
  avisar: el archivo sale, pero sin encabezado, sin colores y sin bordes. Perdí un
  buen rato antes de entender que el problema no era el diseño sino las llaves.
  Quedó anotado en el contrato porque es el error que más se repite.
- **No hay validación automática del JSON contra el esquema.** Hoy la
  verificación es visual. Si una corrida devuelve una clave de menos, me entero
  cuando falla el diff, no cuando pasa.
- **Está probado con un solo modelo.** La clase decía diseñar con el grande y
  operar con el chico; no llegué a probar si un modelo liviano sostiene la
  restricción de no elegir entre homónimos, que es la más frágil de todas.
- **La anonimización fue trabajo extra no previsto.** Al preparar esta entrega
  encontré que los nombres reales no estaban solamente en los datos: estaban
  incrustados en los prompts, en los ejemplos del contrato, en los nombres de
  archivo y hasta en el glosario de contexto que se le pasa al transcriptor —una
  línea de código que nombraba el sector entero. Ver más abajo.

---

## Qué aprendí

Lo que separó la primera versión de la última no fue el modelo ni el código: fue
darme cuenta de qué pieza faltaba cada vez. Las cuatro iteraciones tocaron tres
piezas distintas —tarea, contexto, restricciones— y en ninguna hubo que
reescribir el contrato entero.

La pieza que más me sorprendió fue el contexto, y en particular la memoria. Yo
creía tener un problema de instrucciones y tenía un problema de estado: el agente
razonaba bien cada vez y arrancaba amnésico cada vez, así que el equipo repetía
discusiones ya saldadas. Escribir dónde vive lo que hay que recordar valió más
que cualquier ajuste de redacción.

También aprendí que la restricción más valiosa es la que dice **cuándo no
actuar**. "Si hay dos fichas con el mismo apellido y el chat no distingue, no
elijas" parece una limitación y es lo contrario: un verde puesto sobre la ficha
equivocada hace que nadie vuelva a llamar a esa persona, y eso no se detecta
hasta el día de la elección.

Lo último lo aprendí preparando esta entrega. Tuve que separar el contrato de los
datos para poder publicarlo, y ahí vi cuánto caso concreto se me había filtrado
adentro de las instrucciones: nombres propios en los ejemplos, códigos reales en
las rutas, el sector nombrado en un glosario. Un contrato bien escrito tendría
que poder mostrarse sin mostrar a nadie. El mío no podía, y arreglarlo lo dejó
mejor de lo que estaba.

---

## Nota de anonimización

Este agente trabaja con **intención de voto de personas identificables**, que es
de los datos más sensibles que existen. Nada de lo que hay acá es real. La
estructura del problema y las iteraciones sí lo son; los datos, los nombres y la
organización, no.

| Lo real | Lo publicado |
|---|---|
| La organización y su sector | "una asociación civil de socios", sin nombre ni rubro |
| Las dos listas | Lista Horizonte · Lista Consenso |
| El código de la región y la palabra que la designa | Región M (`R<letra>`) |
| El nombre del grupo de WhatsApp | EQUIPO TERRITORIAL RM — HORIZONTE |
| Apellidos y razones sociales de socios | Alsina, Rivas, Quiroga, Solari, Campos, Leiva, Arenas, Molina, Paz, Ferreyra, Vidal, Otamendi · La Serena S.R.L., Servicios del Valle S.A. |
| Integrantes del equipo | Ana, Beto, Carla, Darío |
| Teléfonos | rango `5555-01xx`, sin línea asignada |
| El glosario de contexto del transcriptor | reescrito sin mención al sector |

**Lo que no está en este repositorio y no va a estar:** el padrón real, el export
del chat, los audios, las transcripciones y el archivo de casos abiertos. La
muestra de `datos-sinteticos/` tiene 15 filas inventadas.

Los apellidos y las razones sociales fueron elegidos sin relación con los
originales. Cualquier coincidencia con personas o empresas existentes es casual.

---

## Estructura del repositorio

```
README.md                          este archivo
prompts/
  system-prompt.md                 el contrato: las seis piezas, identidad estable
  user-prompt.md                   el pedido de cada corrida, y el disparador OK
formato-salida/
  output-schema.json               el esquema del JSON que devuelve toda corrida
corridas/
  corrida-1.md                     primera corrida: lee el histórico completo
  corrida-2.md                     incremental, disparada con OK
  corrida-3.md                     incremental: las tres veces que se abstiene
datos-sinteticos/
  padron-RM-muestra.csv            15 filas inventadas
  LEEME.md                         qué es sintético y por qué
```
