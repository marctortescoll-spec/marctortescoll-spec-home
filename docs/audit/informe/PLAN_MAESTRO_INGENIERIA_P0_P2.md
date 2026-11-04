# PLAN MAESTRO DE INGENIERÍA — P0–P2
## Propuesta de ejecución priorizada · materializar Experience V1.3

**Naturaleza:** planificación documental (A0). No autoriza implementación, despliegue ni actuación.
**Principio rector:** **reutilizar lo que ya funciona** antes de construir nada nuevo. Cada paquete de
trabajo (WP) incluye: objetivo, trabajo mínimo, dependencias, riesgo, criterio de aceptación y puerta.

---

## P0 — Cimientos: producto fiable y seguro

| WP | Objetivo | Trabajo mínimo | Dependencias | Riesgo | Aceptación | Puerta |
|---|---|---|---|---|---|---|
| P0.1 | **CI completo** | Ampliar `ci.yml` a `pytest tests/ tests_bdm/ core/tests/` + script factory | — | regresiones ocultas | CI verde sobre ~2563 casos | A1/A2 |
| P0.2 | **Verificación en hierro** | Instalar `factory` en nodo laboratorio, arranque/reboot, egress nft activo y **persistente** (save/restore), backup/restore | P0.1 | fallo físico | evidencia de install+boot+reboot+egress | **A3 (mandato aparte)** |
| P0.3 | **Consolidar EMS** | Confirmar `home_discovery_source` canónico; deprecar `home_ems` con migración segura (no borrado sin acreditar) | — | romper dependencia oculta | un solo camino EMS + tests | A1/A2 |
| P0.4 | **Taxonomía única + proyección 12→5** | Una fuente canónica de 12 categorías + tabla declarativa → 5 verticales | — | dispersión | tabla + tests | A1/A2 |
| P0.5 | **Máquina kill-switch + CCR** | Estados `ARMED_DENY_ALL`/`ARMING_SAFE_STOP`; safe-stop **estático** (no "previo") con timeout duro; **sin bypass**; evidencia Clase A por dispositivo | P0.3 | colisión kill-switch↔CCR | tests de estados + fail-closed | A1/A2 |

## P1 — Experiencia unificada + actuación segura

| WP | Objetivo | Trabajo mínimo | Dependencias | Riesgo | Aceptación | Puerta |
|---|---|---|---|---|---|---|
| P1.1 | **Override** | Detección de cambio manual → suspensión + invalidación de evaluación | P0.5 | pelea humano-automático | tests de conflicto | A1/A2 |
| P1.2 | **Verificación de actuación correcta** | Read-back del **atributo correcto** (consignas = temperatura, no modo); idempotencia; timeout | P0.5 | orden sin confirmación | tests de setpoint | A1/A2 |
| P1.3 | **Oportunidades (5 momentos + A/B/C)** | Tarjeta sobre EMS shadow + `home_ha_adapter_core` + policy (sin tocar Core) | P0.5, P1.1, P1.2 | sobrepromesa | oportunidad end-to-end en shadow | A1/A2 |
| P1.4 | **Dashboard narrativo + continuidad** | Contrato visual Liloo (barra persistente, estados, Revertir/Ver resultados); continuidad por usuario | — | UX sin datos reales | UI + estados verificables | A1/A2 |

## P2 — Inteligencia que demuestra valor (primer piloto)

| WP | Objetivo | Trabajo mínimo | Dependencias | Riesgo | Aceptación | Puerta |
|---|---|---|---|---|---|---|
| P2.1 | **Piloto de UNA oportunidad energética** | Una casa piloto, un equipo **Clase A verificado** (TTL nativo), una oportunidad (p. ej. excedente solar → climatización) | P0.2, P0.5, P1.1–P1.3 | actuación física | recorrido completo con evidencia A/B/C y recuperación | **A3 (mandato aparte)** |

## Camino crítico (orden real)

`P0.1 CI → P0.2 hierro → P0.3 EMS → P0.4 taxonomía → P0.5 CCR → P1.1 Override → P1.2 verificación →
P1.3 Oportunidades → P1.4 dashboard → P2.1 piloto`

## Puertas de autorización (para el propietario)

1. **A1/A2** (desarrollo en worktree + integración) — cada WP técnico lo tramita el departamento con
   auditoría independiente (`07-auditor`), **sin tocar Core v1.0 sin bump+auditoría**.
2. **A3** (entorno/hardware) — P0.2 (instalación/egress en hierro) y P2.1 (actuación física) requieren
   **mandato aparte del propietario** con entorno controlado.

## Riesgo global

El mayor riesgo sigue siendo **físico**: activar actuación sin CCR verificada. Por eso P0.5 (CCR) y P1.1
(Override) van **antes** que P2.1 (piloto con actuación), y P2.1 exige **Clase A verificada**.

*Plan A0. No autoriza implementación; es la propuesta de ejecución priorizada para tu aprobación.*
