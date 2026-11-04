# CIERRE P0–P2 — Estado final del alcance software (A1/A2)
## Plan Maestro ejecutado · `home @ 549700b9` · 2026-11-04

## Resultado: **8 paquetes construidos, probados y gateados** (integración A2 pendiente)

| Paquete | Rama `engineering/*` | Qué aporta | Gate |
|---|---|---|---|
| P0.1 CI | `p0-1-ci` | fix regresión S11 + CI ampliado (2528 verdes) | r17 ✅ · r18 ✅ |
| P0.3 EMS | `p0-3-ems` | test equivalencia EMS (`home_ems` canónico) | r19 ✅ |
| P0.4 Taxonomía | `p0-4-taxonomia` | proyección 12→5 | r20 ✅ |
| P0.5 CCR | `p0-5-ccr` | safe-stop Clase B (sin bypass, timeout duro) | r21 ✅ |
| P1.1 Override | `p1-1-override` | intervención manual prevalece + invalida | r22 ✅ |
| P1.2 Verificación | `p1-2-verify` | verificación por atributo (consignas clima) | r23 ✅ |
| P1.3 Oportunidades | `p1-3-opportunities` | ciclo de vida 15 estados + evidencia A/B/C | r24 ✅ |
| P1.4 Liloo | `p1-4-dashboard` | contrato visual mínimo | r25 ✅ |

**P2 (expediente de piloto):** [EXPEDIENTE_PILOTO_P2.md](EXPEDIENTE_PILOTO_P2.md) — primera oportunidad
energética + candidato CCR Clase A + criterios de aceptación + FMEA. **No requiere código** (documental).

## Lo que queda para declarar terminado el mandato (mandato §11)

1. **Integración A2**: merge de las 8 ramas a `integration/connectors-20261022` — **requiere autorización
   del Integrador único** (token). Única decisión de autoridad pendiente.
2. **Veredicto final 07-auditor** sobre el conjunto (los gates por paquete ya pasaron).
3. **A3**: instalación/egress en hierro y verificación CCR Clase A — **excluido de este mandato** (target
   + rollback + grant específico).

## Evidencia

- 8 ramas `engineering/*` en `origin` (verificado `git ls-remote`).
- Actas de concilio r14–r25 publicadas en `docs/audit/actas/` (rama `audit/global-v1.3`).
- Suites: P0.1 = 2528 verdes · cada paquete con su test dedicado en verde.

## No se ha hecho (respetando límites)

- No se tocó Core v1.0 congelado · no se activó `HomeActionExecutor` contra dispositivos · no se
  desplegó · no se ejecutaron pruebas con efectos externos · no se filtraron secretos · no se modificó
  `main` · no se introdujo bypass.

*Cierre A1/A2. No autoriza A3, producción ni integración a integration/* sin el Integrador único.*
