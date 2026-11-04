# FICHA EJECUTABLE — Paquete P0.3 · Consolidación EMS
## Preparación A0 · pendiente de revisión 07-auditor y autorización A1/A2

**Rama futura propuesta:** `engineering/p0-3-ems` · **Fuente:** `home @ 549700b9` · **No implementa, no
elimina ni depreca código.**

## 1. Mapa de consumidores (estático + dinámico)

- **Estático (AST):** `home_discovery_source` importado por `home_node/catalog_service.py`,
  `home_connectivity/discovery_bridge.py`, `home_catalog/observation.py`, `core/home_experience/brain.py`.
  `home_ems` **sin consumidores estáticos** (grep fuera de `home_ems/` y tests).
- **Dinámico (a verificar en A1):** buscar `importlib`/`__import__`, `entry_points`/`PKG-INFO`, carga por
  YAML/JSON/env, systemd/cron/CLI, y *fallback* silencioso en Wizard/Liloo/Core.

## 2. Inventario de las dos implementaciones

| Implementación | Módulos | Funciones | Tests |
|---|---|---|---|
| `home_discovery_source/` | `ems.py`, `ems_service.py` | `energy_opportunity`, `cold_water_setpoint`, `hot_water_setpoint` | en `tests/` |
| `home_ems/` | `shadow.py`, `service.py`, `reader.py`, `plan.py`, `permission.py`, `models.py`, `math.py`, `contract.py` | EmsShadow (proposals), matemática, permisos | sin consumidores; tests a localizar |

## 3. Matriz de equivalencia funcional y de contratos

| Capacidad | `home_discovery_source` | `home_ems` | Equivalencia |
|---|---|---|---|
| Oportunidad energética (escalar) | `energy_opportunity` | `oportunidad_energetica` (service.py) | a verificar |
| Consignas ACS (frío/calor) | `cold/hot_water_setpoint` | — | exclusiva discovery_source |
| Shadow Mode (propuestas tipadas) | — | `shadow.py` | exclusiva home_ems |
| Matemática tarifa→precio→consigna | — | `math.py`, `plan.py` | exclusiva home_ems |
| Permisos EMS | — | `permission.py` | exclusiva home_ems |

*(preliminar; la matriz definitiva se rellena en A1 con evidencias de contrato)*

## 4. Diferencias y capacidades exclusivas

- `home_discovery_source`: integrada (catálogo/observación), consignas ACS, escalar de oportunidad.
- `home_ems`: Shadow tipado + matemática completa + permisos, pero **no conectada** a consumidores.

## 5. Recomendación de canónico (basada en evidencias)

**Preliminar:** consolidar sobre **`home_ems`** (capacidad más completa: Shadow + matemática + permisos)
**reutilizando** las funciones de consigna/oportunidad de `home_discovery_source`, y re-cablear los 4
consumidores. Alternativa a confirmar en A1: migrar a `home_discovery_source` si la equivalencia y el
coste de re-cableado lo justifican. **Decisión final solo tras la matriz de equivalencia (A1).**

## 6. Estrategia de migración progresiva y reversible

- **Feature flag / adaptador puente** durante la transición (sin corte brusco); mantener ambas
  implementaciones hasta pruebas de equivalencia verdes.
- Retirada en paquete posterior con **autorización específica** (no en P0.3).

## 7. Criterios de aceptación y no regresión

- Matriz de equivalencia funcional + de contratos completa; pruebas de equivalencia (golden cases);
  no-regresión de los 4 consumidores; cobertura de capacidades únicas; plan de reversión.

## 8. Rama futura

`engineering/p0-3-ems` (a abrir cuando se autorice A1/A2).

*Ficha A0. No autoriza implementación ni eliminación de código; requiere revisión 07-auditor y aprobación
A1/A2.*
