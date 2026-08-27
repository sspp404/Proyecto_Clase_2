# Corrida 2 — Región M · corte 14/08/2026

**Tipo:** incremental. Arranca del punto de control de la corrida 1.
**User prompt:** `OK`

> Datos sintéticos. Ver `README.md` → "Nota de anonimización".

---

## Entrada (fragmento del chat, solo lo posterior al 07/08)

```
[09/08/2026 10:22] Ana: ‹audio 00:51› ...pasé por Arenas, la de la
esquina de la plaza, y me dijo que este año no va a ir a votar, que ya
está grande para esas cosas.
[10/08/2026 16:05] Beto: Quiroga (el de La Serena) me pidió que le
avisemos la fecha apenas se confirme.
[11/08/2026 21:38] Carla: ‹audio 01:04› ...Molina me dijo que va con
nosotros. El de la fila 133, el que tiene el taller.
[12/08/2026 09:14] Darío: Confirmo lo de Solari, hablé yo directamente.
Va con Consenso.
[12/08/2026 19:50] Ana: Falleció Otamendi el padre. La ficha de la
sociedad sigue igual, el que firma es el hijo.
[13/08/2026 08:40] Beto: Vidal pasa a verde, me lo confirmó por teléfono.
[13/08/2026 20:11] Carla: Los dos amarillos de la calle 12 (Paz y
Ferreyra) dijeron que se deciden la semana que viene.
```

## Salida — JSON

```json
{
  "region": "M",
  "corte": "2026-08-14",
  "corrida": 2,
  "fuentes": {
    "mensajes_analizados": 128,
    "audios_transcriptos": 31,
    "padron_filas": 214,
    "vista_filtrada": false
  },
  "cambios": [
    {
      "fila": 55,
      "ficha": "ARENAS, NÉLIDA",
      "hoy": "AMARILLO",
      "poner": "NO VOTA",
      "confianza": "firme",
      "cita": "me dijo que este año no va a ir a votar, que ya está grande para esas cosas",
      "autor": "Ana",
      "fecha": "09/08 10:22",
      "afecta_universo": true
    },
    {
      "fila": 133,
      "ficha": "MOLINA, RUBÉN",
      "hoy": "SIN CLASIFICAR",
      "poner": "VERDE",
      "confianza": "firme",
      "cita": "Molina me dijo que va con nosotros. El de la fila 133, el que tiene el taller",
      "autor": "Carla",
      "fecha": "11/08 21:38",
      "afecta_universo": false
    },
    {
      "fila": 104,
      "ficha": "SOLARI, MARÍA INÉS",
      "hoy": "AMARILLO",
      "poner": "ROJO",
      "confianza": "firme",
      "cita": "Confirmo lo de Solari, hablé yo directamente. Va con Consenso",
      "autor": "Darío",
      "fecha": "12/08 09:14",
      "afecta_universo": false
    },
    {
      "fila": 71,
      "ficha": "VIDAL, ERNESTO",
      "hoy": "SIN CLASIFICAR",
      "poner": "VERDE",
      "confianza": "firme",
      "cita": "Vidal pasa a verde, me lo confirmó por teléfono",
      "autor": "Beto",
      "fecha": "13/08 08:40",
      "afecta_universo": false
    },
    {
      "fila": 12,
      "ficha": "QUIROGA, MARTÍN",
      "hoy": "SIN CLASIFICAR",
      "poner": "VERDE",
      "confianza": "firme",
      "cita": "me pidió que le avisemos la fecha apenas se confirme",
      "autor": "Beto",
      "fecha": "10/08 16:05",
      "afecta_universo": false
    },
    {
      "fila": 201,
      "ficha": "OTAMENDI, JOSÉ LUIS",
      "hoy": "AMARILLO",
      "poner": "FALLECIDO",
      "confianza": "firme",
      "cita": "Falleció Otamendi el padre. La ficha de la sociedad sigue igual, el que firma es el hijo",
      "autor": "Ana",
      "fecha": "12/08 19:50",
      "afecta_universo": true
    }
  ],
  "inconsistencias": [
    {
      "tipo": "contradiccion_padron_chat",
      "detalle": "El padrón muestra a PAZ y a FERREYRA como AMARILLO desde el 22/07. El chat del 13/08 los confirma indefinidos, así que el estado es correcto, pero llevan tres semanas sin movimiento: entran en el listado de amarillos a llamar.",
      "fichas": ["fila 96 PAZ, SILVIA", "fila 97 FERREYRA, DANIEL"]
    }
  ],
  "casos_abiertos": [
    {
      "fichas": ["fila 190 SERVICIOS DEL VALLE S.A."],
      "estado_hoy": "sin clasificar, sin firmante, sin teléfono",
      "que_falta": "el contacto",
      "preguntar_a": "quien tenga el legajo de la sociedad",
      "pregunta": "¿Alguien tiene contacto de SERVICIOS DEL VALLE S.A.? En el padrón no figura firmante ni teléfono.",
      "abierto_desde": "07/08"
    }
  ],
  "casos_cerrados_en_esta_corrida": [
    {
      "caso": "OTAMENDI HNOS. S.A. duplicado (filas 88 y 152)",
      "como_se_resolvio": "Carla confirmó que son dos membresías distintas, una del padre y otra de la sociedad. No es carga duplicada.",
      "quien_lo_cerro": "Carla, 10/08"
    }
  ],
  "conteos": {
    "VERDE": 41,
    "ROJO": 22,
    "AMARILLO": 24,
    "SIN CLASIFICAR": 117,
    "FALLECIDO": 5,
    "RENUNCIA": 2,
    "NO VOTA": 2,
    "EN BLANCO": 1
  },
  "delta": {
    "VERDE": 3,
    "ROJO": 1,
    "AMARILLO": -2,
    "SIN CLASIFICAR": -4,
    "FALLECIDO": 1,
    "NO VOTA": 1
  },
  "objetivos": {
    "votos_en_juego": 204,
    "ideal": 103,
    "muy_bueno": 82,
    "piso": 61,
    "faltan_para_ideal": 62
  }
}
```

## Salida — informe

Se movieron 6 fichas desde el corte del 07/08, con saldo de +3 verdes.

**Cambios firmes**

| Fila | Ficha | Hoy | Poner | Por qué |
|---|---|---|---|---|
| 12 | QUIROGA, MARTÍN | SIN CLASIFICAR | VERDE | *"me pidió que le avisemos la fecha apenas se confirme"* — Beto, 10/08 16:05 |
| 133 | MOLINA, RUBÉN | SIN CLASIFICAR | VERDE | *"me dijo que va con nosotros"* — Carla, 11/08 21:38 (audio) |
| 71 | VIDAL, ERNESTO | SIN CLASIFICAR | VERDE | *"me lo confirmó por teléfono"* — Beto, 13/08 08:40 |
| 104 | SOLARI, MARÍA INÉS | AMARILLO | ROJO | *"hablé yo directamente. Va con Consenso"* — Darío, 12/08 09:14 |
| 55 | ARENAS, NÉLIDA | AMARILLO | NO VOTA | *"este año no va a ir a votar"* — Ana, 09/08 10:22 (audio) |
| 201 | OTAMENDI, JOSÉ LUIS | AMARILLO | FALLECIDO | *"Falleció Otamendi el padre"* — Ana, 12/08 19:50 |

⚠️ **Dos fichas salen del universo** (55 y 201). Los votos en juego bajan de 206
a 204 y los tres objetivos bajan con ellos: IDEAL 103 (era 104), MUY BUENO 82,
PISO 61 (era 62).

*Nota:* el caso 104 venía como "a verificar" del corte anterior y esta vez sí hay
fuente directa: Darío habló él mismo. Pasa a firme.

**A verificar antes de tocar** — ninguno en este corte.

**Confirmaciones que no requieren cambio** — ninguna en este corte.

**Inconsistencias**

- PAZ (fila 96) y FERREYRA (fila 97) llevan tres semanas en amarillo sin
  movimiento. El estado es correcto, pero entran al listado de llamados.

**Casos abiertos**

*Cerrado en este corte:* OTAMENDI HNOS. S.A. — Carla confirmó el 10/08 que son
dos membresías distintas, no una carga duplicada. No se vuelve a discutir.

*Para el grupo:*
> ¿Alguien tiene contacto de SERVICIOS DEL VALLE S.A.? En el padrón no figura
> firmante ni teléfono.
