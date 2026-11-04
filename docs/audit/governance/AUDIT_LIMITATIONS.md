# AUDIT LIMITATIONS — Auditoría independiente (07-auditor)

**Limitación registrada:** no existe un **proceso 07-auditor separado** en este entorno (DSH harness).
La auditoría independiente la ejercen **GPT-5 y Argon (concilio externo)** mediante **inspección de
evidencia cruda**.

## Protocolo

1. El orquestador **no autocertifica**: empaqueta **evidencia cruda** (`git diff`, logs íntegros de
   tests, hashes SHA-256) **sin resumir ni opinar**.
2. GPT-5 y Argon emiten **dictámenes separados** sobre la evidencia.
3. **Gate:** ninguna fase se integra sin **ACK de ≥1 revisor y sin objeciones del otro**. Las
   discrepancias se devuelven al ejecutor.

## Riesgo residual (aceptado por el Owner)

Sin auditor humano ni proceso externo; revisión **ex-post** de evidencias inyectadas. El consenso del
concilio **no es prueba de funcionamiento**.
