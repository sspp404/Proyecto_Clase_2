# Corrida 3 — Región M · corte 21/08/2026

**Tipo:** incremental. Es la corrida que pone a prueba las restricciones: en este
corte el contrato tiene que **abstenerse** dos veces.
**User prompt:** `OK`

> Datos sintéticos. Ver `README.md` → "Nota de anonimización".

---

## Entrada (fragmento del chat, solo lo posterior al 14/08)

```
[16/08/2026 09:02] Ana: ‹audio 00:27› ...Alsina nos vota, ya está
confirmado.
[17/08/2026 14:33] Beto: Ferreyra me dijo que sí, que cuenta con
nosotros.
[17/08/2026 22:10] Carla: ‹audio 00:44› ...ojo con Ferreyra que a mí me
dijo que todavía no se decide, que lo llamemos más cerca de la fecha.
[18/08/2026 11:20] Darío: Paz confirmó, va con nosotros.
[19/08/2026 17:45] Ana: Conseguí el teléfono de Servicios del Valle, el
que firma es el contador. Se los paso por privado.
[20/08/2026 08:55] Beto: Sumamos a Quiroga el hermano, el que vive
afuera. Nos vota por poder.
```

## Salida — JSON

```json
{
  "region": "M",
  "corte": "2026-08-21",
  "corrida": 3,
  "fuentes": {
    "mensajes_analizados": 96,
    "audios_transcriptos": 22,
    "padron_filas": 214,
    "vista_filtrada": false
  },
  "cambios": [
    {
      "fila": 96,
      "ficha": "PAZ, SILVIA",
      "hoy": "AMARILLO",
      "poner": "VERDE",
      "confianza": "firme",
      "cita": "Paz confirmó, va con nosotros",
      "autor": "Darío",
      "fecha": "18/08 11:20",
      "afecta_universo": false
    },
    {
      "fila": 190,
      "ficha": "SERVICIOS DEL VALLE S.A.",
      "hoy": "SIN CLASIFICAR",
      "poner": "SIN CLASIFICAR",
      "confianza": "confirmacion_sin_cambio",
      "cita": "Conseguí el teléfono de Servicios del Valle, el que firma es el contador",
      "autor": "Ana",
      "fecha": "19/08 17:45",
      "afecta_universo": false
    }
  ],
  "inconsistencias": [
    {
      "tipo": "informantes_contradictorios",
      "detalle": "Beto la da por confirmada el 17/08 14:33; Carla dice cinco horas después que no se decide. No se promedia ni se elige: queda como está y va a caso abierto.",
      "fichas": ["fila 97 FERREYRA, DANIEL"]
    },
    {
      "tipo": "voto_inexistente",
      "detalle": "Se menciona un hermano de QUIROGA que votaría por poder. No hay ninguna ficha adicional a nombre de QUIROGA en el padrón más allá de la fila 12. Un voto por poder no agrega una ficha: o ya está en el padrón bajo otro nombre, o no existe.",
      "fichas": ["fila 12 QUIROGA, MARTÍN"]
    }
  ],
  "casos_abiertos": [
    {
      "fichas": ["fila 71 ALSINA, CARLOS MARÍA", "fila 72 ALSINA, RICARDO JERÓNIMO"],
      "estado_hoy": "las dos sin clasificar",
      "que_falta": "saber a cuál de los dos se refirió Ana",
      "preguntar_a": "Ana",
      "pregunta": "Cuando dijiste que Alsina nos vota, ¿era Carlos María o Ricardo Jerónimo? Están los dos en el padrón.",
      "abierto_desde": "16/08"
    },
    {
      "fichas": ["fila 97 FERREYRA, DANIEL"],
      "estado_hoy": "amarillo, sin tocar",
      "que_falta": "cuál de las dos versiones vale",
      "preguntar_a": "Beto y Carla",
      "pregunta": "Con Ferreyra tenemos dos lecturas del mismo día: Beto dice que confirmó y Carla que no se decide. ¿Quién habló último y qué le dijo exactamente?",
      "abierto_desde": "17/08"
    },
    {
      "fichas": ["fila 12 QUIROGA, MARTÍN"],
      "estado_hoy": "verde",
      "que_falta": "si el hermano tiene ficha propia en el padrón",
      "preguntar_a": "Beto",
      "pregunta": "Dijiste que el hermano de Quiroga nos vota por poder. ¿Está en el padrón con ficha propia? Buscándolo por apellido aparece solo Martín, en la fila 12.",
      "abierto_desde": "20/08"
    }
  ],
  "casos_cerrados_en_esta_corrida": [
    {
      "caso": "SERVICIOS DEL VALLE S.A. sin firmante ni teléfono (abierto desde el 07/08)",
      "como_se_resolvio": "Ana consiguió el contacto: firma el contador. Se carga el dato; la ficha sigue sin clasificar hasta que alguien hable con él.",
      "quien_lo_cerro": "Ana, 19/08"
    }
  ],
  "conteos": {
    "VERDE": 42,
    "ROJO": 22,
    "AMARILLO": 23,
    "SIN CLASIFICAR": 117,
    "FALLECIDO": 5,
    "RENUNCIA": 2,
    "NO VOTA": 2,
    "EN BLANCO": 1
  },
  "delta": {
    "VERDE": 1,
    "AMARILLO": -1
  },
  "objetivos": {
    "votos_en_juego": 204,
    "ideal": 103,
    "muy_bueno": 82,
    "piso": 61,
    "faltan_para_ideal": 61
  }
}
```

## Salida — informe

Corte corto: se movió **una sola ficha**. Lo importante de esta semana no son los
cambios sino **las tres cosas que el contrato decidió no tocar**.

**Cambios firmes**

| Fila | Ficha | Hoy | Poner | Por qué |
|---|---|---|---|---|
| 96 | PAZ, SILVIA | AMARILLO | VERDE | *"Paz confirmó, va con nosotros"* — Darío, 18/08 11:20 |

**A verificar antes de tocar** — ninguno.

**Confirmaciones que no requieren cambio**

| Fila | Ficha | Hoy | Poner | Por qué |
|---|---|---|---|---|
| 190 | SERVICIOS DEL VALLE S.A. | SIN CLASIFICAR | SIN CLASIFICAR | Se cargó el firmante y el teléfono. Que ahora se sepa a quién llamar no cambia el estado: nadie habló con él todavía. |

**Inconsistencias**

- **Informantes contradictorios — FERREYRA (fila 97).** Beto la da por confirmada
  el 17/08 a las 14:33; Carla dice a las 22:10 del mismo día que no se decide. No
  se promedia ni se elige la más reciente: la ficha **queda como está** y va a
  caso abierto con las dos versiones.
- **Voto que no existe en el padrón — QUIROGA.** Se menciona un hermano que
  votaría por poder. Buscando por apellido aparece solo Martín, fila 12. Un voto
  por poder no agrega una ficha.

**Casos abiertos**

*Para Ana:*
> Cuando dijiste que Alsina nos vota, ¿era Carlos María o Ricardo Jerónimo? Están
> los dos en el padrón.

*Para Beto y Carla:*
> Con Ferreyra tenemos dos lecturas del mismo día: Beto dice que confirmó y Carla
> que no se decide. ¿Quién habló último y qué le dijo exactamente?

*Para Beto:*
> Dijiste que el hermano de Quiroga nos vota por poder. ¿Está en el padrón con
> ficha propia? Buscándolo por apellido aparece solo Martín, en la fila 12.

*Cerrado en este corte:* SERVICIOS DEL VALLE S.A. — Ana consiguió el contacto el
19/08. Firma el contador.
