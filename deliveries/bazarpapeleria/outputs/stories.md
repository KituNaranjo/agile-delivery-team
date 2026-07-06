# Historias refinadas (INVEST + Definition of Ready) — Bazar / Papelería

Cada historia cumple INVEST: independiente (dependencias resueltas y trazadas),
negociable, valiosa (el *para* es un outcome real), estimable, pequeña (≤ 8 pts) y
testeable (criterios Gherkin). Todas trazan a una user story y un requisito del
`inbox/`. Verificadas por el gate `dor-invest-gate.py` al escribir este archivo.

---

## Núcleo del MVP

### US-01 · Calcular el precio antes de imprimir   ·   épica E-01   ·   3 pts
**Como** Propietaria, **quiero** calcular el precio de una impresión a partir del
número de páginas y el tipo (blanco/negro o color) **antes** de imprimir, **para**
informarle el total al cliente sin haber gastado aún insumos.

Criterios de aceptación (Gherkin):
- Dado un documento con N páginas y un tipo de impresión, cuando ingreso esos datos, entonces el sistema muestra el precio total **antes** de ejecutar la impresión.
- Dado que aún no he impreso, cuando obtengo el precio, entonces no se ha consumido papel ni tinta.

Dependencias: US-02 (conteo de páginas), US-03 (tarifa configurada) · Origen: us:US-01 · R-01

### US-04 · Mostrar el precio y confirmar antes de imprimir   ·   épica E-01   ·   3 pts
**Como** Propietaria, **quiero** mostrarle al cliente el precio estimado antes de
imprimir y registrar su confirmación, **para** imprimir solo cuando el cliente ya
aceptó el costo.

Criterios de aceptación (Gherkin):
- Dado un precio calculado, cuando lo comunico al cliente, entonces el sistema ofrece confirmar o cancelar **antes** de imprimir.
- Dado que el cliente cancela, cuando registro la cancelación, entonces no se ejecuta la impresión y no se gasta insumo.
- Dado un precio mostrado, cuando confirmo o cancelo, entonces lo hago en un solo paso, sin agregar pasos adicionales al flujo de venta actual (R-09).

Dependencias: US-01 · Origen: us:US-04 · R-02

### US-05 · El cliente conoce el precio antes de imprimir   ·   épica E-01   ·   2 pts
**Como** Cliente de impresiones, **quiero** conocer el precio antes de que se
imprima el trabajo, **para** decidir si me conviene gastar ahí o buscar otra opción
sin sorpresas cuando ya está impreso.

Criterios de aceptación (Gherkin):
- Dado un trabajo solicitado, cuando lo entrego, entonces se me informa el precio estimado **antes** de imprimir y puedo aceptar o desistir.
- Dado un trabajo pequeño pero costoso (p. ej. una hoja con imagen a color), cuando pregunto, entonces el precio mostrado refleja ese costo antes de ejecutar.

Dependencias: US-04 · Origen: us:US-05 · R-02

### US-02 · Contar páginas sin imprimir   ·   épica E-02   ·   5 pts
**Como** Propietaria, **quiero** conocer cuántas páginas tiene el documento (desde
USB, celular o archivo) sin imprimirlo, **para** poder cotizar antes de ejecutar la
impresión.

Criterios de aceptación (Gherkin):
- Dado un archivo recibido, cuando lo cargo, entonces el sistema indica el número de páginas sin necesidad de imprimirlo.
- Dado un documento físico para copias, cuando ingreso la cantidad de páginas manualmente, entonces el conteo queda disponible para la cotización.

Dependencias: ninguna · Origen: us:US-02 · R-03
> Nota de refinamiento: la variedad de formatos (PDF, imágenes, ofimática desde
> celular/USB) motiva el ADR-0001; el conteo manual es el fallback siempre disponible.

### US-03 · Configurar la tarifa por tipo   ·   épica E-02   ·   2 pts
**Como** Propietaria, **quiero** configurar la tarifa por hoja según el tipo de
impresión (blanco/negro, color), **para** que el precio calculado use mis precios
reales y no un valor fijo.

Criterios de aceptación (Gherkin):
- Dado que defino una tarifa por tipo, cuando se calcula un precio, entonces usa la tarifa configurada y no un valor fijo.
- Dado que cambio una tarifa, cuando cotizo de nuevo, entonces el precio refleja el nuevo valor.

Dependencias: ninguna · Origen: us:US-03 · R-04

---

## Backlog posterior (refinado, fuera del sprint del MVP)

Estas historias cumplen la DoR pero el MVP Canvas las declara **fuera de alcance
inicial**; no entran al Sprint 1. Se refinan para tener el backlog listo cuando se
valide el núcleo.

### US-06 · Registrar trabajos cancelados   ·   épica E-03   ·   3 pts
**Como** Propietaria, **quiero** registrar los trabajos de impresión cancelados/no
cobrados, **para** dejar rastro de la pérdida que hoy no anoto en ningún lado.

Criterios de aceptación (Gherkin):
- Dado un trabajo cancelado tras cotizar, cuando lo registro, entonces quedan anotados el número de páginas, el tipo de impresión y la fecha del trabajo cancelado.

Dependencias: US-04 · Origen: us:US-06 · R-06

### US-07 · Separar ventas de impresiones y papelería   ·   épica E-03   ·   3 pts
**Como** Propietaria, **quiero** registrar las ventas de impresiones por separado de
las de papelería, **para** distinguir de dónde viene mi ingreso.

Criterios de aceptación (Gherkin):
- Dado un cobro, cuando lo registro, entonces queda clasificado como impresión o como producto de papelería.

Dependencias: ninguna · Origen: us:US-07 · R-05

### US-08 · Reporte de pérdida por periodo   ·   épica E-03   ·   5 pts
**Como** Propietaria, **quiero** un reporte por semana/mes de cuánto perdí en
impresiones canceladas y con qué frecuencia, **para** dimensionar el problema que
hoy solo percibo.

Criterios de aceptación (Gherkin):
- Dado un periodo, cuando solicito el reporte, entonces muestra el número de trabajos cancelados y el total de páginas perdidas en ese periodo.

Dependencias: US-06 · Origen: us:US-08 · R-07

### US-09 · Registrar costo de insumos   ·   épica E-03   ·   5 pts
**Como** Propietaria, **quiero** registrar el costo de insumos (papel, tinta),
**para** estimar el costo real por impresión en lugar de calcularlo de memoria.

Criterios de aceptación (Gherkin):
- Dado el costo de una resma y de un cartucho, cuando configuro el rendimiento, entonces el sistema estima el costo de insumo por página.

Dependencias: ninguna · Origen: us:US-09 · R-08

---

**Resumen:** 9 historias, todas listas (DoR). Núcleo del MVP (US-01…US-05) = 15 pts.
Backlog posterior (US-06…US-09) = 16 pts. Total = 31 pts.
