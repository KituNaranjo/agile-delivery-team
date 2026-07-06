# Demostración del gate de Definition of Ready / INVEST

Evidencia de que el equipo **no confía** en que el modelo declare una historia
"lista": el hook `dor-invest-gate.py` vuelve a leer el `backlog.json` estructurado y
**bloquea** la escritura de `stories.md` / `sprint-plan.md` si alguna historia
incumple INVEST + la Definition of Ready. Gobernanza ejecutable.

## Cómo funciona el gate

- Es un hook **PreToolUse** (matcher `Write|Edit`) declarado en `.claude/settings.json`.
- Se dispara **antes** de escribir un archivo custodiado: `stories.md` o `sprint-plan.md`.
- No valida la *prosa* del artefacto: relee el `backlog.json` del **mismo delivery**
  (lo deriva de la carpeta del archivo) y verifica cada historia.
- Salida: `exit 0` deja pasar · `exit 2` **BLOQUEA** y devuelve el motivo por stderr.

Reglas que verifica, por historia (ver `.claude/hooks/dor-invest-gate.py`):

| Chequeo | Regla |
|---|---|
| `as_a` / `want` / `so_that` no vacíos | formato + **V**aliosa |
| `acceptance_criteria` con ≥ 1 criterio | **T**esteable |
| `estimate` entero > 0 | **E**stimable |
| `estimate` ≤ `DELIVERY_MAX_POINTS` (8) | **S**mall / pequeña |
| `dependencies` apuntan a IDs existentes | **I**ndependiente / trazabilidad |
| `open_questions` vacío | Definition of Ready |
| `want` sin término vago sin criterio que lo aterrice | **T**esteable (anti "sistema rápido") |

> **Nota de entorno (Windows).** El hook en `settings.json` se invoca con `python3`,
> que en esta máquina es el alias de la Microsoft Store (no ejecuta el script; sale
> con error 49). Por eso, para reproducir el gate de forma fiable se ejecuta a mano
> con el lanzador `py`, que es el Python real del equipo. La lógica del gate es la
> misma; solo cambia cómo se lanza el intérprete.

## Reproducir la demostración (paso a paso)

Los archivos de prueba viven aislados en un directorio temporal para no tocar el
delivery real. `payload.json` simula el evento que Claude Code envía al hook por
stdin y apunta a un `stories.md` custodiado; el hook deriva el `backlog.json` de esa
misma carpeta.

`payload.json`:

```json
{
  "tool_name": "Write",
  "tool_input": { "file_path": "<carpeta-demo>/stories.md" }
}
```

### Paso 1 — Historia floja `US-DEMO` (campos vacíos)

Entrada en `backlog.json` que incumple varias reglas a la vez:

```json
{
  "id": "US-DEMO",
  "epic": "E-01",
  "as_a": "Propietaria",
  "want": "una herramienta donde todo sea rapido e intuitivo para manejar el negocio",
  "so_that": "",
  "acceptance_criteria": [],
  "estimate": 0,
  "dependencies": ["US-404"],
  "open_questions": ["Falta definir que abarca 'el negocio' y con que alcance."],
  "priority": 2,
  "origin": []
}
```

### Paso 2 — Ejecutar el gate → BLOQUEA (exit=2)

```bash
py .claude/hooks/dor-invest-gate.py < <carpeta-demo>/payload.json
echo "exit code = $?"
```

Salida real:

```
dor-invest-gate: BLOQUEADO — el backlog no está listo para el sprint.

  ✗ US-DEMO: falta 'para' (formato de historia incompleto).
  ✗ US-DEMO: sin criterios de aceptación (no es testeable).
  ✗ US-DEMO: sin estimación válida en puntos (no es estimable).
  ✗ US-DEMO: depende de 'US-404', que no existe en el backlog.
  ✗ US-DEMO: tiene preguntas abiertas sin resolver (no está 'ready').
  ✗ US-DEMO: usa un término vago ('una herramienta donde todo sea rapido e intuitivo para manejar el negocio') sin criterio medible (no es testeable).

Corrige las historias señaladas (no evadas el gate):
  · formato Como/Quiero/Para completo
  · al menos un criterio de aceptación verificable (testeable)
  · una estimación en puntos > 0 (estimable)
  · tamaño <= 8 pts; si es mayor, divídela (pequeña)
  · sin preguntas abiertas bloqueantes (Definition of Ready)

exit code = 2
```

Cada `✗` corresponde a una letra de INVEST o a la Definition of Ready.

### Paso 3 — Corregir `US-DEMO` a INVEST → PERMITE (exit=0)

Historia reescrita para cumplir todas las reglas:

```json
{
  "id": "US-DEMO",
  "epic": "E-01",
  "as_a": "Propietaria",
  "want": "configurar la tarifa por hoja segun el tipo de impresion (blanco/negro, color)",
  "so_that": "que el precio calculado use mis precios reales y no un valor fijo",
  "acceptance_criteria": [
    "Dado que defino una tarifa por tipo, cuando se calcula un precio, entonces usa la tarifa configurada y no un valor fijo.",
    "Dado que cambio una tarifa, cuando cotizo de nuevo, entonces el precio refleja el nuevo valor."
  ],
  "estimate": 2,
  "dependencies": [],
  "open_questions": [],
  "priority": 2,
  "origin": ["us:US-03", "req:R-04"]
}
```

Mismo hook, mismo payload:

```bash
py .claude/hooks/dor-invest-gate.py < <carpeta-demo>/payload.json
echo "exit code = $?"
```

Salida real (el gate guarda silencio cuando todo cumple):

```
exit code = 0
```

## Resumen

| Paso | Estado de `US-DEMO` | Gate |
|---|---|---|
| 1 | campos vacíos (floja) | — |
| 2 | sin cambios | **BLOQUEA · exit=2** + 6 motivos |
| 3 | corregida a INVEST | **PERMITE · exit=0** |

El gate solo deja avanzar el backlog cuando cada historia es Independiente,
Negociable, Valiosa, Estimable, Pequeña y Testeable, con criterios de aceptación,
estimación y sin preguntas abiertas.
