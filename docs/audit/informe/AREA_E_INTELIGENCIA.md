# ÁREA E — Inteligencia, EMS y Opportunities
## Informe técnico · Fase 2 · Auditoría Global V1.3

**Fuente:** `home/integration/connectors-20261022 @ 549700b9` · A0 (solo lectura).

| Requisito V1.3 | Código | Estado |
|---|---|---|
| Lectura energética + telemetría | `home_ems/reader.py`, `home_connectivity/v2c.py` | implementado |
| EMS Shadow (propone sin ejecutar) | `home_ems/shadow.py` (`actuation_enabled=False`, `executed=False`) | implementado |
| Matemática tarifa→precio→consigna | `home_ems/math.py`, `home_ems/plan.py` | implementado |
| Permisos EMS | `home_ems/permission.py` | implementado |
| **Tarjeta de Oportunidades (Anexo A: 5 momentos, CCR, prueba, evaluación A/B/C)** | **NO codificado** | **pendiente** |

## Hallazgos

- El **EMS shadow + matemática están codificados** y bien acotados (proponen, no ejecutan; no proponen
  sobre dato inválido).
- La **"oportunidad energética"** que aparece en `home_ems/service.py` / `home_discovery_source/ems.py`
  es un **escalar 0–100**, **no** la tarjeta de Oportunidades del Anexo A. El ciclo completo
  (simular→autorizar→probar→evaluar con CCR/Override/evidencia A/B/C) **no está implementado**.
- **Duplicación detectada:** `home_ems/` y `home_discovery_source/ems.py` contienen lógica EMS paralela
  (a consolidar en Área F).

*Informe A0. No autoriza desarrollo.*
