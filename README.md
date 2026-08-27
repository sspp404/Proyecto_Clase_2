# Entrega 2 — Una instrucción repetible

**Creación de Agentes de IA · MADE UCEMA 2026 · Clase 2**

---

## La tarea que elegí

Clasificar fichas de un padrón de socios según su intención de voto en una
elección interna de una asociación civil.

Es una tarea que hago de verdad y que se repite: el padrón tiene varios cientos de fichas, se
re-exporta casi todos los días durante la campaña, y cada ronda mueve entre 5 y
32 fichas. La evidencia para clasificar no está en la planilla: está en un grupo
de WhatsApp donde un grupo de voluntarios reportan, en lenguaje coloquial y en
notas de voz, lo que les contestó cada socio.

Por eso es un buen caso: la entrada es sucia y no estructurada, la salida tiene
que ser un dato limpio y comparable entre corridas, y el criterio para decidir es
genuinamente discutible — que es exactamente donde un contrato mal escrito se
nota.

**Qué se hace después con la salida.** Vuelve a la columna `SEMAFORO` de la
planilla, y de ahí salen tres productos: el estado del padrón, los listados de
llamados repartidos por tramos, y el listado completo por estado.

**Una decisión de alcance.** El agente clasifica y funda; no cuenta. Los totales,
el padrón neto y los porcentajes salen de un script sobre el JSON, no del modelo.
Aritmética sobre cientos de filas es justamente donde un LLM deja de ser repetible, y la
repetibilidad es el punto de todo esto.

---

## Nota sobre los datos

Los datos de este repo están **anonimizados**: socios como `Contacto NN`,
teléfonos como `XXX`, voluntarios como `Voluntario N`, y las listas electorales
como `Lista A` y `Lista B`.

Son datos personales de miembros de una asociación, alcanzados por la la normativa de protección de datos personales.
Corrí el prompt con los datos reales; lo que subo acá es la versión anonimizada.
La suciedad de los datos y el texto literal de las menciones se conservaron
intactos, porque es eso lo que pone a prueba el contrato.

---

## El contrato

| Pieza | Dónde vive |
|---|---|
| 1 · Rol | system prompt |
| 2 · Contexto | system prompt |
| 3 · Tarea | user prompt |
| 4 · Restricciones | system prompt |
| 5 · Formato | system prompt |
| 6 · Ejemplos | system prompt |

**Por qué la división cae ahí.** El rol, el contexto, las nueve categorías del
semáforo, las reglas y el formato de salida no cambian nunca: son la identidad
del agente. Lo único que cambia en cada corrida es qué fichas se clasifican y qué
se dijo de ellas: eso va en el user prompt.

La salida es un array JSON, un objeto por ficha, con el estado propuesto, si
cambia respecto del actual, la cita textual que lo funda, quién lo reportó, el
nivel de confianza y las alertas de calidad de dato.

---

## Las tres corridas

Las tres usan **el mismo lote de 12 fichas**, a propósito: es la única forma de
atribuir un cambio en la salida al cambio en el contrato y no al cambio en los
datos. El lote no es una muestra al azar — son los doce casos que más problemas
dieron en el trabajo real: homonimia, sociedades donde el que atiende no es el
que vota, informantes que se contradicen, fichas sin ninguna mención.

| Corrida | Contrato | Qué salió |
|---|---|---|
| 1 | v1 | 4 fichas mal clasificadas, todas por el mismo motivo |
| 2 | v2 (restricciones nuevas) | las 4 corregidas, 1 regresión nueva |
| 3 | v3 (ejemplos nuevos) | formato corregido; la regresión **no** se arregló |

---

## Iteración 1 — RESTRICCIONES

### Qué falló en la corrida 1

Antes de correrla anoté dos fallas que esperaba. **Las dos no ocurrieron.** El
modelo resolvió bien la cronología de Contacto 07 y rechazó por su cuenta el
"yo diría es rojo" de Contacto 11. Lo que sí falló era otra cosa, y era una sola:

**el modelo trataba la deducción de un voluntario como si fuera lo que dijo el
socio.**

| Ficha | Devolvió | Debía devolver | Por qué |
|---|---|---|---|
| Contacto 05 | `ROJO` | `AMARILLO` | Lo pintó de rojo porque "es de Álvarez". Nadie lo contactó nunca |
| Contacto 12 | `ROJO` | `AMARILLO` | Simpatiza con el oficialismo pero **dice que todavía no votó** |
| Contacto 03 | `ROJO` | `NO VOTA` + alerta | Tres voluntarios reportan cosas incompatibles; eligió una en silencio |
| Contacto 04 | `AMARILLO` | `VERDE` | Al revés: se quedó corto ante un "positivo !!!" reportado por el equipo |

Aparecieron además dos errores míos, no del modelo:

- Puse a Contacto 08 como ejemplo **y** lo dejé en el lote: el modelo copió la
  salida textual, así que esa ficha no probó nada.
- Mi Ejemplo B afirmaba que Contacto 02 no tenía menciones, y sí las tiene. El
  modelo obedeció el ejemplo por encima de los datos, y arrastró la alerta
  `sin_firmante` a una persona física, donde no significa nada.

### Qué pieza toqué

Sólo **RESTRICCIONES**. Le agregué una jerarquía de evidencia en tres niveles:

1. **El socio lo dijo** — un voluntario reporta lo que le contestó.
2. **Un tercero lo transmite** — alguien cuenta lo que le contó quien sí habló.
3. **Inferencia u opinión** — deducción por parentesco, sociedad o impresión.

Los niveles 1 y 2 pueden fijar un semáforo. **El nivel 3 nunca cambia un
estado**: queda anotado como sospecha, con alerta. De ahí salieron dos reglas
derivadas —"inclinación no es voto" y el manejo de informantes en conflicto— y
dos reglas chicas nacidas de mis propios errores: que los ejemplos no son datos,
y que `sin_firmante` no aplica a personas físicas.

Los dos valores nuevos del vocabulario de `alertas` (`contradiccion`,
`inferencia_sin_confirmar`) son la consecuencia mecánica de esas reglas, no un
rediseño del formato.

### Qué cambió en la corrida 2

Las cuatro fichas se corrigieron. Contacto 05 y Contacto 12 bajaron a `AMARILLO`
con la sospecha registrada aparte; Contacto 03 se quedó en `NO VOTA` con las dos
citas y la alerta `contradiccion`; Contacto 04 subió a `VERDE` identificando bien
el nivel 2 ("Voluntario 1, transmite lo hablado por Persona 04-B").

Un detalle que no esperaba: en Contacto 03 el modelo agregó por su cuenta que
"ambas versiones indican que sí votaría, lo que no cuadra con el NO VOTA
cargado". Es una observación correcta que nadie del equipo había hecho en el chat
real.

---

## Iteración 2 — EJEMPLOS

### Qué falló en la corrida 2

**Rompí algo que antes andaba.** Contacto 07 lo resolvía bien en la v1: usaba el
mensaje de cuatro días después y descartaba el anterior. En la v2 lo marcó
`contradiccion` y lo congeló.

La regla de informantes en conflicto era demasiado ancha. Trata como conflicto lo
que en realidad es **el mismo voluntario corrigiendo su propio reporte**. "Ya
votó", seguido cuatro días después de "me dijo que todavía no voté", no es una
contradicción: es una actualización, y vale la última.

Quedó suelto también un detalle de formato: cuando hay dos citas del mismo
informante que **no** se contradicen, las pegaba con `/` porque nunca le dije qué
hacer en ese caso.

### Qué pieza toqué

Sólo **EJEMPLOS**. Reemplacé los dos ejemplos originales por tres nuevos, sobre
fichas inventadas que no están en el lote — el error que había cometido en la v1:

- **A** — el mismo informante se corrige días después → vale la mención más
  reciente, sin alerta.
- **B** — dos informantes distintos reportan lo mismo de forma incompatible →
  el estado no se mueve, van las dos citas y la alerta.
- **C** — dos menciones del mismo informante que se complementan → orden
  cronológico, separador propio, sin alerta.

**Por qué ejemplos y no otra restricción.** La diferencia entre "dos personas se
contradicen" y "una persona se corrige" es incómoda de escribir como regla —
tendría que enumerar casos y quedaría frágil— y es muy fácil de mostrar. Cuando
la regla empieza a retorcerse, conviene un ejemplo.

### Qué cambió en la corrida 3

A medias — y lo que no funcionó es más útil que lo que sí.

**Lo que se arregló.** El formato. Las citas múltiples del mismo informante
pasaron a ir con `+` en orden cronológico (Contacto 04, 09 y 12), como muestra el
Ejemplo C; antes las pegaba con `/` sin criterio. Además el modelo empezó a
completar `interlocutor` en fichas donde antes lo dejaba en `null` (Contacto 01).

**Lo que no.** Contacto 07 sigue marcado como `contradiccion` y congelado. La
iteración 2 apuntaba a esa regresión y no la tocó.

El motivo es un error mío de diagnóstico, no una falla del modelo. Mi Ejemplo A
muestra al **mismo** voluntario corrigiéndose a sí mismo. Pero en Contacto 07 los
dos mensajes son de **voluntarios distintos** — Voluntario 4 dice "ya votó",
Voluntario 2 dice cuatro días después "todavía no voté" — así que la regla de
contradicción se aplica al pie de la letra y el ejemplo no cubre el caso. El
modelo está cumpliendo el contrato tal como lo escribí. El que está mal es el
contrato.

Y hay algo que sólo se ve mirando las tres corridas juntas: en la v1 esta ficha
salía bien **por casualidad**. Ninguna regla le decía que priorizara lo más
reciente; lo hizo por su cuenta. Lo que la v2 rompió no era una conducta
diseñada, era una coincidencia — y yo la había anotado como acierto.

La v4 sería una regla de precedencia temporal que valga también entre informantes
distintos: cuando dos reportes se refieren a momentos distintos y el posterior es
una declaración directa del socio, gana el posterior. No la escribí porque no me
quedaba tiempo de correrla, y una regla sin una corrida que la pruebe es
exactamente lo que esta entrega sostiene que no hay que hacer.

---

## Reflexión

**El contrato no se escribe: se descubre corriéndolo.** Anoté dos fallas que
esperaba de la v1 y ninguna de las dos ocurrió; las que sí ocurrieron no las
había visto. Si hubiera entregado el prompt sin correrlo, habría entregado un
documento que parece bueno y que no sé si funciona.

**Un ejemplo malo enseña con más fuerza que una regla buena.** Mi Ejemplo B decía
que una ficha no tenía menciones, y sí las tenía. El modelo le creyó al ejemplo
por encima de los datos que tenía adelante. La pieza más subestimada también es
la más peligrosa.

**Arreglar una cosa rompe otra.** La regla que corrigió Contacto 03 rompió
Contacto 07. Si en esa vuelta hubiera cambiado tres cosas a la vez —que es lo que
uno hace cuando tiene apuro— no habría podido saber cuál de las tres fue.
Cambiar de a una pieza no es prolijidad: es lo único que convierte la prueba y
error en información.

**Un ejemplo sólo enseña el caso que muestra.** Escribí un ejemplo sobre un
informante que se corrige a sí mismo, esperando que el modelo generalizara a dos
informantes en momentos distintos. No generalizó, y hacía bien: yo le había dado
una regla explícita que decía lo contrario. Un ejemplo no derrota a una
restricción — la ilustra. Si querés cambiar la conducta, cambiá la regla.

**Un campo que no gobierna ninguna decisión es decoración.** `confianza` sigue
sin hacer nada: hay fichas que cambiaron de estado con confianza "baja" y fichas
que no cambiaron con "media". Lo dejo así a propósito y queda anotado como lo
pendiente: o se ata a una regla —por ejemplo, que nada con confianza baja pueda
entrar en el conteo que se reporta— o se saca.

**Lo que más me sirvió no fue automatizar.** Fue que escribir el contrato me
obligó a poner por escrito criterios que el equipo nunca había acordado. El caso
de "ya voté" sin decir a quién se discutió durante semanas en el chat, con cuatro
posiciones distintas, y nunca se cerró: cada uno clasificaba con el suyo. No
podés escribirle una restricción a un agente sin antes decidir qué pensás. El
agente no resolvió la ambigüedad: la hizo visible, que era el problema real.

---

## Estructura del repo

```
├── README.md
├── prompts/
│   ├── system_prompt_v1.md      ← contrato inicial
│   ├── system_prompt_v2.md      ← iteración 1: restricciones
│   ├── system_prompt_v3.md      ← iteración 2: ejemplos
│   └── user_prompt_v1.md        ← tarea + datos (no cambia entre corridas)
├── datos/
│   └── lote_anonimizado.md      ← las 12 fichas de prueba
└── corridas/
    ├── corrida_1.json
    ├── corrida_2.json
    └── corrida_3.json
```
