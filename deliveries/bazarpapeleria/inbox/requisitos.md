# Requisitos candidatos — Bazar / Papelería

Derivados de la evidencia. Cada requisito cita su origen. Lo que la evidencia no
respalda no se incluye.

## Funcionales

- **[R-01]** Estimar el precio de una impresión **antes** de ejecutarla, a partir
  de la cantidad de páginas y el tipo (blanco/negro o color).
  - Tipo: funcional
  - Origen: propietaria_seguimiento.md, propietaria.md · Propietaria

- **[R-02]** Comunicar/mostrar el precio estimado al cliente antes de imprimir,
  como paso explícito de cotización previa a la ejecución, para que el cliente
  pueda decidir si procede o no.
  - Tipo: funcional
  - Origen: propietaria_seguimiento.md, observador.md, cliente.md · Propietaria, Cliente de impresiones

- **[R-03]** Determinar el número de páginas del documento a imprimir (desde USB,
  celular o archivo) sin necesidad de imprimirlo primero.
  - Tipo: funcional
  - Origen: propietaria_seguimiento.md · Propietaria

- **[R-04]** Configurar y aplicar tarifas por hoja según el tipo de impresión
  (blanco/negro, color).
  - Tipo: funcional
  - Origen: propietaria.md, observador.md · Propietaria

- **[R-05]** Registrar las ventas de impresiones de forma separada de las ventas
  de productos de papelería.
  - Tipo: funcional
  - Origen: propietaria.md, observador.md · Propietaria

- **[R-06]** Registrar los trabajos de impresión cancelados/no cobrados, para
  dejar rastro de la pérdida que hoy no se anota en ningún lado.
  - Tipo: funcional
  - Origen: propietaria.md, propietaria_seguimiento.md · Propietaria

- **[R-07]** Reportar por periodo (semana/mes) cuánto se perdió en impresiones
  canceladas y con qué frecuencia ocurrió.
  - Tipo: funcional
  - Origen: propietaria_seguimiento.md · Propietaria

- **[R-08]** Registrar el costo de insumos (papel, tinta) para poder estimar el
  costo real por impresión. (Candidato secundario; hoy se maneja "de memoria".)
  - Tipo: funcional
  - Origen: propietaria_seguimiento.md · Propietaria

## No funcionales

- **[R-09]** Simplicidad: la solución no debe agregar pasos complicados al proceso
  de venta; debe permitir hacer lo mismo que hoy, solo en otro orden.
  - Tipo: no funcional
  - Origen: propietaria_seguimiento.md · Propietaria

- **[R-10]** Velocidad: la cotización debe poder hacerse rápido, sin alargar la
  atención, dado que la propietaria atiende sola y a veces con varios clientes y
  que el cliente valora la rapidez por encima de todo (no debe sacrificarse).
  - Tipo: no funcional
  - Origen: propietaria_seguimiento.md, observador.md, cliente.md · Propietaria, Cliente de impresiones

- **[R-11]** Usabilidad por una sola operadora, sin conocimientos técnicos ni
  apoyo adicional.
  - Tipo: no funcional
  - Origen: propietaria.md, propietaria_seguimiento.md · Propietaria
