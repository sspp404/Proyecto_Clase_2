# Datos sintéticos

Nada de lo que hay en esta carpeta es real.

- `padron-RM-muestra.csv` — 15 filas inventadas, con la misma estructura de
  columnas y los mismos estados que usa el contrato. El padrón real tiene 214
  filas y **no está en este repositorio, ni lo va a estar.**
- Los teléfonos usan el rango `5555-01xx`, que no corresponde a ninguna línea. El
  de la fila 97 está deliberadamente mal cargado —le falta la característica—
  para poder probar la detección de `telefono_roto`.
- Las filas 88 y 152 repiten la misma razón social a propósito, para probar la
  detección de `duplicado`. Las filas 71 y 72 comparten apellido a propósito,
  para probar que el agente **no elige** entre dos fichas homónimas.
- Los apellidos y las razones sociales son inventados. Cualquier coincidencia con
  personas o empresas existentes es casual.
- Las fechas son de agosto de 2026 y no corresponden a ningún calendario
  electoral real.

Ver `README.md` → "Nota de anonimización" para el mapeo completo.
