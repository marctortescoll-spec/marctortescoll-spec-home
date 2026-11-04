# ÍNDICE — Auditoría Global Kaenori V1.3
## `_audit_global_v1_3_2026-11-04/` · Fase 0 · Inventario y línea base

**Fecha:** 2026-11-04 · **Naturaleza:** auditoría de lectura (A0). No modifica código, no despliega, no
actúa sobre equipos.
**Rama de publicación:** `audit/global-v1.3` en `marctortescoll-spec-home` (documental).

---

## 1. Línea base inmutable (pinning de hashes — minuto cero)

| Repo (SSH) | Rol | Rama / ref | Commit SHA |
|---|---|---|---|
| `marctortescoll-spec/home` | **Código de producto** | `main` | `d3a1a84b` |
| `marctortescoll-spec/home` | Código de producto | `integration/connectors-20261022` | `549700b9` |
| `marctortescoll-spec/home` | Código de producto | `integration/kaenori-discovery-unified` | `9f60a17` |
| `marctortescoll-spec/home` | Código de producto | `integration/p2-core-discovery-20261018` | `3ab863b` |
| `marctortescoll-spec/home` | Código de producto | `integration/p3-debian-ha-foundation-20261018` | `eb70b99` |
| `marctortescoll-spec/kaenori-governance` | **Gobierno/autonomía + snapshot de código** | `main` (local) | `09b61430` |
| `marctortescoll-spec/marctortescoll-spec-home` | **Documental (V1.3 congelada)** | `main` | `695266c` |

> La auditoría se realiza contra **estos SHAs**. Si algo se mueve, se registra como cambio de línea base.

---

## 2. Inventario de repositorios y ramas

- **`home`** — repositorio de producto. **43 ramas**. Relevantes: 4 ramas `integration/*`, `main`, ramas
  `autonomy/pilot-*`, `bdm/kaenori-core/*`.
- **`kaenori-governance`** — gobierno/autonomía (workspace). Ramas: `main` + `autonomy/department-v0`,
  `autonomy/mandate-ing-001-r2`, `autonomy/mandate-ing-002`. Local `main` **adelantado 5 commits** a
  `origin/main` (sin publicar).
- **`marctortescoll-spec-home`** — documental. `main` con la V1.3 congelada.

## 3. Componentes de producto identificados (módulos "home")

`core/home_experience` · `home_onboarding` · `home_connectivity` · `home_ems` · `home_catalog` ·
`home_synergy` · `home_voice` · conectores (`security`, `ha-adapter`, `discovery`, `ems`, `liloo`,
`experience`) — vistos en las **copias del workspace** (`homologacion_ws/`, `_audit_homologacion/`,
`_audit_07_ems_20261028/`).

## 4. Hallazgos críticos de la Fase 0

1. **El workspace (`kaenori-governance`) contiene COPIAS del código "home"** (`homologacion_ws/`,
   `_audit_*`) incrustadas en su working tree (`main @ 09b61430`). **No son worktrees del repo `home`**;
   pueden estar **desactualizadas** respecto a `home/integration/connectors-20261022 @ 549700b9`.
2. **`kaenori-governance` local `main` está 5 commits por delante de `origin/main`** (trabajo sin publicar).
3. La **rama canónica de integración** es `home/integration/connectors-20261022 @ 549700b9`.
4. El **criterio de producto** (V1.3 + 4 anexos) vive en `marctortescoll-spec-home main @ 695266c`.

**Limitación registrada (NO VERIFICADO):** no hay evidencia accesible desde este entorno de los entornos
reales (t520/laboratorio) ni de su estado de despliegue. Se audita **código y documentación**, no
instalaciones vivas.

---

## 5. Estructura de trabajo

```
_audit_global_v1_3_2026-11-04/        (local, worktrack)
├── INDICE.md                          ← este fichero
├── evidencia/                         ← punteros a commits/tests (y copia en el repo remoto)
├── informe/
├── matriz/
├── registro/
└── actas/
```

**Fases:** 0 Inventario (✅) → 1 Áreas A–F (✅) → 2 Recorridos 1–4 (✅) → 3 Matriz+bloqueos (✅) →
4 Plan (✅) → 5 Consolidación y voto (✅, rondas 12–13).
**Publicación: BLOQUEADA — SSH GitHub "Permission denied (publickey)".**

**Regla anti-pérdida:** toda afirmación va ligada a un SHA + archivo + evidencia, o a `NO VERIFICADO`.

*Documento de auditoría (A0). No autoriza cambios de código ni despliegues.*
