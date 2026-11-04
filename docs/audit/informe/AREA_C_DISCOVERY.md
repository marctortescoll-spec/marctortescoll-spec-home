# ÁREA C — Discovery, catálogo, conectores y verticales
## Informe técnico · Fase 2 · Auditoría Global V1.3

**Fuente:** `home/integration/connectors-20261022 @ 549700b9` · A0 (solo lectura).

| Requisito V1.3 | Código | Estado |
|---|---|---|
| Descubrimiento pasivo/activo | `home_discovery/engine.py`, `home_netdiscovery/`, `home_scout/` | implementado |
| Catálogo canónico + inventario | `home_connectivity/catalog.py`, `core/kaenori_core/discovery/catalog.py` | implementado |
| Identificación / reconciliación / dedupe | `core/kaenori_core/discovery/{unified,dedupe,contract_alignment}.py` | implementado |
| Conectores solo-lectura | `home_connectivity/` (airzone, apsystems_ez1, goe, openevse, panasonic_intesis, shelly, v2c, victron, zha_bridge) | implementado |
| Homologación / puente Discovery→catálogo | `home_connectivity/{homologation,discovery_bridge}.py` | implementado |
| Taxonomía técnica (12 categorías) | `home_connectivity/vocabulary.py` (`CATEGORY_KEYWORDS`) | implementado |
| **Proyección 12 categorías → 5 verticales de producto** | **NO codificada** (el código mapea categoría→funciones, no→vertical comercial) | **pendiente** |
| Compartidos entre verticales (persiana=Confort+Energía+Seguridad) | **NO codificado** explícitamente | **pendiente** |

## Hallazgos

- La **columna de conectividad está sólida** (escáneres + catálogo + dedupe + 9 conectores solo-lectura).
- La **proyección de producto (12→5)** de V1.3 **no existe en código**: el código trabaja con categorías
  técnicas granulares y funciones, no con las 5 verticales comerciales. Hay que construir el mapeo.

*Informe A0. No autoriza desarrollo.*
