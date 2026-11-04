# Anexo A — Liloo Opportunities v1.2
## Especificación funcional del componente de Oportunidades · Anexo de "Kaenori Experience v1.2"

**Fecha:** 2026-11-04 · **Estado:** consolidado (A0) — congelación funcional. No autoriza
implementación ni acciones físicas. **Ámbito:** Dashboard, Liloo, Discovery, EMS, Experience, Core y
conectores.

> **Cambios v1.1 → v1.2 (ronda 7 del concilio + precisiones del Owner):**
> (1) **CCR** con exigencia de evidencia real de reversión en Clase A; (2) **Override** (la
> intervención manual se respeta y la evaluación se invalida); (3) **Kill-switch local** con
> distinción "detener órdenes nuevas" vs "recuperar dispositivo ya ordenado"; (4) **FMEA por
> oportunidad**; (5) nivel **descriptivo/no-concluyente**; (6) **persistencia de vencimiento +
> boot_id**.

---

## 1. Objetivo del componente

Convertir las capacidades detectadas en propuestas personalizadas, verificables y comprensibles.
**Asesoramiento continuo**, no listado de automatizaciones. Distinguir **ejecución técnica / efecto
observado / beneficio demostrado**. Cinco momentos: **Descubrir → Simular → Autorizar → Probar →
Verificar**.

## 2. Estructura de la tarjeta

- **Compacta:** identidad, título (beneficio cotidiano), resumen, ámbito, estado, beneficio
  (medido/estimado/pendiente), acción principal. **Sin métricas ficticias.**
- **Expandida:** **Comprender · Simular · Autorizar · Probar · Evaluar**, manteniendo la conversación.

## 3. Ciclo de vida (completo y reversible)

`detected · needs_data · ready_to_simulate · simulated · permission_pending · trial_ready ·
trial_running · trial_paused · trial_completed · evaluated · adopted_active · adopted_paused ·
adopted_expired · dismissed · failed`.

**Reversibilidad:** `simulated → ready_to_simulate` por invalidación (`simulation_stale`) ·
`adopted_* → evaluated/paused/expired` con motivo (`user_revoked / safety_violation / conflict /
schedule_end`) · `evaluated → permission_pending / ready_to_simulate` (repetir/ajustar) ·
`failed → needs_data / ready_to_simulate` (hard/soft) · `dismissed` con motivo y ventana de
reactivación. **Terminar una prueba nunca equivale a autorizar una automatización permanente.**

## 4. Reglas de elegibilidad (sin motor nuevo)

Experience evalúa **plantillas declarativas** sobre Discovery y EMS, **sin motor independiente**.
Asignación: Discovery = evidencia · Catálogo/Experience = elegibilidad · EMS = cálculo/simulación ·
Liloo = interacción/explicación · Core+seguridad = autorización · Conectores = ejecución ·
Registro local = trazabilidad. **Ningún módulo salta los 6 puertos de Core v1.0.**

## 5. Simulación

Información mínima (producción/balance, consumo, temperatura, consigna/estado, capacidad de control,
límites, tarifa). Distinguir datos **actuales/históricos/inferidos/ausentes**. Sin tarifa válida →
simular **energía**, no ahorro económico. Etiquetas: **medido / simulado / estimado / no disponible**.
`SimulationRun` incluye `data_window`, `inputs_hash`, `template_version`; invalidación determinista.
Explicación conversacional: sencilla + desglose técnico opcional.

## 6. Permisos

Finalidad · equipo · capacidad · límites · horario · duración · condiciones · prioridad (manual) ·
revocación · recuperación. Lectura/acción puntual/automatización temporal **por separado**. Sin
casillas premarcadas. **Multiusuario asimétrico:** **Admin** autoriza; **cualquier Habitante** puede
**pausar/cancelar** la prueba (fail-safe de confort).

## 7. Prueba temporal supervisada — con CCR, Override y Kill-switch

**7.1 Comprobaciones previas** (identidad, permiso, datos recientes, actuador compatible, sin
conflicto, reglas técnicas, finalización segura **verificada**).

**CCR — Clasificación de Capacidad de Reversión (v1.2):**

- **Clase A (Lease/TTL nativo):** el equipo recupera por sí mismo un estado seguro al expirar el TTL.
  **Se exige evidencia de que lo hace de verdad** (prueba de corte de red observada), **no solo el
  anuncio del fabricante**.
- **Clase B (sin TTL):** requiere ack de latido; sin reversión autónoma.
- **Regla fail-closed:** los equipos **Clase B sin recuperación segura verificada** no participan en
  pruebas automáticas de riesgo. `TrialPlan.safe_stop = {type, verified_at, verifier=connector_id}`;
  si `safe_stop` no verificado → `trial_ready` bloqueado. Para Clase B apto, se exige **Watchdog local
  + heartbeat** antes de cada consigna.

**Override (v1.2):** cualquier actuación manual detectada sobre un dispositivo en prueba **cancela la
fase de ejecución** (estado "Abortado por Usuario"), **respeta el nuevo estado** (no restaura ciegamente
la consigna anterior) y marca la evaluación como **"Anulada por intervención manual"**.

**Kill-switch local (v1.2):** funciona **sin BDM, Internet ni LLM**. Distingue: (a) **detener nuevas
órdenes** (bloqueo de la cola) de (b) **recuperar un dispositivo que ya recibió una orden** (política
de restauración según CCR). Ambas son independientes y ambas deben estar disponibles.

**7.2 Durante la prueba:** estado (activa/pausada/degradada/finalizada) · duración restante (reloj
monotónico) · acciones · mediciones · incidencias · controles. **Las decisiones manuales prevalecen.**
La lógica de ejecución es **local**.

**7.3 Vencimiento:** cancela programación futura · política de finalización segura · comprobación del
estado · registro restauración verificada/pendiente · evaluación. **No se renueva silenciosamente.**
**Persistencia (v1.2):** expiración absoluta (UTC) + `boot_id` + `monotonic_start`; tras reinicio con
incertidumbre de reloj, la prueba queda en **"suspendida-segura"** hasta verificación NTP/RTC.

## 8. Resultado verificado: tres niveles de evidencia

- **A — Ejecución verificada** · **B — Efecto observado** · **C — Beneficio demostrado o estimado**
  (con `method` + intervalo de confianza + sensibilidad; sin método → "**estimación no concluyente**").
- **Nivel descriptivo (v1.2):** en pilotos con datos insuficientes, se admite resultado
  **descriptivo** (qué ocurrió) **sin forzar precisión estadística artificial**.
- **Informe final:** qué hicimos · qué observamos · qué creemos que aportó · qué no sabemos ·
  recomendación. La recomendación **nunca auto-autoriza**.

## 9. Decisión después de la prueba

Mantener / Repetir / Ajustar / Pausar / Descartar / Explicar. **Sin "Mantener para siempre" por
defecto.** Adopción reversible y auditable.

## 10. Liloo como interfaz conversacional

Misma identidad en todas las fases. **La interpretación del LLM no equivale a consentimiento de una
acción física.**

## 11. Contratos conceptuales y FMEA

`OpportunityRecord · SimulationRun · PermissionRequest · TrialPlan · TrialEvent · ActionVerification ·
OpportunityEvaluation · OpportunityDecision`. Provenance: `source={sensor_id, read_time, cache_age}`.

**FMEA por oportunidad (v1.2):** cada oportunidad incluye un **FMEA resumido** (modos de fallo,
efecto, mitigación), límites operativos y **kill-switch local** que revierte y bloquea reintentos.

## 12. Errores y conflictos

Datos insuficientes (mostrar qué falta, no inventar) · Equipo sin control verificado (lectura y
simulación; bloquear prueba) · Conflicto (`ConflictPolicy`: Life Safety > límites > manual > objetivos
> rutinas; `trial_paused(conflict)`, EMS replanifica) · Fallo de internet (solo local seguro) · Pérdida
de sensor crítico (detener + política segura) · Revocación · **Override** (manual) · Resultado ambiguo
(declarar incertidumbre).

## 13. Experiencia responsive

Escritorio / móvil (autorización legible) / tableta mural (autorizaciones sensibles requieren persona
autenticada). Conversación persistente; animaciones reflejan estados reales. Accesibilidad y movimiento
reducido.

## 14. Primera oportunidad MVP

**"Aprovechar mejor mi energía solar"** — vertical Energía y Clima (+ Confort). Una vivienda piloto, un
sistema térmico compatible, medición fiable, lectura real, permiso temporal específico, una estrategia
acotada. **Usar EMS y la infraestructura de lectura existentes** antes de nada alternativo.

## 15. Criterios de aceptación

No hay datos → sin beneficio inventado · Sin permiso → sin actuación · Permiso expirado → rechazo ·
Equipo desconectado → no actúa a ciegas · **Cambio manual → se respeta (Override)** · Orden sin
confirmación → no verificada · Prueba vencida → sin órdenes · Dispositivo compartido → sin duplicados ·
Ahorro no demostrable → estimado/descriptivo · Revocación → bloqueo + recuperación · Sin LLM remoto →
protecciones locales · Reinicio → estado consistente y seguro.

**Métricas:** oportunidades exploradas, simulaciones completadas, autorizaciones concedidas/retiradas,
pruebas finalizadas, resultados comprensibles, decisiones de adopción.

## 16. Decisión de producto

**Aprobar como concepto funcional.** Antes de implementar: contrastar contratos con el código (Anexo
C), identificar capacidades reales, completar bloqueos de seguridad/recuperación. **No autorizado:**
cambios de Core, ampliación de permisos, actuación en equipos reales, pruebas físicas o despliegue.

**Principio final:** *La oportunidad no termina cuando Kaenori actúa. Termina cuando el cliente
entiende qué ocurrió, qué mejoró, qué incertidumbre existe y conserva el derecho a decidir.*
