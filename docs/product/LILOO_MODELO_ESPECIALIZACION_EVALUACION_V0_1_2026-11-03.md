# Liloo — Asistente único y capacidades especializadas
## Documento de evaluación de arquitectura y experiencia v0.1

**Fecha:** 2026-11-03  
**Estado:** **PROPUESTA ABIERTA / NO APROBADA**. Análisis de ventajas, riesgos y coste; no constituye mandato de implementación.  
**Relacionado:** [Wizard funcional v1](KAENORI_WIZARD_FUNCIONAL_V1_2026-11-03.md) y [UX Wizard + Dashboard v0.1](KAENORI_UX_WIZARD_DASHBOARD_V0_1_2026-11-03.md).

## 1. Problema que se quiere resolver

El cliente habla con una única asistente, **Liloo**, y espera que conozca el hogar y sepa responder consultas tan distintas como:
- «¿A qué temperatura está la casa?» — lectura estructurada y respuesta inmediata.
- «¿Cómo están las cámaras?» — comprobación de permisos, disponibilidad y, solo si se autoriza, acceso a imagen.
- «¿Podemos reducir el gasto de mañana sin pasar frío?» — combinar predicción, energía, climatización y preferencias.
- «Pon música en el salón» — controles multimedia compatibles, permisos y dispositivo destino.

Se plantea si conviene crear varios bots/identidades por vertical, un asistente monolítico o **una Liloo única con capacidades especializadas internas**.

## 2. Principio de experiencia — Liloo siempre es Liloo

**Una identidad, una voz de producto y un punto de entrada por vivienda.** El usuario no elige bots ni debe saber qué módulo se utiliza. Liloo está siempre accesible desde el Wizard y desde el Dashboard por **texto y voz**. El acceso puede abrir una conversación ampliada, pero el panel principal permanece navegable. El contexto de la pantalla actual puede ayudar a formular respuestas, sin conceder más privilegios.

**Decisión UX propuesta para el prototipo, no decisión de motor conversacional:** barra persistente visible con identidad Liloo, campo de texto y micrófono; accesible en escritorio, móvil y tableta. Usar un estado claramente visible de micrófono, enviar, procesamiento, falta de conexión y error. Nunca activar el micrófono sin una acción expresa del usuario. La transcripción de voz debe ser editable antes de confirmar una acción sensible.

## 3. Alternativas de arquitectura

| Alternativa | Experiencia | Complejidad / coste | Riesgos |
| --- | --- | --- | --- |
| **A. Liloo única con herramientas indiferenciadas** | Una conversación coherente | Bajo al inicio, puede aumentar con el catálogo | Instrucciones y herramientas demasiado amplias, pruebas difíciles |
| **B. Liloo única + enrutador de intenciones + módulos especializados** | Una misma Liloo que entiende cada ámbito | **Incremento moderado**, reutiliza Core y catálogo | Enrutamiento incorrecto, límites de responsabilidad poco claros |
| **C. Varios agentes autónomos coordinados** | Debe simular continuidad | Alto: memorias, coordinación, costes de inferencia y latencia | Conflictos entre agentes, permisos inconsistentes, opacidad |

**Recomendación provisional:** evaluar **B**, sin desplegar múltiples agentes persistentes. No hay evidencia de que C compense hoy. A puede ser suficiente mientras el número de herramientas y casos sea limitado.

## 4. Modelo operativo propuesto

```text
Usuario (voz/texto)
     ↓
Liloo — identidad y contexto conversacional por vivienda/usuario
     ↓
Clasificación de intención + selección de capacidades permitidas
     ├─ Lecturas directas: temperatura, disponibilidad, sensores, consumo
     ├─ Energía y Clima: datos, explicación, simulaciones, propuestas
     ├─ Seguridad y Accesos: estado, avisos, imágenes según permisos
     ├─ Confort y Ambiente: escenas y control de entorno
     ├─ Media y Entretenimiento: contenido y dispositivos AV
     └─ Jardín y Exteriores: riego, piscina, clima exterior
     ↓
Kaenori Core: inventario canónico + permisos + planner + Safety Gate
     ↓
Conector autorizado → lectura/acción verificada → auditoría → Liloo responde
```

**Reglas inviolables:**
- Un especialista es un conjunto de capacidades e instrucciones acotadas, **no una autoridad nueva** ni una memoria alternativa de la casa.
- En casos triviales, consultar datos estructurados y responder sin invocación adicional de un LLM.
- Liloo no inventa estados ni afirma haber controlado dispositivos antes de confirmar el resultado.
- Core determina autorizaciones, prioridad manual, límites técnicos y protección Life Safety.
- Memoria y contexto: separar preferencias globales por hogar, permisos por persona y contexto temporal de conversación; minimización y aislamiento entre viviendas.
- Los especialistas pueden cruzar verticales en una **petición compuesta**, pero el plan único se evalúa en Core para evitar acciones contradictorias.
- BDM/LLM remoto no son autoridad necesaria para el funcionamiento local.
- Acceso a vídeo/audio requiere permisos específicos y tratamiento de privacidad separado.

## 5. Impacto para el cliente: hipótesis comprobables

**Hipótesis H1 — confianza:** una única Liloo con una respuesta contextual y verificable transmite conocimiento real de la casa; múltiples personalidades crearían confusión.  
**H2 — rapidez:** lecturas simples directas son más rápidas que encadenar agentes.  
**H3 — calidad:** módulos especializados reducen herramientas irrelevantes y permiten instrucciones específicas.  
**H4 — gobernabilidad:** una única autoridad de acciones ofrece respuestas y explicaciones consistentes.

Estas son **hipótesis de diseño** y requieren evaluación con usuarios. Un router por sí solo no aporta valor sin dispositivos integrados, inventario fiable y buena UX.

## 6. Coste total y riesgo

La opción B añade clasificación de intención, catálogo de capacidades por módulo, contratos, pruebas de enrutamiento y observabilidad. Puede ahorrar trabajo futuro frente a un monolito que crece desordenadamente. La opción C exige coordinación de agentes, sincronización de contexto, protección de permisos, manejo de respuestas contradictorias y más inferencia/latencia.

**No asignar cifra de euros ni porcentaje de ahorro sin estimación real**: habrá que medir horas de implementación, cobertura de tests, coste por petición, p50/p95 de latencia, tasa de llamadas a modelo, tasa de errores, regresiones y mantenimiento. La voz tiene costes y latencias adicionales respecto a texto.

## 7. Estrategia de evaluación recomendada

1. Mantener interfaz Liloo única y persistente por voz/texto; definir estados de UI, permisos y accesibilidad.
2. Crear batería de consultas de referencia que combine lectura simple, consulta de seguridad, control reversible y petición transversal.
3. Instrumentar el sistema actual (línea base) en precisión y latencia, con pruebas simuladas sin actos físicos.
4. Pilotar internamente solo **Energía y Clima** y **Seguridad**, con enrutamiento a herramientas acotadas.
5. Comparar B frente a A: porcentaje de tareas bien resueltas, permisos respetados, explicaciones correctas, costes, latencia y fallos de enrutamiento.
6. Añadir otras capacidades por necesidad demostrada. C solo se justificaría con mejoras netas verificadas y complejidad gobernable.

## 8. Preguntas abiertas antes de ADR

- ¿Cuándo basta una consulta estructurada y cuándo se invoca razonamiento?
- ¿Qué interfaz contractual tendrá cada conjunto de herramientas?
- ¿Cómo se recuperan errores de clasificación y cómo se explica al cliente?
- ¿Dónde y cuánto tiempo se guarda la memoria conversacional? ¿Cómo se revoca?
- ¿Cómo se autentica a cada miembro para cámara, accesos y operaciones sensibles?
- ¿Qué motor STT/TTS local/externo proporciona la experiencia de voz, con qué disponibilidad?
- ¿Qué umbrales de calidad/latencia/coste justifican especialización real?

**Conclusión de evaluación:** diseñar Liloo como **interfaz única y permanente**, con arquitectura preparada para especialización interna. **No aprobar todavía la creación de múltiples agentes.**

**Este documento es análisis, no orden de construir ni actuar sobre dispositivos.**
