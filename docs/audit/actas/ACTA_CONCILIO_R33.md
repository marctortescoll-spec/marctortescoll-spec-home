# ACTA DEL CONCILIO — Ronda 33 (revisión ING-025) · 2026-11-05

**Concilio:** DeepSeek (ejecutor) + GPT-5 (arquitectura) + Argon (validación) · **A1**.

## Objeto

Revisión de la ejecución de MANDATO-ING-025 (cryptography + Tuya-local), ya APROBADO PASS 2/3.

## Veredicto: **aprueba-con-cambios** (2/2) → cambios incorporados

| IA | Voto | Cambio clave exigido |
|---|---|---|
| GPT-5 | aprueba-con-cambios | `--only-binary` + sha256 en lock/CI; restringir import a `tuya_local.py`. |
| Argon | aprueba-con-cambios | fail-closed versión desconocida/JSON corrupto; local key por dispositivo (no booleano global); guard con AST. |

## Cambios incorporados (commit `c82fe9f`)

1. `requirements-crypto.lock` con 26 hashes sha256 de ruedas + `--only-binary=:all: --require-hashes`.
2. `decrypt_tuya_state` → `ValueError` para versión desconocida; `decrypt_tuya_state_json` → `ValueError` si no es JSON UTF-8 válido.
3. `suggest_local_onboarding` → `tuya_local_keys` (set por dispositivo) en vez de booleano global.
4. Guard de import reescrito con `ast` (detecta imports diferidos/nested).

## Estado

MANDATO-ING-025 **CERRADO** (A1: código + tests 2645 verde). Sin despliegue (requiere grant A3).

*Acta A1. El Concilio no concede autoridad: el despliegue requiere autorización del OWNER.*
