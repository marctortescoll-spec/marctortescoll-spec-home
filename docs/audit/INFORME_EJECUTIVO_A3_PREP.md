# INFORME EJECUTIVO — Preparación A3 HOME0001
## Cierre de bloqueos A1/A2 + solicitud A3 acotada · 2026-11-04

**Rama:** `integration/connectors-20261022` · commit `4c9567f`.

---

## 1. CI reproducible y estado de las pruebas

- **Dependencia `requests`**: estaba ausente (usada por `home_voice`/`home_ha_adapter`). Fijada en
  `dependencies = ["requests>=2,<3"]`.
- **`test_wizard_proxy.py`**: fallaba por colisión de puerto (el `0` efímero era tratado como falsy y
  caía a 8443). Fijado `port if port is not None else config.port` + `server_close()` + `port=0`.
  **Ahora incluido** en CI (ya no se excluye).
- **Suite completa**: **2597 passed, 5 skipped, 269 subtests** (0 failed), reproducible.

## 2. Cierre del riesgo de permisos de safe-stop

Durante `ARMING_SAFE_STOP`, la recuperación ahora exige **dos condiciones** (no bypass):
1. La acción con **recurso concreto** (`capability.resource.action`) está en la lista blanca.
2. El **principal** tiene regla normal para esa acción.

Verificado: revert OK para principal+recurso autorizados · denegado para otro principal · denegado para
otro recurso · deny-all tras timeout · **tras reinicio la ventana se pierde → deny-all** (fail-safe).

## 3. Dictamen independiente de seguridad (ronda 27)

- GPT-5: aprueba-con-cambios (test de reinicio exigido → **añadido**) · Argon: **aprueba**.
- Ambos confirman: sin bypass por identidad ni por recurso; comportamiento fail-safe ante
  reinicio/pérdida de proceso/expiración.

## 4. Estado y preparación de HOME0001

- Equipo de banco de pruebas (**t520/HOME0001**); **no** presta servicios productivos (a verificar en
  la Etapa 1 antes de instalar).
- Inventario de servicios/dispositivos accesibles y copia de seguridad previa: **parte de la Etapa 1**
  (requiere acceso físico, pendiente de A3).

## 5. Procedimiento de instalación y rollback

Ver [EXPEDIENTE_A3_HOME0001.md](EXPEDIENTE_A3_HOME0001.md): instalación por SHA (`install.sh` +
first-boot + bootstrap + fingerprint), validación de servicios/red/arranque, verificación de aislamiento
de red (egress persistente), pruebas de reinicio/recuperación, y **rollback que restaura el estado
seguro anterior** (no borrado indiscriminado de reglas/servicios).

## 6. Solicitud A3 concreta y acotada

**Etapa 1** (instalación y diagnóstico en HOME0001, sin control de aparatos reales) — requiere **grant
A3 explícito** para ese equipo. **Etapas 2–3** (recuperación/resistencia y dispositivo CCR Clase A)
requieren **autorizaciones A3 adicionales** por separado. No hay autorización implícita por aprobar este
plan.

---

*Informe A1/A2. No autoriza A3, despliegue, reinicio, alteración de red ni control de hardware.*
