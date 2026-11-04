# ACTA DEL CONCILIO — Ronda 24 (auditoría gate P1.3: ciclo de vida Oportunidades) · 2026-11-04

**Concilio:** GPT-5 + Argon (rol 07-auditor, evidencia cruda) · **A1**.

## Objeto

Gate del ciclo de vida de Oportunidades (`home_opportunity/lifecycle.py` + test, rama
`engineering/p1-3-opportunities`, base `549700b9`).

## Veredicto: **APRUEBA-CON-CAMBIOS (gate superado tras confirmación + guarda)**

- GPT-5: aprueba-con-cambios — pidió confirmar transiciones de pausa/datos/caducidad/reapertura (todas
  **ya presentes** en el grafo completo; el extracto solo mostró las destacadas).
- Argon: aprueba — condicionado a confirmar esos arcos + propuso una **guarda de evidencia** para la
  adopción.

**Mejoras incorporadas (commit `72ba62d`):**
1. **Guarda de adopción**: `can_adopt(evidencia)` exige **B (efecto) o C (beneficio)**; la sola ejecución
   verificada (A) **no** autoriza adoptar. (Argon, Anexo A §8).
2. Tests que confirman explícitamente: bucles de pausa/reanudación (prueba y adopción), bucle de datos,
   caducidad natural, y reapertura desde `DISMISSED` (no es sumidero).

## Resultado

- **14 passed** (0 failed) · rama → `7d7a12a` + `72ba62d`.

## Votos

| IA | Voto |
|---|---|
| GPT-5 | aprueba-con-cambios (arcos confirmados) |
| Argon | aprueba (condiciones cumplidas) |

## Estado

- **P1.3 (núcleo) cerrado**: 15 estados reversibles + evidencia A/B/C + guarda de adopción. La
  **tarjeta/simulación/permisos** (resto de Anexo A) se completan en P1.4/P2 junto con el dashboard.

*Acta A1. No autoriza A3 ni producción.*
