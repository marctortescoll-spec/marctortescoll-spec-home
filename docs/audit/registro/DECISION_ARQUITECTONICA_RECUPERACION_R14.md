# DECISIÓN ARQUITECTÓNICA PROPUESTA — Recuperación segura frente al kill-switch
## Ronda 14 · para votación del concilio

## Problema

El kill-switch (`home_security/killswitch.py` + `policy.py`) es **fail-closed y deny-all**: armado,
deniega **toda** acción. Si se implementa la recuperación física (CCR) como una orden más, el
kill-switch armado **la bloquearía**, impidiendo devolver un equipo a su estado seguro.

## Criterios (del mandato)

No aceptar un *bypass* general sin análisis de amenazas. Preservar invariantes de Core, permisos y
clasificación CCR. Separar riesgos actuales de vulnerabilidades que solo aparecerían al habilitar
actuación futura.

## Opciones evaluadas

| Opción | Mecanismo | Riesgo | Veredicto |
|---|---|---|---|
| **A. Bypass privilegiado general** | Un canal que salta el kill-switch para cualquier comando | Alto: rompe deny-by-default; cualquier invocador lo usaría | **Rechazar** |
| **B. Recuperación autónoma del dispositivo (CCR Clase A)** | El equipo tiene TTL/dead-man nativo y vuelve solo a estado seguro al perder contacto | Bajo (no depende de Kaenori); solo Clase A | **Preferir siempre que exista** |
| **C. Safe-stop acotado (kill-switch en dos fases)** | Al armar kill-switch: 1º "stop-new" (deniega nuevas), 2º "safe-stop" emite **solo** comandos *revert-only* de una lista blanca auditada, durante la transición de cierre | Medio (bounded, auditable); no es bypass general | **Recomendado para Clase B** |
| **D. Watchdog local + heartbeat** | Watchdog detecta pérdida de conexión y dispara el revert | Requiere canal de envío (misma colisión) | Complemento, no solución |

## Propuesta (a votar)

1. **No** crear un *bypass* general sobre `AuthorizationPolicy`/kill-switch.
2. **CCR Clase A** (recuperación autónoma por TTL de hardware) es la vía preferente: no necesita
   canal Kaenori. Verificar evidencia real por dispositivo antes de habilitar actuación.
3. **CCR Clase B**: implementar **safe-stop acotado** — un paso del kill-switch que, **solo** durante la
   transición de cierre y **solo** sobre una lista blanca de acciones *revert-only* (restaurar consigna/
   modo/estado previo), emite los comandos de retorno y los audita. Nunca un permiso abierto.
4. El safe-stop hereda **permisos + límites + auditoría** de Core; no eleva privilegios del invocador.

## Riesgos actuales vs futuros

- **Actual (hoy, sin actuación habilitada):** kill-switch solo-lectura es suficiente; no hay actuación
  en producción, por lo que la colisión es **latente**, no explotable.
- **Futuro (al habilitar actuación):** la colisión se vuelve real; por eso CCR + safe-stop acotado deben
  implementarse **antes** de habilitar cualquier actuación.

*Documento A0 para votación del concilio. No autoriza implementación.*
