# KAENORI — Wizard de incorporación inteligente
## Diseño funcional v1.0 · producto

**Fecha:** 2026-11-03  
**Estado:** **CERRADO COMO DISEÑO FUNCIONAL DE REFERENCIA** en conversación de producto; pendiente de validación técnica, formalización ADR y autorización de implementación.  
**Autoridad:** documento de producto, no mandato de despliegue, integración, escaneo real ni gobierno de actuadores.  
**Procedencia:** evolución del [borrador v0.1](KAENORI_WIZARD_PERFILES_VERTICALES_DRAFT_V0_1_2026-11-03.md).  
**Documento relacionado:** [Liloo: especialización interna y análisis de coste — v0.1](LILOO_MODELO_ESPECIALIZACION_EVALUACION_V0_1_2026-11-03.md) (**aún no decidido**).

## 1. Visión y promesa

Kaenori comprende primero qué valora el cliente, descubre después los recursos que hay en su vivienda y propone cómo utilizarlos. Su fortaleza tecnológica es la inteligencia energética, **sin imponer el ahorro como prioridad a todos los hogares**. La experiencia debe ser sencilla, explicable, reversible y segura.

**Principio de UX:** «Dime qué quieres conseguir; yo te mostraré qué puede hacer tu casa, qué necesito conectar y qué te recomiendo activar».

El **perfil de cliente** responde a *para qué*; las **verticales** responden a *en qué parte del hogar*; las **plantillas** responden a *cómo se consigue*; las **capacidades verificadas** delimitan *qué es realmente posible*. No confundir estos planos con el modelo de especialización conversacional de Liloo, que queda fuera de este documento.

## 2. Tres planos, un inventario

- **Plano 0 — Conectividad (L1, fundación técnica transversal).** Descubrimiento autorizado, detección de puentes y de dispositivos dependientes, identificación, emparejamiento de catálogo, inventario canónico y estado de integración. No es una vertical comercial.
- **Plano 1 — Verticales fijas del producto.** Agrupan las capacidades del hogar de manera comprensible. Un mismo dispositivo puede participar en varias verticales sin multiplicar su identidad.
- **Plano 2 — Objetivos y preferencias.** Definen las prioridades de planificación dentro de permisos, límites de confort, restricciones técnicas y reglas de seguridad.

**Orden visible del Wizard:** primero preguntar objetivo, después solicitar consentimiento y ejecutar conectividad, y a continuación construir verticales y propuestas. El orden de presentación no altera la dependencia técnica de Conectividad como fundamento.

## 3. Perfiles de cliente — cinco opciones de partida

| Perfil | Promesa comprensible | Regla orientativa |
| --- | --- | --- |
| **Ahorro inteligente** | Pagar menos sin renunciar a lo imprescindible | Preferir costes menores con límites explícitos de confort |
| **Máximo confort** | Que la vivienda se adapte a mis hábitos | Mantener condiciones preferidas; optimizar el gasto como segundo criterio |
| **Equilibrado** | Una buena combinación de ahorro y bienestar | Buscar compromiso entre ambos criterios, siempre dentro de los límites |
| **Tranquilidad** | Sentirme al corriente y protegido | Favorecer supervisión y avisos autorizados; nunca rebajar reglas Life Safety en otros perfiles |
| **Autoconsumo y sostenibilidad** | Aprovechar mejor mi propia energía | Desplazar cargas flexibles a producción local disponible cuando sea útil y permitido |

**Reglas de selección:** un objetivo principal y opcionalmente uno secundario; **Equilibrado** como sugerencia inicial, no imposición; configuración editable después del alta; posibilidad futura de prioridades por vertical, zona o dispositivo. El perfil elegido **no concede permisos de control** ni cambia automáticamente rangos técnicos o protecciones.

**Ejemplo:** dos viviendas con el mismo equipo solar y de aerotermia pueden escoger objetivos distintos. En Ahorro se priorizan horarios eficientes; en Confort se anticipa la climatización para mantener temperaturas deseadas. En ambos casos se respetan límites autorizados y condiciones de seguridad.

## 4. Verticales estables — cinco

1. **Energía y Clima:** consumo y coste, tarifas, solar FV, baterías, aerotermia, calefacción, refrigeración, ACS y movilidad eléctrica.
2. **Seguridad y Accesos:** cámaras, alarmas, detectores, cerraduras y videoporteros.
3. **Confort y Ambiente:** persianas, iluminación, escenas, estores y ambientes.
4. **Media y Entretenimiento:** televisores, altavoces, sistemas AV, reproducción y multiroom.
5. **Jardín y Exteriores:** riego, piscina, depuradora, meteorología y zonas exteriores.

**Un dispositivo, varios usos:** persianas = Confort + apoyo a Energía; depuradora = Jardín + carga gestionable por Energía. Inventario y permisos únicos por capacidad. No duplicar dispositivos entre verticales. Una vertical sin equipos operativos puede ocultarse de la pantalla inicial y seguir disponible en catálogo.

Las verticales son **fijas y se pueblan o activan** por detección y conectividad, no se generan dinámicamente.

## 5. Recorrido del Wizard — seis etapas

### 1. Tu objetivo
Pantalla con cinco tarjetas de valor, lenguaje no técnico, ejemplo breve de lo que cambia y selección opcional secundaria. Salida: `HomeGoals` preliminar, **sin permisos**.

### 2. Descubramos tu vivienda
Explicación de alcance, privacidad y consentimiento específico para el escaneo. Mostrar actividad y progreso en directo sin prometer un tiempo exacto: «Buscando equipos», «Identificando puentes», «Descubriendo dispositivos asociados», «Comprobando compatibilidad». Detectar en dos fases: red, servicios y *bridges*; después, hijos de esos *bridges*. Considerar VLANs, mDNS limitado, equipos dormidos, equipos offline y latencias. Separar lectura de red, conexión con cuentas y emparejamientos que requieren autorización adicional.

**Máquina de estados por dispositivo:** `detectado → identificado → conectado → autorizado → operativo`; los estados pueden quedar pendientes o fallar con explicación. «Detectado» **nunca implica** acceso a datos, imagen ni control.

### 3. Entendamos tu casa
Unificar identidades y reconciliar resultados del matcher con catálogo; agrupar por habitaciones y zonas mediante evidencias, pidiendo confirmación cuando haya incertidumbre. Mostrar inventario con categoría, marca si está verificada, capacidad disponible, estado y permisos pendientes. Asignar capacidades a las verticales fijas. Permitir correcciones sin requerir conocimientos de protocolos.

### 4. Estas son las mejoras que te proponemos
Motor de recomendaciones = **objetivos + dispositivos/capacidades operativas + contexto de vivienda + reglas de seguridad**. Una plantilla representa una intención reusable (p. ej., «aprovechar excedente solar para ACS») con prerrequisitos, telemetría requerida, acciones posibles, restricciones, compatibilidad, explicación y opción de simular. Distinguir **lista para usar / necesita conexión o permiso / no compatible / datos insuficientes**. No recomendar una automatización ejecutable si su actuador no ha sido comprobado.

### 5. Decide hasta dónde puedo llegar
Definir límites personales y físicos: temperaturas y horarios aceptados, prioridades por estancia, excepciones, accesos, privacidad de cámaras, confirmación de operaciones sensibles. Escalones de autonomía: **solo observar y explicar / proponer y pedir autorización / ejecutar automáticamente solo dentro de permisos específicos vigentes**. Mostrar cómo pausar y revocar cada automatización.

### 6. Empecemos a aprender
Activación progresiva. Observar línea base antes de afirmar ahorro real; explicar las primeras oportunidades y pedir la autorización que corresponda. Registrar decisión, acción, resultado y causa en formato accesible. La vivienda debe seguir funcionando localmente si se desconecta BDM o un proveedor LLM.

## 6. Plantillas por capacidad, no por marca

**Contrato funcional mínimo de una plantilla:** ID, nombre legible, objetivo compatible, verticales beneficiadas, capacidades requeridas de lectura/control, calidad mínima de datos, permisos, precondiciones, límites, conflictos, propuesta o plan, verificación, deshacer/pausa, métricas y descripción comprensible.

**Ejemplo «Excedentes solares para climatización/ACS»:** requiere medida actual de producción/consumo, dispositivo térmico compatible y autorizado, rango de confort permitido, capacidad de modificar consigna o programación. Si no hay control autorizado, **mostrar solo recomendación**. Si no hay datos solares, no presentar cálculo de ahorro como hecho.

**Ejemplo «Estado de seguridad al salir»:** requiere disponibilidad verificada de sensores/cámaras y lectura permitida. La visualización de cámaras necesita permisos aparte. No activar cerraduras o alarmas por deducción implícita.

## 7. Reglas de gobierno

Arquitectura de referencia:

```text
Discovery → Inventario canónico → Catálogo de capacidades
        → Verticales → Objetivos y preferencias
        → Planificador compartido → Safety Gate
        → Conector/actuador autorizado → Verificación + auditoría
```

- **Una autoridad de gobierno local por vivienda:** Kaenori Core; Home Assistant, cuando corresponda, actúa como adaptador de integraciones, no como autoridad superior.
- **Seguridad vital** (incendio, evacuación, protección física), límites y enclavamientos técnicos prevalecen en todo momento. Una preferencia de ahorro/confort no los anula.
- **Acción manual sobre un recurso:** prima sobre la optimización automática ordinaria durante una ventana de respeto configurable, salvo protecciones superiores.
- **Objetivos de cliente:** orientan la selección entre acciones seguras y autorizadas; rutinas y confort complementario quedan por debajo de lo anterior.
- **Permisos:** explícitos, minimizados, auditables y revocables por vivienda/usuario/capacidad. Ningún perfil añade autorización de manera silenciosa.
- **Fallos y conflictos:** comportamiento seguro y explicación; no afirmar éxito hasta recibir evidencia del dispositivo.
- **Privacidad:** no usar imágenes de cámaras, vídeo o información sensible fuera del permiso y finalidad autorizados.

## 8. Experiencia y criterios de aceptación

El Wizard será satisfactorio cuando:
- Una persona sin conocimientos técnicos pueda elegir un objetivo y entender qué hará Kaenori.
- El descubrimiento autorizado informe de avance, fallos y posibles faltantes sin inventar detecciones.
- Cada equipo tenga identidad única y capacidades compartibles entre varias verticales.
- La interfaz separe claramente «encontrado», «conectado» y «puedo usarlo».
- Las sugerencias solo prometan acciones respaldadas por capacidades y permisos reales.
- Cambiar el perfil actualice objetivos sin alterar seguridad ni conceder permisos nuevos.
- El cliente pueda consultar «por qué», pausar y revocar.
- Una vivienda sin Internet, BDM o LLM remoto conserve el control local habilitado y sus protecciones.
- Existan pruebas de conflictos de confort/ahorro, intervención manual, equipos offline y dispositivos compartidos.

## 9. Límites y siguientes artefactos

**Cerrado a nivel funcional:** cinco perfiles, cinco verticales, conectividad L1 transversal, seis etapas, inventario único, plantillas por capacidades, autoridad de Core y prioridad de seguridad.

**Pendiente antes de implementación:** contratos exactos del matcher y de plantillas, permisos por miembro, política de privacidad de medios, tolerancias de red, diseño visual, instrumentación, criterios cuantitativos de aceptación y revisión frente a ADR previos. Estas pendientes **no reabren el concepto funcional** por defecto; se documentarán como detalles técnicos o modificaciones explícitas.

**Sin autorización aquí** para desarrollar, escanear redes, abrir servicios, modificar dispositivos, iniciar despliegues ni cambiar ADR vigentes.

---

**Control documental:** v1.0 de producto. Sustituye como referencia de discusión el borrador v0.1 del Wizard, que permanece como historial. La especialización de Liloo se evalúa separadamente y no es condición de validez del Wizard.
