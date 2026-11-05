# MANDATO CONSOLIDADO — Rediseño onboarding Kaenori (verticales + gateways-primero)
## Consenso Concilio R34 · 2026-11-05 · lista para ejecutar (sin desplegar)

> Concilio: DeepSeek (orquestador/ejecutor) + GPT-5 (arquitectura/producto/marketing) + Argon (validación). El OWNER quiere UNA propuesta; este es el documento único A–G.

---

## A · Diagnóstico (qué hay y qué falta)

**Ya construido y verificado:** verticales modeladas (`home_onboarding/verticals.py`, 12→5) · ecosistemas detectados (`ecosystems_status`) · discovery reparado (23 dispositivos con categorías y fabricantes) · catálogo `manufacturers.json` · conector Tuya-local (ING-025) · V2C real (E2E) · Liloo (NLU `device_info`/`device_status`) · wizard (`setup.js`) + chat (`app.js`) · identidad visual (`brand-tokens.json` + logo Liloo).

**Lo que falta (la desconexión):**
1. **No hay mapper** de categoría/fabricante → vertical: el discovery escupe `smart_home`/`plug`/`other` crudas, y el OWNER quiere "clima, audio, jardín…".
2. **Faltan ecosistemas gateway**: `ecosystems_status` no contempla **MQTT** ni **Smart Life (Tuya)** como servidores de control.
3. **No hay deduplicación**: si se conecta HA/Matter, el hub expone los mismos equipos que mDNS ya ve (p.ej. V2C, TRADFRI) → duplicados.
4. **La UI no fluye en tiempo real**: el chat no emite el progreso del discovery ("he encontrado X en clima…").
5. **No hay gateways-primero** como paso obligatorio previo.

---

## B · Arquitectura del recorrido (reutilizando lo existente)

```
PASO 0 — GATEWAYS (obligatorio, skippable con confirmación)
  barra fija superior tipo checklist (sticky): Matter Border Router · Home Assistant ·
  Zigbee/Thread · MQTT · Smart Life/Tuya · (Alexa/Google como opcionales)
  → cada gateway conectado levanta su "servidor de control" y re-lanza discovery

PASO 1 — DESCUBRIMIENTO CONTINUO (mDNS/SSDP + DNS opt-in)
  → device_info enriquecido (fabricante/modelo/categoría) vía manufacturers.json

PASO 2 — AGRUPACIÓN EN VERTICALES (mapper determinista + override manual)
  → 5 verticales: Energía/Clima · Seguridad · Confort · Media/Entretenimiento · Jardín
  → DEDUP por MAC/UUID + prioridad: Gateway > Protocolo local > Cloud (nunca duplicar)

PASO 3 — INSTALACIÓN POR VERTICAL (panel derecho = chat Liloo, panel izquierdo = menú)
  → por vertical: "he encontrado N equipos en Clima"
  → por equipo: Local (check "conectado") · Cloud (modal credenciales/OAuth, con 2FA) ·
    verificación de lectura (autolectura si hay telemetría, si no "pulsa encender ¿ves la luz?")

PASO 4 — CIERRE
  → "he encontrado las verticales del menú izquierdo; pincha cada una para asistirte"
  → resumen por vertical + prueba de salud + sugerencia de escenas iniciales
```

**Reglas de consenso:**
- **Gateways primero, sticky, checklist** (clave de GPT-5): desbloquea y acelera el discovery.
- **Dedup + prioridad** (clave de Argon): `Gateway > Local > Cloud`; la identidad es por MAC/UUID.
- **Estados**: detectado · identificado · compatible · conectado (local) · requiere_login (cloud) · lectura_verificada · no_disponible.
- **Nunca "descubierto" = "conectado"**: Tuya-local exige `localKey`; HomeKit exige PIN; el check solo aparece con lectura/evidencia real.

---

## C · Línea de marketing + visión de cliente + usabilidad

**Línea de marketing (promesa):** *"Conecta tus cajas, Kaenori hace el resto."*

**Storyboard de 4 pantallas (visión de cliente):**
1. *"Detectamos tus gateways."* (el hub/controlador como cimiento).
2. *"Encontramos tus cosas."* (auto-descubrimiento en vivo).
3. *"Te las organizamos por mundo."* (verticales, no una lista desordenada).
4. *"Probamos que todo responde."* (verificación de lectura/acción).

**Tono Liloo:** *"He encontrado 7 equipos en Clima. ¿Conectamos Panasonic?"* — transparente, sin fricción.

**Usabilidad (benchmark Homey/Apple/Google aplicada):**
- **Izquierda**: menú de verticales con badge de conteo/estado (listo / acción requerida). Nunca color como único indicador (icono + texto).
- **Derecha**: chat Liloo con timeline en vivo (SSE/WebSocket): "escaneando… → he encontrado… → necesito tu permiso… → verificado". Respeta `prefers-reduced-motion`.
- **Gateways anclados arriba** con estado y CTA primario; al conectar uno → toast + re-scan.
- **Modal de credenciales único** reutilizable (login/password + OAuth2 + 2FA/captcha para cloud).
- **Fallo parcial no bloquea**: una vertical vacía o sin gateway muestra "a la espera" y deja continuar.

**Identidad visual (ya definida, aplicar tal cual):**
- Fondo `surface.page #F7F9FC`; paneles `#FFFFFF`; acentos `action.primary #00D9F5` para acciones; `gold/copper` para hitos; `fire #FF7139` solo para error crítico.
- Logo "sol + rombos" en cabecera; modo oscuro `brand.night #0B1020`; **corregir contraste** del cian (no texto blanco sobre cian); `es`+`ca`.

---

## D · Hoja de ruta (bloques que entregan resultado comprobable)

| Bloque | Objetivo | Prueba de aceptación |
|---|---|---|
| **B0 Mapper vertical** | Mapeo determinista categoría/fabricante → 5 verticales + override manual | Las 23 categorías del discovery caen en una vertical sin `other` huérfana |
| **B1 Gateways** | Añadir MQTT + Smart Life/Tuya a `ecosystems_status` + barra sticky checklist | Se detectan los 6 gateways con estado y CTA |
| **B2 Dedup** | Motor dedup por MAC/UUID + prioridad Gateway>Local>Cloud | HA/Matter activo + mDNS → V2C/TRADFRI NO duplicados |
| **B3 Streaming** | Chat Liloo emite progreso real (SSE/WebSocket) desde el discovery | "he encontrado X" aparece en vivo, sin salto de UI |
| **B4 Onboarding por vertical** | Menú izquierdo + por vertical: Local (check) / Cloud (modal credenciales/2FA) / verificación lectura | Panasonic por cloud y TRADFRI por local, con evidencia real |
| **B5 Cierre** | "he encontrado las verticales… pincha cada una" + resumen + salud | Recorrido E2E sin intervención manual del cliente |

---

## E · Riesgos y soluciones (máx. 5)

1. **Duplicados hub↔mDNS** → dedup por MAC/UUID + prioridad Gateway>Local>Cloud (B2).
2. **Cloud con 2FA/captcha** → modal OAuth + 2FA, no solo login/password; fallo → "requiere login" sin bloquear.
3. **Falso "conectado" local** → el check solo con evidencia real (localKey Tuya, PIN HomeKit, lectura); si no, "requiere pairing".
4. **Zero-gateway / vertical huérfana** → estado "a la espera" + rutas locales visibles; no bloquea el resto.
5. **Asincronía de red (saltos de UI)** → el menú izquierdo se actualiza en vivo pero con dedup estable (no muta mientras el usuario interactúa).

---

## F · Validación del Concilio

- **DeepSeek:** propuso el recorrido gateways-primero → auto-verticales, reutilizando verticales/ecosistemas/discovery/catálogo/Liloo.
- **GPT-5:** aprueba la arquitectura; aporta **línea de marketing** ("Conecta tus cajas, Kaenori hace el resto"), storyboard de 4 pantallas, UX con badges/timeline/modal único, y **1 cambio clave: gateways como barra sticky superior**.
- **Argon:** valida el estándar (Homey/Apple) y corrige 3 cosas: **mapper de taxonomía** (clima/audio vs 5 macro-verticales), **ecosistemas faltantes** (MQTT, Smart Life), y **dedup + prioridad** como clave técnica. También alerta del contraste del cian y de 2FA/race-conditions.

**Discrepancia resuelta:** GPT-5 priorizó la barra sticky de gateways; Argon priorizó el dedup. **Se adoptan ambas** (barra sticky + motor dedup), porque son complementarias.

---

## G · Resultado final esperado

El OWNER considera superado el rediseño cuando: (1) al abrir Kaenori, el **menú izquierdo muestra las verticales auto-descubiertas** (no un botón); (2) el **chat derecho narra el descubrimiento en vivo** ("he encontrado X en clima"); (3) los **gateways aparecen primero como checklist sticky** y al conectar uno se re-lanza el discovery; (4) por vertical, cada equipo se instala por **local (check conectado)** o **cloud (modal credenciales/2FA)** con verificación de lectura real; (5) **no hay duplicados** entre hub y mDNS; y (6) todo con los **colores/logo ya definidos** y en `es`+`ca`.
