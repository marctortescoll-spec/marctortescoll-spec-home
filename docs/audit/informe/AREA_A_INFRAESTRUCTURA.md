# ÁREA A — Infraestructura y producto instalable
## Informe técnico de soporte · Fase 1A · Auditoría Global V1.3

**Fuente canónica:** `marctortescoll-spec/home` · `integration/connectors-20261022` @
`549700b9c25c2336af8f699cbc1cf1270145fc96` · contrastado con `main @ d3a1a84b`.
**Naturaleza:** lectura (A0). Sin ejecutar instalaciones, despliegues ni pruebas físicas.

## Resumen

La **receta de fábrica existe y es detallada** (instalación reproducible + first-boot idempotente +
backup/restore + tests offline), pero está en **VERSION 0.1.0** y **no hay evidencia de una
instalación real ejecutada** (el manifiesto de HOME0001 lo declara explícitamente: "NO ejecutado").
Estado global: **parcial — codificado, no validado en entorno real**.

## Evidencia por capacidad

| Requisito V1.3 | Código (commit 549700b9) | Tests | Estado | Riesgo / limitación |
|---|---|---|---|---|
| Instalación reproducible | `deployment/factory/install.sh` + `bootstrap.sh` | `factory/tests/test_units.sh`, `test_config.sh` | parcial | Requiere root+red; no ejecutada aquí |
| Primer arranque idempotente + identidad Ed25519 | `deployment/factory/first-boot.sh` | `factory/tests/test_first_boot.sh` | parcial | keygen con fallback inyectable |
| Fingerprint / manifiesto verificable | `deployment/factory/fingerprint.sh` + `MANIFEST.sha256` | `factory/tests/test_fingerprint.sh` | parcial | MANIFEST con 1 hash (imagen base) |
| Servicios systemd | `deployment/factory/units/*.service` (first-boot, node, catalog, egress, ha-adapter, experience, webui, home-assistant…) | `factory/tests/test_units.sh` | parcial | plantillas, no activas |
| Backup / restore | `deployment/factory/scripts/backup.sh`, `backup-restore.sh`, `backup-verify.sh` | — (sin test dedicado visible) | pendiente | sin evidencia de restore verificado |
| Actualización | `deployment/factory/scripts/update.sh`, `release_manifest.py` | — | pendiente | sin evidencia |
| Funcionamiento offline | `factory/tests/run_offline_tests.sh` + `first-boot.sh --stage client` | sí (script) | parcial | tests offline, no hardware |
| CI | `.github/workflows/ci.yml` | — | codificado | no ejecutado desde aquí |
| Emparejamiento seguro de caja | `home_identity/` + `first-boot.sh` (Ed25519) | `core/tests/` | parcial | identidad codificada; pairing no verificado end-to-end |
| Despliegue en nodo real | `deployment/nodes/HOME0001/install-manifest.yaml` (dice "NO ejecutado") | — | **NO VERIFICADO** | sin evidencia de instalación viva |

## Hallazgos y bloqueos (Área A)

1. **No hay evidencia de instalación real.** `deployment/nodes/HOME0001/install-manifest.yaml` declara
   `manifest_version 0.1.0` y "NO ejecutado". El t520/laboratorio queda **NO VERIFICADO** desde este
   entorno.
2. **Backup/restore sin test dedicado** y sin evidencia de restauración verificada.
3. **MANIFEST.sha256** presente pero con contenido mínimo (1 hash); la verificación de la imagen
   completa no está demostrada.
4. **Emparejamiento de caja** (identidad + un solo uso) está codificado a nivel de identidad Ed25519,
   pero el **pairing end-to-end** con el propietario no tiene evidencia en esta rama.

## Próximos pasos verificables (Área A)

- Ejecutar la suite offline de `factory/tests/run_offline_tests.sh` (requiere mandato posterior con
  entorno controlado) y registrar resultado.
- Verificar `backup-restore.sh` contra un nodo de laboratorio (mandato posterior).
- Contrastar el `MANIFEST.sha256` con una imagen construida real.

*Informe de auditoría (A0). No autoriza instalaciones, despliegues ni reinicios.*
