# Anexo B — Registro de decisiones del concilio
## Consolidado · Anexo de "Kaenori Experience v1.1" · 2026-11-04

**Naturaleza:** registro único y cronológico de las decisiones del concilio (DeepSeek + GPT-5 +
Argon). Sustituye a los dictámenes sueltos de las rondas 4, 5 y 6, que quedan como histórico.

---

## Ronda 4 — Resumen Ejecutivo (2026-11-03)

- **Veredicto:** listo-con-cambios.
- **Decidido:** separar "consolidado" de "propuesto"; anclar invariantes (Core v1.0, deny-by-default,
  fail-closed, local-first, sin secretos/PII); añadir trazabilidad (Ref A–L ↔ 1–12) y columna
  "bloqueante"; corregir regla de conflicto con HA (observación para confort, **fail-closed** para
  seguridad).

## Ronda 5 — Revisión estratégica de producto (2026-11-04)

- **Veredicto:** sólida-con-cambios.
- **Validado:** "inteligencia percibida", "vender resultados no automatizaciones", autonomía
  progresiva, perfiles combinables, roadmap P0→P3.
- **Tres correcciones críticas (no negociables):**
  1. **Credenciales jamás por chat/voz** (conector seguro).
  2. **Fail-closed con HA** (no relajar por "evitar sospechas").
  3. **Primer momento de valor solo-lectura.**
- **Aportaciones:** contrato de "Oportunidad", hold manual, memoria local explicable, multiusuario en
  dos fases, offline a P0, acotar la promesa de "comprender frases".

## Ronda 6 — Liloo Opportunities (2026-11-04)

- **Veredicto:** sólido-con-cambios.
- **9 mejoras integradas en la v1.1 del Anexo A:** (1) "motor" → reglas de elegibilidad; (2) máquina
  de estados completa (adopted_active/paused/expired, invalidación, retornos, recuperación);
  (3) finalización segura por Watchdog + heartbeat; (4) multiusuario asimétrico (Admin autoriza,
  Habitante pausa/cancela); (5) versionado de simulación; (6) baseline reproducible para Nivel C;
  (7) ConflictPolicy (Life Safety > límites > manual > objetivos > rutinas); (8) reloj monotónico;
  (9) provenance y auditoría.

---

## Decisiones finales consolidadas (las 12 modificaciones)

| # | Ref | Decisión final | Bloqueante |
|---|---|---|---|
| 1 | A | Aceptar | Sí |
| 2 | B | Aceptar | Sí |
| 3 | C | Aceptar (sin relajar, fail-closed) | Sí |
| 4 | D | Aceptar | Sí |
| 5 | E | Modificar (solo cuando sea necesario) | No |
| 6 | F | Aceptar | No |
| 7 | G | Aceptar | Sí |
| 8 | H | Modificar (Tranquilidad transversal) | No |
| 9 | I | Aceptar | Sí |
| 10 | J | Elevar (P0) | No |
| 11 | K | Elevar (2 fases: Admin+Habitante → roles en P2/P3) | No |
| 12 | L | Aceptar | No |

## Contradicciones resueltas

- "Motor de oportunidades" → **"Reglas de elegibilidad"** (sin motor nuevo).
- "Perfiles excluyentes" → **presets combinables** (Tranquilidad transversal).
- "Conflicto HA: modificar" → **aceptar sin relajar (fail-closed)**.
- "Resumen/Revisión estratégica como documentos paralelos" → **absorbidos en el rector v1.1**.

*Registro A0. No autoriza desarrollo, escaneo, despliegue ni cambios de ADR.*
