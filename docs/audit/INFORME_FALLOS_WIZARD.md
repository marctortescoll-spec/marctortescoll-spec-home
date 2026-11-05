# INFORME DE FALLOS — Walkthrough del Wizard como usuario final
## Kaenori HOME00X · 2026-11-05 · versión desplegada `e5cbb8f`

**Método:** recorrí el wizard punto por punto como un cliente real (emparejar → bienvenida → casa → buscar equipos → seleccionar equipo → ecosistemas). Esto es lo que falla y por qué.

---

## 1 · Emparejamiento — ✅ OK

Pegar el código y pulsar «Emparejar» funciona a la primera (normalización de código aplicada).

## 2 · Buscar equipos — ✅ OK

Pulsa «Buscar equipos» → **26 equipos** descubiertos (audio, energía, hub, sensores…). El informe por verticales aparece. Sin Error 501.

## 3 · Los hubs NO ofrecen conexión — ❌ FALLO

**Lo que pasa:** en la pantalla de ecosistemas (gateways), solo **Home Assistant** tiene botón «Conectar». Los demás solo muestran un cartel:

- **Matter** → "requiere controlador/radio" (sin botón).
- **Zigbee** → "requiere dongle" (sin botón).
- **Thread** → "guía OTBR" (sin botón).
- **MQTT** → **nada** (ni botón ni cartel).
- **Smart Life / Tuya** → **nada** (ni botón ni cartel).

**Lo que debería pasar:** cada gateway debería tener un «Conectar» con su flujo (Matter = escanear código; MQTT = host/puerto/credenciales; Smart Life = login nube o local; Zigbee = detectar dongle).

**Causa raíz:** `ecosystem_connect()` devuelve `unavailable/requires_hardware` para **todo** lo que no es Home Assistant. Y el `ecosystemRow()` del wizard **solo pinta botón para `home_assistant`**. MQTT y Smart Life se añadieron al backend (B1) pero **sin flujo de conexión**.

## 4 · TRADFRI — "faltan credenciales" y no se puede conectar — ❌ FALLO

**Lo que pasa:** al elegir el **IKEA TRADFRI gateway** (aparece como `hub`), la pantalla muestra la pista "Es el propio hub IKEA (Zigbee).", pulsas «Continuar» → organizar → verificar → y al enrolar devuelve **"faltan credenciales"**, **sin ningún formulario** para meter el código de seguridad del hub.

**Lo que debería pasar:** al detectar un hub TRADFRI (o Zigbee), debería salir un **formulario para el código de seguridad del hub** (la credencial que HA pide para la integración `tradfri`).

**Causa raíz:** el tipo de enrolado es `hub` (el catálogo marca `requirements.hub: true`), pero el wizard **no tiene flujo para "hub"**: va directo a enrolar y HA responde `awaiting_credentials` sin que nadie le pida el código al cliente.

## 5 · Los equipos "sin vertical" no tienen salida — ⚠️ FALLO MENOR

**Lo que pasa:** 7 equipos quedan en "sin vertical" (servicios mDNS genéricos como HTTP/CoAP) sin botón ni explicación de qué hacer con ellos.

**Lo que debería pasar:** una opción "Clasificar este equipo" (override manual a una vertical) o "Ignorar".

**Causa raíz:** el mapper B0 los deja `None` (correcto, no inventa), pero la UI no ofrece el override manual que sí soporta el backend (`group_by_vertical(overrides=...)`).

---

## Resumen de fallos y arreglos

| # | Fallo | Arreglo |
|---|---|---|
| 1 | Hubs sin botón de conexión (Matter/Zigbee/Thread/MQTT/Smart Life) | Dar a cada gateway un flujo de conexión (Matter=escaneo, MQTT=host/creds, Smart Life=login/local, Zigbee=dongle) + botón en `ecosystemRow` |
| 2 | TRADFRI "faltan credenciales" sin formulario | Añadir flujo `hub`: formulario de código de seguridad para la integración `tradfri` (y genérico para hubs) |
| 3 | `ecosystem_connect` devuelve `unavailable` para MQTT/Smart Life | Implementar la conexión real de MQTT (broker) y Smart Life (local/cloud) |
| 4 | Equipos "sin vertical" sin salida | Añadir "Clasificar equipo" (override) en la UI |

---

*Informe de QA. Los fallos 1–3 son bloqueantes para la experiencia de cliente; el 4 es menor. No se ha desplegado ningún cambio.*
