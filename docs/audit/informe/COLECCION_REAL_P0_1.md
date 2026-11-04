# P0.1 — Colección real de pytest (conteo verificado)
## Evidencia A1 hermética · `549700b9` · pytest 9.1.1

**Método:** `python3 -m pytest --collect-only -q` en el clone de solo lectura, `PYTHONPATH=.:core`, sin
red ni hardware (solo colección, sin ejecutar tests).

## Conteo real (pytest, no estimación)

| Suite | Tests recolectados | Errores de colección | ¿En CI hoy? |
|---|---|---|---|
| `tests/` (sin `tests/synthetic`) | **1569** | 0 | ✅ (`pytest tests/ --ignore=tests/synthetic`) |
| `tests/synthetic/` | (18, sin recolectar) | **3 errores: falta `yaml`** (dep no instalada en clone) | ❌ (ignorado) |
| `tests_bdm/` | **920** | 0 (1 warning `TestScope.__init__`) | ❌ |
| `core/tests/` | **85** | 0 | ❌ |
| `deployment/factory/tests/` (shell) | 6 scripts | — | ❌ |

## Cifras corregidas

- **CI ejecuta hoy: 1569 tests** (`tests/` menos synthetic).
- **Fuera de CI: 920 + 85 + 18 + 6 = 1029** (~40 % de la suite no corre).
- **Total recolectable ≈ 2574** (1569 + 920 + 85) + 18 synthetic (con `yaml` instalado) + 6 shell.

## Hallazgos

1. La estimación previa por `grep 'def test_'` (~2563) era **aproximada**; el conteo real pytest es
   **~2574** recolectados + 6 shell.
2. `tests/synthetic/` **no recolecta** en el clone por **falta de `pyyaml`** (dependencia de
   `[project.optional-dependencies]` de test); en CI se instalaría con `pip install -e '.[test]'`.
3. **Sin efectos secundarios de importación** observados en la colección (solo el error de dependencia en
   synthetic); la colección es rápida (~1 s por suite).

## Acción P0.1 (cuando se autorice A1/A2)

- Ampliar `ci.yml` a `pytest tests/ tests_bdm/ core/tests/ --ignore=tests/synthetic` (o con marcas por
  clase), manteniendo external/infra/hardware fuera del job por defecto.
- Añadir la suite `contract_core_and_safety` como bloqueante.
- Documentar los 18 synthetic + 6 shell como excluidos con causa.

*Evidencia A1 hermética. No ejecuta tests con efectos, no modifica código, no despliega.*
