# CIERRE — Ejecución del mandato E2E (B0–B6) · primera integración real
## Kaenori HOME00X · Experience V1.3 · 2026-11-05 · código `02fe9d3`

**Resultado: la primera integración real de extremo a extremo está construida, probada y verificada contra Home Assistant REAL (sin desplegar aún).**

---

## 1 · Qué se ha construido (bloques B0–B6)

| Bloque | Qué hace | Verificación |
|---|---|---|
| **B0** | Esquema SQLite `device_readings` (v5): procedencia, `source_ts` nativo de HA, `ingested_at`, `available`. | migración + 6 tests de idempotencia/persistencia |
| **B1** | Puente HA→identidad: `ha_device_inventory()` agrupa las entidades por **dispositivo** (registry WS `config/entity_registry/list` + `unique_id`) y las siembra en `memory.db`. Excluye entidades de sistema (sun/backup/weather/person). | test: V2C = **1 dispositivo con 5 entidades** (no 5 clones) |
| **B2** | `device_reading()` con `age_ms` calculado del `last_updated` NATIVO de HA; `unavailable`→`value: null`+`available: false` (nunca `0` falso). | test `unavailable` no inventa 0 |
| **B3** | Dashboard (`app.js`) con sección **"Lecturas reales"** vía `GET /api/devices`. | — |
| **B4** | Liloo: 2 intenciones deterministas `device_info` y `device_status` (sin LLM) que citan fuente + antigüedad. | 4 tests |
| **B5** | Idempotencia de `seed_device`/`record_reading` + persistencia al reabrir la memoria. | 2 tests |
| **B6** | `demo/e2e_device_reading.py` — Prueba Delta y Corte reproducible. | ejecutada contra HA real (ver §3) |

**Suite completa: 2630 tests en verde (0 fallos).**

## 2 · Nuevos endpoints (solo lectura, autenticados)

- `GET /api/devices` → inventario real de dispositivos vía HA.
- `GET /api/device/<device_id>/reading` → última lectura con `age_ms` + fuente.
- `POST /api/setup/scan` (ya existente) ahora también **siembra los dispositivos de HA** en el catálogo del alta.

## 3 · Evidencia de la Prueba Delta contra HA REAL (hoy, sin desplegar)

Ejecutado `demo/e2e_device_reading.py` desde `/tmp/e2e-code` contra `http://127.0.0.1:8123`:

```
Inventario: 2 dispositivos reales
  - ha://device/home_ems_v2c  EMS V2C  energy  5 entidades
  - ha://device/luces_salon_test  Luces salón  other  1 entidad
W1 potencia_red = 552.0 W · fuente=home_assistant · age=9 s
W2 potencia_red = 541.0 W · age=4 s  →  Delta(W2≠W1)=True
Liloo device_info:  "EMS V2C es un dispositivo de categoría energy
                    (V2C Trydan (cargador VE)). Tiene 5 métricas en tiempo real vía Home Assistant."
Liloo device_status: "EMS V2C — ahora mismo: disponible: on (hace 5 s) ·
                      carga: 0.0 W · casa: 1067.0 W · fv: 525.0 W ·
                      red: 541.0 W · fuente: Home Assistant."
Persistencia: dispositivo conservado tras reabrir → True
```

- La **lectura cambió** (552 → 541 W) y coincide con HA: **lectura real, no mock**.
- El fabricante/modelo salió **del catálogo homologado** (`V2C Trydan (cargador VE)`).
- El **corte** (unavailable→no_disponible) está cubierto por el test unitario `test_unavailable_entity_is_not_zero`.

## 4 · Estados distinguibles (cumplido)

`detectado` · `identificado` · `compatible` · `incorporado` · `lectura_real_verificada` · `no_disponible`. La UI y Liloo nunca presentan "disponible" como "integrado" ni inventan cifras.

## 5 · Qué falta para verlo en el producto

**Re-despliegue a HOME0001** (el código está en `02fe9d3`, sin desplegar). Requiere tu **grant A3** (mismo procedimiento seguro: backup → nueva release → symlink → reinicio de los 5 servicios Kaenori). Tras desplegar, la prueba final es: desde la PWA, preguntar a Liloo «¿qué sabes de este dispositivo?» y «¿cuál es su estado actual?» y ver el V2C en el dashboard con su lectura real.

## 6 · Límites respetados

Solo lectura · sin tocar HA/AdGuard/DNS/router/VPN · sin actuación física · sin desplegar · sin mocks en la prueba final.

---

*Cierre A1/A2. El despliegue queda pendiente de grant A3 del OWNER.*
