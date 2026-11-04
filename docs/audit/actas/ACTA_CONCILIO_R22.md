# ACTA DEL CONCILIO — Ronda 22 (auditoría gate P1.1: Override manual) · 2026-11-04

**Concilio:** GPT-5 + Argon (rol 07-auditor, evidencia cruda) · **A1**.

## Objeto

Gate del Override (`home_ems/override.py` + test, rama `engineering/p1-1-override`, base `549700b9`).

## Veredicto: **APRUEBA-CON-CAMBIOS (gate superado tras mejoras)**

- GPT-5: aprueba-con-cambios — falta el *gate de integración* (que el ejecutor consulte `is_held`),
  persistencia, ACL/auditoría.
- Argon: aprueba-con-cambios — "idempotencia destructiva" (hold machacaba `since`/razón), acoplamiento
  hold↔invalidación, poda de `_invalidated`, volatilidad en memoria.

**Mejoras incorporadas (commit `e6602b6`):**
1. `hold()` ahora **preserva** el `since` y la `reason` originales en holds repetidos.
2. Añadido parámetro `invalidate` a `hold()` para **invalidación atómica** (target + evaluación).

**Diferido (hardening de producción, no defecto del primitivo):** gate de integración (ActionGate en el
ejecutor), persistencia durable (los holds en memoria se pierden en reinicio), ACL/auditoría en
release/invalidate, poda de `_invalidated`. Se registrarán en P1.x/P2.

## Resultado

- **9 passed** (0 failed) · rama → `7bf7eec` + `e6602b6`.

## Votos

| IA | Voto |
|---|---|
| GPT-5 | aprueba-con-cambios (mejoras + diferidos registrados) |
| Argon | aprueba-con-cambios (mejoras incorporadas) |

## Estado

- **P1.1 (Override) cerrado** como primitivo; el *enforcement* (ActionGate) y la persistencia quedan
  para la integración P1.x/P2.

*Acta A1. No autoriza A3 ni producción.*
