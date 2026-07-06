# ADR-0001 · Conteo de páginas por formato, con entrada manual como fallback
**Estado:** aceptado
**Fecha:** 2026-07-01

## Contexto y fuerza
US-02 (R-03) exige conocer el número de páginas **sin imprimir**, y los documentos
llegan en formatos heterogéneos: PDF, imágenes y archivos ofimáticos desde USB o
celular, además de documentos físicos para copiar (personas.md · Cliente de
impresiones; user-stories.md · US-02). Dos fuerzas chocan: obtener el conteo
automático de muchos formatos es costoso y frágil, pero **R-10 (velocidad)** y
**R-11 (usabilidad unipersonal)** prohíben que el flujo se trabe si un formato no se
puede leer.

## Decisión
El conteo automático se soporta para los formatos más comunes en el negocio
(PDF e imágenes = 1 página por archivo, como primer alcance), y **siempre** se
ofrece la **entrada manual del número de páginas** como camino garantizado. La
cotización nunca depende de que el archivo sea legible: si el formato no se
reconoce, la propietaria escribe el número y continúa.

## Alternativas consideradas
- **Solo conteo automático (todos los formatos)** — descartada: alarga el desarrollo
  y arriesga bloquear la atención ante un formato raro (viola R-10/R-11).
- **Solo entrada manual** — descartada como meta final: desaprovecha el caso común
  (archivos digitales) y añade fricción evitable; se conserva solo como fallback.
- **Convertir todo a PDF antes de contar** — descartada para el MVP: agrega un paso
  técnico contrario a R-09.

## Consecuencias
- Ganamos: flujo que nunca se bloquea (R-10/R-11) y alcance de desarrollo acotado.
- Aceptamos: en formatos ofimáticos el conteo puede ser manual al inicio; se puede
  ampliar la cobertura automática después sin cambiar el flujo (la UI ya contempla
  ambos caminos). Queda como open question medir cuánto pesa el conteo automático
  sobre el tiempo total de atención.
