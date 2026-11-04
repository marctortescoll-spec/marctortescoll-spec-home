# ACTA DEL CONCILIO — Ronda 17 (auditoría gate P0.1: fix S11) · 2026-11-04

**Concilio:** GPT-5 + Argon (rol 07-auditor, evidencia cruda) · **A1**.

## Objeto

Gate de auditoría del fix de la regresión `test_s11_read_only_ready_gate` (rama `engineering/p0-1-ci`,
commit `3829f70`, base `549700b9`).

## Evidencia revisada (cruda)

- `git diff` (solo `tests/test_home_experience_enroll.py`, +5/−3): reubica la aserción
  `enroll.status === "completed"` de `renderS11()` a `enrollAndRender()` (donde ahora ocurre el enrolado
  tras el consentimiento ING-020).
- Suite hermética: **1564 passed, 5 skipped, 71 subtests** (0 failed).
- SHA-256: `3fe940e803d030bb8aac956cdff5a425895ec06fdfaa8d9a08b0fc96ed33e49c`.

## Veredicto: **APRUEBA (gate superado)** — 2/2

Ambos revisores confirman que el cambio es mínimo y **no debilita el contrato F3-1** (sigue exigiendo
`completed` para "Conectado", ahora en el lugar correcto).

## Riesgo no bloqueante registrado (deuda técnica)

El test usa **string-matching** sobre la firma exacta (`_function_body` + `"async function
enrollAndRender(d, verify) {"`): un renombrado/refactor cosmético lo rompería sin regresión funcional real.
**Acción futura (no P0.1):** migrar estas validaciones de contrato UI a tests de comportamiento DOM/JS
(JSDOM/Playwright) o análisis AST.

## Votos

| IA | Voto |
|---|---|
| GPT-5 | aprueba |
| Argon | aprueba |

## Estado

- Rama `engineering/p0-1-ci` empujada a `origin` (commit `3829f70`).
- **Pendiente A2:** integrar a `integration/connectors-20261022` (requiere integrador/token).

*Acta A1. No autoriza A3 ni producción.*
