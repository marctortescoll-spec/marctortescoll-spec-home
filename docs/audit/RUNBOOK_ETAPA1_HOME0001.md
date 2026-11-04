# RUNBOOK — Etapa 1 (instalación y diagnóstico HOME0001)
## Procedimiento exacto · versión `018c745` · listo para ejecutar cuando haya acceso

**Pre-requisito de seguridad (puerta previa):** antes de ejecutar, confirmar identidad del nodo y que
no presta servicios productivos. Si algo no cuadra → **detener**.

## 0. Identidad y estado inicial (sin cambios)

```bash
hostnamectl                                  # debe ser HP t520 / HOME0001
uptime                                       # tiempo encendido
df -h /                                      # espacio para respaldo
docker ps -a 2>/dev/null                     # contenedores existentes (inventario)
systemctl list-units --type=service --state=running | head -50
```

## 1. Copia de seguridad completa y verificable

```bash
# snapshot del estado actual (genera .tar.gz + .manifest.json con SHA-256)
bash deployment/factory/scripts/backup.sh /srv/home00x /srv/home00x/backups
# verificación de integridad (debe imprimir "OK")
bash deployment/factory/scripts/backup-verify.sh /srv/home00x/backups/kaenori-backup-*.tar.gz
```

## 2. Instalación (versión fijada por SHA)

```bash
git clone git@github.com:marctortescoll-spec/home.git /opt/kaenori-src
cd /opt/kaenori-src
git checkout 018c7454ac24a606be26390a571eac2996d5d976   # SHA fijado
# instalación no destructiva (revisada: no elimina instalación previa)
bash deployment/factory/install.sh
```

## 3. Verificación básica (sin controlar dispositivos)

```bash
systemctl status kaenori-node kaenori-catalog kaenori-experience --no-pager
# integridad de lo instalado
bash deployment/factory/fingerprint.sh          # debe coincidir con el manifiesto
# egress: NO alterar (solo comprobar estado actual; la alteración es Etapa 2)
```

## 4. Evidencias a recoger

- Salida de `hostnamectl`, `docker ps -a`, `df -h`, inventario de servicios.
- SHA del archive de backup + `OK` de `backup-verify.sh`.
- Salida de `fingerprint.sh` + `systemctl status`.
- Cualquier incidencia.

## 5. Rollback (solo si hay incidencia; no destructivo improvisado)

```bash
bash deployment/factory/scripts/backup-restore.sh /srv/home00x/backups/kaenori-backup-*.tar.gz
```

## Prohibiciones (Etapa 1)

Reiniciar/apagar · alterar cortafuegos/DNS/rutas · habilitar servicios nuevos · tocar termostatos/energía
· pruebas de cortes de red · Etapas 2 y 3.

*Runbook A3-E1. No autoriza la ejecución por sí solo: requiere aprobación + grant A3 + acceso al nodo.*
