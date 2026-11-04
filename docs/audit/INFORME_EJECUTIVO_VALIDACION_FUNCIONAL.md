# INFORME EJECUTIVO — Validación funcional de Kaenori HOME00X (V1.3)
## Qué funciona de verdad, qué falta conectar, y cómo probarlo · 2026-11-04

**Rama de producto:** `integration/connectors-20261022 @ 018c745` (desplegado en HOME0001).

---

## 1. Qué funciona realmente de extremo a extremo

Verificado en HOME0001 (HTTPS `https://192.168.1.196:8443`):

- **Interfaz web de Liloo** carga (`Kaenori · Liloo`, manifest/icono, PWA). ✔
- **API de producto rica y viva**, protegida por emparejamiento (deny-by-default):
  `health` · `pair` · `wizard/catalog` · `wizard/scenarios` · `wizard/sessions` · `discovery` ·
  `catalog` · `catalog-summary` · `verticals` · `ems/shadow` · `security/status` · `enrollment` ·
  `onboarding` · `summary` · `metrics`. ✔ (401 hasta emparejar, correcto)
- **Resiliencia** (A3-E2 A+B): reinicio de servicios 1 a 1 y caída de proceso → recuperación, con
  deny-by-default y datos conservados. ✔
- **Actualización controlada** (A3-E1): `14e1f62` → `018c745` sin pérdida, con backup y rollback. ✔

## 2. Qué está implementado pero todavía NO conectado

Módulos construidos y probados en P0–P2, pero **aún no cableados** al recorrido de usuario (no hay
endpoint/UI que los invoque):

| Módulo | Estado | Dónde vive |
|---|---|---|
| Oportunidades (ciclo 15 estados + evidencia A/B/C) | librería + tests, **sin endpoint** | `home_opportunity/lifecycle.py` |
| Dashboard narrativo (3 secciones) | librería + tests, **sin endpoint** | `home_opportunity/dashboard.py` |
| Override manual (hold + invalidación) | librería + tests, **no cableado** | `home_ems/override.py` |
| Safe-stop acotado | **SÍ conectado** a `AuthorizationPolicy` | `home_security/safe_stop.py` → `policy.py` |
| Verificación por atributo (consignas clima) | **SÍ conectado** | `home_ha_adapter_core` |

**Gap clave:** el flujo "Liloo detecta oportunidad → simular → autorizar → probar → verificar" **no
está expuesto** en la UI/API del producto (existe el cálculo EMS en `/api/ems/shadow`, pero no la
orquestación del ciclo de Oportunidades con Override y evidencia).

## 3. Qué falta para presentar Kaenori a un cliente

1. **Cablear Oportunidades → experiencia**: un endpoint/UI que recorra el ciclo (descubrir → simular →
   autorizar → probar → verificar → explicar) usando `home_opportunity` + `home_ems` + `override`.
2. **Dashboard narrativo real** (qué sucede / qué podría mejorar / qué resultados), hoy solo librería.
3. **Demostración del recorrido completo** con datos simulados (sin tocar dispositivos).

## 4. Problemas corregidos (esta sesión)

- Regresión S11 (contrato UI) · CI ampliado (2597 verdes) · fuga de puerto wizard_proxy ·
  dependencia `requests` · safe-stop acotado por principal+recurso · verificación de consignas por
  atributo · backup fail-closed + restore con snapshot · actualización y resiliencia en HOME0001.

## 5. Demostración que puedes abrir y probar

**Abre en tu navegador:** `https://192.168.1.196:8443` (Kaenori · Liloo). Health verificado:
`{"status":"ok","version":"1.1.0","tls":true}`.

*Nota:* el certificado es autofirmado (acepta la advertencia del navegador). El recorrido completo
(wizard/discovery/oportunidades) requiere emparejamiento; puedo guiarte el emparejamiento y prepararte
una **demo con datos simulados** del ciclo de Oportunidades (Fase 2) sin tocar aparatos.

## 6. Autorizaciones físicas que siguen pendientes

- **Prueba C (reinicio completo)**: pendiente; requiere resolver el DNS doméstico (AdGuard) **dentro del
  producto**, no en el Xeon (propuesta A3-DNS suspendida).
- **Etapa 3 (dispositivo CCR Clase A)**: requiere A3 adicional (dispositivo, acciones, límites, parada).
- **Actuación física / vivienda real**: requiere A3 con target + rollback.

---

*Informe A1/A2. No autoriza cambios físicos ni de red.*
