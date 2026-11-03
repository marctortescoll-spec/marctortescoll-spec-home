# Kaenori Experience — La casa que te entiende
## Documento rector de producto · v1 · Noviembre 2026

**Programa:** KAENORI-HOME00X-AUTONOMY · **Owner:** Marc · **Estado:** propuesta rectora consolidada
(A0); pendiente de validación técnica, formalización ADR y autorización de implementación.
**Procedencia:** evolución de `KAENORI_WIZARD_FUNCIONAL_V1` + `RESUMEN_EJECUTIVO_V2` +
`REVISION_ESTRATEGICA_PRODUCTO` + dictámenes del concilio (rondas 4 y 5).

---

## 1. La promesa

> **Kaenori convierte tu vivienda conectada en una vivienda que te comprende.**
> Liloo descubre lo que tienes, aprende lo que necesitas, encuentra oportunidades para mejorar tu
> hogar y te ayuda a aprovecharlas. Siempre con tu permiso, con resultados comprensibles y sin
> quitarte el control.

Kaenori no es "una app de domótica mejor diseñada": es el paso a que **las personas dejen de
configurar y gestionar su domótica**. El cliente no compra un controlador; compra una vivienda que
**comprende, ayuda y se adapta progresivamente**.

**Criterio de éxito:** no se mide por dispositivos conectados, automatizaciones ejecutadas ni
agentes de IA. Se mide por **cuánto mejora la vida del cliente sin exigirle más esfuerzo**.

---

## 2. Inteligencia técnica → inteligencia percibida

| Ya tenemos (inteligencia técnica) | Debemos construir (inteligencia percibida) |
|---|---|
| Discovery, catálogo, conectores, motores de decisión, permisos, conversación | Una vivienda que **se explica, aprende, propone y demuestra resultados** |

Tres ejes diferenciales combinados en una sola experiencia:

1. **Comprensión automática de la vivienda** — reconoce lo que el cliente ya tiene y reduce el
   trabajo de instalación.
2. **Inteligencia que persigue resultados** — convierte lo detectado en propuestas de ahorro,
   bienestar y tranquilidad, sin obligar a programar automatizaciones.
3. **Autonomía que genera confianza** — explica, respeta a todos los habitantes, demuestra lo hecho
   y conserva el control humano.

---

## 3. Invariantes de gobierno (no negociables)

- **Core v1.0 congelado** (6 puertos); **un conector no toca a otro**; **Home Assistant = adaptador,
  no autoridad.**
- **descubrir ≠ controlar** · **deny-by-default** · **fail-closed** · **local-first**.
- **Sin secretos/PII** en prompts, logs ni Git.

**Tres reglas de experiencia derivadas de esas invariantes (correcciones del concilio):**

1. **Credenciales jamás por chat/voz.** Liloo orquesta y explica el motivo, pero la captura la hace
   el **conector seguro** (WebView/formulario local/QR/NFC). Nunca se dicta un PIN, contraseña o
   token a Liloo.
2. **Fail-closed con Home Assistant.** Ante indicios de que un equipo crítico (clima/inversor) lo
   gobierna una automatización activa en HA u otro controlador, Kaenori permanece en
   **solo-lectura/observación** hasta que el usuario confirme la cesión de autoridad. No se relaja
   por "evitar bloqueos por sospechas".
3. **El primer momento de valor es solo-lectura.** Se demuestra con **lectura verificada**; las
   acciones quedan bloqueadas (fail-closed) hasta límites y permisos.

---

## 4. Tres planos, un inventario

| Plano | Qué es |
|---|---|
| **0 · Conectividad (L1)** | Fundación transversal. Descubrir, detectar puentes, identificar, emparejar con catálogo, inventario canónico. Es el **foco de entrada**, no una vertical comercial. |
| **1 · Verticales fijas** | Líneas de producto que ve el cliente. Se **pueblan/activan** por detección; no se crean dinámicamente. |
| **2 · Objetivos y preferencias** | Prioridades dentro de permisos, límites y reglas de seguridad. |

**Regla de oro:** un dispositivo puede participar en varias verticales **sin duplicar su identidad**
(inventario y permisos únicos por capacidad).

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

> **Nota de arquitectura:** "Media" y "Jardín" aún **no tienen conector dedicado** (solo
> `security, ha-adapter, discovery, ems, liloo, experience`). Se sirven vía `ha-adapter` de inicio,
> **manteniendo el aislamiento de conectores y sin elevar a HA a autoridad** (control bajo los 6
> puertos congelados de Core v1.0; HA actúa como traductor).

El cliente **no está obligado a ver las cinco desde el principio**: solo se destacan las áreas que
realmente contienen funciones conectadas y utilizables.

---

## 6. Perfiles = presets combinables (no personalidades cerradas)

Los cinco perfiles no son identidades excluyentes, sino **configuraciones iniciales ajustables**
sobre un modelo interno combinable:

| Preset | Promesa | Regla orientativa |
|---|---|---|
| **Ahorro inteligente** | Pagar menos sin renunciar a lo esencial | Menor coste con límites de confort |
| **Máximo confort** | La casa se adapta a mis hábitos | Condiciones preferidas; gasto como 2º criterio |
| **Equilibrado** *(sugerencia)* | Buen balance ahorro-bienestar | Compromiso dentro de límites |
| **Tranquilidad** *(transversal)* | Sentirme al corriente y protegido | Supervisión y avisos; **no** es una alternativa a seguridad/ahorro |
| **Autoconsumo y sostenibilidad** | Aprovechar mejor mi energía | Cargas flexibles a producción local |

- El cliente puede **elegir un preset o describir sus prioridades** en lenguaje natural
  (*«No quiero pasar frío, pero tampoco pagar de más»*), que Liloo traduce a **prioridades y límites
  verificables** revisables.
- "Tranquilidad" es una **preferencia transversal**, no una categoría excluyente.
- El perfil **no concede permisos de control** ni cambia rangos técnicos.

---

## 7. El recorrido (Wizard) — seis etapas, vividas como conversación

La identidad y el **emparejamiento seguro de la caja** son un paso previo imprescindible.

1. **Liloo te da la bienvenida** — *«Hola, soy Liloo. ¿Qué es lo que más te gustaría conseguir?»*.
   Se responde hablando, escribiendo o seleccionando; se puede saltar. Sin configuración técnica.
2. **Liloo descubre tu vivienda** — conectividad con consentimiento previo; los dispositivos
   aparecen progresivamente agrupados por funciones (reconocidos / pendientes / no conectables).
   No obliga a esperar al final.
3. **Liloo organiza y conecta** — validación con intervención mínima (*«¿El sistema de climatización
   controla toda la vivienda o una planta?»*). Cuando haga falta código, credenciales o acción
   física, **delega la captura al conector seguro** (nunca por chat/voz). Al terminar, comprueba
   que la conexión persiste y que los datos son reales.
4. **Liloo demuestra lo que ya sabe hacer** — primer momento de valor con **lectura verificada**
   (una lectura real, una consulta contextual o una oportunidad concreta). Sin dispositivo útil,
   no simula: ofrece diagnóstico y vía de recuperación.
5. **Liloo aprende tus límites** — permisos comprensibles y contextualizados (temperaturas,
   horarios, habitaciones, habitantes, acciones permitidas). Se autorizan **capacidades concretas**,
   no autonomía ilimitada.
6. **Tu casa ya empieza a trabajar contigo** — transición al dashboard: lo aprendido, las conexiones
   operativas, las limitaciones y las primeras oportunidades. No es un final: empieza la observación,
   el aprendizaje autorizado y las propuestas útiles.

El Wizard puede finalizarse con **un único sistema útil conectado**; el resto se incorpora después
con el mismo asistente, sin repetir la configuración inicial.

---

## 8. Oportunidades, no automatizaciones

El cliente no quiere programar un flujo inversor↔aerotermia↔sensor. Quiere saber **si puede
aprovechar mejor sus placas sin perder confort**.

**Contrato de una "Oportunidad":**

- requisitos **comprobados**;
- **beneficio esperado** (con nivel de confianza);
- **permisos requeridos**;
- **prueba temporal supervisada y reversible**;
- **métrica de resultado**.

**No se presenta un ahorro como conseguido antes de medirlo.** La propuesta distingue siempre:
*lista para usar / requiere permiso o conexión / no compatible / datos insuficientes*.

Ejemplos: *"Excedentes solares para ACS"*, *"Estado de seguridad al salir"* — con sus requisitos,
límites y condiciones explícitas.

---

## 9. Dashboard que cuenta la historia de la vivienda

El dashboard explica tres cosas, en pocos segundos y sin exigir interpretar gráficos:

1. **Qué sucede** (estado verificado de los sistemas).
2. **Qué podría mejorar** (oportunidades).
3. **Qué resultados han dado** las decisiones anteriores.

- **Adaptativo:** ordena la información según lo relevante para cada hogar, de forma **explicable y
  editable** (no inventa ocupación ni hábitos no autorizados/verificados).
- **Atención selectiva:** distingue información rutinaria, oportunidades y acontecimientos
  relevantes. El cliente elige cuándo y cómo ser interrumpido, **sin silenciar alertas críticas**.
- Liloo no sustituye al dashboard; **lo hace inteligente** (botones, controles o conversación).

---

## 10. Autonomía progresiva y confianza

- **Autonomía ganada, no concedida:** Liloo observa → demuestra que entiende → solicita autorización
  para oportunidades concretas (*«¿Quieres que pruebe durante una semana?»*). El cliente puede
  detener, corregir y retirar el permiso.
- **Control manual prioritario (hold):** toda actuación manual/física detectada activa una
  **suspensión temporal** de la oportunidad activa (modo observación durante un ciclo configurable),
  salvo límites de seguridad superiores.
- **Memoria doméstica explicable:** 100% local; muestra qué se aprendió, su origen y duración, y
  permite corregir o borrar. No se infiere ocupación ni hábitos sin permiso.
- **Gestión de la confianza:** el usuario puede decir *«¿Por qué?»*, *«No vuelvas a proponérmelo»*,
  *«Deshazlo»*, *«Pausa»*, *«Enséñame qué sabes»*. Las decisiones importantes dejan historial
  entendible.

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
| 11 | K | Hogar/usuarios/zonas | **Elevar (2 fases)** | P0/P1: Administrador + Habitante/control manual; roles (invitados/menores) → P2/P3. |
| 12 | L | Simulación y KPIs | **Aceptar** | Beneficios demostrados/simulados/desconocidos diferenciados. |

**Bloqueantes** (protegen invariantes): **1, 2, 3, 4, 7, 9.** Rechazarlos exige redefinir la regla
correspondiente, no solo "saltársela".

---

## 12. Hoja de ruta y criterios de aceptación

| Fase | Prioridad | Contenido |
|---|---|---|
| **P0 · Producto fiable y conectable** | 1 | Emparejamiento seguro, conexión real y persistente, verificación de datos, primer arranque, recuperación y actualización fiable. |
| **P1 · Experiencia unificada de Liloo** | 2 | Unificar asistentes en un recorrido, conversación persistente, Wizard adaptativo, primer resultado real (lectura) y dashboard útil. |
| **P2 · Inteligencia que demuestra valor** | 3 | Validar 3 experiencias completas: optimización energética, confort por habitación y supervisión de seguridad, con equipos y permisos reales. |
| **P3 · Ampliación del ecosistema** | 4 | Multimedia, jardín, nuevas integraciones y altavoz Liloo, tras validar el recorrido principal. |

**Primer piloto (criterios de aceptación):** instalación sin asistencia técnica habitual; al menos
un equipo real incorporado; consulta de un estado verificado; permiso de actuación trazable;
recuperación tras reinicio; funcionamiento local de lo prometido.

**KPIs clave:** tiempo hasta la primera experiencia útil · tasa de enrolado realmente completado ·
problemas de instalación · utilidad percibida (7 días) · proporción de propuestas aceptadas/descartadas.
**Seguridad: cero actuaciones físicas no autorizadas.**

---

## 13. Decisión pendiente

Marca por número **aceptar / cambiar / quitar** para cerrar la v1.1 funcional:

```
1 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)    7 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)
2 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)    8 [ ] aceptar [ ] cambiar [ ] quitar
3 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)    9 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)
4 [ ] aceptar [ ] cambiar [ ] quitar  (bloqueante)    10[ ] aceptar [ ] cambiar [ ] quitar
5 [ ] aceptar [ ] cambiar [ ] quitar                  11[ ] aceptar [ ] cambiar [ ] quitar
6 [ ] aceptar [ ] cambiar [ ] quitar                  12[ ] aceptar [ ] cambiar [ ] quitar
```

*Documento rector de producto (A0). No autoriza desarrollo, escaneo, despliegue ni cambios de ADR.*
