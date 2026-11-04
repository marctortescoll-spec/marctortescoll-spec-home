# ACTA DEL CONCILIO — Ronda 14 (contraste técnico) · 2026-11-04

**Concilio:** DeepSeek (orquestador) + GPT-5 + Gemini 4 Argon · **A0**.

## 1. Rectificaciones: validadas (bien evidenciadas)

Las 6 rectificaciones quedan **confirmadas**: (1) sí hay actuación codificada (`home_ha_adapter_core`),
(2) egress con enforcement codificado (A3), (3) backup con test, (4) 180 ficheros de test y CI parcial,
(5) taxonomía en `home_onboarding/vocabulary.py`, (6) `home_discovery_source` es el EMS activo.

## 2. Decisión arquitectónica (recuperación segura): **aprueba-con-matices** (2/2)

La propuesta **no-bypass + CCR Clase A preferente + safe-stop acotado para Clase B** queda **aprobada**
con matices obligatorios:

1. **Safe-stop a estado seguro ESTÁTICO**, no "estado previo" (Argon: el estado previo puede ser
   inseguro; revertir a un valor seguro predefinido en catálogo).
2. **Timeout duro de transición** (`ARMING_SAFE_STOP` → `ARMED_DENY_ALL`, p. ej. 5 s) para no colgarse
   si falla el read-back o el transporte (Argon).
3. **Safe-stop idempotente, solo lista blanca *revert-only*, firmado/auditado** (log inmutable, reloj
   monotónico), separación de funciones (quien arma no amplía privilegios; whitelist con M-of-N) (GPT-5).
4. **Clase A: evidencia por dispositivo** del TTL/dead-man antes de habilitar actuación (GPT-5).
5. **Riesgo latente activo**: como `HomeActionExecutor` existe, **prohibido activarlo en producción**
   hasta implementar CCR + safe-stop + Override (Argon).

## 3. Bloqueos corregidos / pendientes

- Corregidos en esta ronda: actuación, egress (codificado), backup (test), tests (180), taxonomía, EMS.
- Pendientes (top): **corregir `ci.yml`** (ejecutar los 180, no solo 132) · **implementar máquina de
   estados kill-switch+CCR** con safe-stop estático + timeout · **purgar `home_ems`** y unificar la
   taxonomía de 12 categorías · verificación A3 de egress/instalación en hierro.

## Votos

| IA | Voto |
|---|---|
| GPT-5 | aprueba-con-matices |
| Argon | aprueba-con-matices |

*Acta A0. No autoriza implementación; la decisión queda como propuesta aprobada por el concilio, a
confirmar por el propietario antes de codificar.*
