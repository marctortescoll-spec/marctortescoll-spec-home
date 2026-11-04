# RESUMEN EJECUTIVO ACTUALIZADO — cierre técnico R14 + plan de ingeniería
## Para el propietario · 2026-11-04 · A0

## Dictamen (se mantiene): NO PREPARADO PARA PRODUCCIÓN

Con las rectificaciones de R14, la fotografía es más precisa pero la conclusión no cambia: **el gobierno
y la lectura son sólidos; la recuperación física segura (CCR) y la capa de Oportunidades siguen sin
codificarse, y no hay instalación real verificada.**

## Qué corrigió la ronda 14 (6 rectificaciones)

1. **Sí hay actuación codificada** (`home_ha_adapter_core/action_executor.py`) — no "solo-lectura".
2. **Egress con camino nftables codificado** (`apply_egress.py`) — pero sin verificar en hierro.
3. **Backup tiene test** (`tests/test_backup.py`).
4. **Tests = ~2563 casos** (180 ficheros); **CI solo ejecuta 1558** (deja fuera ~39 %).
5. **Taxonomía en `home_onboarding/vocabulary.py`** (no `home_connectivity`).
6. **`home_discovery_source` es el EMS activo**; `home_ems` parece muerto (sin consumidores ni servicios).

## Precisiones del propietario (5 comprobaciones — resultado)

1. **EMS:** `home_discovery_source` (vía `home_node`) es el activo; `home_ems` no se elimina sin migración
   segura acreditada.
2. **Tests/CI:** 180 ficheros = **~2563 casos**; el CI solo corre 1558. Hay que ampliarlo.
3. **Egress:** nftables codificado pero **fail-open al detener el servicio** y **no persistente en reboot**;
   DNS grueso (sin granularidad de puerto). El aislamiento **no está garantizado**.
4. **Actuación HA:** **envía primero y verifica después** → "no confirmado" ≠ "sin efecto físico". Además,
   la verificación de consignas de clima **compara el modo, no la temperatura** (bug).
5. **CCR Clase B:** el timeout de software **no sustituye una garantía física**. Clase B sin fallback
   hardware (relé) no debe usarse para actuación desatendida relevante para seguridad.

## Decisión arquitectónica (aprobada por el concilio, pendiente de tu confirmación)

**Recuperación sin bypass general**: preferencia por **CCR Clase A** (TTL del dispositivo); para **Clase B**,
**safe-stop acotado a estado seguro estático con timeout duro**, nunca un canal privilegiado abierto.

## Plan de ingeniería propuesto (P0–P2)

- **P0 (cimientos):** CI completo → verificación en hierro (A3) → consolidar EMS → taxonomía única →
  máquina kill-switch+CCR.
- **P1 (experiencia + actuación segura):** Override → verificación correcta de actuación → Oportunidades →
  dashboard narrativo.
- **P2 (piloto):** una oportunidad energética sobre un equipo **Clase A verificado**.

## Lo que requiere tu autorización

1. **Confirmar la decisión arquitectónica** (no-bypass + Clase A + safe-stop acotado).
2. **Aprobar el Plan Maestro P0–P2** como base de ingeniería.
3. Cuando toque: **mandato A3** para instalación/egress en hierro (P0.2) y para el piloto con actuación
   (P2.1). **No se toca código ni se mergea `audit/global-v1.3` a `main` sin tu autorización expresa.**

*Documento A0. No autoriza implementación, despliegue ni actuación física.*
