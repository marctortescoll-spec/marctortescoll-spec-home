# ÁREA D — Experience, Wizard, dashboard y Liloo
## Informe técnico · Fase 2 · Auditoría Global V1.3

**Fuente:** `home/integration/connectors-20261022 @ 549700b9` · A0 (solo lectura).

| Requisito V1.3 | Código | Estado |
|---|---|---|
| Wizard (máquina de estados) | `home_catalog/wizard.py` (`IDLE → VERTICAL → ENTRY → CONFIGURED → VERIFIED → APPROVED`) | implementado |
| Onboarding conversacional | `home_onboarding/` (conversation, interpreter, learning, profile, report) | implementado |
| Liloo Lite (NLU determinista) | `home_liloo_lite/` (brain, command_mapper, catalog) | implementado |
| Cliente de intención | `home_liloo_client/` | implementado |
| Cerebro experiencia (sesiones/memoria/voz) | `core/home_experience/` (brain, sessions, memory, knowledge, auth, tls, voice) | implementado |
| **Dashboard "cuenta la historia"** | **NO codificado** (solo Anexo D) | **pendiente** |
| **Continuidad por usuario (mural/voz)** | **NO codificado** (dependencia marcada en Anexo D) | **pendiente** |

## Hallazgos

- El **motor del Wizard** y **Liloo Lite** están codificados y con máquina de estados explícita.
- La **experiencia percibida** de V1.3 (dashboard narrativo, continuidad por usuario, atención selectiva)
  está **solo en Anexo D**, no en código. Las pantallas/UI existen como `webui/` y `home_liloo_*`, pero
  el contrato visual mínimo de Liloo no está implementado.

*Informe A0. No autoriza desarrollo.*
