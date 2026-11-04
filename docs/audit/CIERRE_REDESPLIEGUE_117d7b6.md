# CIERRE — Redespliegue controlado HOME0001 (`018c745` → `117d7b6`)
## Evidencias · 2026-11-04

**Resultado: ÉXITO** — Kaenori actualizado a `117d7b6` (UI conectada a Oportunidades), sin pérdida de
configuración, con rollback disponible.

## 1. Preparación

- Commit verificado: `117d7b6d0c42494d773604632d1bc5350e8bc74b` (SHA completo coincide).
- Backup dirigido: `/srv/home00x/backups/kaenori-pre-redeploy-20261104T223728Z.tar.gz` (434 MB,
  SHA-256 `0b98037e…`; excluye `home-assistant/config`, que **no** se tocó).

## 2. Procedimiento (no destructivo)

- Código desplegado a **nueva** release `/opt/kaenori-releases/117d7b6`.
- Symlink `/opt/kaenori` → `117d7b6` (la anterior `018c745` se conserva).
- Reinicio de los **5 servicios Kaenori** (sin `install.sh`, sin reinicio del equipo, sin tocar
  AdGuard/DNS/router/HA/VPN).

## 3. Verificación post-despliegue

| Comprobación | Resultado |
|---|---|
| 5 servicios Kaenori | **5/5 active** |
| Health (`/api/health`) | `{"status":"ok","version":"1.1.0","tls":true}` |
| `/api/opportunity` | **401** (autenticado; existe, no 404) |
| Código desplegado | `opportunity_preview` (brain) + sección "Oportunidad energética" (app.js) presentes |
| Datos/configuración | `/srv/home00x` intacto (22 elementos) |
| Versión anterior | `018c745` conservada (rollback disponible) |
| Kill-switch | `{"active": false}` (deny-by-default) |

## 4. Rollback disponible

`ln -sfn /opt/kaenori-releases/018c745 /opt/kaenori` + `systemctl restart kaenori-*` → vuelve a
`018c745`. Restaurar datos si fuera necesario: `backup-restore.sh` con el backup §1.

## 5. Límites respetados

No se reinició el equipo · no se modificó DNS/AdGuard/router/HA/VPN · no se actuó sobre dispositivos ·
no se ejecutó `install.sh`.

---

*Cierre A3. Requiere dictamen de 07-auditor. No incluye Etapas posteriores.*
