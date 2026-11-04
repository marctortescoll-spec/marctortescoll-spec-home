# EXPEDIENTE A3 — Primer laboratorio HOME0001 (banco de pruebas)

**Naturaleza:** expediente preparado para solicitar **A3**. **No autoriza** la ejecución: requiere
**mandato A3 específico** con target y rollback, y **grant A3 comprobado** (GRANT_ACTIVO.md solo cubre
A0–A2).

## 1. Objetivo

Instalar y verificar en el **banco de pruebas t520/HOME0001** (nunca producción/HOME0000 ni vivienda
real) el software integrado en `integration/connectors-20261022 @ 25c27ae`.

## 2. Target

- **HOME0001 (laboratorio t520)** · Debian 13 + Docker + Home Assistant.
- **Excluido:** HOME0000, producción, DNS, contratos/precios, ESP32/altavoz físico, actuación en
  equipos reales no inventariados.

## 3. Pruebas a ejecutar (A3)

1. **Instalación** del factory (`deployment/factory/install.sh` + first-boot + bootstrap + fingerprint)
   en HOME0001.
2. **Egress**: `apply_egress` cargado con `nft -f` y **activo tras reinicio** (persistencia save/restore);
   verificar deny-by-default efectivo (bloqueo de salida a Internet).
3. **Backup/restore**: `backup.sh` + `backup-verify.sh` + `backup-restore.sh`; restauración real
   verificada.
4. **Suite completa** sobre el entorno instalado (2594 tests) + heartbeat del scheduler.
5. **Reinicio**: recuperación de estado consistente (kill-switch + safe-stop + boot_id/monotonic).
6. **Verificación CCR Clase A**: prueba de **corte de red observado** del dispositivo candidato (evidencia
   de TTL real), antes de cualquier actuación.

## 4. Rollback

- Snapshot/rollback del egress (`nft delete table inet kaenori_egress`).
- Restauración del estado previo del nodo (backup pre-instalación).
- Desactivación de unidades systemd (`systemctl disable --now kaenori-*`).

## 5. Criterios de autorización A3

- **Grant A3 específico** (target HOME0001 + rollback) — hoy **no existe** (GRANT_ACTIVO = A0–A2).
- Sin actuación física hasta que la **CCR Clase A** del dispositivo esté verificada con evidencia.
- Rollback probado antes de cualquier prueba de actuación.

## 6. Lo que queda excluido de este expediente

Actuación en viviendas reales, producción, compras, y cualquier dispositivo sin CCR verificada.

*Expediente A3 preparado. Requiere mandato A3 del Owner con target + rollback.*
