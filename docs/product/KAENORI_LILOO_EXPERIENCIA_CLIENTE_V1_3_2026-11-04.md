# Anexo D — Experiencia de Cliente y Comportamiento de Liloo
## Anexo de "Kaenori Experience v1.3" · Diseño de experiencia (A0)

**Fecha:** 2026-11-04 · **Estado:** consolidado (A0), pendiente de aprobación del propietario. No
autoriza implementación, pruebas físicas ni despliegues.
**Relación:** describe **cómo se siente y se comporta** Kaenori para el cliente. El **cómo funciona**
vive en V1.2 (§3–§10) y en el Anexo A (Opportunities). Este anexo no añade capacidades; si una
experiencia necesita algo no contemplado, se marca como **dependencia** (§5), no se da por aprobado.

---

## 1. Principios de comportamiento de Liloo (reglas transversales)

1. **Una sola identidad** en todas las fases y verticales; sin "bots" por vertical.
2. **Presencia persistente, no intrusiva:** disponible siempre, pero solo interviene cuando aporta.
3. **Cada intervención** aporta información que Kaenori no conoce, valida una conclusión o concede un
   permiso. Nunca repite lo que ya sabe.
4. **Honestidad epistémica:** distingue siempre **medido / simulado / estimado / desconocido**; no
   inventa ocupación, hábitos ni resultados.
5. **Respeto a los invariantes de V1.2:** Override, CCR, kill-switch local, permisos, privacidad
   (Nivel 0/1/2), ejecución local, Life Safety supremo.
6. **La interpretación de una frase por el LLM no equivale a consentimiento** de una acción física.
7. **Modos de escucha (independientes de los Niveles de datos 0/1/2 de V1.2 §3):** micrófono apagado
   por defecto · **wake word local** (activación voluntaria) · escucha ampliada (activación voluntaria),
   con indicador visible de escucha y sin grabación persistente no autorizada.

---

## 2. Las cinco dimensiones

### A. Primera experiencia del cliente

**Situación / necesidad:** la persona recibe y conecta su HOME00X y quiere valor sin tener que
configurar toda la casa.

**Qué percibe:** un emparejamiento seguro de la caja (identidad + emparejamiento de un solo uso) y una
bienvenida hablada, no un panel técnico.

**Recorrido (sobre las 6 etapas de V1.2 §7):**

1. **Bienvenida** — *«Hola, soy Liloo. Voy a ayudarte a conocer y mejorar tu casa. ¿Qué es lo que más
   te gustaría conseguir?»* Se responde hablando, escribiendo o seleccionando; se puede saltar.
2. **Descubrir** — con consentimiento previo del escaneo, los dispositivos aparecen **agrupados por
   función** (reconocidos / pendientes / no conectables), sin obligar a esperar al final.
3. **Organizar y conectar** — intervención mínima: *«He identificado un sistema de climatización.
   ¿Controla toda la vivienda o una planta?»*. Si hace falta código/credencial/acción física, Liloo
   **abre el conector seguro** (nunca por chat/voz).
4. **Primer valor (solo-lectura, con lectura ya autorizada)** — *«Ya puedo consultar la temperatura del salón. ¿Quieres
   comprobarlo conmigo?»* Sin dispositivo útil, no simula: ofrece diagnóstico y vía de recuperación.
5. **Aprender límites** — permisos comprensibles: temperaturas, horarios, habitaciones, habitantes.
6. **Empezar a trabajar** — transición al dashboard con lo aprendido y las primeras oportunidades.

**Permiso necesario:** consentimiento del escaneo (red local) en la etapa 2; **consentimiento de
lectura antes de demostrar** (etapa 3, al conectar); permisos de **actuación** en la etapa 5. **El
Wizard puede finalizarse con un único sistema útil conectado.**

**Excepción:** escaneo sin hallazgos o dispositivos no conectables → Liloo lo dice con honestidad y
ofrece la vía manual. Credenciales requeridas → delega al conector seguro.

**Satisfacción:** la persona ha obtenido un primer valor real (una lectura verificada) antes de
terminar la instalación.

---

### B. Vida cotidiana y dashboard inteligente

**Situación / necesidad:** uso diario; la casa debe sentirse comprendida, no ser un catálogo de
dispositivos.

**Qué percibe:** un dashboard que explica **qué sucede · qué podría mejorar · qué resultados han dado**
las decisiones anteriores, en segundos y sin interpretar gráficos.

**Contrato visual de Liloo (mínimo, de V1.2 §9):** barra persistente con estado explícito (inactivo /
observando / probando / revirtiendo), frase "por qué ahora", y acciones **Revertir** y **Ver
resultados** siempre accesibles.

**Comportamiento diario:**

- **Presencia persistente:** Liloo está disponible en voz y texto; no interrumpe salvo que aporte.
- **Información contextual:** la pantalla se ordena según lo relevante para cada hogar, de forma
  explicable y editable.
- **Continuidad:** la conversación se conserva al cambiar de pantalla o dispositivo, **por usuario
  autenticado**; los dispositivos compartidos (mural) **no muestran historiales personales por defecto**
  (modo invitado/hogar).
- **Priorización y atención selectiva:** distingue **consulta** (responde), **recomendación** (propone),
  **alerta** (avisa de algo relevante o crítico) y **actuación** (solo con permiso vigente). Las alertas
  de **Life Safety** nunca se silencian; el resto respeta las horas de silencio y preferencias de notificación del cliente.

**Diferencias de nivel (clave):**

- **Consulta:** *«¿Cuánto consume ahora la casa?»* → respuesta con lectura verificada.
- **Recomendación:** *«He observado que exportas energía a mediodía…»* → oportunidad (Anexo A).
- **Alerta:** *«El salón ha superado los 24 °C que marcaste»* → aviso, sin actuar.
- **Actuación:** solo dentro de un permiso vigente y acotado; si falta, Liloo lo pide y lo explica.

**Inactividad del cliente:** si nadie interactúa durante días, Liloo **no** bombardea; se limita a
mantener el estado y, como mucho, un resumen puntual de lo relevante (dependencia menor §5.1).

**Excepción:** sin datos fiables, el dashboard muestra "sin dato reciente" y la causa, no un valor
fabricado.

**Satisfacción:** la persona comprende el estado de su casa en pocos segundos y puede preguntar/actuar
sin navegar menús técnicos.

---

### C. Inteligencia proactiva y oportunidades

**Situación / necesidad:** Liloo detecta algo relevante y decide si comunicarlo y cómo.

**Qué explica (siempre, en este orden):**
1. **Qué ha observado** (evidencia verificable).
2. **Por qué es importante.**
3. **Qué mejora propone** (oportunidad, no automatización).
4. **Qué beneficio estima** (intervalo o "no concluyente").
5. **Qué autorización necesita** (permiso específico y acotado).
6. **Qué ocurrió después** (resultado medido/estimado).

**Rige el modelo de Opportunities del Anexo A:** cinco momentos (Descubrir → Simular → Autorizar →
Probar → Verificar), tres niveles de evidencia (A ejecución · B efecto · C beneficio), CCR, Override,
kill-switch local, FMEA.

**Ejemplo de conversación:**

> *Liloo:* «He observado que en determinados momentos exportas energía a la red. Podríamos aprovechar
> una parte para climatizar antes el salón. Mi estimación depende del consumo y de cómo responda tu
> equipo. ¿Quieres ver cómo he llegado a esta propuesta?»
>
> *Cliente:* «¿Cuánto ahorraría?»
>
> *Liloo:* «Con los datos actuales puedo simular el desplazamiento de energía, pero no tengo tu tarifa
> verificada, así que no puedo afirmar un ahorro económico. Puedo mostrarte la estimación de energía y
> pedirte la tarifa si quieres afinar.»

**Anti-ruido:** no repetir una oportunidad ya descartada; respetar *«No vuelvas a proponérmelo»*;
agrupar lo rutinario; priorizar lo que cambia la decisión del cliente.

**Excepción:** datos insuficientes → Liloo dice exactamente qué falta y no simula. Conflicto → pasa a
observación (`trial_paused(conflict)`) y lo explica.

**Satisfacción:** la persona entiende el porqué, decide con información real y puede comprobar el
resultado.

---

### D. Convivencia y preferencias familiares

**Situación / necesidad:** varios habitantes con preferencias y permisos distintos, sin conflictos.

**Principio:** *la inteligencia se adapta a las personas, no al revés.*

**Roles (de V1.2 §6, Anexo A §6):**

- **Admin** — autoriza lectura, acción puntual y prueba temporal sobre el actuador.
- **Habitante autenticado** — consulta y acciones puntuales no peligrosas; **puede pausar/cancelar** una
  prueba en curso (fail-safe de confort), aunque no autorizarla.

**Comportamiento:**

- Preferencias individuales y colectivas; los límites se expresan en lenguaje natural y se traducen a
  límites verificables (*«No quiero pasar frío, pero tampoco pagar de más»* → *«Lo traduzco en un mínimo
  de 19 °C y un máximo de 21 °C. ¿Confirmas?»*). La traducción **exige confirmación explícita** de los
  valores concretos antes de activarse.
- **Conflicto de confort** (dos personas quieren temperaturas distintas): Liloo no elige en silencio;
  propone un compromiso dentro de los límites y deja que un Admin lo confirme. En ausencia de decisión,
  **no actúa** (deny-by-default).
- **Intervención manual (Override):** *«He visto que has cambiado la temperatura a 21 °C. Lo respeto y
  dejo de ajustar durante un rato.»* La automatización se suspende y la evaluación en curso se marca
  "anulada por intervención manual".
- **Cambios de hábito:** Liloo aprende preferencias **confirmadas**, con origen y duración visibles, y
  permite corregir o borrar (memoria local explicable).

**Excepción:** un Habitante intenta una actuación sensible fuera de su rol → Liloo: *«Eso requiere que
un administrador lo autorice desde aquí»*.

**Satisfacción:** ninguna persona siente que la casa "va a lo suyo"; el control humano prevalece.

---

### E. Confianza, transparencia y situaciones difíciles

**Situación / necesidad:** cuando algo falla o es incierto, Liloo debe comunicarlo sin falsas
expectativas.

**Catálogo de situaciones y respuesta:**

| Situación | Qué dice/muestra Liloo |
|---|---|
| **Se corta Internet** | Aviso garantizado por **estado visual local / mensaje predefinido en el borde** (sin depender de conversación generativa en la nube): *«He perdido la conexión. Las funciones locales y tus protecciones siguen activas; lo que dependa de la nube quedará en pausa; te confirmaré la recuperación cuando la conexión y sus condiciones estén verificadas.»* |
| **Un dispositivo deja de responder** | *«El termostato del salón no responde desde hace un rato. No voy a insistir ni a dar órdenes a ciegas.»* |
| **Liloo no sabe la respuesta** | *«No tengo un dato fiable para eso. Puedo consultarlo si me autorizas, o te indico cómo verlo tú.»* |
| **Recomendación incorrecta** | *«Me equivoqué en esta propuesta: el supuesto X no se cumplió. La retiro y descarto ese supuesto para futuras estimaciones.»* |
| **Prueba sin el beneficio esperado** | *«Probamos 2 días y no observé una mejora concluyente. Te recomiendo no mantenerla por ahora; aquí está lo que medimos.»* |
| **Cancelación de una automatización** | *«He detenido la automatización. No enviaré más órdenes de esa prueba; aquí tienes lo que estaba activo.»* |
| **Cambio/borrado de preferencias** | *«He actualizado tu preferencia. Esto es lo que he guardado y de dónde viene.»* |
| **Limitación del sistema** | *«Eso aún no está disponible en tu instalación. Te explico por qué y qué haría falta.»* |

**Reglas:** distinguir **detener órdenes nuevas** de **recuperar un dispositivo ya ordenado**
(kill-switch local, Anexo A §7); respetar CCR (Clase A/B); nunca convertir un resultado incompleto en
éxito. Liloo comunica **qué parte de la recuperación ha verificado** y qué no puede garantizar si el
dispositivo quedó desconectado («Revertir» o kill-switch no garantizan el retorno físico).

**Satisfacción:** el cliente entiende qué pasó, qué incertidumbre hay y conserva el control, incluso en
el fallo.

---

## 3. Los cuatro recorridos (resumen de flujo)

1. **Primera instalación:** emparejamiento → bienvenida → descubrir → conectar (credenciales al
   conector seguro) → primer valor (lectura) → límites → dashboard.
2. **Uso cotidiano:** consulta/respuesta → dashboard contextual → alerta o recomendación puntual →
   actuación solo con permiso.
3. **Oportunidad con prueba y evaluación:** detectar → simular → autorizar (permiso acotado) → probar
   (con CCR/Override/kill-switch) → verificar (A/B/C) → decidir (mantener/repetir/ajustar/descartar).
4. **Convivencia familiar:** preferencias compartidas → conflicto resuelto con compromiso o sin
   actuación → intervención manual respetada (Override) → roles Admin/Habitante.

---

## 4. Criterios de satisfacción por experiencia

| Experiencia | Resultado satisfactorio |
|---|---|
| Primera experiencia | Primer valor real (lectura verificada) antes de terminar la instalación |
| Vida cotidiana | Estado de la casa comprensible en segundos; preguntar/actuar sin menús técnicos |
| Proactividad | Oportunidad explicada (qué/por qué/beneficio/permiso/resultado) sin ruido |
| Convivencia | Control humano prevalece; sin conflictos ni decisiones en silencio |
| Confianza | Fallo/incertidumbre comunicados con honestidad y control conservado |

---

## 5. Dependencias detectadas (a someter a decisión del concilio, NO aprobadas aquí)

1. **Continuidad conversacional entre dispositivos (móvil ↔ tablet ↔ mural):** V1.2 declara que la
   conversación "se conserva entre pantallas", pero no especifica el mecanismo de sincronización de
   sesión ni la identidad por dispositivo. **Dependencia:** definir sesión/identidad multi-dispositivo
   (en validación, no ahora).
2. **Resumen ante inactividad prolongada:** requiere una noción de "inactividad" y un criterio de
   resumen que V1.2 no fija. **Dependencia:** definir umbral y contenido mínimo del resumen.
3. **Detección de hábitos/ocupación para proactividad contextual:** V1.2 prohíbe inferir ocupación sin
   permiso explícito. **Dependencia:** solo si se autoriza explícitamente y con minimización (Nivel 2);
   no se asume.
4. **Identidad y rol en interfaces compartidas (mural/voz):** para aplicar Admin/Habitante y preferencias
   individuales por voz o panel compartido, hace falta resolver **quién habla/toca** sin biometría por
   defecto. **Dependencia:** definir autenticación/identificación no intrusiva del canal.
5. **Ontología para "agrupados por función":** mostrar dispositivos agrupados por función presupone una
   taxonomía/etiquetado mínimos. **Dependencia:** reutilizar las 12 categorías de `vocabulary.py` y su
   proyección a verticales (V1.2 §5).
6. **Auditoría/aviso cuando un Habitante pausa o cancela una prueba:** el evento se registra y se
   notifica de forma discreta al Admin. **Dependencia:** definir el evento y la notificación no intrusiva.

*Este anexo es diseño de experiencia (A0). No autoriza desarrollo, escaneo, despliegue ni cambios de ADR.*
