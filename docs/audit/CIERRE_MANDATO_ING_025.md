# CIERRE — MANDATO-ING-025 (cryptography + Tuya-local) · 2026-11-05
## Código `c82fe9f` · Suite 2645 tests verde · Veredicto Concilio: aprueba-con-cambios (incorporados)

## Qué se ha ejecutado (P0/P1/P2)

**P0 — Dependencia auditada:**
- `cryptography==44.0.0` pinnada en `pyproject.toml`.
- `requirements-crypto.lock` con **26 hashes sha256 de ruedas oficiales** + instalación `--only-binary=:all: --require-hashes` (sin compilar desde fuente).
- **Guard de import** con análisis AST (`tests/test_crypto_import_guard.py`): `cryptography` solo en `tuya_local.py`/`secrets.py` (este último con import diferido preexistente del vault).

**P1 — Conector `tuya_local.py` (solo lectura):**
- `decrypt_tuya_ecb` (AES-128-ECB + PKCS7, v3.1/v3.3), `decrypt_tuya_gcm` (AES-128-GCM + tag + AAD, v3.4), `decrypt_tuya_state` (dispatch; **versión desconocida → ValueError**), `decrypt_tuya_state_json` (parseo JSON; **no-JSON → ValueError**).
- Fail-closed: clave ≠16 bytes, padding corrupto, tag inválido, JSON corrupto → excepción, nunca datos parciales.
- Local key por parámetro (nunca en claro en reposo; el guardado en vault lo hace el llamador). Import `cryptography` diferido.

**P2 — Onboarding:** `suggest_local_onboarding(devices, tuya_local_keys=...)` reconoce Tuya-local **por dispositivo** (cada Tuya tiene su propia local key), además de Sonos.

## Veredicto del Concilio (ronda 33)

- **GPT-5: aprueba-con-cambios** — faltaba `--only-binary`+sha256 y restringir el import. → **Incorporado** (lockfile con hashes + guard AST).
- **Argon: aprueba-con-cambios** — versiones desconocidas/JSON corrupto sin cubrir, `has_tuya_local_key` global erróneo (debe ser por dispositivo), guard debe usar AST. → **Incorporado** (fail-closed + JSON + key por dispositivo + guard AST).

## Fuera de alcance (respetado)

A3/producción · control cifrado Tuya (follow-up) · HomeKit/Matter · passwords crudos · despliegue.

---

*Cierre A1 (código + tests, sin desplegar). El despliegue de la dependencia en HOME0001 requiere grant A3.*
