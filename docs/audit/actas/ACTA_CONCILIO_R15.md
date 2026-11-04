# ACTA DEL CONCILIO — Ronda 15 (mandato P0.1 + P0.3) · 2026-11-04

**Concilio:** DeepSeek (orquestador) + GPT-5 + Gemini 4 Argon · **A0**.

## Veredicto: **aprueba-con-cambios** (2/2)

El mandato preparatorio P0.1 (CI) + P0.3 (EMS) es **correcto y seguro** (preparatorio, sin tocar `main`,
sin activar `HomeActionExecutor`, sin desplegar, sin alterar comportamiento). Se aprueba con las
siguientes **condiciones obligatorias**:

## Condiciones obligatorias (consolidadas GPT-5 + Argon)

1. **Aislamiento en P0.1 (Argon):** el descubrimiento de los ~1005 casos fuera de `tests/` debe hacerse
   en entorno **aislado sin red ni hardware**, para evitar efectos secundarios en *import-time*.
2. **Suite de contratos Core + guardrails (Argon + GPT-5):** separar como categoría prioritaria
   independiente las **pruebas de contrato de puertos Core/V1.3** (CDC) y los **guardrails de
   `HomeActionExecutor`** (que verifiquen que está bloqueado), además de compatibilidad de esquema/eventos.
3. **Búsqueda no-estática de dependencias `home_ems` (Argon + GPT-5):** no basta el AST; hay que buscar
   carga dinámica (`importlib`, `__import__`), `entry_points`, YAML/JSON/env, systemd/cron/CLI y
   side-effects (telemetría/logging).
4. **Criterio de aceptación P0.3 (Argon):** exigir una **Matriz de Equivalencia Funcional y de Contratos**
   antes de recomendar la implementación canónica.
5. **CI endurecido (GPT-5):** etiquetado formal por marcas pytest (unit/contract/sim/external/infra/hw/
   flaky/slow), matriz segura-por-defecto (externas/hardware nunca por defecto), **dry-run + rollback** de
   workflows versionados.
6. **Sin cambios de dependencias/lockfiles/toolchains** salvo para reproducibilidad controlada (GPT-5).

## Votos

| IA | Voto |
|---|---|
| GPT-5 | aprueba-con-cambios |
| Argon | aprueba-con-cambios |

*Acta A0. No autoriza implementación; el mandato queda pendiente de aprobación expresa del propietario
con estas condiciones incorporadas.*
