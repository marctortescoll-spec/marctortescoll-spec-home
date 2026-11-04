# GRANT ACTIVO — Ejecución Autónoma Kaenori Experience V1.3

**Tipo:** Grant estático (file-based Control Plane) · **Origen:** autorización directa del Owner.
**Fecha:** 2026-11-04 · **Ref:** MANDATO MAESTRO OWNER + confirmación de presupuesto.

## Alcance

- **Niveles:** A0 (observar) · A1 (desarrollar) · A2 (integrar). **A3 excluido** (requiere grant específico).
- **Objetivo:** ejecutar el Plan Maestro P0–P2 (software) sobre `home @ 549700b9`.
- **Repos:** `marctortescoll-spec/home` (código) · `marctortescoll-spec-home` (documental, rama `audit/global-v1.3`).

## Límites duros

- **Tope de gasto: 5 €** (ledger `PRESUPUESTO.json`, cap_eur=5.0). Si el gasto acumulado lo supera → **abortar** y pedir nueva autorización.
- **Sin A3**, sin actuación física, sin producción/viviendas reales.
- **Sin modificar Core v1.0 congelado** (solo bump + auditoría).
- **Sin bypass** de permisos/kill-switch · sin secretos/PII en Git/logs/prompts.

## Revocación

El Owner puede revocar este grant en cualquier momento.

*Este fichero sustituye al Control Plane no consultable en este entorno; es la autorización operativa.*
