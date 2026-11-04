# KAENORI — Cierre técnico de ejecución A3 P0 en HOME0001

**Fecha:** 2026-11-04  
**Permiso OWNER:** aprobación expresa en conversación del 2026-11-04, registrada en `docs/audit/governance/GRANT_ACTIVO.md`.  
**Target:** HOME0001 (`192.168.1.196`), laboratorio.  
**Commit objetivo:** `644cd4efd153d48616d76dc5e998de31761b0ff4`  
**Estado de ejecución:** **despliegue técnico terminado; auditoría independiente y aceptación OWNER pendientes**.

## Preflight y fuentes

- CI GitHub del SHA objetivo: [run 41923289257](https://github.com/marctortescoll-spec/home/actions/runs/41923289257) — **2613 passed, 5 skipped, 0 failed, 269 subtests**.
- El commit objetivo parte de `08d26ea` y solo incorpora `tests/test_scan_fail_soft.py`; el diff de `117d7b6` a `644cd4e` afecta **seis archivos**, cuatro de producto y dos tests.
- Fuente de código: diferencias oficiales de GitHub aplicadas a **copia aislada** de la versión instalada anterior, sin cambiar configuración DNS.
- Verificación de integridad: los cuatro blobs originales coincidían con GitHub en `117d7b6`; los **seis blobs de destino** coinciden exactamente con GitHub en `644cd4e` tras aplicar el parche.
- Comprobación de permiso mediante `tools/governance/grant.py`: operaciones incluidas aceptadas con SHA exacto; SHA distinto rechazado. El registro del grant se conserva por separado.

## Respaldo y reversión

- Respaldo anterior al cambio: `/srv/home00x/backups/kaenori-pre-p0-644cd4e-20261104T222258Z.tar.gz`.
- Tamaño: **264388743 bytes**. SHA-256: `9dcc3a170adcf548ff17ae1b4ef9a566278948d9a1de420b39e8498f650e9fd4`.
- Archive validado por `tar -tzf` + checksum. Incluye las carpetas y configuraciones Kaenori previstas; **no** incluye ni altera `home-assistant`, `adguard` ni `music_assistant`.
- Versión previa intacta: `/opt/kaenori-releases/117d7b6`.
- Release nueva: `/opt/kaenori-releases/644cd4e`.
- Reversión disponible: restaurar symlink a `117d7b6` y reiniciar exclusivamente `kaenori-experience-web`, con restauración de datos desde el backup únicamente si fuera imprescindible y de manera controlada.

## Comprobaciones funcionales y despliegue

- Prueba local sin pytest: normalización del código en **7 casos** — PASS.
- Prueba local sin hardware real: fallo simulado de AirPlay conservando resultados del catálogo — PASS.
- `pytest` no está disponible en el venv de HOME0001; el CI remoto del commit es el que acredita la suite completa.
- Conmutación reversible de symlink a `644cd4e`.
- Reinicio **únicamente** de `kaenori-experience-web`; no se reinició el equipo.
- Los cinco servicios Kaenori: **active**.
- `GET /api/health`: HTTP 200, `status=ok`, TLS.
- `POST /api/setup/scan` sin token: **401** (ruta existente, autenticación exigida).
- `GET /api/opportunity` sin token: **401** (ruta existente, autenticación exigida).
- `/sw.js`: versión `home-experience-shell-v27`.
- Home Assistant: **active**; AdGuard DNS: UDP `192.168.1.196:53` escuchando; uptime del equipo conservado.
- Sin cambios en router, DNS, AdGuard, Home Assistant, VPN, firewall ni dispositivos físicos.

## Pendientes para cerrar el P0

1. **07-auditor:** dictamen independiente sobre evidencias y limites. Este documento no finge ni sustituye sus votos.
2. **OWNER:** prueba única real desde navegador: generar código, copiar y pegar sin editar, emparejar, buscar equipos y comprobar que no aparece 501, sin perder progreso.
3. **Autenticación del escaneo:** las verificaciones remotas dieron 401 esperado sin credenciales; **aún no se ha observado un 200 autenticado ni el resultado real del escaneo desde el navegador del OWNER**.

**El grant A3 era de un solo uso y ha sido consumido por esta operación.** Ningún SHA posterior ni nueva actuación quedan autorizados.
