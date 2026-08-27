# Lote de prueba — 12 fichas anonimizadas

Datos reales de un padrón de socios, anonimizados antes de subirlos a este repo.
Nombres reemplazados por `Contacto NN`, teléfonos por `XXX`, nombres de
voluntarios por `Voluntario N`. Se conservó intacto todo lo demás: el texto de
las menciones, la suciedad de los datos y la estructura de las columnas, porque
es exactamente eso lo que pone a prueba el contrato.

---

## Fichas

Columnas: `fila | nombre | ASIGNACION | SEMAFORO | Categoría | Tipo de socio | Teléfono | Contactos / Firmantes | CONTACTADO | FECHA | OBSERVACIONES`

```
3 | Contacto 01 S.A. | VOLUNTARIO 6 |  | Activo | PERSONA JURÍDICA | XXX | PRESIDENTE: CONTACTO 01, PRESIDENTE | Presidente: CONTACTO 01, PRESIDENTE | SI | |
16 | Contacto 02 |  |  | Honorario | PERSONA FÍSICA | XXX | | | |
50 | Contacto 03 Hnos. y Cía. |  | NO VOTA | Activo | PERSONA JURÍDICA | XXX | SOCIO GERENTE: CONTACTO 03, SOCIO GERENTE 1 | SOCIO GERENTE: CONTACTO 03, SOCIO GERENTE 2 | | |
89 | Contacto 04 |  | AMARILLO | Activo | PERSONA FÍSICA | XXX | | | |
186 | Contacto 05 S.R.L. |  | AMARILLO | Activo | PERSONA JURÍDICA | XXX / texto suelto | GERENTE | | |
199 | Contacto 06 S.R.L. |  | AMARILLO | Activo | PERSONA JURÍDICA | XXX | | | |
278 | Contacto 07 S.A. |  |  | Activo | PERSONA JURÍDICA | XXX | CONTACTO 07, PRESIDENTE | PRESIDENTE: CONTACTO 07, PRESIDENTE | | |
305 | Contacto 08 |  | VERDE | Activo | PERSONA FÍSICA | XXX | | | |
325 | Contacto 09 S.C.A. |  | NO INFORMA VOTO | Activo | PERSONA JURÍDICA | XXX | ADMINISTRADOR: CONTACTO 09, ADMINISTRADOR | Apoderado: CONTACTO 09, APODERADO | | |
378 | Contacto 10 y Cía. S.A. |  | AMARILLO | Activo | PERSONA JURÍDICA | XXX / texto suelto | PRESIDENTE: CONTACTO 10, PRESIDENTE | CONTACTO 10, FIRMANTE 2 | | |
400 | Contacto 11 Amuleto S.A. |  |  | Activo | PERSONA JURÍDICA | XXX | CONTACTO 11 PERSONA 11 | PRESIDENTE: CONTACTO 11 PERSONA 11 | | |
492 | Contacto 12 |  |  | Honorario | PERSONA FÍSICA | XXX | | | |
```

---

## Menciones del chat

```
[Contacto 02] Voluntario 2: "siempre nos votaba, me daba el sobre pobre que hice
yo, pero yo creo que ya la última vez fue un despelote hace seis años... ya no sé
si vive este señor, está por la A, pero no sé si alguien lo contactó, quisiera
saber."

[Contacto 03] Voluntario 2: "El socio gerente y su sociedad no va votar hasta último
momento y no dijo por quien pero estimo es por Lista A"
[Contacto 03] Voluntario 3: "a mí me dijo que firmó el petitorio
pero iba x Lista B"
[Contacto 03] Voluntario 2, audio: "lo han cagado a pedos... Lo que está tratando
es de distraer, es así de simple."

[Contacto 04] Voluntario 1: "A Contacto 04 la llama Persona 04-B La conoce."
[Contacto 04] Voluntario 1, dos días después: "Contacto 04 positivo !!!"

[Contacto 05] Voluntario 1: "Están Contacto 05 es de Álvarez !! Rojo.
Qué clarividencia para el nombre !! El final del oficialismo!! Jajaja"

[Contacto 07] Voluntario 4: "El presidente ya voto me acaba de responder"
[Contacto 07] Voluntario 5: "El presidente no me aparece como socio"
[Contacto 07] Voluntario 2: "Es Contacto 07. Ya lo contacte. Es presidente de la
Sociedad"
[Contacto 07] Voluntario 2, audio, cuatro días después: "me dijo que todavía no
había votado, o sea que no le podemos poner nada porque me dijo todavía no voté,
tengo que hablar con la familia."

[Contacto 08] Voluntario 1: "Contacto 08 nos vota pero le salió que tiene deuda
se estaba fijando cuánto es"

[Contacto 09] Voluntario 1: "Recién hablé con Contacto 09. Muy atento pero no me
dijo a quien voto. Dudoso pero me quedó algo de duda"
[Contacto 09] Voluntario 2, audio: "acabo de hablar con el administrador y él me dice que el
hermano le había comentado que si pasaba por la sede iba a votar ahí... dijo que
se fue de vacaciones el hermano y que viene la semana que viene."

[Contacto 11] Voluntario 2: "Persona 11 no quiso responder a quien vota, yo diría
es rojo"

[Contacto 12] Voluntario 3: "Contacto 12, se inclina por seguir como está pero
todavía no voto"
[Contacto 12] Voluntario 3, audio: "lo estoy torturando un poco... me dice que él
está conforme con la actuación del presidente hasta ahora... lo más que puedo
llegar a sacar con la tortura que le estoy haciendo es que no vote."
```
