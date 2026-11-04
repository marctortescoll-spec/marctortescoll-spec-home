# Anexo B — Registro de decisiones del concilio
## Consolidado · Anexo de "Kaenori Experience v1.3" · 2026-11-04

**Naturaleza:** registro único y cronológico de las decisiones del concilio (DeepSeek + GPT-5 +
Argon). Sustituye a los dictámenes sueltos de las rondas 4–10, que quedan como histórico.

## Aprobación del propietario — Congelación funcional V1.3 (2026-11-04)

El propietario **aprueba** la congelación funcional de V1.3 (rector + anexos A, B, C, D) y los 12
puntos del §13: **aceptar** 1, 2, 3, 4, 6, 7, 9, 12 · **aceptar con modificaciones** 5 y 8 · **aceptar
con priorización/fases** 10 y 11. **No se reabre la estrategia** salvo contradicción demostrable de
seguridad, privacidad o viabilidad. Siguiente mandato: **auditoría integral del desarrollo** (A0, sin
código).

---

## Ronda 4 — Resumen Ejecutivo (2026-11-03)

- **Veredicto:** listo-con-cambios.
- **Decidido:** separar consolidado/propuesto; anclar invariantes; trazabilidad (A–L ↔ 1–12) y columna
  bloqueante; conflicto HA: observación (confort) vs **fail-closed** (seguridad).

## Ronda 5 — Revisión estratégica de producto (2026-11-04)

- **Veredicto:** sólida-con-cambios.
- **Validado:** inteligencia percibida · vender resultados · autonomía progresiva · perfiles
  combinables · roadmap P0–P3.
- **Correcciones críticas:** credenciales jamás por chat/voz · fail-closed con HA · primer valor
  solo-lectura.
- **Aportaciones:** contrato de Oportunidad · hold manual · memoria local · multiusuario 2 fases ·
  offline a P0 · acotar "comprender frases".

## Ronda 6 — Liloo Opportunities (2026-11-04)

- **Veredicto:** sólido-con-cambios.
- **9 mejoras:** reglas de elegibilidad (sin motor) · máquina de estados completa · Watchdog+heartbeat ·
  multiusuario asimétrico · versionado de simulación · baseline reproducible · ConflictPolicy · reloj
  monotónico · provenance.

## Ronda 7 — Validación de la revisión del experto sobre V1.1 (2026-11-04)

- **Veredicto:** la revisión del experto es **excelente**; adoptar su hoja de ruta a V1.2.
- **7 hallazgos validados** (6 de acuerdo + 1 matizado: taxonomía = 12 categorías, no 6).
- **Aportaciones nuevas:** **CCR** (Clase A/B) · **Override** (intervención manual invalida evaluación)
  · **proyección taxonómica 12→5** · matriz de trazabilidad · kill-switch por oportunidad + FMEA ·
  contrato UX mínimo · política de datos/LLM (Nivel 0/1/2) · pruebas de campo.

---

## Ronda 8 — Validación final de V1.2 (2026-11-04)

- **Veredicto:** congelar-con-ajustes menores (GPT-5 y Argon).
- **Ajustes aplicados:** desopacar la tabla §12 (descripción resumida) · "riego/piscina (previsto en
  P3)" · estados explícitos del Anexo C (implementado/parcial/pendiente/bloqueado) · nota ha-adapter →
  CCR Clase B por defecto si no se demuestra TTL nativo.

## Ronda 9 — Anexo D (Experiencia de Cliente y Comportamiento de Liloo) (2026-11-04)

- **Veredicto:** aprueba-con-cambios (GPT-5 y Argon).
- **6 dimensiones evaluadas:** coherencia estratégica · experiencia de cliente · viabilidad conceptual ·
  seguridad y confianza · consistencia documental · calidad de producto → todas cumplen (con matices).
- **Ajustes aplicados al Anexo D:** voz/micrófono ↔ niveles de privacidad (Nivel 0/1/2) · "alertas
  críticas" = solo Life Safety (el resto respeta horas de silencio) · "descarto ese supuesto" (no
  "ajusto el modelo") · corte de internet = mensaje local predefinido en el borde · confirmación
  explícita al traducir lenguaje natural a límites · dependencias añadidas: identidad/rol en canales
  compartidos, ontología para "agrupados por función", auditoría/aviso cuando un Habitante pausa o
  cancela una prueba.

## Ronda 10 — Validación de los 5 ajustes finales sobre V1.3 (2026-11-04)

- **Veredicto:** aprueba congelación (GPT-5 y Argon), condicionada a aplicar los 5 ajustes.
- **5 ajustes validados (de acuerdo):** lectura antes de la etapa 4 · separar datos/modos de escucha ·
  no prometer «se recuperará solo» · continuidad por usuario autenticado · editorial V1.3.
- **Ajustes aplicados** al Anexo D y al Anexo B (referencia a V1.3). Nota Anexo A: «Revertir»/kill-switch
  no garantiza retorno físico; Liloo comunica qué parte verificó.

## Decisiones finales consolidadas (las 12 modificaciones)

| # | Ref | Decisión del concilio | Bloqueante |
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

- "Motor de oportunidades" → **reglas de elegibilidad** (sin motor nuevo).
- "Perfiles excluyentes" → **presets combinables** (Tranquilidad transversal).
- "Conflicto HA: modificar" → **aceptar sin relajar (fail-closed)**.
- "6 agrupaciones en brain.py" → **12 categorías en vocabulary.py** (proyección 12→5).

## Cuestiones pendientes de aprobación (del propietario)

Los 12 puntos siguen **pendientes de aprobación del Owner** (no los cierra el concilio). Además quedan
por decidir en validación: la política de datos/LLM exacta, el contrato visual completo de Liloo, y la
elección de la **una** oportunidad energética del piloto.

*Registro A0. No autoriza desarrollo, escaneo, despliegue ni cambios de ADR.*
