# CIERRE — Reinstalación completa de Kaenori en HOME0001 (t520) · 2026-11-05
## Código `95a8265` · Discovery reparado · 23 dispositivos visibles

## Qué se ha hecho

| Paso | Resultado |
|---|---|
| Desplegar release verificada `c82fe9f` (V2C + Tuya-local) | ✅ versión activa `c82fe9f` |
| Reparar servicios systemd (venv correcto `/opt/kaenori-venv`) | ✅ 5 servicios activos |
| Eliminar `shadow-selfcheck` roto (venv inexistente + script ausente) | ✅ timer `disabled` |
| Reinstalar el disparador del discovery | ✅ `kaenori-discovery-scan.timer` `enabled` (cada 30 min) |
| Re-ejecutar el escaneo (autorización `allow_lan_scan` + `enp1s0`) | ✅ 28–33 candidatos por ronda |
| Cablear el puente escáner→catálogo (`ingest_scan.py`) | ✅ `catalog.json` con 24 dispositivos |
| Verificar wizard "Buscar equipos" | ✅ **23 dispositivos** (audio 7, smart_home 8, sensor 4, energy 1, hub 1, plug 1, other 1) |

## La causa raíz (por qué "no encontraba nada")

1. El disparador periódico (`shadow-selfcheck`) estaba **roto**: apuntaba a un venv inexistente y a un script que no estaba en el release.
2. El escáner es **deny-by-default**: sin `--authorize-network` no abre ningún socket. Al reparar el disparador + pasarle la autorización, **funciona**.
3. Además, faltaba el **puente** entre el escáner (`inventory.json`) y el catálogo que lee el wizard (`catalog.json`): `record_candidates` solo estaba en tests. Se cableó con `ingest_scan.py`.

## Evidencia (wizard real)

`POST /api/setup/scan` → `total: 23 · pendientes: 23 · por categoría: audio 7, energy 1, hub 1, other 1, plug 1, sensor 4, smart_home 8`. Ejemplos identificados: **Apple HomePod** (audio), **V2C Trydan (cargador VE)** (energy), **IKEA TRADFRI gateway** (hub).

## Límites respetados

Solo lectura (mDNS/SSDP) · sin tocar Home Assistant/AdGuard/DNS/identidad/claves/certificados · sin reiniciar el equipo.

---

*Cierre A2/A3. Reinstalación + discovery completados y verificados.*
