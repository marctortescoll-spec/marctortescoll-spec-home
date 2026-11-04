# ACTA DEL CONCILIO — Ronda 14 · 2026-11-04

**Concilio:** DeepSeek (orquestador), GPT-5, Gemini 4 Argon · **A0**.
**Objeto:** contraste técnico R14 y decisión de recuperación segura.

## Rectificaciones

Se confirman las 9 rectificaciones de `RECTIFICACIONES_R14.md`, incluidas:

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
