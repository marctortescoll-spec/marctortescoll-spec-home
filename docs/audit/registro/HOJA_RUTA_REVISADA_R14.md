# HOJA DE RUTA REVISADA — R14

## Correcciones que cambian el orden

1. **Backup**: corregir `backup.sh` (quitar `|| true`, validar tar/contenido) y
   añadir test de fallo. Antes de considerar backup “verificado”.
2. **CI**: ejecutar `tests/ tests_bdm/ core/tests/` y los 6 shell de
   `deployment/factory/tests`; hoy solo corre 1569/2533 (38,1 % fuera).
3. **Actuación**: no habilitar producción hasta CCR + safe-stop + verificación
   por atributo (clima, brillo, posición, volumen).
4. **Egress**: añadir snapshot/rollback y persistencia; el stop actual retira la
   jaula (fail-open).
5. **EMS/taxonomía**: consolidar `home_discovery_source`; no borrar `home_ems`
   sin deprecación y comprobar imports dinámicos.

## Orden P0–P2

| Paso | Acción | Dependencia |
|---|---|---|
| 1 | Backup fail-closed + test de fallo | — |
| 2 | CI completo (2533 casos + shell) | 1 |
| 3 | CCR Clase A/B + safe-stop acotado | decisión R14 |
| 4 | Verificación por atributo en actuación | 3 |
| 5 | Egress enforcement verificado + rollback/persistencia | A3 |
| 6 | Override + Oportunidades | 3–4 |
| 7 | Consolidar EMS + proyección 12→5 | — |
| 8 | Piloto energético | 1–7 |

**Desacuerdo registrado:** Argon prioriza entorno/instalación; GPT/orquestador
priorizan CCR. Se resuelve con backup+CI como P0-paso-1 y CCR como P0-paso-3;
ambos son bloqueantes antes de actuación.
