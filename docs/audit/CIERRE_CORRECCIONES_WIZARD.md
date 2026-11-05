# CIERRE — Corrección de los 4 fallos del wizard · 2026-11-05
## Código `f3b42ee` · suite 2662 tests verde · sin desplegar

## Fallos corregidos

| # | Fallo | Corrección |
|---|---|---|
| 1 | Hubs sin botón de conexión | Botones «Conectar» para **MQTT** y **Smart Life** en Ecosistemas; `connectEcosystem` muestra el estado real (connected/available/detected) + guía |
| 2 | TRADFRI "faltan credenciales" sin salida | Los **hubs/gateways** (TRADFRI, coordinador Zigbee) ahora **se conectan en Ecosistemas**, no como un dispositivo más |
| 3 | `ecosystem_connect` devolvía `unavailable` para MQTT/Smart Life | Ahora consulta el **catálogo vivo de HA por dominio**: `connected` si la integración está cargada, `available` si está en el catálogo |
| 4 | Equipos "sin vertical" sin salida | Botón **«Clasificar»** en los equipos "otros" → asigna una vertical manual (endpoint `/api/setup/classify-device` + override persistido) |

## Evidencia

- Suite completa: **2662 tests en verde** (incluidos los nuevos de `classify_device`).
- Commit: `integration/connectors-20261022` · **`f3b42ee`**.

## Qué verás al desplegar

- **Ecosistemas**: MQTT y Smart Life ya tienen botón «Conectar» (y Matter/Thread/Zigbee explican qué hardware necesitan).
- **TRADFRI** (hub): al pulsarlo te lleva a **Ecosistemas** para conectar el gateway, en vez de "faltan credenciales" sin salida.
- **Equipos "otros"**: tienen un botón **«Clasificar»** para asignarlos a una vertical.

## Pendiente de ti

**Desplegar** `f3b42ee` a HOME0001 (grant A3). Ahora mismo la web sigue en `e5cbb8f` (sin estas correcciones).

---

*Cierre A1/A2. Despliegue pendiente de autorización A3.*
