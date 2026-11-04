# GOLDEN CASES — Equivalencia EMS (P0.3) · COMPLETO · verificado en código
## `home_ems/math.py` vs `home_discovery_source/ems.py` · `549700b9` · A1 hermético

**Método:** ejecución local de funciones puras (sin red/hardware/efectos), `PYTHONPATH=.:core`, entradas
idénticas.

## Resultados (todas las funciones comparables)

| Par de funciones | Resultado | Nota |
|---|---|---|
| `energy_opportunity` ↔ `oportunidad_energetica` | **idénticas salvo 1 caso** | con excedente bajo + sin tarifa: d=45 (fabrica), h=`None` (honesto) |
| `cold_water_setpoint` ↔ `consigna_agua_frio` | **idénticas (24/24)** | — |
| `hot_water_setpoint` ↔ `consigna_agua_calor` | **idénticas (misma demanda en 3 zonas)** | `home_ems` es más rico (3 zonas: suelo/habs/escalera) |
| `thermal_demand` ↔ `demanda_termica` | **idénticas (6/6)** | — |
| `thermal_mode` ↔ `modo_termico` | **idénticas (6/6)** | — |

## Robustez (hallazgo adicional)

| Caso | `home_discovery_source` | `home_ems` |
|---|---|---|
| `setpoint_cold = None` en demanda térmica | **CRASHEA** (`TypeError`) | devuelve `None` (fail-closed) |
| tarifa ausente + excedente bajo (oportunidad) | devuelve `45` (valor fabricado) | devuelve `None` (honesto) |

## Conclusión definitiva (P0.3)

1. **La matemática es idéntica** para entradas normales (oportunidad, consignas frío/calor, demanda y modo
   térmico): **0 divergencias en 60+ casos válidos**.
2. **`home_ems` es el candidato canónico** porque es: **superset** (Shadow + PDP/permisos + plan + lector
   tipado + `derive` + consigna caliente por 3 zonas) **y más robusto** (no crashea con `None`, no fabrica
   valores).
3. `home_discovery_source/ems.py`+`ems_service.py` es una **copia paralela más simple y menos segura**.

**Recomendación P0.3 (firme):** consolidar sobre **`home_ems`**; retirar `home_discovery_source/ems.py` +
`ems_service.py` en un paquete posterior con autorización específica y tests de equivalencia (no en P0.3).

*Evidencia A1 hermética. No modifica código, no despliega, no ejecuta tests con efectos externos.*
