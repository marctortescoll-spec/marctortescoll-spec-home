# GRANT ACTIVO — Ejecución Autónoma Kaenori Experience V1.3

**Tipo:** Grant estático (file-based Control Plane) · **Origen:** autorización directa del OWNER.
**Fecha:** 2026-11-04 · **Ref:** MANDATO MAESTRO OWNER + confirmación de presupuesto.

## Alcance general

- **Niveles:** A0 (observar) · A1 (desarrollar) · A2 (integrar).
- **A3 general:** excluido. Solo existe la excepción A3 concreta detallada abajo; no autoriza otras releases.
- **Objetivo general:** ejecutar el Plan Maestro P0–P2 (software).
- **Repos:** `marctortescoll-spec/home` (código) · `marctortescoll-spec-home` (documental, rama `audit/global-v1.3`).

## A3 específico autorizado por OWNER — P0 aceptación HOME0001

- **Autorización expresa:** conversación OWNER del 2026-11-04: «pues lo autorizo», en respuesta a la aprobación específica solicitada para este grant.
- **Target exclusivo:** laboratorio HOME0001, HP t520, `192.168.1.196`. Sin otras máquinas de destino.
- **Código exacto:** repositorio `marctortescoll-spec/home`, commit completo `644cd4efd153d48616d76dc5e998de31761b0ff4`.
- **Versión previa:** `117d7b6d0c42494d773604632d1bc5350e8bc74b`, conservar para rollback.
- **Operaciones permitidas:** inventario y preflight; backup verificable de datos/configuración Kaenori afectados; nueva release aislada sin instalador destructivo; conmutación reversible de symlink; reinicio de los servicios Kaenori estrictamente necesarios; smoke tests de los cinco servicios, emparejamiento, `/api/setup/scan` autenticado, catálogo/wizard, UI Liloo; informe y auditoría independiente `07-auditor`.
- **Puerta previa obligatoria:** validación del grant por el Control Plane/ejecutor independiente; código exacto verificado y CI verde; respaldo verificable y plan de recuperación; no continuar si falta fuente de código verificable o falla preflight.
- **No autorizados:** reinicio de HOME0001 entero; cambios en DNS/AdGuard/router/DHCP/Home Assistant/VPN/firewall; actuación sobre dispositivos físicos; cambios en Xeon u otros nodos; compras; despliegue de otros SHA; pruebas A3-E2-C o A3-E3.
- **Condición de seguridad:** si falla un gate o smoke test, detener siguientes actuaciones y aplicar únicamente recuperación autorizada, sin alterar otros servicios.
- **Vigencia:** limitada a una ejecución y cierre del despliegue P0; revocable en cualquier momento. La aceptación definitiva del P0 requiere la prueba del OWNER desde el navegador.
- **Presupuesto:** sin ampliación de gasto; se mantienen los límites/ledger ya vigentes.

**Estado del grant A3 P0:** **UTILIZADO / CONSUMIDO** — despliegue técnico ejecutado sobre HOME0001 a `644cd4e` con backup verificado, cinco servicios activos y recuperación disponible. Validación operativa de operaciones+SHA realizada; la revisión independiente de `07-auditor` y la aceptación real del OWNER siguen pendientes. **No autoriza repetir la operación ni otros SHA.** Evidencias: [CIERRE_EJECUCION_A3_P0_644cd4e_HOME0001.md](../CIERRE_EJECUCION_A3_P0_644cd4e_HOME0001.md).

## Límites generales duros

- **Tope de gasto: 5 €** (ledger `PRESUPUESTO.json`, cap_eur=5.0). Si el gasto acumulado lo supera → abortar y pedir nueva autorización.
- **Sin A3 fuera de la excepción exacta anterior**; sin producción, clientes ni viviendas reales.
- **Sin modificar Core v1.0 congelado** (solo bump + auditoría).
- **Sin bypass** de permisos/kill-switch · sin secretos/PII en Git/logs/prompts.

## Revocación

El OWNER puede revocar este grant en cualquier momento.

*Este fichero es el registro documental operativo del Control Plane file-based; no sustituye la validación de los gates previa a cada operación.*
