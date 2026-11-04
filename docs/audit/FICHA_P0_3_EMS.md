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

## 3. Matriz de equivalencia funcional y de contratos (CORREGIDA por 07-auditor)

**Ambos módulos contienen la MISMA matemática y las MISMAS consignas** (implementaciones paralelas):

| Función | `home_discovery_source/ems.py` | `home_ems/math.py` |
|---|---|---|
| Tarifa → precio | `tariff_period`, `current_price` | `periodo_actual`, `precio_actual` |
| Oportunidad energética | `energy_opportunity` | `oportunidad_energetica` |
| Consigna ACS frío / calor | `cold_water_setpoint` / `hot_water_setpoint` | `consigna_agua_frio` / `consigna_agua_calor` |
| Demanda/modo térmico | `thermal_mode`, `thermal_demand`, `heating_demand_zone` | `modo_termico`, `demanda_termica`, `demanda_calor_zona`, `fase_calor` |
| Capa de servicio | `ems_service.py` (`ems_decision`, `make_handler`) | `service.py` (`ems_decision`, `make_handler`) |

**Capacidades exclusivas reales de `home_ems`** (superset): modelos tipados (`models.py`), **EmsShadow**
(`shadow.py`), **PDP/permisos** (`permission.py`), **planificación** (`plan.py`), lector tipado
(`reader.py`), contrato (`contract.py`).

## 4. Hallazgo crítico: **ningún motor EMS tiene consumidor externo ni servicio**

- `pyproject.toml` solo expone `kaenori-discovery`; **no hay entry point** de `home_ems` ni
  `home_discovery_source.ems_service`.
- Ninguna unidad systemd arranca EMS.
- Los "consumidores" de `home_discovery_source` importan submódulos de **discovery** (`catalog_store`,
  `consent`, `identification`…), **no** `ems`. El motor EMS está **codificado pero no conectado** (a la
  espera de la futura capa de Oportunidades).

## 5. Recomendación de canónico (CORREGIDA)

**`home_ems` es el candidato canónico** (superset: matemática + Shadow + PDP + plan + lector tipado). La
migración: (1) validar equivalencia con golden cases contra `home_discovery_source/ems.py`; (2) **no
eliminar nada en P0.3**; (3) retirar `home_discovery_source/ems.py`+`ems_service.py` solo en un paquete
posterior con autorización específica. **Decisión final tras la matriz de equivalencia (A1).**

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
