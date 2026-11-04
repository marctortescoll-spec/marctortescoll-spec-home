# ACTA DEL CONCILIO — Ronda 14 · 2026-11-04

**Concilio:** DeepSeek (orquestador), GPT-5, Gemini 4 Argon · **A0**.
**Objeto:** contraste técnico R14 y decisión de recuperación segura.

## Rectificaciones

Se confirman las 10 rectificaciones de `RECTIFICACIONES_R14.md`, incluidas:

- backup puede declarar OK un archivo incompleto (`backup.sh` oculta fallo de `tar`);
- 180 ficheros (no 203) y **2533 casos**; CI solo ejecuta **1569** (38,1 % fuera);
- clima no verifica consigna (`expected_state=None`); la orden sale antes;
- egress sin snapshot/rollback y fail-open al parar;
- taxonomía en `home_onboarding`; `home_discovery_source` activo, `home_ems` sin consumidores;
- kill-switch solo deny-all; safe-stop/CCR no implementados.

## Votación arquitectónica

| IA | Voto | Matiz |
|---|---|---|
| GPT-5 | **aprueba-con-matices** | Safe-stop estático, timeout, lista revert-only, auditoría, sin bypass |
| Gemini 4 Argon | **aprueba-con-matices** | Clase A preferente; Clase B sin hardware solo supervisada; riesgo de depender de red |
| DeepSeek | **aprueba-con-matices** | No bypass; CCR A; Clase B acotada; no habilitar actuación hasta verificación por atributo |

## Desacuerdos registrados

1. **Orden**: Argon quiere entorno/instalación primero; GPT/orquestador quieren
   CCR primero. Se acuerda backup+CI → CCR → actuación.
2. **Clase B**: Argon exige fallback hardware para uso desatendido; GPT acepta
   safe-stop acotado solo en transición viva. Se refleja como “solo supervisada”
   si no hay relé/TTL.
3. **Alcance**: la decisión no autoriza implementación; requiere Owner.

## Estado

- Rectificaciones publicadas en rama documental.
- Ninguna actuación física, despliegue ni cambio de permisos.
- Sin A3.

## Aclaraciones finales del Owner (R14 cerrada como auditoría)

1. **CI**: 2533 son casos *recopilados*, no superados. En `549700b9` el CI real
   da **1 failed, 1563 passed, 5 skipped, 71 subtests**; `tests/synthetic` queda
   fuera (18 recopilados + 3 errores). El fallo es `test_s11_read_only_ready_gate`.
2. **Egress**: la unidad `kaenori-egress.service` está **enabled/active** en
   HOME0001 (`WantedBy=multi-user.target`). No se puede afirmar categóricamente
   que no se recupera tras reboot; queda **no verificado** si la tabla nftables
   está presente y es efectiva (sin root no se puede listar). Stop sigue siendo
   fail-open; no hay snapshot/rollback.
3. **Pilotos**: separar **solo lectura** (sin CCR) de **control físico** (CCR,
   safe-stop, verificación por atributo y A3).

**Estado:** R14 cerrada como auditoría técnica. **No autoriza desarrollo,
despliegue ni actuación.** Producción sigue bloqueada.
