# GOLDEN CASES — Equivalencia EMS (P0.3) · verificado en código
## `home_ems/math.py` vs `home_discovery_source/ems.py` · `549700b9` · A1 hermético

**Método:** ejecución local de las funciones puras (sin red, sin hardware, sin efectos externos) sobre
entradas idénticas, con `PYTHONPATH=.:core`.

## Resultado 1 — Oportunidad energética: **equivalentes salvo 1 caso**

| Entrada (surplus, price) | `energy_opportunity` (discovery) | `oportunidad_energetica` (home_ems) | ¿Igual? |
|---|---|---|---|
| surplus ≥ 0.5 (con o sin precio) | 100 / 85 / 70 | 100 / 85 / 70 | ✅ |
| surplus < 0.5, precio ≤ barato | 65 | 65 | ✅ |
| surplus < 0.5, precio ≥ caro | 20 | 20 | ✅ |
| **surplus < 0.5, precio ausente** | **45** | **`None`** | ⚠️ **diverge** |

**Divergencia:** con excedente bajo y **sin tarifa**, `home_discovery_source` devuelve `45` (un valor
"por defecto" que **finge** una oportunidad media); `home_ems` devuelve `None` (honesto: sin tarifa no
hay oportunidad cuantificable). **`home_ems` es el más correcto (fail-closed, no fabrica).**

## Resultado 2 — Consigna de agua fría: **idénticas (24/24 casos)**

| demand / opportunity | `cold_water_setpoint` | `consigna_agua_frio` | ¿Igual? |
|---|---|---|---|
| 0 / cualquiera | 0 | 0 | ✅ |
| 15 / 0,50,70,90 | 10,10,9,8 | 10,10,9,8 | ✅ |
| 25 / 0,50,70,90 | 8,8,7,6 | 8,8,7,6 | ✅ |
| 45 / 0,50,70,90 | 7,7,6,5 | 7,7,6,5 | ✅ |
| 65 / 0,50,70,90 | 6,6,5,5 | 6,6,5,5 | ✅ |
| 85 / 0,50,70,90 | 5,5,5,5 | 5,5,5,5 | ✅ |

## Conclusión verificada (P0.3)

1. **La matemática es idéntica** (mismos umbrales, mismos mapeos) para oportunidad y consignas.
2. **La única divergencia** es el caso "excedente bajo + tarifa ausente": `home_ems` devuelve `None`
   (honesto), `home_discovery_source` devuelve `45` (valor por defecto que fabrica oportunidad).
3. **`home_ems` es el candidato canónico**: superset (Shadow + PDP + plan + lector tipado) **y** más
   correcto en el caso límite.

**Siguiente paso (A1):** extender golden cases a `hot_water_setpoint`/`consigna_agua_calor` (fases
día/pre-noche/noche) y a `thermal_demand`/`demanda_termica`, y formalizar el test de equivalencia como
parte de la suite de P0.3.

*Evidencia A1 hermética. No modifica código, no despliega, no ejecuta tests con efectos externos.*
