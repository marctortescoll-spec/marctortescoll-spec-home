# INFORME FINAL — Actualización controlada HOME0001 (A3-E1)
## `14e1f62` → `018c745` · 2026-11-04

**Resultado: ÉXITO** — Kaenori actualizado, servicios sanos, datos y configuración conservados, con
respaldo y rollback acreditados.

---

## 1. Identidad y estado inicial

- Nodo: `home0001` (HP t520, Debian 13, x86-64) · SSH `marc@192.168.1.196` (acceso confirmado).
- Estado previo: Kaenori **ya desplegado** en `/opt/kaenori-releases/14e1f62` (5 servicios activos),
  junto a Home Assistant (docker 2026.9.2), AdGuard Home y Music Assistant.
- Servicios Kaenori en marcha: node (8137) · catalog (8138) · experience · experience-web (8443/TLS) ·
  ha-adapter.

## 2. Copia de seguridad (verificada)

- Archive: `/srv/home00x/backups/kaenori-full-pre-upgrade-20261104T204325Z.tar.gz` (458 MB).
- SHA-256: `f7e56d3d842871e9bde892b78b7d6f581c4e3a6e8d94d481bc27b81fe65466ef` → **verificado OK**.
- Alcance: `/srv/home00x` (datos+config) + `/opt/kaenori-releases` (todas las releases).

## 3. Procedimiento de actualización (no destructivo)

1. Despliegue del código `018c745` a una **nueva** release `/opt/kaenori-releases/018c745`.
2. Cambio del symlink `/opt/kaenori` → `/opt/kaenori-releases/018c745` (la anterior `14e1f62` se conserva).
3. Reinicio de los 5 servicios Kaenori (no se tocó `install.sh` ni el instalador original).
4. Verificación de salud.

## 4. Estado final (verificado)

- Symlink activo: `/opt/kaenori-releases/018c745`.
- Servicios: **5/5 active**.
- Health: `{"status":"ok","version":"1.1.0","tls":true,"uptime_s":31.1}` (HTTPS 8443).
- Datos intactos: `/srv/home00x` (22 elementos, sin cambios).

## 5. Rollback acreditado

- Volver a `14e1f62`: `ln -sfn /opt/kaenori-releases/14e1f62 /opt/kaenori` + `systemctl restart kaenori-*`.
- Restaurar datos si fuera necesario: `backup-restore.sh` con el archive del §2 (hace snapshot previo).

## 6. Límites respetados

- No se tocó el cortafuegos/DNS/rutas · no se reinició el equipo · no se actuó sobre dispositivos · no
  se tocó HOME0000 · no Etapas 2/3 · no se ejecutó `install.sh`.

---

*Informe A3-E1. Requiere dictamen independiente de 07-auditor.*
