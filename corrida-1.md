# Corrida 1 — Región M · corte 07/08/2026

**Tipo:** primera corrida de la región. Lee el histórico completo.
**User prompt:** forma larga (no hay punto de control todavía).

> Datos sintéticos. Ver `README.md` → "Nota de anonimización".

---

## Entrada (fragmento del chat)

```
[04/08/2026 19:41] Ana: Pasé por lo de Rivas, el de la calle 9. Dice que
nos acompaña, que ya lo hablamos el año pasado.
[04/08/2026 20:02] Beto: ‹audio 00:38› ...hablé con Quiroga, el firmante
de La Serena, me dijo que contemos con él sin problema.
[05/08/2026 08:15] Carla: Ojo que Otamendi Hnos figura dos veces en la
planilla, fila 88 y fila 152.
[05/08/2026 22:30] Darío: ‹audio 01:12› ...me dijeron que Solari este año
va con Consenso, que está muy enojado por lo de la cuota.
[06/08/2026 11:47] Ana: Campos falleció en junio, ya lo sabíamos pero
nunca lo cargamos.
[06/08/2026 18:20] Beto: Leiva me confirmó que sigue con nosotros, ya
estaba en verde igual.
```

## Salida — JSON

```json
{
  "region": "M",
  "corte": "2026-08-07",
  "corrida": 1,
  "fuentes": {
    "mensajes_analizados": 1842,
    "audios_transcriptos": 312,
    "padron_filas": 214,
    "vista_filtrada": false
  },
  "cambios": [
    {
      "fila": 17,
      "ficha": "RIVAS, JORGE ALBERTO",
      "hoy": "SIN CLASIFICAR",
      "poner": "VERDE",
      "confianza": "firme",
      "cita": "Dice que nos acompaña, que ya lo hablamos el año pasado",
      "autor": "Ana",
      "fecha": "04/08 19:41",
      "afecta_universo": false
    },
    {
      "fila": 43,
      "ficha": "LA SERENA S.R.L.",
      "hoy": "SIN CLASIFICAR",
      "poner": "VERDE",
      "confianza": "firme",
      "cita": "me dijo que contemos con él sin problema",
      "autor": "Beto",
      "fecha": "04/08 20:02",
      "afecta_universo": false
    },
    {
      "fila": 104,
      "ficha": "SOLARI, MARÍA INÉS",
      "hoy": "AMARILLO",
      "poner": "ROJO",
      "confianza": "a_verificar",
      "cita": "me dijeron que Solari este año va con Consenso",
      "autor": "Darío",
      "fecha": "05/08 22:30",
      "afecta_universo": false
    },
    {
      "fila": 61,
      "ficha": "CAMPOS, HÉCTOR",
      "hoy": "AMARILLO",
      "poner": "FALLECIDO",
      "confianza": "firme",
      "cita": "Campos falleció en junio, ya lo sabíamos pero nunca lo cargamos",
      "autor": "Ana",
      "fecha": "06/08 11:47",
      "afecta_universo": true
    },
    {
      "fila": 29,
      "ficha": "LEIVA, OSVALDO",
      "hoy": "VERDE",
      "poner": "VERDE",
      "confianza": "confirmacion_sin_cambio",
      "cita": "me confirmó que sigue con nosotros, ya estaba en verde igual",
      "autor": "Beto",
      "fecha": "06/08 18:20",
      "afecta_universo": false
    }
  ],
  "inconsistencias": [
    {
      "tipo": "duplicado",
      "detalle": "La misma razón social aparece en dos filas con estados distintos (fila 88 AMARILLO, fila 152 SIN CLASIFICAR). Hay que decidir si son dos membresías o una carga repetida.",
      "fichas": ["fila 88 OTAMENDI HNOS. S.A.", "fila 152 OTAMENDI HNOS. S.A."]
    },
    {
      "tipo": "sociedad_sin_firmante",
      "detalle": "Sociedad sin firmante cargado y sin teléfono. No se sabe a quién llamar.",
      "fichas": ["fila 190 SERVICIOS DEL VALLE S.A."]
    }
  ],
  "casos_abiertos": [
    {
      "fichas": ["fila 88 OTAMENDI HNOS. S.A.", "fila 152 OTAMENDI HNOS. S.A."],
      "estado_hoy": "una amarilla, otra sin clasificar",
      "que_falta": "saber si son dos membresías distintas o una carga duplicada",
      "preguntar_a": "Carla",
      "pregunta": "OTAMENDI HNOS. S.A. figura en la fila 88 y en la 152. ¿Son dos socios distintos o está cargado dos veces? Si son dos, ¿quién firma cada uno?",
      "abierto_desde": "05/08"
    },
    {
      "fichas": ["fila 190 SERVICIOS DEL VALLE S.A."],
      "estado_hoy": "sin clasificar, sin firmante, sin teléfono",
      "que_falta": "el contacto",
      "preguntar_a": "quien tenga el legajo de la sociedad",
      "pregunta": "¿Alguien tiene contacto de SERVICIOS DEL VALLE S.A.? En el padrón no figura firmante ni teléfono.",
      "abierto_desde": "07/08"
    }
  ],
  "casos_cerrados_en_esta_corrida": [],
  "conteos": {
    "VERDE": 38,
    "ROJO": 21,
    "AMARILLO": 26,
    "SIN CLASIFICAR": 121,
    "FALLECIDO": 4,
    "RENUNCIA": 2,
    "NO VOTA": 1,
    "EN BLANCO": 1
  },
  "delta": {},
  "objetivos": {
    "votos_en_juego": 206,
    "ideal": 104,
    "muy_bueno": 82,
    "piso": 62,
    "faltan_para_ideal": 66
  }
}
```

## Salida — informe

Primera corrida de la Región M: se leyó el histórico completo, así que el
informe sale más largo de lo habitual.

**Cambios firmes**

| Fila | Ficha | Hoy | Poner | Por qué |
|---|---|---|---|---|
| 17 | RIVAS, JORGE ALBERTO | SIN CLASIFICAR | VERDE | *"Dice que nos acompaña, que ya lo hablamos el año pasado"* — Ana, 04/08 19:41 |
| 43 | LA SERENA S.R.L. | SIN CLASIFICAR | VERDE | *"me dijo que contemos con él sin problema"* — Beto, 04/08 20:02 (audio) |
| 61 | CAMPOS, HÉCTOR | AMARILLO | FALLECIDO | *"falleció en junio, ya lo sabíamos pero nunca lo cargamos"* — Ana, 06/08 11:47 |

⚠️ **La fila 61 sale del universo.** Los votos en juego bajan de 207 a 206 y los
tres objetivos bajan con ellos: IDEAL 104, MUY BUENO 82, PISO 62.

**A verificar antes de tocar**

| Fila | Ficha | Hoy | Poner | Por qué |
|---|---|---|---|---|
| 104 | SOLARI, MARÍA INÉS | AMARILLO | ROJO | *"me dijeron que Solari este año va con Consenso"* — Darío, 05/08 22:30 (audio). Es de tercera mano: Darío no habló con ella. |

**Confirmaciones que no requieren cambio**

| Fila | Ficha | Hoy | Poner | Por qué |
|---|---|---|---|---|
| 29 | LEIVA, OSVALDO | VERDE | VERDE | *"me confirmó que sigue con nosotros"* — Beto, 06/08 18:20 |

**Inconsistencias**

- **Duplicado.** OTAMENDI HNOS. S.A. en fila 88 (amarillo) y fila 152 (sin
  clasificar). Si es una carga repetida, el padrón tiene un voto de más.
- **Sociedad sin firmante.** Fila 190, SERVICIOS DEL VALLE S.A.: sin firmante y
  sin teléfono.

**Casos abiertos**

*Para Carla:*
> ¿OTAMENDI HNOS. S.A. son dos socios distintos o está cargado dos veces? Figura
> en la fila 88 y en la 152. Si son dos, ¿quién firma cada uno?

*Para el grupo:*
> ¿Alguien tiene contacto de SERVICIOS DEL VALLE S.A.? En el padrón no figura
> firmante ni teléfono.
