# ACTA DEL CONCILIO — Ronda 21 (auditoría gate P0.5: safe-stop CCR Clase B) · 2026-11-04

**Concilio:** GPT-5 + Argon (rol 07-auditor, evidencia cruda) · **A1 · seguridad crítica**.

## Objeto

Gate del safe-stop acotado (`home_security/safe_stop.py` + test, rama `engineering/p0-5-ccr`, base
`549700b9`), conforme a la decisión R14 (sin bypass, estado seguro estático, timeout duro).

## Veredicto: **APRUEBA-CON-CAMBIOS OBLIGATORIOS (gate superado tras endurecimiento)**

- **GPT-5:** (vacío) — fallo transitorio de la llamada (2ª vez consecutiva); registrado, no se inventa
  voto. Se apoya en el dictamen de Argon.
- **Argon:** aprueba-con-cambios obligatorios. Hallazgos reales corregidos:

1. **NaN/Inf en timeout** (la ventana podía quedar abierta indefinidamente) → ahora
   `math.isfinite(timeout_seconds)` + `> 0`.
2. **Reloj manipulable** (parámetro `now` público congelable) → ahora el reloj se inyecta **solo por
   constructor**; la API pública no acepta `now`.
3. **Reapertura de ventana** (`disarm()`+`begin_safe_stop()` tras deny-all) → ahora `seal()` es
   **irrecuperable** salvo `reset()` privilegiado.
4. **`revert_only` como str** (frozenset lo descomponía en caracteres) → ahora `TypeError`.
5. Documentada la semántica `DISARMED` (el safe-stop no es autoridad en operación normal).

## Resultado

- **9 passed** (0 failed) · SHA-256 final `7ac0f5a2…` · rama → `641954c` + `6ba8d59`.

## Votos

| IA | Voto |
|---|---|
| GPT-5 | (vacío — fallo transitorio) |
| Argon | aprueba-con-cambios (obligatorios cumplidos) |

## Estado

- **P0.5 (software) cerrado**: máquina safe-stop Clase B implementada y endurecida. El **CCR Clase A**
  (evidencia TTL por dispositivo) y la **integración con `AuthorizationPolicy`** quedan como pasos
  siguientes (P1/validación A3), con el límite explícito de que el timeout de software **no sustituye una
  garantía física**.

*Acta A1. No autoriza A3 ni producción.*
