# Kaenori Experience — La casa que te entiende
## Documento rector de producto · v1.1 · Noviembre 2026

**Programa:** KAENORI-HOME00X-AUTONOMY · **Owner:** Marc · **Estado:** rector consolidado (A0);
pendiente de validación técnica, formalización ADR y autorización de implementación.

---

## 0. Mapa documental (cómo se organiza todo)

| Rol | Documento |
|---|---|
| **Rector (único vivo)** | **Este documento** — Kaenori Experience v1.1 |
| **Anexo A** | `KAENORI_LILOO_OPPORTUNITIES_V1_1` — especificación funcional de la tarjeta de Oportunidades |
| **Anexo B** | `REGISTRO_DECISIONES_CONCILIO` — decisiones del concilio (rondas 4, 5, 6) consolidadas |
| **Histórico (superado, no se edita)** | `RESUMEN_EJECUTIVO` v1/v2 · `REVISION_ESTRATEGICA_PRODUCTO` · `KAENORI_WIZARD_FUNCIONAL_V1` · `SINTESIS_VERTICALES` R1/R2 |

**Regla:** toda decisión nueva se **fusiona aquí** (o en su anexo), nunca se crea un rector paralelo.

---

## 1. La promesa

> **Kaenori convierte tu vivienda conectada en una vivienda que te comprende.**
> Liloo descubre lo que tienes, aprende lo que necesitas, encuentra oportunidades para mejorar tu
> hogar y te ayuda a aprovecharlas. Siempre con tu permiso, con resultados comprensibles y sin
> quitarte el control.

**Criterio de éxito:** no se mide por dispositivos conectados, automatizaciones ejecutadas ni agentes
de IA. Se mide por **cuánto mejora la vida del cliente sin exigirle más esfuerzo**.

---

## 2. Inteligencia técnica → inteligencia percibida

| Ya tenemos (inteligencia técnica) | Debemos construir (inteligencia percibida) |
|---|---|
| Discovery, catálogo, conectores, motores de decisión, permisos, conversación | Una vivienda que **se explica, aprende, propone y demuestra resultados** |

Tres ejes diferenciales en una sola experiencia: **comprensión automática de la vivienda** ·
**inteligencia que persigue resultados** · **autonomía que genera confianza**.

---

## 3. Invariantes de gobierno (no negociables)

- **Core v1.0 congelado** (6 puertos); **un conector no toca a otro**; **Home Assistant = adaptador,
  no autoridad.**
- **descubrir ≠ controlar** · **deny-by-default** · **fail-closed** · **local-first**.
- **Sin secretos/PII** en prompts, logs ni Git.

**Tres reglas de experiencia derivadas (correcciones del concilio):**

1. **Credenciales jamás por chat/voz** — Liloo orquesta y explica; la captura la hace el **conector
   seguro** (WebView/formulario local/QR/NFC).
2. **Fail-closed con Home Assistant** — ante indicios de que un equipo crítico lo gobierna una
   automatización activa en HA, Kaenori permanece en **solo-lectura/observación** hasta confirmación
   del usuario. No se relaja por "evitar bloqueos por sospechas".
3. **El primer momento de valor es solo-lectura** — se demuestra con **lectura verificada**; las
   acciones quedan bloqueadas hasta límites y permisos.

---

## 4. Tres planos, un inventario

| Plano | Qué es |
|---|---|
| **0 · Conectividad (L1)** | Fundación transversal. Descubrir, detectar puentes, identificar, emparejar con catálogo, inventario canónico. Es el **foco de entrada**, no una vertical comercial. |
| **1 · Verticales fijas** | Líneas de producto que ve el cliente. Se **pueblan/activan** por detección; no se crean dinámicamente. |
| **2 · Objetivos y preferencias** | Prioridades dentro de permisos, límites y reglas de seguridad. |

**Regla de oro:** un dispositivo puede participar en varias verticales **sin duplicar su identidad**.

---

## 5. Verticales de producto (5, fijas)

1. **Energía y Clima** — consumo/coste, tarifas, solar FV, baterías, aerotermia, calefacción,
   refrigeración, ACS, movilidad eléctrica. *(Internamente un solo dominio; la interfaz permite
   consultar energía y climatización por separado.)*
2. **Seguridad y Accesos** — cámaras, alarmas, detectores, cerraduras, videoporteros.
3. **Confort y Ambiente** — persianas, iluminación, escenas, estores, ambientes. *(Puede participar
   de Energía y de Seguridad sin duplicar dispositivos.)*
4. **Media y Entretenimiento** — televisores, altavoces, sistemas AV, multiroom.
5. **Jardín y Exteriores** — riego, piscina, depuradora, meteorología, exteriores.

**Compartidos:** persianas = Confort + apoyo a Energía; depuradora = Jardín + carga gestionable por
Energía.

> **Nota de arquitectura:** "Media" y "Jardín" aún **no tienen conector dedicado** (solo `security,
> ha-adapter, discovery, ems, liloo, experience`). Se sirven vía `ha-adapter` de inicio, **manteniendo
> el aislamiento de conectores y sin elevar a HA a autoridad** (control bajo los 6 puertos congelados
> de Core v1.0; HA actúa como traductor).

---

## 6. Perfiles = presets combinables (no personalidades cerradas)

| Preset | Promesa | Regla orientativa |
|---|---|---|
| **Ahorro inteligente** | Pagar menos sin renunciar a lo esencial | Menor coste con límites de confort |
| **Máximo confort** | La casa se adapta a mis hábitos | Condiciones preferidas; gasto como 2º criterio |
| **Equilibrado** *(sugerencia)* | Buen balance ahorro-bienestar | Compromiso dentro de límites |
| **Tranquilidad** *(transversal)* | Sentirme al corriente y protegido | Supervisión y avisos; **no** es alternativa a seguridad/ahorro |
| **Autoconsumo y sostenibilidad** | Aprovechar mejor mi energía | Cargas flexibles a producción local |

- El cliente puede elegir un preset **o describir sus prioridades** en lenguaje natural, que Liloo
  traduce a **prioridades y límites verificables** revisables.
- "Tranquilidad" es una **preferencia transversal**, no una categoría excluyente.
- El perfil **no concede permisos de control** ni cambia rangos técnicos.

---

## 7. El recorrido (Wizard) — seis etapas, vividas como conversación

La identidad y el **emparejamiento seguro de la caja** son un paso previo imprescindible.

1. **Liloo te da la bienvenida** — *«¿Qué es lo que más te gustaría conseguir?»*. Sin configuración
   técnica; se puede saltar.
2. **Liloo descubre tu vivienda** — conectividad con consentimiento previo; dispositivos agrupados por
   funciones (reconocidos / pendientes / no conectables), sin obligar a esperar al final.
3. **Liloo organiza y conecta** — validación con intervención mínima; si hace falta código/credencial/
   acción física, **delega la captura al conector seguro** (nunca por chat/voz).
4. **Liloo demuestra lo que ya sabe hacer** — primer valor con **lectura verificada**; sin dispositivo
   útil, no simula: ofrece diagnóstico y vía de recuperación.
5. **Liloo aprende tus límites** — permisos comprensibles y contextualizados; se autorizan
   **capacidades concretas**, no autonomía ilimitada.
6. **Tu casa ya empieza a trabajar contigo** — transición al dashboard; empieza la observación, el
   aprendizaje autorizado y las propuestas útiles.

El Wizard puede finalizarse con **un único sistema útil conectado**; el resto se incorpora después.

---

## 8. Oportunidades, no automatizaciones

El cliente no quiere programar un flujo inversor↔aerotermia↔sensor. Quiere saber **si puede
aprovechar mejor sus placas sin perder confort**. Esto lo materializa la **tarjeta de Oportunidades**
(especificación completa en **Anexo A**).

**Cinco momentos:** **1 Descubrir → 2 Simular → 3 Autorizar → 4 Probar → 5 Verificar.**

**Contrato de una "Oportunidad":** requisitos comprobados · beneficio esperado (con nivel de
confianza) · permisos requeridos · prueba temporal supervisada y reversible · métrica de resultado.
**No se presenta un ahorro como conseguido antes de medirlo.** Estados: *lista para usar / requiere
permiso o conexión / no compatible / datos insuficientes*.

**Tres niveles de evidencia (obligatorio separar):**

- **A — Ejecución verificada** (¿hizo el dispositivo lo que se pidió?).
- **B — Efecto observado** (¿ocurrió el cambio esperado?).
- **C — Beneficio demostrado o estimado** (¿mejoramos autoconsumo/coste/confort?) — con **método de
  línea base** e intervalo de confianza; sin método → "**estimación no concluyente**".

**Reglas de integración (resoluciones del concilio, ronda 6):**

1. **No existe un "motor de oportunidades" nuevo.** Es un **conjunto de reglas de elegibilidad**:
   Experience evalúa **plantillas declarativas** sobre Discovery y las simulaciones de EMS, sin
   instanciar un motor independiente.
2. **Máquina de estados completa y reversible** (incluye post-adopción: `adopted_active / paused /
   expired`, invalidación de simulación, retornos desde `evaluated`, recuperación de `failed`).
3. **Finalización segura sin bloquear el MVP:** si un equipo no tiene expiración nativa por hardware,
   se cumple con **Watchdog local persistente (Core/EMS) + heartbeat** antes de cada consigna.
4. **Multiusuario asimétrico:** el **Admin** autoriza la prueba; **cualquier Habitante** puede
   **pausar/cancelar** la prueba en curso (fail-safe de confort).
5. **Árbitro de conflictos:** **Life Safety > límites > manual > objetivos > rutinas**; ante
   conflicto → `trial_paused(conflict)` y EMS replanifica.

---

## 9. Dashboard que cuenta la historia de la vivienda

Explica, en pocos segundos y sin exigir interpretar gráficos: **qué sucede · qué podría mejorar ·
qué resultados han dado** las decisiones anteriores. **Adaptativo** (orden relevante, explicable y
editable) y con **atención selectiva** (rutina vs oportunidad vs acontecimiento, sin silenciar
alertas críticas). Liloo **no sustituye** al dashboard; **lo hace inteligente**.

---

## 10. Autonomía progresiva y confianza

- **Autonomía ganada, no concedida:** observar → demostrar → solicitar permiso para una oportunidad
  concreta (*«¿Quieres que pruebe durante una semana?»*). Detener, corregir, retirar.
- **Control manual prioritario (hold):** toda actuación manual/física detectada activa **suspensión
  temporal** de la oportunidad activa (modo observación durante un ciclo configurable), salvo límites
  de seguridad superiores.
- **Memoria doméstica explicable:** 100 % local; muestra qué se aprendió, su origen y duración; permite
  corregir o borrar; no infiere ocupación ni hábitos sin permiso.
- **Gestión de la confianza:** *«¿Por qué?»*, *«No vuelvas a proponérmelo»*, *«Deshazlo»*, *«Pausa»*,
  *«Enséñame qué sabes»*. Decisiones importantes con historial entendible.

---

## 11. Las 12 modificaciones (dictamen consolidado)

| # | Ref | Área | Decisión final | Nota |
|---|---|---|---|---|
| 1 | A | Verificación y permisos | **Aceptar** | Separar lectura/control/autorización; el éxito se demuestra. |
| 2 | B | Autoridad de Core | **Aceptar** | Gobierno local como contrato inmutable. |
| 3 | C | Conflictos con HA | **Aceptar (sin relajar)** | Fail-closed ante indicios; no actuar sin confirmación. |
| 4 | D | Permisos granulares | **Aceptar** | Presentados como decisiones humanas comprensibles. |
| 5 | E | Contexto adicional | **Modificar** | Preguntar tarifa/potencia solo cuando sea necesario. |
| 6 | F | Identidades y habitaciones | **Aceptar** | Agrupación sugerida, confirmable y corregible. |
| 7 | G | Protecciones técnicas | **Aceptar** | Reglas verificadas por sistema/fabricante. |
| 8 | H | Perfil Tranquilidad | **Modificar** | Objetivo transversal; no categoría excluyente. |
| 9 | I | Privacidad | **Aceptar** | Consentimiento explícito, minimización, transparencia. |
| 10 | J | Offline y reanudación | **Elevar (P0)** | Requisito esencial, no mejora opcional. |
| 11 | K | Hogar/usuarios/zonas | **Elevar (2 fases)** | P0/P1: Admin + Habitante; roles (invitados/menores) → P2/P3. |
| 12 | L | Simulación y KPIs | **Aceptar** | Beneficios demostrados/simulados/desconocidos diferenciados. |

**Bloqueantes** (protegen invariantes): **1, 2, 3, 4, 7, 9.**

---

## 12. Hoja de ruta y criterios de aceptación

| Fase | Prioridad | Contenido |
|---|---|---|
| **P0 · Producto fiable y conectable** | 1 | Emparejamiento seguro, conexión persistente, verificación, primer arranque, recuperación, actualización fiable. |
| **P1 · Experiencia unificada de Liloo** | 2 | Un solo recorrido, conversación persistente, Wizard adaptativo, primer resultado real (lectura), dashboard útil. |
| **P2 · Inteligencia que demuestra valor** | 3 | Validar 3 experiencias: optimización energética, confort por habitación, supervisión de seguridad. |
| **P3 · Ampliación del ecosistema** | 4 | Multimedia, jardín, nuevas integraciones, altavoz Liloo. |

**Primer piloto:** instalación sin asistencia técnica · al menos un equipo real · estado verificado ·
permiso trazable · recuperación tras reinicio · funcionamiento local. **KPIs:** tiempo hasta primera
experiencia útil · enrolado completado · problemas de instalación · utilidad percibida (7 días) ·
propuestas aceptadas/descartadas. **Seguridad: cero actuaciones físicas no autorizadas.**

---

## 13. Decisión pendiente

Marca por número **aceptar / cambiar / quitar** para cerrar la v1.1 funcional (bloqueantes marcados):

```
1 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)    7 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)
2 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)    8 [ ] aceptar [ ] cambiar [ ] quitar
3 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)    9 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)
4 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)    10[ ] aceptar [ ] cambiar [ ] quitar
5 [ ] aceptar [ ] cambiar [ ] quitar                  11[ ] aceptar [ ] cambiar [ ] quitar
6 [ ] aceptar [ ] cambiar [ ] quitar                  12[ ] aceptar [ ] cambiar [ ] quitar
```

---

## 14. Consolidación documental (qué se unificó/absorbió)

Esta v1.1 resuelve los solapes y contradicciones acumulados:

- **`Revisión estratégica de producto`** → absorbida (su visión, los 6 cambios, el roadmap y la
  promesa ya están en §§1–2, 6–10 y 12).
- **`Resumen Ejecutivo` v1/v2** → absorbido (superado por este rector).
- **`Wizard Funcional v1`** → queda como **histórico**; su recorrido de 6 etapas y las 12
  modificaciones viven en §§7 y 11.
- **`Liloo Opportunities v1`** → **Anexo A** (v1.1), con las 9 mejoras del concilio integradas; su
  resumen vive en §8.
- **Dictámenes (rondas 4, 5, 6)** → **Anexo B** (registro único de decisiones).
- **Contradicción resuelta:** "motor de oportunidades" → **"reglas de elegibilidad"** (sin motor
  nuevo); "perfiles excluyentes" → **presets combinables**; "conflicto HA: modificar" → **aceptar sin
  relajar (fail-closed)**.

*Documento rector de producto (A0). No autoriza desarrollo, escaneo, despliegue ni cambios de ADR.*
