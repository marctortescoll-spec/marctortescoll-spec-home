# KAENORI — MANDATO PREPARATORIO DE INGENIERÍA P0.1 + P0.3
## Propuesta pendiente de autorización A1/A2 del OWNER

**Referencia:** Experience V1.3 congelada + Auditoría Global R14.
**Fuente canónica:** `marctortescoll-spec/home @ 549700b9c25c2336af8f699cbc1cf1270145fc96`.
**Objetivo:** preparar el primer incremento de ingeniería controlado, reutilizando el código existente y
**sin alterar el comportamiento del producto**.

---

## 1. P0.1 — Calidad y CI

Revisar la totalidad de pruebas existentes y la configuración de integración continua. Separar las
pruebas en: unitarias y de contrato (sin efectos externos) · integración simulada · pruebas que requieren
servicios externos · infraestructura/privilegios · hardware/dispositivos reales.

Determinar el número de **casos realmente recogidos por pytest** y obtener, cuando esté autorizado, sus
resultados reproducibles. Preparar una ampliación de CI con **suites independientes**, sin mezclar pruebas
seguras con pruebas que puedan producir efectos sobre equipos.

**Criterio de aceptación:** cobertura de las suites previstas, resultados trazables y ausencia de
regresiones, distinguiendo claramente pruebas excluidas o pendientes de entorno.

## 2. P0.3 — Consolidación EMS

Auditar las dependencias reales de `home_discovery_source/ems.py`, `home_discovery_source/ems_service.py`
y `home_ems/`. Identificar para cada implementación: funcionalidades disponibles · servicios y puntos de
entrada · consumidores reales · tests asociados · capacidades únicas no presentes en la alternativa ·
dependencias del Wizard, Core, Liloo y futuras Opportunities.

Proponer una **implementación canónica**, manteniendo las capacidades que deban reutilizarse. **No
eliminar ni deprecar código automáticamente**: la retirada será un paquete posterior, con pruebas de
equivalencia y autorización específica.

## 3. Gobierno

- Respetar los **contratos congelados de Core** y las decisiones de Experience V1.3.
- Paquetes de trabajo separados, con **ramas de desarrollo propuestas**, criterios de aceptación y
  revisión independiente de `07-auditor`.
- **No modificar `main`** directamente.
- **No activar `HomeActionExecutor`**, no alterar permisos, no modificar dispositivos ni desplegar software.

## 4. Entregables al propietario

1. Diagnóstico de CI: cobertura real, exclusiones y dependencias.
2. Propuesta de modificación mínima del workflow.
3. Mapa completo de dependencias EMS.
4. Recomendación de implementación canónica y plan de migración.
5. Riesgos, criterios de aceptación y plan de reversión.
6. Dictamen independiente del concilio.

Presentar ambos paquetes para aprobación A1/A2.

> Este mandato autoriza **preparar y revisar** la ejecución, pero **no implementar cambios** hasta la
> aprobación expresa del propietario. El objetivo es empezar a construir con garantías, no seguir
> auditando indefinidamente.

## 5. Condiciones del concilio (ronda 15) — incorporadas

1. **Aislamiento en P0.1:** el descubrimiento de casos fuera de `tests/` en entorno aislado sin red ni
   hardware.
2. **Suite de contratos Core + guardrails `HomeActionExecutor`** como categoría prioritaria independiente.
3. **Búsqueda no-estática de dependencias `home_ems`** (importlib/entry_points/config/systemd/CLI).
4. **P0.3 exige Matriz de Equivalencia Funcional y de Contratos** antes de recomendar canónico.
5. **CI endurecido:** etiquetado pytest, matriz segura-por-defecto, dry-run + rollback versionado.
6. **Sin cambios de dependencias/lockfiles** salvo reproducibilidad controlada.

*Veredicto: aprueba-con-cambios (2/2). Pendiente de aprobación expresa del propietario.*
