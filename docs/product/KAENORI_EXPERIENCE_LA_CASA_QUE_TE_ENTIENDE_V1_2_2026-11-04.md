# Kaenori Experience — La casa que te entiende
## Documento rector de producto · v1.2 (punto de congelación funcional) · Noviembre 2026

**Programa:** KAENORI-HOME00X-AUTONOMY · **Owner:** Marc · **Estado:** consolidado (A0) — congelación
funcional para iniciar validación. **No autoriza** implementación, cambios de Core, pruebas físicas ni
despliegues.

---

## 0. Mapa documental

| Rol | Documento |
|---|---|
| **Rector (único vivo)** | **Este documento** — Kaenori Experience v1.2 |
| **Anexo A** | `KAENORI_LILOO_OPPORTUNITIES_V1_2` — especificación funcional de la tarjeta de Oportunidades |
| **Anexo B** | `REGISTRO_DECISIONES_CONCILIO` — decisiones del concilio (rondas 4–8) |
| **Anexo C** | `MATRIZ_TRAZABILIDAD` — capacidad → módulo → código → test → evidencia → estado (implementado / parcial / pendiente / bloqueado) |
| **Histórico (no se edita)** | Experience v1.1 · Opportunities v1.1 · Resumen v1/v2 · Revisión estratégica · Wizard Funcional v1 · Síntesis R1/R2 |

**Regla:** toda decisión nueva se **fusiona aquí** (o en su anexo), nunca en un rector paralelo.

---

## 1. La promesa

> **Kaenori convierte tu vivienda conectada en una vivienda que te comprende.**
> Liloo descubre lo que tienes, aprende lo que necesitas, encuentra oportunidades para mejorar tu
> hogar y te ayuda a aprovecharlas. Siempre con tu permiso, con resultados comprensibles y sin
> quitarte el control.

**Criterio de éxito:** no se mide por dispositivos conectados, automatizaciones ni agentes de IA. Se
mide por **cuánto mejora la vida del cliente sin exigirle más esfuerzo**.

---

## 2. Inteligencia técnica → inteligencia percibida

Tres ejes diferenciales en una sola experiencia: **comprensión automática de la vivienda** ·
**inteligencia que persigue resultados** · **autonomía que genera confianza**.

---

## 3. Invariantes de gobierno (no negociables)

- **Core v1.0 congelado** (6 puertos); **un conector no toca a otro**; **Home Assistant = adaptador,
  no autoridad.**
- **descubrir ≠ controlar** · **deny-by-default** · **fail-closed** · **local-first**.
- **Sin secretos/PII** en prompts, logs ni Git.

**Reglas de experiencia derivadas (correcciones del concilio):**

1. **Credenciales jamás por chat/voz** — la captura la hace el **conector seguro**.
2. **Fail-closed con Home Assistant** — solo-lectura/observación ante indicios de automatización
   externa activa sobre un equipo crítico, hasta confirmación del usuario.
3. **El primer momento de valor es solo-lectura.**
4. **Kill-switch local:** funciona **sin BDM, Internet ni LLM**, y distingue **detener nuevas órdenes**
   de **recuperar un dispositivo que ya recibió una orden** (ver Anexo A, CCR).

**Política de datos y LLM (v1.2):**

- **Nivel 0** — telemetría anónima agregada. **Nivel 1** — estado de dispositivo sin identificadores.
  **Nivel 2** — contexto sensible (horarios, presencia).
- **Nada ≥ Nivel 1 sale del dispositivo** sin consentimiento explícito + minimización + redacción.
- **Modo offline obligatorio** para toda función crítica. La memoria doméstica es local; el LLM remoto
  solo recibe lo autorizado y anonimizado.

---

## 4. Tres planos, un inventario

| Plano | Qué es |
|---|---|
| **0 · Conectividad (L1)** | Fundación transversal: descubrir, identificar, emparejar, inventario canónico. Foco de entrada, no vertical comercial. |
| **1 · Verticales fijas** | Líneas de producto que ve el cliente; se pueblan/activan por detección. |
| **2 · Objetivos y preferencias** | Prioridades dentro de permisos, límites y reglas de seguridad. |

**Regla de oro:** un dispositivo puede participar en varias verticales **sin duplicar su identidad**.

---

## 5. Verticales de producto (5) y proyección taxonómica (12→5)

**Verticales fijas:** 1 Energía y Clima · 2 Seguridad y Accesos · 3 Confort y Ambiente · 4 Media y
Entretenimiento · 5 Jardín y Exteriores.

**Proyección desde las 12 categorías técnicas** (`home_onboarding/vocabulary.py`) **→ 5 verticales:**

| Categoría técnica | Vertical de producto |
|---|---|
| aerotermia, climatización, fotovoltaica, batería, cargador_ve, vehículo_eléctrico | Energía y Clima |
| cámara | Seguridad y Accesos |
| iluminación, persiana | Confort y Ambiente (persiana además apoya Energía y Seguridad) |
| multimedia | Media y Entretenimiento |
| electrodoméstico | Energía y Clima (si es gestionable) o Confort |
| red | **Infraestructura transversal oculta** (sin vertical comercial) |
| — riego / piscina (previsto en P3) | Jardín y Exteriores |

**Nota de arquitectura:** "Media", "Jardín" y la infraestructura "red" se sirven vía `ha-adapter` de
inicio, manteniendo el aislamiento de conectores y **sin elevar a HA a autoridad**. Todo dispositivo
vía `ha-adapter` cuyo TTL nativo no se pueda demostrar (criptográfica o empíricamente) se degrada por
defecto a **CCR Clase B** (excluido de pruebas automáticas de riesgo) y opera bajo **fail-closed** ante
indicios de automatización externa activa.

---

## 6. Perfiles = presets combinables (no personalidades cerradas)

Presets: **Ahorro inteligente · Máximo confort · Equilibrado (sugerencia) · Tranquilidad
(transversal) · Autoconsumo y sostenibilidad.** El cliente puede elegir un preset **o describir sus
prioridades** en lenguaje natural, que Liloo traduce a prioridades y límites verificables. El perfil
**no concede permisos** ni cambia rangos técnicos.

---

## 7. El recorrido (Wizard) — seis etapas, vividas como conversación

1 **Bienvenida** · 2 **Descubrir** · 3 **Organizar y conectar** (credenciales → conector seguro) ·
4 **Demostrar** (primer valor, solo-lectura) · 5 **Aprender límites** (permisos concretos) ·
6 **Empezar a trabajar** (transición al dashboard).

El Wizard puede finalizarse con **un único sistema útil conectado**.

---

## 8. Oportunidades, no automatizaciones

La tarjeta de Oportunidades (especificación completa en **Anexo A**) materializa la promesa.
**Cinco momentos:** Descubrir → Simular → Autorizar → Probar → Verificar. **Tres niveles de evidencia:**
A ejecución verificada · B efecto observado · C beneficio demostrado/estimado.

**Requisitos formales v1.2 (incorporados del concilio):**

- **CCR — Clasificación de Capacidad de Reversión:** **Clase A** (recupera un estado seguro al expirar
  el TTL, **con evidencia de que lo hace de verdad**, no solo por anuncio del fabricante) · **Clase B**
  (sin TTL). Los equipos **Clase B sin recuperación segura verificada** no participan en pruebas
  automáticas de riesgo.
- **Override:** la intervención manual detiene la automatización y **respeta el nuevo estado** (no
  restaura ciegamente la consigna anterior). La evaluación queda **invalidada** cuando la intervención
  impide atribuir resultados.
- **Kill-switch local:** detener órdenes nuevas ≠ recuperar un dispositivo ya ordenado (Anexo A).

---

## 9. Dashboard que cuenta la historia de la vivienda

Explica **qué sucede · qué podría mejorar · qué resultados han dado**, en segundos y sin exigir
interpretar gráficos. **Adaptativo** (orden relevante, explicable y editable) y con **atención
selectiva** (sin silenciar alertas críticas).

**Contrato visual mínimo de Liloo (v1.2):** barra persistente de Liloo con estado explícito
(inactivo / observando / probando / revirtiendo), frase de "por qué ahora", y acciones **Revertir** y
**Ver resultados** siempre accesibles. Voz, texto y estados son continuos entre pantallas.

---

## 10. Autonomía progresiva y confianza

- **Autonomía ganada, no concedida:** observar → demostrar → permiso concreto → prueba temporal.
- **Control manual prioritario (hold/override):** la actuación manual **prevalece** y suspende la
  automatización (ver Anexo A, Override).
- **Memoria doméstica explicable:** 100 % local; ver qué se aprendió, su origen y duración; corregir o
  borrar; no inferir ocupación/hábitos sin permiso.
- **Gestión de la confianza:** *¿Por qué? · No vuelvas a proponérmelo · Deshazlo · Pausa · Enséñame qué
  sabes*.

---

## 11. Cuatro recorridos de usuario (sobre la misma arquitectura)

1. **Bienvenida y primera conexión** — valor antes de tener toda la casa configurada.
2. **Dashboard cotidiano** — Liloo siempre presente, no un catálogo de dispositivos.
3. **Oportunidades** — recomendación → prueba real, evaluada y reversible.
4. **Convivencia** — preferencias de varias personas, control manual y automatizaciones sin conflictos.

---

## 12. Dictamen del concilio (recomendación técnica, NO aprobación del Owner)

Las 12 modificaciones, con la **recomendación del concilio** (la **aprobación es del propietario**,
§13):

| # | Ref | Descripción resumida | Recomendación del concilio | Bloqueante |
|---|---|---|---|---|---|
| 1 | A | Verificación y permisos: separar lectura/control/autorización; el éxito se demuestra | Aceptar | Sí |
| 2 | B | Autoridad de Core: gobierno local inmutable | Aceptar | Sí |
| 3 | C | Conflictos con HA: fail-closed ante indicios | Aceptar (sin relajar) | Sí |
| 4 | D | Permisos granulares: lectura / acción puntual / automatización, revocables | Aceptar | Sí |
| 5 | E | Contexto adicional: tarifa/potencia solo cuando sea necesario | Modificar | No |
| 6 | F | Identidades y habitaciones: dedup + agrupación confirmable | Aceptar | No |
| 7 | G | Protecciones técnicas: reglas verificadas por sistema/fabricante | Aceptar | Sí |
| 8 | H | Perfil Tranquilidad: objetivo transversal | Modificar | No |
| 9 | I | Privacidad: consentimiento explícito, minimización, transparencia | Aceptar | Sí |
| 10 | J | Offline y reanudación: requisito esencial | Elevar (P0) | No |
| 11 | K | Hogar/usuarios/zonas: 2 fases (Admin+Habitante → roles en P2/P3) | Elevar | No |
| 12 | L | Simulación y KPIs: beneficios demostrados/simulados/desconocidos | Aceptar | No |

---

## 13. Aprobación del propietario (gate, pendiente)

El propietario marca por número **aceptar / cambiar / quitar**. Los bloqueantes (1, 2, 3, 4, 7, 9)
protegen invariantes; rechazarlos exige redefinir la regla, no "saltársela".

```
1 [ ] aceptar [ ] cambiar [ ] quitar (bloqueante)   7 [ ] aceptar [ ] cambiar [ ] quitar (bloqueante)
2 [ ] aceptar [ ] cambiar [ ] quitar (bloqueante)   8 [ ] aceptar [ ] cambiar [ ] quitar
3 [ ] aceptar [ ] cambiar [ ] quitar (bloqueante)   9 [ ] aceptar [ ] cambiar [ ] quitar (bloqueante)
4 [ ] aceptar [ ] cambiar [ ] quitar (bloqueante)  10 [ ] aceptar [ ] cambiar [ ] quitar
5 [ ] aceptar [ ] cambiar [ ] quitar                11 [ ] aceptar [ ] cambiar [ ] quitar
6 [ ] aceptar [ ] cambiar [ ] quitar                12 [ ] aceptar [ ] cambiar [ ] quitar
```

---

## 14. Hoja de ruta y criterios de aceptación

**P0** producto fiable y conectable · **P1** experiencia unificada de Liloo · **P2** inteligencia que
demuestra valor (3 experiencias) · **P3** ampliación del ecosistema. **Sin ampliar verticales, agentes
ni conectores en P0–P2.**

**Primer piloto:** instalación sin asistencia técnica · ≥1 equipo real · estado verificado · permiso
trazable · recuperación tras reinicio · funcionamiento local. **KPIs:** tiempo hasta primera
experiencia útil · enrolado completado · problemas de instalación · utilidad percibida (7 días) ·
propuestas aceptadas/descartadas. **Seguridad: cero actuaciones físicas no autorizadas.**

**Antes de implementar (paso de validación):** contrastar cada capacidad prometida con el código real
(ver **Anexo C — Matriz de trazabilidad**).

---

## 15. Consolidación documental

V1.2 cierra el diseño funcional (congelación para validación). No se reabre el debate conceptual salvo
**contradicción de seguridad o limitación técnica demostrada**. Lo absorbido: Revisión estratégica,
Resumen v1/v2, Wizard Funcional v1 (histórico), Opportunities v1.1 (→ Anexo A v1.2), dictámenes
rondas 4–7 (→ Anexo B).

*Documento rector de producto (A0). No autoriza desarrollo, escaneo, despliegue ni cambios de ADR.*
