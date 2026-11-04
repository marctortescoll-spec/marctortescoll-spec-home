# ÁREA B — Core, gobierno, seguridad y recuperación
## Informe técnico de soporte · Fase 1B · Auditoría Global V1.3

**Fuente canónica:** `marctortescoll-spec/home` · `integration/connectors-20261022` @
`549700b9c25c2336af8f699cbc1cf1270145fc96` · contrastado con `main @ d3a1a84b`.
**Naturaleza:** lectura (A0). Sin modificar código ni ejecutar pruebas.

## Resumen

El **núcleo de autoridad está bien construido** (6 puertos + política deny-by-default + kill-switch
fail-closed), pero las **garantías de recuperación física (CCR, Override, reversión por dispositivo)
existen solo en la especificación V1.3, no en el código**. El egress jail es declarativo y **sin
enforcement real**. Estado global: **parcial — seguridad de decisión fuerte, recuperación física
pendiente**.

## Evidencia por capacidad

| Requisito V1.3 | Código (commit 549700b9) | Tests | Estado |
|---|---|---|---|
| Core contrato + 6 puertos | `core/kaenori_core/runtime/ports.py` (6 Protocol) | `core/tests/` | implementado (codificado) |
| AuthorizationPolicy deny-by-default + fail-closed | `home_security/policy.py` | `core/tests/test_he_auth*.py` | implementado |
| Kill-switch persistente (fail-closed) | `home_security/killswitch.py` | — (ver policy tests) | implementado |
| Secretos efímeros (TTL) | `home_security/ephemeral.py` | — | implementado |
| Egress jail | `home_security/egress.py` + `apply_egress.py` + `egress_compile.py` | — | **parcial** (declarativo; `is_enforced` siempre `False`) |
| HA como adaptador (solo lectura) | `core/home_ha_adapter/readonly_snapshot.py` | `core/tests/test_ha_readonly_snapshot.py` | parcial |
| Identidad del nodo | `home_identity/` + `first-boot.sh` (Ed25519) | `factory/tests/test_first_boot.sh` | parcial |
| **CCR Clase A/B + safe_stop** | **NO CODIFICADO** (grep sin resultados) | — | **pendiente (solo spec Anexo A)** |
| **Override (intervención manual)** | **NO CODIFICADO** | — | **pendiente (solo spec)** |
| **Recuperación física de un dispositivo ya ordenado** | **NO CODIFICADO** (kill-switch solo deniega nuevas) | — | **pendiente / bloqueado** |

## Hallazgos críticos (Área B)

1. **⚠️ Kill-switch NO recupera equipos.** `home_security/killswitch.py` implementa un flag persistente
   que, armado, **deniega toda acción nueva** (deny-all vía `policy.allows`). **No existe** en el código
   un mecanismo independiente que **recupere** un dispositivo que ya recibió una orden (p. ej. restaurar
   consigna). Esto responde a la pregunta prioritaria del mandato: **solo bloquea nuevas órdenes; la
   recuperación física no está implementada ni probada.**
2. **⚠️ CCR (Clase A/B) solo en especificación.** No hay clasificación de capacidad de reversión por
   equipo, ni `safe_stop`, ni TTL de *hardware* en el código (el TTL existente es para secretos
   efímeros, alarmas y estado de sinergias, no para dispositivos).
3. **⚠️ Override no codificado.** La regla "la intervención manual prevalece e invalida la evaluación"
   (V1.2 §8, Anexo A §7) no tiene implementación identificable en esta rama.
4. **Egress jail sin enforcement.** `home_security/egress.py` declara explícitamente: "el enforcement
   real queda como hueco documentado; `is_enforced` devuelve siempre `False`". El cortafuegos no se
   aplica desde este conector.
5. **Fortaleza confirmada:** la política de autorización es correcta (deny-by-default, fail-closed,
   kill-switch primero, sin firmar/grants/decisión del Owner). El contrato de 6 puertos coincide con el
   congelado V1.0.

## Próximos pasos verificables (Área B)

- Implementar (en un mandato posterior) la **CCR** (Clase A/B) y el **safe_stop** por dispositivo,
  verificado físicamente — es el bloqueo de seguridad más importante antes de cualquier prueba de
  actuación.
- Implementar el **Override** (suspensión por intervención manual + invalidación de evaluación).
- Decidir el **enforcement real del egress jail** (nftables/cgroups) — hoy solo declarativo.

*Informe de auditoría (A0). No autoriza cambios de código, Core ni despliegues.*
