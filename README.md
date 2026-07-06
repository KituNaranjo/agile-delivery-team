# Agile Delivery Team

Un **equipo ágil de entrega potenciado por IA** que convierte el descubrimiento de
un producto (las salidas de un Discovery Agent) en un **plan de entrega listo para
construir**: épicas, backlog priorizado, historias de usuario refinadas,
arquitectura con ADRs y un Sprint Plan.

No escribe código de producción. **Planifica el delivery** con criterio ágil, y lo
hace bajo reglas verificables: cada artefacto debe trazar al descubrimiento y cada
historia debe cumplir INVEST + Definition of Ready antes de avanzar.

## ¿Qué es y para qué sirve?

El proyecto está constituido en [`CLAUDE.md`](CLAUDE.md) como un equipo de **cuatro
roles especializados**, cada uno definido como subagente en `.claude/agents/`:

| Rol | Responsabilidad | Pregunta guía |
|-----|-----------------|----------------|
| **Product Owner** (`product-owner`) | Descompone el MVP en épicas y ordena el backlog por valor. | *¿Estamos construyendo lo correcto?* |
| **Developer** (`developer`) | Refina historias hasta que cumplan INVEST y arma el plan del sprint. | *¿Cómo lo construimos bien?* |
| **Scrum Master** (`scrum-master`) | Vigila el flujo y la salud del backlog; custodia la Definition of Ready. | *¿Qué frena al equipo?* |
| **Architect** (`architect`) | Decide la estructura técnica y registra el porqué en ADRs. | *¿Qué estructura sostiene esto en el tiempo?* |

### Reglas no negociables

El equipo opera bajo las reglas definidas en `CLAUDE.md`:

1. **Cero invención.** Toda épica, historia, requisito o decisión traza a algo del
   `inbox/`. Lo que el descubrimiento no respalda se declara supuesto abierto, no hecho.
2. **Trazabilidad.** Cada épica cita los ítems del MVP que la originan; cada historia
   cita su requisito o user story; cada ADR cita la fuerza que lo motiva.
3. **INVEST + Definition of Ready es ley.** Una historia solo está "lista" si es
   Independiente, Negociable, Valiosa, Estimable, Pequeña y Testeable, con criterios
   de aceptación, estimación y sin preguntas abiertas bloqueantes. Un hook lo
   verifica y **bloquea** si algo no cumple.
4. **Valor antes que solución.** Se prioriza por valor (output → outcome → impact).
5. **Aislamiento.** Cada delivery vive en su propia carpeta bajo `deliveries/`.
6. **Idioma español** y formato definido en la skill de delivery.

## Estructura del repositorio

```
agile-delivery-team/
├── CLAUDE.md                     ← constitución del proyecto (roles y reglas)
├── README.md                    ← este documento
├── Preguntas_Unidad2_NaranjoChristian.pdf  ← reflexión de 1 página (3 preguntas)
├── docs/                        ← evidencia de que los gates funcionan
│   ├── EvidenciaGateDoR_NaranjoChristian.md
│   └── captura-1..3.png
├── .claude/                     ← el equipo y su gobernanza
│   ├── agents/                  ← los cuatro roles como subagentes
│   │   ├── product-owner.md · developer.md · scrum-master.md · architect.md
│   ├── commands/delivery/       ← los comandos del flujo de trabajo
│   │   ├── generate-epics.md · generate-stories.md · architecture.md
│   │   ├── sprint-plan.md · report.md
│   ├── hooks/
│   │   └── dor-invest-gate.py   ← gate de calidad (INVEST + DoR)
│   ├── scripts/
│   │   └── build-report.py      ← genera el reporte HTML (determinista)
│   ├── skills/delivery/
│   │   └── SKILL.md             ← formatos canónicos de cada artefacto
│   └── settings.json            ← cablea el hook como PreToolUse
└── deliveries/
    └── <nombre>/
        ├── inbox/               ← entradas: salidas del Discovery Agent (solo lectura)
        └── outputs/             ← lo que produce el equipo
```

### El `inbox/` (entradas)

Es el insumo de todo el flujo. Contiene las salidas del Discovery Agent y es de solo
lectura para el equipo:

- `mvp-canvas.md` · `user-stories.md` · `requisitos.md` · `personas.md` · `evidence-map.json`

Si el `inbox/` está vacío, el equipo pide copiar ahí esas salidas antes de continuar.

### El `outputs/` (productos)

Lo que el equipo genera para un delivery:

| Artefacto | Autor | Descripción |
|-----------|-------|-------------|
| `epics.md` | PO | Épicas legibles + diagrama Mermaid del backlog |
| `backlog.json` | PO | Backlog estructurado (lo leen el gate y el reporte) |
| `stories.md` | Developer | Historias refinadas — **custodiado por el gate** |
| `architecture.md` | Architect | Componentes (Mermaid) + justificación de valor |
| `adr/ADR-NNNN-*.md` | Architect | Decisiones de arquitectura (formato MADR) |
| `sprint-plan.md` / `.json` | SM / Developer | Sprint Plan — **custodiado por el gate** |
| `report.html` | `build-report.py` | Dashboard visual determinista del delivery |

## Flujo de trabajo

Cada paso es un comando que lanza al rol correspondiente. El insumo de todo es el
`inbox/`.

```
/delivery:generate-epics <delivery>    → PO: epics.md + backlog.json
/delivery:generate-stories <delivery>  → Developer+SM: stories.md   (pasa por el gate)
/delivery:architecture <delivery>      → Architect: architecture.md + ADRs
/delivery:sprint-plan <delivery>       → SM+Developer: sprint-plan.md (pasa por el gate)
/delivery:report <delivery>            → reporte HTML visual
```

Al escribir `stories.md` y `sprint-plan.md` se ejecuta el hook
`dor-invest-gate.py`: relee el `backlog.json` estructurado y **bloquea** la escritura
si alguna historia no cumple INVEST + Definition of Ready. La validación es sobre el
dato, no sobre la prosa: no se puede esquivar renombrando el artefacto.

## Caso de estudio: `bazarpapeleria`

Ejemplo real desarrollado de punta a punta. Parte del descubrimiento de una
**papelería con servicio de impresiones** atendida por una sola propietaria, donde
hoy se imprime *antes* de cotizar y se pierden papel y tinta cuando el cliente
rechaza el precio ya impreso. El MVP invierte el orden: **cotizar → confirmar →
imprimir**.

### Épicas (priorizadas por valor)

| Épica | Prioridad | Valor (outcome) |
|-------|-----------|-----------------|
| **E-01 · Cotizar y confirmar el precio antes de imprimir** | 1 | El cliente decide antes de que se gaste insumo; deja de ocurrir imprimir-antes-de-cotizar. |
| **E-02 · Preparar una cotización fiable y rápida** | 2 | Obtener el número de páginas sin imprimir y aplicar tarifas reales, para que el precio sea correcto y rápido. |
| **E-03 · Dar visibilidad a la pérdida y al ingreso** | 3 | Dimensionar la pérdida y separar ingresos. *Fuera del MVP inicial* (backlog posterior). |

### Historias

Núcleo del MVP (E-01 y E-02):

| ID | Historia | Épica | Pts |
|----|----------|-------|-----|
| US-01 | Calcular el precio antes de imprimir | E-01 | 3 |
| US-04 | Mostrar el precio y confirmar antes de imprimir | E-01 | 3 |
| US-05 | El cliente conoce el precio antes de imprimir | E-01 | 2 |
| US-02 | Contar páginas sin imprimir | E-02 | 5 |
| US-03 | Configurar la tarifa por tipo | E-02 | 2 |

Backlog posterior (E-03, refinado pero fuera del sprint del MVP): US-06 (registrar
cancelados), US-07 (separar ventas de impresiones y papelería), US-08 (reporte de
pérdida por periodo), US-09 (registrar costo de insumos).

### Sprint 1

> **Sprint Goal:** que la propietaria pueda cotizar el precio de una impresión
> **antes** de ejecutarla y que el cliente confirme o desista antes de que se gaste
> insumo.

**Capacidad:** 20 pts · **Comprometido:** 15 pts (US-03, US-02, US-01, US-04, US-05,
el núcleo completo del MVP). El backlog posterior queda fuera hasta validar el núcleo.

Los artefactos completos de este caso están en
[`deliveries/bazarpapeleria/outputs/`](deliveries/bazarpapeleria/outputs/):
épicas, backlog, historias, arquitectura, dos ADRs, el sprint plan y el reporte HTML.

## Evidencia: los gates funcionan

La carpeta [`docs/`](docs/) documenta, con capturas paso a paso, cómo el gate de
Definition of Ready / INVEST **bloquea** una historia floja (`exit=2` con los motivos
exactos) y la **deja pasar** solo tras corregirla a INVEST (`exit=0`):

- **[docs/EvidenciaGateDoR_NaranjoChristian.md](docs/EvidenciaGateDoR_NaranjoChristian.md)**

El guion reproducible de esa demostración también está en
[`deliveries/bazarpapeleria/outputs/demo-gate.md`](deliveries/bazarpapeleria/outputs/demo-gate.md).

Es la idea central del proyecto: no se confía en que el modelo declare una historia
"lista"; se **verifica de forma independiente** el dato estructurado y se bloquea el
avance si algo incumple. Gobernanza ejecutable.

## Reflexión

La reflexión de una página de la Unidad 2 —qué cambió al separar el trabajo en
cuatro roles, qué historia costó más dejar lista según INVEST y para qué serviría el
gate en un equipo real— está en la raíz del repositorio:

- **[Preguntas_Unidad2_NaranjoChristian.pdf](Preguntas_Unidad2_NaranjoChristian.pdf)**
