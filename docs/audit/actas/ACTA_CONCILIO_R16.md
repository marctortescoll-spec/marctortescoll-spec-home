# ACTA DEL CONCILIO — Ronda 16 (resolución de gobernanza) · 2026-11-04

**Concilio:** DeepSeek (orquestador) + GPT-5 + Gemini 4 Argon · **A0**.

## Veredicto: **resuelto-con-condiciones** (2/2)

Los 3 bloqueos de gobernanza quedan resueltos de forma pragmática, adaptando la gobernanza a la realidad
del harness DSH, sin violar no-autoauditoría ni simular consenso.

## Resolución final (consolidada)

| Bloqueo | Solución acordada |
|---|---|
| **1. Grant Control Plane** | La **autorización directa y expresa del Owner** (MANDATO MAESTRO + confirmación de 5 €) actúa como **Grant Estático (file-based)**. Se fija un tope duro de **5 €**: si el gasto acumulado lo supera, la ejecución aborta y se pide nueva autorización. |
| **2. Scheduler** | La **continuación por rondas (goal)** es el scheduler oficial (**1 ronda = 1 tick**). Cada cierre de ronda registra un **heartbeat** (commit/test/estado) en `ESTADO_ACTUAL.md`; si 2 rondas seguidas no producen cambios, el bucle se detiene. |
| **3. Auditor independiente** | **GPT-5 + Argon asumen el rol `07-auditor`** vía **evidencia cruda**: el orquestador inyecta `git diff`, logs íntegros de tests y **hashes SHA-256** de entregables **sin resumir ni opinar**. Se registra la limitación: *"no existe proceso 07-auditor separado; auditoría ejercida por el concilio externo"*. |

## Condición crítica (Argon)

El orquestador **cesa toda autocertificación**. GPT-5/Argon revisan **solo evidencias crudas** (no
resúmenes del ejecutor), y las discrepancias se devuelven al ejecutor. Ninguna fase se integra sin **ACK
de al menos un revisor y sin objeciones del otro**.

## Artefactos a crear (para operar)

1. `governance/GRANT_ACTIVO.md` — grant estático (mandato, alcance P0–P2 A0–A2, tope 5 €, revocación).
2. `governance/RUNBOOK_SCHEDULER.md` — scheduler por rondas + heartbeat + parada por inactividad.
3. `governance/AUDIT_LIMITATIONS.md` — limitación de auditoría (sin proceso 07-auditor separado).

## Votos

| IA | Voto |
|---|---|
| GPT-5 | resuelto-con-condiciones |
| Argon | resuelto-con-condiciones |

*Acta A0. Desbloquea la ejecución A0–A2; no concede A3.*
