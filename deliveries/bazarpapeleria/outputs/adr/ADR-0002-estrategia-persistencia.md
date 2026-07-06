# ADR-0002 · Persistencia local en un solo dispositivo, sin backend
**Estado:** aceptado
**Fecha:** 2026-07-01

## Contexto y fuerza
El sistema tiene **una sola operadora sin conocimientos técnicos** (R-11), debe ser
**simple** (R-09) y **rápido** (R-10), y hoy todo se lleva a mano en un cuaderno, en
un único punto de atención (personas.md · Propietaria; propietaria.md). Hay que
persistir las tarifas (US-03) y las decisiones de confirmación/cancelación (US-04, y
más adelante US-06…US-09), pero no hay evidencia de necesidad de múltiples
dispositivos, usuarios ni acceso remoto.

## Decisión
Persistir en **almacenamiento local del propio dispositivo** (archivo/DB embebida,
sin servidor, sin cuentas, sin conexión obligatoria). Los datos viven donde se
opera, igual que el cuaderno actual pero digital.

## Alternativas consideradas
- **Backend en la nube + cuenta de usuario** — descartada: introduce login,
  conectividad y administración que violan R-09/R-11 y no responden a ninguna
  necesidad evidenciada; encarece el MVP que aún debe validar su supuesto de negocio.
- **Hoja de cálculo compartida** — descartada: frágil para el flujo rápido de
  cotización y propensa a errores manuales que el MVP busca eliminar.

## Consecuencias
- Ganamos: cero fricción de setup, funciona sin internet, mínimo costo para permitir
  el experimento de negocio barato antes de invertir más (ver MVP Canvas · riesgos).
- Aceptamos: sin respaldo automático fuera del dispositivo (mitigable con exportación
  simple más adelante) y sin acceso multi-dispositivo. Reabrir esta decisión solo si
  aparece evidencia de esa necesidad.
