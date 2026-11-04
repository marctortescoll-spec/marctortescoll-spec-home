# DECISIÓN ARQUITECTÓNICA — Recuperación segura frente al kill-switch
## Ronda 14 · propuesta para votación

**Estado:** propuesta A0; **no implementada**.

## Problema

`KillSwitch` armado implica deny-all (`policy.py:68-70`). Si la recuperación
física se implementa como una orden normal, el kill-switch la bloquea. No se
acepta un bypass general.

## Opciones

| Opción | Mecanismo | Riesgo | Veredicto |
|---|---|---|---|
| A. Bypass general | Saltar policy/kill-switch para cualquier orden | Alto; rompe deny-by-default | **Rechazar** |
| B. CCR Clase A | TTL/dead-man nativo del dispositivo; vuelve solo a estado seguro | Bajo; no depende de Kaenori | **Preferir** |
| C. Safe-stop acotado Clase B | Transición `stop-new` → `safe-stop` con lista blanca revert-only, auditoría y timeout | Medio; solo durante transición viva | **Aceptar con matices** |
| D. Watchdog local | Heartbeat + revert | Necesita canal; no resuelve pérdida de red | Complemento |

## Propuesta

1. **No bypass general.**
2. **CCR Clase A preferente**; verificar TTL/read-back por dispositivo antes de
   habilitar actuación.
3. **Clase B**: safe-stop acotado, **solo** durante transición de cierre, lista
   blanca revert-only, estado seguro **estático** (no “estado previo”), timeout
   duro, idempotente, auditado y con permisos normales (sin elevar al invocador).
4. Si Clase B no tiene fallback hardware (relé/TTL), **solo actuación
   supervisada**; no desatendida para seguridad.

## Riesgos actuales vs futuros

- **Hoy:** no hay actuación habilitada en producción; la colisión es latente.
- **Al habilitar:** sin CCR/safe-stop, la recuperación puede ser imposible.
- **Clase B sin hardware:** no cubre pérdida de red, alimentación ni caída de
  proceso; no debe prometerse como garantía física.

## Invariantes

Core v1.0, permisos, CCR, deny-by-default, auditoría y clasificación de acciones
se preservan. El safe-stop no eleva privilegios ni crea un canal abierto.
