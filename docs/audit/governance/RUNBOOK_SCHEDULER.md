# RUNBOOK SCHEDULER — Ejecución autónoma por rondas

**Mecanismo:** la **continuación por rondas (goal)** es el scheduler oficial. **1 ronda = 1 tick.**

## Ritmo

- Una ronda por sub-tarea, con **checkpoint** al cierre.
- Antes de ejecutar: registrar objetivo de la ronda.
- Al cerrar: registrar **heartbeat** en `ESTADO_ACTUAL.md` (ronda N, commit/hash, estado de tests, coste).

## Condiciones de parada

- Gasto ≥ 5 € (tope) · error crítico / regresión bloqueante · fin de objetivo · sin grant vigente.

## Anti-bucle

- Si **2 rondas seguidas** no producen cambios en repo/tests → **detener** y registrar el bloqueo.

## Reintento

- Hasta 2 reintentos por sub-tarea con backoff y registro de causa.
