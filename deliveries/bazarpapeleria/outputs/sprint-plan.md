# Sprint 1 — Cotizar y confirmar el precio antes de imprimir

**Sprint Goal:** que la propietaria pueda cotizar el precio de una impresión **antes**
de ejecutarla y que el cliente confirme o desista antes de que se gaste insumo.

**Capacidad:** 20 pts · **Comprometido:** 15 pts

Se seleccionan, en orden de prioridad por valor, solo historias que cumplen la
Definition of Ready. Entra el **núcleo completo del MVP** (E-01 + E-02) y cabe
holgado en la capacidad. El backlog posterior (E-03 · US-06…US-09) queda fuera,
como declara el MVP Canvas.

| Historia | Pts | Épica | Prioridad |
|----------|-----|-------|-----------|
| US-03 · configurar tarifa | 2 | E-02 | 5 |
| US-02 · contar páginas sin imprimir | 5 | E-02 | 4 |
| US-01 · calcular precio antes de imprimir | 3 | E-01 | 1 |
| US-04 · mostrar precio y confirmar | 3 | E-01 | 2 |
| US-05 · el cliente conoce el precio | 2 | E-01 | 3 |

**Orden de construcción (por dependencias):** US-03 y US-02 (habilitadores, sin
dependencias) → US-01 (necesita US-02 y US-03) → US-04 (necesita US-01) → US-05
(necesita US-04). Al cierre, el flujo cotizar → confirmar → imprimir está completo.

**Fuera del sprint:** US-06, US-07, US-08, US-09 (E-03), aplazadas hasta validar que
el núcleo reduce el insumo perdido (supuesto abierto del MVP Canvas).
