# INFORME EJECUTIVO FINAL — Validación funcional de Kaenori HOME00X (V1.3)
## Producto visible y funcional · estado real end-to-end · 2026-11-04

**Rama de producto:** `integration/connectors-20261022 @ 816b8c4` · **en HOME0001:** `018c745`
(pendiente re-despliegue de `816b8c4`).

---

## Resumen de una línea

**Kaenori ya es un producto visible y estable; el recorrido del cliente está cableado de punta a punta
en el código (y demostrable), y solo falta re-desplegar para verlo vivo en HOME0001.**

## 1 · Qué funciona de verdad end-to-end (verificado en el sistema vivo)

- **Interfaz Liloo** carga (PWA) en `https://192.168.1.196:8443` (`{"status":"ok","version":"1.1.0"}`).
- **API de producto rica y auth-gated**: wizard, discovery, catalog, verticals, ems/shadow,
  security/status, enrollment, onboarding, summary, metrics.
- **Resiliencia** (A3-E2 A+B): reinicios de servicio y caídas de proceso → recuperación, deny-by-default.
- **Actualización sin pérdida** (A3-E1): `14e1f62`→`018c745` con backup + rollback.

## 2 · Qué estaba implementado pero NO conectado → ahora CONECTADO

| Pieza | Antes | Ahora |
|---|---|---|
| Oportunidades (ciclo 15 estados + A/B/C) | librería suelta | **`OpportunityService` + ruta `/api/opportunity`** |
| Override manual | librería suelta | integrado en el servicio |
| Dashboard narrativo | librería suelta | usado por la demo |
| Safe-stop acotado | conectado a `AuthorizationPolicy` | (ya estaba) |
| Verificación por atributo (consignas) | conectado | (ya estaba) |

**Recorrido cableado:** `servidor (/api/opportunity) → brain.opportunity_preview() → OpportunityService
→ (EMS + ciclo de vida + Override + evidencia)`.

## 3 · Qué falta para presentar a un cliente

1. **Re-desplegar `816b8c4` en HOME0001** (para que `/api/opportunity` esté vivo) — pendiente de tu OK.
2. **Fuente real de excedente/tarifa** (hoy la vista previa marca "simulado").
3. **VPN de producto** (ver Fase 1): hoy el canal de gestión termina en el Xeon.

## 4 · Fase 1 — Aislamiento del producto (verificado)

- ✅ **DNS independiente**: el nodo usa DNS del ISP (`80.58.61.250/254`), **no** AdGuard. Kaenori no
  depende de AdGuard para funcionar.
- ✅ **Sin DHCP en el nodo**: Kaenori no modifica DHCP/DNS del router.
- ⚠️ **VPN de gestión (wg0)** termina en el **Xeon (`192.168.1.34`)** — es infraestructura de ingeniería,
  **no parte del producto** para el cliente. Debe sustituirse por un endpoint propio (BDM/P1).

## 5 · Problemas corregidos (esta sesión, verificados)

S11 (contrato UI) · CI 2597+ verdes · fuga de puerto wizard_proxy · dep `requests` · safe-stop acotado
principal+recurso · verificación consignas por atributo · backup fail-closed + restore con snapshot ·
actualización y resiliencia HOME0001 · **Oportunidades cableadas a la API**.

## 6 · Demostración funcional (accesible)

- **Producto vivo:** `https://192.168.1.196:8443` (Liloo · certificado autofirmado).
- **Recorrido simulado:** `PYTHONPATH=.:core python3 demo/journey.py` (commit `026b311`).
- **Endpoint nuevo:** `GET /api/opportunity` (vista previa solo lectura, `simulated:true`).

## 7 · Autorizaciones físicas pendientes

- Re-despliegue de `816b8c4` a HOME0001 (Kaenori solo, sin tocar nada más).
- Prueba C (reinicio completo) — requiere resolver DNS doméstico **dentro del producto**.
- Etapa 3 (dispositivo CCR Clase A) · actuación en vivienda real.

---

*Informe A1/A2. Enlaces verificables: commits en `marctortescoll-spec/home` (`integration/connectors-20261022`)
y documentos en `marctortescoll-spec-home` (`audit/global-v1.3/docs/audit/`).*
