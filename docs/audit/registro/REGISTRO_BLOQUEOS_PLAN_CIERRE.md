# REGISTRO DE BLOQUEOS Y PLAN DE CIERRE
## Soporte · Auditoría Global V1.3

**Naturaleza:** registro priorizado de incidencias/limitaciones/riesgos y tareas mínimas (A0).

| # | Bloqueo / limitación | Efecto en el cliente | Acción mínima para cerrarlo | Fase |
|---|---|---|---|---|
| 1 | **Sin CCR (Clase A/B + safe_stop)** | Una orden ya enviada no se revierte si el equipo se desconecta (riesgo físico) | Codificar `safe_stop` con **canal privilegiado** (resolver colisión kill-switch↔CCR) y verificar por tipo de equipo | P0 |
| 2 | **Instalación real no verificada** | No se sabe si la caja se instala/arranca/reinicia en hierro | Ejecutar `deployment/factory` en un nodo de laboratorio (mandato posterior con entorno controlado) y registrar evidencia | P0 |
| 3 | **Egress jail sin enforcement** | La caja no está aislada de Internet | Aplicar nftables/iptables + tests de fuga | P0 |
| 4 | **Sin Override** | La automatización puede "pelear" con la intervención humana | Codificar detección de cambio manual → suspensión de la oportunidad + invalidación de evaluación | P1 |
| 5 | **Oportunidades (5 momentos + A/B/C) no codificadas** | No existe la tarjeta que materializa la promesa | Codificar la tarjeta sobre EMS shadow + policy (sin tocar Core) | P1 |
| 6 | **Proyección 12→5 verticales no codificada** | Las verticales de producto no están conectadas a la taxonomía técnica | Añadir tabla declarativa categoría→vertical (sin cambiar conectores) | P1 |
| 7 | **Dashboard narrativo no codificado** | El cliente ve "panel técnico", no "casa que comprende" | Implementar el contrato visual mínimo de Liloo (Anexo D) | P1 |
| 8 | **Duplicación de código EMS** | Riesgo de mantenimiento y divergencia | Consolidar `home_ems` y `home_discovery_source/ems.py` | P1 |
| 9 | **CI/tests no ejecutados aquí** | No se conoce el estado real de la suite | Ejecutar suite en entorno controlado (mandato posterior) y registrar resultado | P0 |

**Plan de cierre (resumen):** cerrar 1→3 (P0, seguridad/entorno) antes que 4→7 (P1, experiencia); 8–9
como higiene. El **camino crítico** es: colisión kill-switch↔CCR → safe_stop → instalación real → Override
→ Oportunidades → piloto de una oportunidad energética.

*Registro A0. No autoriza desarrollo, despliegue ni actuación sobre equipos.*
