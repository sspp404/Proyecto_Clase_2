# User prompt — el pedido de cada corrida

> Lo que cambia entre una corrida y la siguiente. El system prompt no se toca.

---

## Forma larga (la que se usa la primera vez, o cuando algo se rompió)

```
Corré el corte de la Región M al <FECHA DE CORTE>.

Archivos en la carpeta de trabajo:
- padrón:  PADRON RM.html
- chat:    WhatsApp Chat - EQUIPO TERRITORIAL RM — HORIZONTE/_chat.txt
- audios:  la misma carpeta (transcribilos todos)
- memoria: casos-abiertos-RM.md

Analizá únicamente lo posterior a <ÚLTIMO MENSAJE DEL CORTE ANTERIOR>.
Si es la primera corrida de la región, leé todo y avisá que el informe
va a salir más largo.

Devolvé el JSON del esquema y, debajo, el informe en tabla.
Reescribí casos-abiertos-RM.md al terminar.
```

---

## Forma corta: el disparador `OK`

Una vez que la región tiene su `region.json` y su punto de control, **el user
prompt es literalmente dos letras**:

```
OK
```

Y el contrato lo interpreta como la corrida completa:

```
Al recibir "OK":
- abrí el punto de control de la región y tomá el último mensaje y el
  último audio analizados;
- parseá el padrón nuevo y diffealo contra el corte anterior;
- analizá solo el tramo de chat y los audios posteriores;
- devolvé el JSON + el informe, y adjuntá el PNG del estado y los dos PDF;
- cerrá el punto de control con el nuevo último mensaje.

No preguntes nada. No pidas confirmación. No propongas un plan. Corré.
```

**Casos en que falta algo — un `OK` nunca queda trabado:**

| Situación | Qué hace |
|---|---|
| Chat viejo pero audios al día | corre con los audios y lo avisa |
| Sin padrón nuevo | usa el último corte y lo aclara |
| Primera corrida de la región | lee todo y avisa que el informe sale más largo |
| Nada nuevo | una línea, y no regenera los archivos |

---

## Pedidos puntuales

```
estado
```
→ solo la tarea 4: el PNG del estado de la región.

```
listados
```
→ solo la tarea 5: los dos PDF, amarillos y sin clasificar, separados.

```
las dudas para pasar al grupo
```
→ los casos abiertos como texto plano listo para copiar a WhatsApp,
agrupado por persona responsable, con asteriscos para negrita.
