# RESUMEN EJECUTIVO — Paquetes P0.1 y P0.3
## Preparación A0 · para autorización A1/A2

## Objetivo

Preparar (sin implementar) los dos primeros paquetes de ingeniería: **P0.1 (CI/calidad)** y **P0.3
(consolidación EMS)**. Ambos reutilizan código existente y no alteran comportamiento.

## Los dos paquetes

| Paquete | Qué prepara | Rama futura | Entregable clave |
|---|---|---|---|
| **P0.1 — CI y calidad** | Inventario de ~2563 casos en 5 clases de riesgo; procedimiento seguro de recogida; suite de contratos Core + guardrails `HomeActionExecutor`; ampliación mínima de CI; rollback | `engineering/p0-1-ci` | diagnóstico CI + propuesta workflow + criterios |
| **P0.3 — Consolidación EMS** | Mapa de consumidores (estático + dinámico); matriz de equivalencia entre `home_ems` y `home_discovery_source`; recomendación canónica; migración progresiva reversible | `engineering/p0-3-ems` | matriz de equivalencia + recomendación + plan migración |

## Dependencias entre paquetes

- **P0.1 antes que P0.3**: la línea base de CI (suite completa + contratos) debe existir **antes** de
  validar la equivalencia EMS con pruebas de no-regresión. P0.3 usa la suite endurecida de P0.1.

## Riesgos principales

1. **P0.1:** efectos secundarios en *import-time* al recoger los ~1005 casos fuera de `tests/` → mitigado
   con recogida aislada (sin red/hardware).
2. **P0.1:** regresiones ocultas al ampliar CI → mitigado con dry-run en rama + rollback versionado.
3. **P0.3:** dependencia **oculta** de `home_ems` (carga dinámica/plugins) no visible en AST → mitigado con
   búsqueda no-estática (importlib/entry_points/config/systemd).
4. **P0.3:** romper una capacidad exclusiva al consolidar → mitigado con matriz de equivalencia + feature
   flag + retirada solo en paquete posterior.

## Orden recomendado

**P0.1 primero** (cimientos de calidad), **P0.3 después** (sobre la suite endurecida). Ambos son
preparatorios; **no implementan, no eliminan, no despliegan, no tocan `main` ni `Core`**.

## Estado y siguiente paso

- Ambas fichas están **redactadas (A0)** y **pendientes de revisión independiente 07-auditor**.
- Se solicita **autorización A1/A2 específica para cada paquete** (separadas), para abrir sus ramas
  (`engineering/p0-1-ci`, `engineering/p0-3-ems`) y ejecutar la preparación real (análisis, no código).
- **No se inicia implementación** por el mero hecho de aprobar esta planificación.

*Resumen A0. No autoriza implementación.*
