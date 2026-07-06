# User stories (INVEST) — Bazar / Papelería

Ancladas en la **Propietaria** (operadora del proceso) y en el **Cliente de
impresiones**, ambas con respaldo `primera mano` (propietaria.md /
propietaria_seguimiento.md y cliente.md). Cada historia cita su entrevista fuente
y deriva de un requisito candidato (`requisitos.md`). Lo que la evidencia no
respalda no se escribe.

> Las historias **[US-01]–[US-05]** forman el núcleo del MVP (ver `mvp-canvas.md`).
> Las **[US-06]–[US-09]** son backlog posterior (quedan fuera del alcance inicial).

## Núcleo del MVP

- **[US-01]** Como Propietaria, quiero calcular el precio de una impresión a partir
  del número de páginas y el tipo (blanco/negro o color) **antes** de imprimir,
  para informarle el total al cliente sin haber gastado aún insumos.
  - Criterios de aceptación:
    - Dado un documento con N páginas y un tipo de impresión, cuando ingreso esos
      datos, entonces el sistema muestra el precio total **antes** de ejecutar la impresión.
    - Dado que aún no he impreso, cuando obtengo el precio, entonces no se ha
      consumido papel ni tinta.
  - Fuente: propietaria_seguimiento.md, propietaria.md · R-01

- **[US-02]** Como Propietaria, quiero conocer cuántas páginas tiene el documento
  (desde USB, celular o archivo) sin imprimirlo, para poder cotizar antes de ejecutar.
  - Criterios de aceptación:
    - Dado un archivo recibido, cuando lo cargo, entonces el sistema indica el
      número de páginas sin necesidad de imprimirlo.
    - Dado un documento físico para copias, cuando ingreso la cantidad de páginas
      manualmente, entonces el conteo queda disponible para la cotización.
  - Fuente: propietaria_seguimiento.md · R-03

- **[US-03]** Como Propietaria, quiero configurar la tarifa por hoja según el tipo
  de impresión (blanco/negro, color), para que el precio calculado use mis precios reales.
  - Criterios de aceptación:
    - Dado que defino una tarifa por tipo, cuando se calcula un precio, entonces
      usa la tarifa configurada y no un valor fijo.
    - Dado que cambio una tarifa, cuando cotizo de nuevo, entonces el precio refleja
      el nuevo valor.
  - Fuente: propietaria.md, observador.md · R-04

- **[US-04]** Como Propietaria, quiero mostrarle al cliente el precio estimado
  antes de imprimir y registrar su confirmación, para imprimir solo cuando el
  cliente ya aceptó el costo.
  - Criterios de aceptación:
    - Dado un precio calculado, cuando lo comunico al cliente, entonces el sistema
      ofrece confirmar o cancelar **antes** de imprimir.
    - Dado que el cliente cancela, cuando registro la cancelación, entonces no se
      ejecuta la impresión y no se gasta insumo.
  - Fuente: propietaria_seguimiento.md, observador.md · R-02

- **[US-05]** Como Cliente de impresiones, quiero conocer el precio antes de que se
  imprima, para decidir si me conviene gastar ahí o buscar otra opción sin llevarme
  una sorpresa cuando ya está impreso.
  - Criterios de aceptación:
    - Dado un trabajo solicitado, cuando lo entrego, entonces se me informa el
      precio estimado **antes** de imprimir y puedo aceptar o desistir.
    - Dado un trabajo pequeño pero costoso (p. ej. una hoja con imagen a color),
      cuando pregunto, entonces el precio mostrado refleja ese costo antes de ejecutar.
  - Fuente: cliente.md · R-02

## Backlog posterior (fuera del MVP inicial)

- **[US-06]** Como Propietaria, quiero registrar los trabajos de impresión
  cancelados/no cobrados, para dejar rastro de la pérdida que hoy no anoto en ningún lado.
  - Criterios de aceptación:
    - Dado un trabajo cancelado tras cotizar, cuando lo registro, entonces queda
      anotado el insumo estimado consumido y la fecha.
  - Fuente: propietaria.md, propietaria_seguimiento.md · R-06

- **[US-07]** Como Propietaria, quiero registrar las ventas de impresiones por
  separado de las de papelería, para distinguir de dónde viene mi ingreso.
  - Criterios de aceptación:
    - Dado un cobro, cuando lo registro, entonces queda clasificado como impresión
      o como producto de papelería.
  - Fuente: propietaria.md, observador.md · R-05

- **[US-08]** Como Propietaria, quiero un reporte por semana/mes de cuánto perdí en
  impresiones canceladas y con qué frecuencia, para dimensionar el problema que hoy
  solo percibo.
  - Criterios de aceptación:
    - Dado un periodo, cuando solicito el reporte, entonces muestra el número de
      trabajos cancelados y el insumo estimado perdido en ese periodo.
  - Fuente: propietaria_seguimiento.md · R-07

- **[US-09]** Como Propietaria, quiero registrar el costo de insumos (papel, tinta),
  para estimar el costo real por impresión en lugar de calcularlo de memoria.
  - Criterios de aceptación:
    - Dado el costo de una resma y de un cartucho, cuando configuro el rendimiento,
      entonces el sistema estima el costo de insumo por página.
  - Fuente: propietaria_seguimiento.md · R-08

## Restricciones transversales (no funcionales)

Aplican a todas las historias del MVP:

- **Simplicidad** — no agregar pasos complicados; hacer lo mismo de hoy en otro
  orden (R-09 · propietaria_seguimiento.md).
- **Velocidad** — cotizar rápido, sin alargar la atención; la propietaria atiende
  sola y a veces con varios clientes, y el cliente valora la rapidez por encima de
  todo (R-10 · propietaria_seguimiento.md, observador.md, cliente.md).
- **Usabilidad unipersonal** — operable por una sola persona sin conocimientos
  técnicos (R-11 · propietaria.md, propietaria_seguimiento.md).
