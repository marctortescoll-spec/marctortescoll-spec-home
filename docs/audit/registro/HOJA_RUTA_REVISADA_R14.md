# HOJA DE RUTA REVISADA — R14
## Dependencias y orden hacia el primer piloto energético (tras rectificaciones)

## Cambios respecto a la hoja de ruta original

- Se **elimina** el bloqueo "sin capacidad de escritura" (existe `home_ha_adapter_core`).
- Se **matiza** el egress: codificado (nftables) pero sin verificar (A3).
- El **bloqueo dominante sigue siendo CCR + safe-stop + Override + instalación real**.

## Orden recomendado (P0–P2)

| Paso | Acción | Fase | Dependencia previa |
|---|---|---|---|
| 1 | **Instalación real + ejecución de la suite (132 tests) + egress enforcement verificado** | P0 | entorno controlado (A3 posterior) |
| 2 | **CCR Clase A/B + safe-stop acotado** (sin bypass general; Clase A preferente, Clase B bounded) | P0 | decisión arquitectónica R14 |
| 3 | **Override** (intervención manual prevalece + invalida evaluación) | P1 | CCR |
| 4 | **Oportunidades** (5 momentos + A/B/C) sobre EMS shadow + `home_ha_adapter_core` | P1 | CCR + Override |
| 5 | **Consolidar EMS** (decidir `home_discovery_source` vs `home_ems`) | P1 | — |
| 6 | **Proyección 12→5** (tabla declarativa) | P1 | — |
| 7 | **Piloto de una oportunidad energética** | P2 | 1–4 |

**Desacuerdo de orden (registrado en actas):** entorno primero (Argon) vs CCR primero (GPT-5/orquestador).
Esta hoja de ruta resuelve el desacuerdo colocando **instalación real + suite + egress** como P0-paso-1,
y **CCR/safe-stop** como P0-paso-2 (ambas críticas; el entorno se verifica primero porque condiciona la
validez de cualquier prueba de CCR).

*Hoja de ruta A0. No autoriza implementación.*
