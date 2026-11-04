# Kaenori · Liloo Opportunities
## Diseño funcional v1.1 · Componente de inteligencia proactiva para el dashboard HOME00X

- **Fecha:** 4 de noviembre de 2026 · **Estado:** Propuesta para revisión y aprobación de producto.
- **Ámbito:** Dashboard, Liloo, Discovery, EMS, Experience, Core y conectores.
- **Naturaleza:** Diseño funcional. No autoriza implementación ni acciones físicas.
- **Procedencia:** v1.0 + dictamen del concilio (ronda 6, GPT-5 + Argon).

### Cambios incorporados en v1.1 (resumen)

| # | Cambio | Sección |
|---|---|---|
| 1 | "Motor" → **"Reglas de elegibilidad"** (sin motor independiente) | 4 |
| 2 | Máquina de estados completa (post-adopción, invalidación, retornos, dismissed/failed) | 3 |
| 3 | Finalización segura vía **Watchdog local + heartbeat** (sin bloquear equipos sin lease-time nativo) | 7 |
| 4 | **Multiusuario asimétrico** (Admin autoriza; Habitante pausa/cancela) | 6, 9 |
| 5 | **Versionado/invalidación** determinista de simulación | 5 |
| 6 | **Baseline reproducible** para el Nivel C (método + IC + "no concluyente") | 8 |
| 7 | **ConflictPolicy** operativa (Life Safety > límites > manual > objetivos > rutinas) | 12 |
| 8 | **Reloj monotónico** y política de reinicios fail-closed | 7 |
| 9 | **Provenance y auditoría** de cada dato y acción | 11, 7 |

---

## 1. Objetivo del componente

Convertir las capacidades detectadas en una vivienda en propuestas personalizadas, verificables y
comprensibles que el propietario pueda explorar, simular, probar temporalmente y, si el resultado es
satisfactorio, mantener activas. No debe comportarse como un listado de automatizaciones predefinidas:
debe generar una **experiencia de asesoramiento continuo**.

**Principio fundamental:** *Liloo propone una mejora, explica por qué, demuestra su viabilidad,
solicita autorización y aprende del resultado.* No se considera una oportunidad completada por haber
enviado una orden. Debe distinguirse entre **ejecución técnica, efecto observado y beneficio
demostrado**.

La experiencia se resume en **cinco momentos**: 1 Descubrir · 2 Simular · 3 Autorizar · 4 Probar ·
5 Verificar.

## 2. Estructura de la tarjeta

**2.1 Vista compacta** — identidad (logo Liloo), título (beneficio en lenguaje cotidiano), resumen,
ámbito (energía/confort/seguridad/otros), estado, beneficio (medido/estimado/pendiente), acción
principal. **No mostrar métricas ficticias, porcentajes de confianza sin calibrar ni ahorros sin
evidencia.**

**2.2 Vista expandida** — al abrir una oportunidad se mantiene la conversación con Liloo y aparecen
cinco secciones progresivas: **Comprender · Simular · Autorizar · Probar · Evaluar**. La persona puede
regresar al dashboard sin perder el estado.

## 3. Ciclo de vida de una oportunidad

| Estado | Comportamiento |
|---|---|
| detected | Posibilidad identificada, no validada |
| needs_data | Faltan datos, capacidades o contexto |
| ready_to_simulate | Requisitos mínimos para modelar una alternativa |
| simulated | Simulación válida con fecha y supuestos |
| permission_pending | Se necesita autorización específica |
| trial_ready | Permisos y comprobaciones previas correctos |
| trial_running | Prueba temporal activa |
| trial_paused | Prueba interrumpida; no se ejecutan nuevas acciones |
| trial_completed | Prueba finalizada y preparada para evaluación |
| evaluated | Resultado explicado y cuantificado hasta donde permiten los datos |
| **adopted_active** | El usuario concedió un permiso para mantenerla; oportunidad activa |
| **adopted_paused** | Automatización adoptada, temporalmente suspendida |
| **adopted_expired** | Automatización adoptada cuyo permiso venció |
| dismissed | Oportunidad descartada |
| failed | Fallo con causa y vía de recuperación |

**Transiciones y reversibilidad (v1.1):**

- `simulated → ready_to_simulate` (o `needs_data`) por **caducidad/invalidación** de datos o cambio
  de topología, con `invalidated_reason` (`simulation_stale`).
- `adopted_active → adopted_paused | adopted_expired | evaluated`, con motivo: `user_revoked |
  safety_violation | conflict | schedule_end`. La adopción **nunca** es irreversible.
- `evaluated → permission_pending | ready_to_simulate` para "repetir" o "ajustar condiciones".
- `dismissed` registra **motivo** (falta de datos / preferencia / riesgo / conflicto) y **ventana de
  reactivación**.
- `failed` distingue **hard/soft** y ruta de recuperación (`trial_ready` o `simulated` tras mitigación).
- `needs_data`, `trial_paused` y `failed` permiten recuperación sin reconstruir el proceso completo.

**Regla:** terminar una prueba nunca equivale a autorizar una automatización permanente.

## 4. Reglas de elegibilidad de oportunidades (antes "motor de detección")

*(v1.1: renombrado para no inducir un motor independiente; es un pipeline declarativo sobre módulos
existentes.)*

**Experience evalúa plantillas declarativas** consumiendo el catálogo de **Discovery** y las
simulaciones de **EMS**, **sin instanciar un motor de ejecución independiente**. Para crear una
oportunidad se requieren: objetivo compatible con las preferencias del hogar · al menos una evidencia
verificable · plantilla de oportunidad aplicable · ausencia de incompatibilidades conocidas ·
identificación de datos y permisos pendientes.

Puede combinar capacidades de distintas verticales **sin duplicar la identidad de los dispositivos**.
Ejemplo: producción solar + consumo eléctrico + aerotermia + temperatura del salón + preferencias de
confort. **Asignación:** Discovery = evidencias de equipos; Catálogo = capacidades; EMS = cálculos
energéticos; Experience/Liloo = presenta y explica; Core = autoridad de las actuaciones.

## 5. Simulación: antes de pedir permisos de control

**5.1 Información mínima** (para "aprovechar excedentes solares para climatizar"): producción y
balance solar · consumo eléctrico · temperatura interior · consigna y estado del equipo · capacidad
de control · límites del usuario · tarifa eléctrica. La duración mínima del historial y el grado de
confianza se definen **por plantilla** y se validan mediante pruebas. El sistema distingue **datos
actuales, históricos, inferidos y ausentes**. Si no existe tarifa válida, se puede simular **energía**,
pero no atribuir un **ahorro económico** fiable.

**5.2 Resultado de simulación** — mostrar: situación actual/línea base · cambio propuesto y
dispositivos afectados · beneficio esperado como intervalo (si hay evidencia) · costes, consumo
adicional, efectos secundarios, restricciones · supuestos · calidad y vigencia de los datos · qué se
comprobará en la prueba. Etiquetas inequívocas: **medido / simulado / estimado / no disponible**.

**Versionado e invalidación (v1.1):** `SimulationRun` incluye `data_window=[t0,t1]`,
`inputs_hash=SHA256(capacidades + límites + tarifa + modelo)` y `template_version`. Si cambia
cualquiera, la simulación pasa de `simulated → ready_to_simulate` con `invalidated_reason`, antes de
autorizar la prueba.

**5.3 Explicación conversacional** — *"He observado que en determinados momentos exportas energía a
la red. Podríamos aprovechar una parte para climatizar antes el salón. Mi estimación depende del
consumo y del comportamiento de tu equipo. ¿Quieres ver cómo he llegado a esta propuesta?"*.
Explicación sencilla + desglose técnico opcional.

## 6. Permisos: autorización específica y comprensible

El cliente no autoriza genéricamente que Liloo controle toda la vivienda. La pantalla de autorización
describe: finalidad · equipo · capacidad · límites · horario · duración · condiciones · prioridad
(respeto a intervención manual) · revocación (inmediata) · recuperación (política segura).

Los permisos de **lectura, acción puntual y automatización temporal** se gestionan por separado. Frase
de confirmación concreta: *«Autorizo esta prueba durante 48 horas, únicamente sobre el termostato
indicado y con los límites mostrados»*. **No se admiten casillas premarcadas ni concesión implícita de
permisos permanentes.**

**Matriz de roles (v1.1):** en entornos multiusuario P0/P1,

- **Admin** (rol con permiso sobre el actuador): puede autorizar lectura, acción puntual y prueba
  temporal (`permission_pending → trial_ready`).
- **Habitante autenticado**: consulta la propuesta autorizada para su rol y puede ejecutar acciones
  puntuales no peligrosas (según catálogo de riesgo), pero **no** autoriza actuaciones sensibles.
- **Cualquier Habitante autenticado puede `pausar` o `cancelar` una prueba en curso** (fail-safe de
  confort), aunque no pueda autorizarla. Las solicitudes fuera de rol muestran "requiere Admin
  autenticado localmente".

## 7. Prueba temporal supervisada

**7.1 Comprobaciones previas** — identidad validada · permiso vigente y exacto · equipo y sensores con
datos recientes · actuador admite la operación · sin conflicto incompatible · reglas técnicas conocidas
· mecanismo seguro de finalización/recuperación verificado.

**Finalización segura (v1.1):** `TrialPlan.safe_stop = {type: native_expiry | scheduled_restore |
fallback_mode, verified_at, verifier=connector_id}`. Para equipos **sin expiración nativa por
hardware**, el requisito se cumple mediante un **Watchdog local persistente (Core/EMS) + heartbeat de
conectividad** antes de cada consigna. Si `safe_stop` no está verificado → `trial_ready` bloqueado. No
se promete restauración instantánea cuando el dispositivo esté desconectado.

**7.2 Durante la prueba** — mostrar: estado (activa/pausada/degradada/finalizada) · duración restante
· acciones (solicitadas/autorizadas/enviadas/confirmadas) · mediciones · incidencias (límites,
intervención manual, fallos) · controles (pausar/finalizar/consultar). La pausa impide nuevas
actuaciones y aplica la política de parada. **Las decisiones manuales prevalecen.** La pérdida de datos
críticos, autorización, contexto o capacidad impide nuevas acciones. **La lógica de ejecución permanece
local**, sin depender de un LLM remoto.

**Reloj y reinicios (v1.1):** usar **reloj monotónico** para duraciones; registrar zona horaria y
límites DST. En reinicio, recalcular `remaining_duration` y ejecutar política fail-closed.

**7.3 Vencimiento** — al cumplirse la duración: se cancela programación futura · se ejecuta política de
finalización segura · se comprueba el estado del equipo · se registra restauración verificada/pendiente
· se inicia evaluación. **La prueba no se renueva silenciosamente.**

## 8. Resultado verificado: tres niveles de evidencia

- **Nivel A — Ejecución verificada** (¿hizo el dispositivo lo que se pidió?): acuse no basta si hay
  que comprobar estado físico.
- **Nivel B — Efecto observado** (¿ocurrió el cambio esperado?): períodos de medición, fuentes, datos
  ausentes.
- **Nivel C — Beneficio demostrado o estimado** (¿mejoramos autoconsumo/coste/confort?).

**Baseline reproducible (v1.1):** `OpportunityEvaluation.C` declara **método** (`difference-in-
differences | weather-normalized | control-period`), variables usadas, **intervalo de confianza** y
sensibilidad. Si no hay método aplicable o los datos quedaron fragmentados (p. ej. por cambios
manuales frecuentes), se declara "**estimación no concluyente**". **No atribuir ahorro causal a Liloo
solo porque el consumo total bajó.**

**8.1 Informe final** — qué hicimos · qué observamos · qué creemos que aportó (impacto e
incertidumbre) · qué no sabemos · mi recomendación (mantener/repetir/ajustar/descartar, con motivo).
La recomendación de Liloo **nunca autoriza automáticamente** una nueva ejecución.

## 9. Decisión después de la prueba

Mantener (abre autorización independiente con duración/horario/límites) · Repetir (nueva duración y
consentimiento) · Ajustar condiciones · Dejar en pausa · Descartar · Explicar el resultado. **No
presentar "Mantener para siempre" como opción predeterminada.** La continuidad debe ser reversible,
auditable y limitada por políticas explícitas. **(v1.1)** La adopción resultante vive como
`adopted_active / adopted_paused / adopted_expired`, y es **revocable** por Admin y pausable por
cualquier Habitante.

## 10. Liloo como interfaz conversacional de oportunidades

Misma identidad en todas las fases; no se crean bots por vertical. Ejemplos: *«¿Por qué me recomiendas
esto?» → explica la evidencia y lo que falta comprobar* · *«No quiero que el salón supere los 22
grados» → lo usa como límite y recalcula* · *«¿Qué hiciste ayer?» → muestra órdenes registradas y
confirmadas*. **La interpretación de una frase por el LLM no equivale a consentimiento de una acción
física.**

## 11. Contratos conceptuales de ingeniería

No son endpoints existentes ni cambios aprobados en Core; son objetos funcionales para definir la
integración: **OpportunityRecord · SimulationRun · PermissionRequest · TrialPlan · TrialEvent ·
ActionVerification · OpportunityEvaluation · OpportunityDecision.** Deben preservar el inventario
canónico y referenciar capacidades por identidad, sin duplicar dispositivos.

**Campos añadidos (v1.1):** `SimulationRun.data_window / inputs_hash / template_version`;
`TrialPlan.safe_stop`; `TrialEvent.source = {sensor_id, read_time, cache_age}`;
`ActionVerification.source = {sensor_id, read_time, cache_age}`; `OpportunityEvaluation.method /
confidence_interval / sensitivity`.

Asignación: Discovery (evidencia/presencia) · Catálogo/Experience (elegibilidad/oportunidades) · EMS
(cálculo/simulación/planificación) · Liloo (interacción/explicación/interpretación) · Core+seguridad
(autorización efectiva) · Conectores (ejecución/comprobación) · Registro local (trazabilidad).
**Ningún módulo debe saltarse los seis puertos congelados de Core v1.0**; las ampliaciones se diseñan
fuera de ese contrato o se proponen mediante ADR específico.

## 12. Tratamiento de errores y condiciones especiales

Datos insuficientes (mostrar qué falta, no inventar) · Equipo sin control verificado (lectura y
simulación; bloquear prueba física) · Conflicto con otra automatización · Fallo de internet (solo
operaciones locales seguras) · Pérdida de sensor crítico (detener y política segura) · Revocación
(bloquear nuevas, gestionar enviadas) · Intervención manual (respetar con ventana de prioridad, salvo
protecciones) · Resultado ambiguo (declarar incertidumbre).

**ConflictPolicy operativa (v1.1):** prioridad **Life Safety > límites técnicos > manual > objetivos >
rutinas**. Ante conflicto, la oportunidad pasa a `trial_paused(conflict)` y EMS replanifica; si la
resolución degrada confort, requiere confirmación del usuario. La UI muestra medido / simulado /
estimado con **procedencia** (icono + tooltip).

## 13. Experiencia responsive

Escritorio (tarjeta compacta + panel expandido) · Móvil (una columna, autorización legible) · Tableta
mural (resúmenes y voz/texto; autorizaciones sensibles requieren persona autenticada). Conversación
persistente; animaciones reflejan estados reales (nunca escucha ficticia). Accesibilidad, teclado,
estados textuales, movimiento reducido.

## 14. Primera oportunidad para el MVP

**Nombre:** "Aprovechar mejor mi energía solar". **Verticales:** Energía y Clima (+ Confort).
**Objetivo:** desplazar climatización flexible a ventanas de excedente solar, sin superar límites.
**Alcance MVP:** una vivienda piloto, un sistema térmico compatible, medición eléctrica fiable, lectura
real de temperatura, permiso temporal específico y una estrategia acotada. **No incluir inicialmente:**
optimización multi-actuador, ACS sin protecciones homologadas, aprendizaje autónomo que modifique
permisos, decisiones coordinadas de alto riesgo. **Usar el motor EMS y la infraestructura de lectura
existentes** antes de desarrollar un motor alternativo.

## 15. Criterios de aceptación

- No hay datos → no aparece beneficio inventado. · No existe permiso → no se envía actuación. · Permiso
  expirado → se rechaza la orden. · Equipo desconectado → no actúa a ciegas. · Cambio manual → se
  respeta. · Orden sin confirmación → no se declara verificada. · Prueba vencida → no se generan
  órdenes. · Dispositivo compartido → no se duplican identidades/autorizaciones. · Ahorro no
  demostrable → estimado/no concluyente. · Revocación → se bloquean órdenes y se gestiona recuperación.
  · Sin LLM remoto → protecciones locales operan. · Reinicio del servicio → estado consistente y seguro.

Métricas de cliente: oportunidades exploradas, simulaciones completadas, autorizaciones concedidas y
retiradas, pruebas finalizadas, resultados comprensibles y decisiones de adopción. **No establecer
objetivos comerciales/de rendimiento arbitrarios sin datos de piloto.**

## 16. Decisión de producto propuesta

**Aprobar como concepto funcional:** tarjeta de oportunidades con cinco fases progresivas, presencia
continua de Liloo, permisos acotados y resultados separados por niveles de evidencia. **Antes de
implementar:** contrastar contratos con los repositorios, identificar capacidades reales y completar
bloqueos de seguridad/recuperación. **No autorizado:** cambios de Core, ampliación de permisos,
actuación en equipos reales, pruebas físicas o despliegue en viviendas.

**Principio final:** *La oportunidad no termina cuando Kaenori actúa. Termina cuando el cliente
entiende qué ocurrió, qué mejoró, qué incertidumbre existe y conserva el derecho a decidir qué sucede
después.*
