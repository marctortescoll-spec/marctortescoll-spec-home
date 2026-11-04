# INFORME FINAL — Continuidad V1.3 · redespliegue y experiencia real
## Funcionalidades operativas / simuladas / pendientes · 2026-11-04

**Código:** `integration/connectors-20261022 @ 117d7b6` (UI conectada + fuentes distinguidas).

---

## 1 · Funcionalidades OPERATIVAS (verificadas en HOME0001)

- Interfaz Liloo (PWA) en `https://192.168.1.196:8443` — carga, salud OK (`version 1.1.0`).
- API de producto (wizard, discovery, catalog, verticals, ems/shadow, security, enrollment, onboarding).
- Resiliencia (A3-E2 A+B) y actualización sin pérdida (A3-E1).
- Suite CI: **2602 tests en verde**.

## 2 · Funcionalidades SIMULADAS (marcadas, sin inventar)

- **Oportunidad energética** en el panel (sección "Oportunidad energética"): nivel 85/100, consigna 35 °C,
  con etiqueta **"simulado"** y `surplus_source`/`tariff_source` = `"simulado"`.
- Demo del recorrido: `demo/journey.py` (11 pasos, módulos reales, datos simulados).

## 3 · Funcionalidades PENDIENTES de integración

- **Lecturas reales de energía/tarifa**: `opportunity_preview` ya detecta cuántos sensores de energía
  hay en HA (`energy_sensors_disponibles`); falta **cablear la fuente real** (excedente solar + tarifa)
  para pasar de "simulado" a "real".
- **Fuente de tarifa**: no existe hoy en HA; se necesita un origen verificado (proveedor/API de solo
  lectura).

## 4 · Funcionalidades que requieren AUTORIZACIÓN FÍSICA

- **Redespliegue de `117d7b6` a HOME0001** (Kaenori solo; sin tocar AdGuard/DNS/router/HA).
- **Prueba C (reinicio completo)** — requiere resolver DNS doméstico dentro del producto.
- **Etapa 3 (dispositivo CCR Clase A)** · actuación en vivienda real.

## 5 · VPN de ingeniería (separación preparada)

La VPN `wg0` de HOME0001 termina hoy en el Xeon (`192.168.1.34`) — es **canal de gestión del
laboratorio**, no parte del producto. Ficha preparada para migrar a un endpoint propio (BDM/P1):
[FICHA_SEPARACION_VPN.md](FICHA_SEPARACION_VPN.md).

## 6 · Qué falta para la demo web verificable

1. Tu **confirmación del redespliegue** (grant A3 para el SHA `117d7b6`).
2. Tras desplegar: verificar 5 servicios + `/api/opportunity` + UI (pasos de validación ya definidos).

---

*Informe A1/A2. No autoriza A3 ni despliegue.*
