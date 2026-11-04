# DICTAMEN 07-AUDITOR — fichas P0.1 y P0.3 (revisión independiente A0)

**Revisor:** 07-auditor (independiente del ejecutor) · **Fecha:** 2026-11-04 · **Fuente:** `home @ 549700b9`
· **Objeto:** fichas P0.1 (CI) y P0.3 (EMS) publicadas en `audit/global-v1.3 @ 053f7cf`.

---

## 1. Dictamen P0.1 — CI y calidad → **LISTA con 1 corrección menor**

- La ficha es correcta en: inventario por suites, clasificación en 5 clases, procedimiento de recogida
  aislado, suite de contratos Core + guardrails `HomeActionExecutor`, y rollback de workflow.
- **Corrección (no bloqueante):** el conteo "~2563 casos" es una **estimación por `grep 'def test_'`**, no
  la colección real de pytest. La ficha ya lo prevé, pero debe quedar explícito que el **número real**
  solo lo da `pytest --collect-only` (en A1, aislado). No confundir "funciones `test_`" con "casos
  recogidos por pytest" (parametrizaciones, fixtures, subtests).
- **Confirmado:** ningún punto de P0.1 exige modificar Core, activar actuación ni desplegar.

## 2. Dictamen P0.3 — EMS → **NO LISTO: la matriz de equivalencia es incorrecta (corrección bloqueante)**

Verificado en código (`549700b9`):

### 2.1 La matriz publicada es errónea

La ficha afirmaba: consignas = "exclusiva discovery_source", matemática = "exclusiva home_ems". **Falso.**
Ambos módulos contienen **la misma matemática energética y las mismas consignas**:

| Función | `home_discovery_source/ems.py` | `home_ems/math.py` |
|---|---|---|
| Tarifa → precio | `tariff_period`, `current_price` | `periodo_actual`, `precio_actual` |
| Oportunidad energética | `energy_opportunity` | `oportunidad_energetica` |
| Consigna ACS frío | `cold_water_setpoint` | `consigna_agua_frio` |
| Consigna ACS calor | `hot_water_setpoint` | `consigna_agua_calor` |
| Demanda/modo térmico | `thermal_mode`, `thermal_demand`, `heating_demand_zone` | `modo_termico`, `demanda_termica`, `demanda_calor_zona`, `fase_calor` |

Son **implementaciones paralelas** de la misma matemática, no complementarias. Además, **ambas** tienen
capa de servicio duplicada (`ems_decision`, `make_handler`, `main` en `ems_service.py` / `service.py`).

### 2.2 `home_ems` es la implementación MÁS completa (capacidades exclusivas reales)

`home_ems/` añade, sobre la matemática: **modelos tipados** (`models.py`: DomainSnapshot, Proposal,
ReadingStatus), **EmsShadow** (`shadow.py`), **PDP/permisos** (`permission.py`: ClimatePDP, ShadowExecutor),
**planificación** (`plan.py`: plan_climate_consigna, plan_from_climate_intent), **lector tipado**
(`reader.py`: EmsStateReader) y **contrato** (`contract.py`: domain_spec). `home_discovery_source/ems.py`
**no** tiene nada de esto.

### 2.3 Corrección crítica: **ningún motor EMS tiene consumidor externo ni servicio de arranque**

- `pyproject.toml [project.scripts]` solo expone `kaenori-discovery = "home_discovery.cli:main"`. **No hay
  entry point** para `home_ems` ni `home_discovery_source.ems_service`.
- Ninguna unidad systemd arranca EMS (`home_node`, `catalog_service`, `home_catalog`, `home_experience`,
  `home_ha_adapter`, `apply_egress` — ninguna es EMS).
- Los "4 consumidores" que citaba la ficha importan **submódulos de discovery** (`catalog_store`,
  `consent`, `identification`, `matter`, `mqtt`…), **no** `home_discovery_source.ems`. El `import ems`
  detectado es **interno** (de `ems_service.py` a su hermano `ems.py`).

**Conclusión:** el motor EMS (ambas implementaciones) está **codificado pero no conectado** a ningún
servicio ni consumidor runtime. Es una biblioteca a la espera de la capa de Oportunidades (que aún no
existe). La consolidación **no** es "reconectar consumidores" (no los hay), sino **elegir la biblioteca
canónica** para el futuro.

### 2.4 Recomendación canónica corregida

**`home_ems` es el candidato canónico** (superset: matemática + Shadow + PDP + plan + lector tipado). La
migración debe: (1) validar equivalencia con golden cases contra `home_discovery_source/ems.py`; (2)
**no eliminar nada en P0.3**; (3) mantener `home_discovery_source` como paquete de discovery (sus otros
submódulos sí están conectados) y retirar solo su `ems.py`/`ems_service.py` en un paquete posterior con
autorización.

## 3. Correcciones bloqueantes (a incorporar antes de A1/A2)

1. **P0.3 — reescribir la matriz de equivalencia** (ambos = matemática + consignas; home_ems = superset;
   sin consumidores externos).
2. **P0.3 — separar "comprobado en código"** (funciones existentes, sin consumidores) **de "requiere
   ejecución"** (equivalencia numérica, cobertura real de pytest).
3. **P0.1 — sustituir el conteo estimado** por "conteo real vía `pytest --collect-only` (en A1, aislado)".

## 4. Criterios de aceptación (refinados)

- **P0.1:** CI ampliado verde en suites herméticas; suite `contract_core_and_safety` bloqueante; reporte
  de casos **reales** por suite; rollback versionado; dry-run en rama aislada.
- **P0.3:** matriz de equivalencia reescrita y firmada por 07-auditor; golden cases que demuestren que
  `home_ems` reproduce las salidas de `home_discovery_source/ems.py`; no-regresión; plan de migración
  progresiva con feature flag; **sin borrado en P0.3**.

## 5. Recomendación de autorización

- **P0.1: APROBAR A1/A2** (tras aplicar la corrección de conteo).
- **P0.3: NO aprobar todavía** — requiere **reescribir la matriz** y firmar el dictamen corregido. Se
  puede autorizar A1 (análisis no-estático + golden cases) **una vez** corregida la ficha.

*Dictamen A0. No implementa, no ejecuta pruebas con efectos externos, no modifica main ni despliega.*
