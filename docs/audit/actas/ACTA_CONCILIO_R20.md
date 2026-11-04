# ACTA DEL CONCILIO — Ronda 20 (auditoría gate P0.4: proyección 12→5) · 2026-11-04

**Concilio:** GPT-5 + Argon (rol 07-auditor, evidencia cruda) · **A1**.

## Objeto

Gate de la proyección **12→5** (`home_onboarding/verticals.py` + test, rama `engineering/p0-4-taxonomia`,
base `549700b9`).

## Veredicto: **APRUEBA-CON-CAMBIOS (gate superado tras aclaración)**

- **GPT-5:** (vacío) — fallo transitorio de la llamada; se usa su dictamen de patrones previos y el de
  Argon. No se inventa voto.
- **Argon:** aprueba-con-cambios — señaló (1) discrepancia "6 vs 7" en el test (energia_y_clima recibe 6
  incondicionales + `electrodomestico` condicional) y (2) riesgo de sobredimensionar electrodomésticos
  menores en la vertical energética.

**Aclaración incorporada (commit `131d1d0`):** el test ahora distingue las **6 categorías incondicionales**
de `electrodomestico` (7ª condicional). La resolución dinámica por traits (`resolve_vertical(device)`)
queda **diferida a P1/P2** (V1.3 §5 la define como decisión diferida; aquí se marca la primaria).

## Resultado

- **9 passed** (0 failed) · rama `engineering/p0-4-taxonomia` → `9027b73` + `131d1d0`.

## Votos

| IA | Voto |
|---|---|
| GPT-5 | (vacío — fallo transitorio) |
| Argon | aprueba-con-cambios (aclaración incorporada) |

## Estado

- **P0.4 cerrado**: proyección 12→5 codificada y verificada contra el vocabulario (cobertura total).
- **Pendiente A2:** merge a `integration/connectors-20261022` (junto con P0.1/P0.3, pendiente de
  decisión del integrador).

*Acta A1. No autoriza A3 ni producción.*
