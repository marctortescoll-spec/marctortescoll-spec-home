# KAENORI — Experiencia de cliente: Wizard + Dashboard + Liloo
## Especificación visual y funcional UX/UI v0.1 · 3 de noviembre de 2026

**Estado:** DISEÑO PROPUESTO PARA REVISIÓN (no validación técnica, no producto implementado).  
**Alcance:** recorrido completo desde encender HOME00X hasta utilizar el panel principal; diseño de escritorio, tableta y móvil; interacción persistente con Liloo por voz y texto; información, estados y criterios verificables.  
**No implica:** escaneo real, autorización de controles, activación de micrófonos, instalación, cambios de firmware o despliegue.

**Documentos coordinados:**
- [1 · Wizard funcional v1.0](KAENORI_WIZARD_FUNCIONAL_V1_2026-11-03.md) — base funcional de referencia.
- [2 · Liloo especialización v0.1](LILOO_MODELO_ESPECIALIZACION_EVALUACION_V0_1_2026-11-03.md) — hipótesis de arquitectura, no aprobada.
- **3 · Este documento** — diseño de experiencia continua Wizard → dashboard y presencia de Liloo.

> **Regla de producto no negociable para este diseño:** Liloo está **siempre visible o accesible en el mismo contexto**, mediante logotipo-identidad y **compositor persistente de conversación**, con teclado y control explícito de micrófono. La experiencia no se divide en varios bots visibles ni obliga al usuario a abandonar la pantalla para conversar.

---

## 1. Visión de experiencia

**Promesa:** «Tu vivienda, entendida y adaptada a ti». El cliente no tiene que elegir protocolos ni construir automatizaciones desde cero. Entra por sus prioridades, ve qué equipos están realmente disponibles y puede actuar de forma gradual. La experiencia final no debe parecer una consola de Home Assistant expuesta al cliente: prioriza lenguaje natural, objetivos, estado fiable, propuestas accionables y transparencia.

### Principios
1. **Liloo en todo momento:** identidad coherente durante alta y uso diario, chat disponible abajo y experiencia ampliable.
2. **Primero valor, después tecnología:** explicar qué gana el cliente; detalle técnico desplegable, no impuesto.
3. **No fingir capacidades:** distinguir «encontrado», «conectado», «autorizado» y «operativo».
4. **Una casa, un inventario:** habitaciones, verticales y automatizaciones referencian los mismos dispositivos y permisos.
5. **Gobierno explícito:** seguridad, límites y consentimiento antes de cada clase de acción.
6. **Resultados explicables:** el sistema comunica qué sabe, qué ha recomendado, qué ha ejecutado y si funcionó.
7. **Diseño adaptable:** escritorio, tablet fija de pared, móvil y accesibilidad con teclado y lector de pantalla.

## 2. Arquitectura de navegación visible

Niveles:
- **Inicio · Mi hogar** — dashboard general.
- **Energía y Clima** — consumo, producción, gasto, temperatura, ACS, movilidad, ajustes autorizados.
- **Seguridad y Accesos** — sensores, cámaras y accesos sujetos a permisos.
- **Confort y Ambiente** — iluminación, persianas, escenas y ambiente.
- **Media y Entretenimiento** — TV, altavoces y reproducción, con autorización cuando corresponda.
- **Jardín y Exteriores** — piscina, riego y meteorología.
- **Habitaciones** — vista transversal por estancia.
- **Actividad** — decisiones, alertas, propuestas y cambios con trazabilidad.
- **Ajustes** — perfiles, límites, autorizaciones, miembros, privacidad, dispositivos y diagnóstico.

**Reglas:** menú vertical en escritorio; barra inferior y navegación compacta en móvil; al entrar en una vertical no cambia el personaje ni se pierde la conversación con Liloo. El perfil de cliente es un **objetivo transversal**, no una pestaña ni una vertical.

### Esqueleto escritorio (wireframe funcional)
```text
┌──────────────────────────────────────────────────────────────────────────────────┐
│ ◇☀ KAENORI              Hogar > Inicio       Perfil: Equilibrado       ⚙ Ajustes │
├─────────────────┬────────────────────────────────────────────────────────────────┤
│ Inicio          │ Buenos días. Esta es tu casa hoy.                            │
│ Energía y Clima │ [Estado vivienda] [Energía] [Temperatura] [Seguridad]        │
│ Seguridad       │                                                                │
│ Confort         │ [Línea de energía]        [Habitaciones / clima]              │
│ Media           │                                                                │
│ Jardín          │ [Oportunidades de Liloo]  [Escenas / controles rápidos]      │
│ Habitaciones    │                                                                │
│ Actividad       │ [Actividad reciente / información de fiabilidad]             │
├─────────────────┴────────────────────────────────────────────────────────────────┤
│ ◇☀ Liloo   Pregúntame por tu casa…                                    [🎙][Enviar] │
└──────────────────────────────────────────────────────────────────────────────────┘
```

**Esqueleto móvil:** cabecera reducida con estado y perfil; una columna de tarjetas ordenables; barra de navegación compacta; compositor Liloo **fijo por encima del área segura y del teclado**. Al abrir chat se muestra un panel deslizante con historial, contexto y regreso claro; nunca tapa accidentalmente un consentimiento sensible.

## 3. Identidad visual propuesta

**Carácter:** tecnológico pero doméstico, claro y confiable. No estetizar el producto como centro de ciberseguridad ni como tablero de desarrollo.

- **Logotipo Liloo:** tomar como referencia el símbolo escogido de rombos alrededor de un centro solar. No sustituir identidad de marca sin aprobación; mantener zona de reserva para el vector oficial.
- **Animación:** latido suave o respiración de los rombos al escuchar; pulso distinto al procesar; estado de respuesta visible. La animación no debe aparentar escucha continua.
- **Tipografía:** familia sans moderna legible, jerarquía corta y etiquetas en lenguaje corriente; tamaño base de texto ~16 px web, botones de al menos 44×44 px en móvil.
- **Paleta orientativa, pendiente de contrastar con manual de marca:** fondos claros suaves (p.ej. #F7F9FC), superficies blancas, texto azul carbón (#142238), identidad azul-violeta (#5665E9), acento turquesa (#19ACAB), énfasis cálido solar (#F2B45C), bordes gris suave. Se proponen líneas/acento de marketing discretos; **no** toda la UI coloreada por vertical.
- **Color semántico:** usar estado de seguridad con icono + texto, nunca confiar solo en color. Gráficos muestran unidades, periodo, origen de dato y actualización.

## 4. Wizard completo — seis pantallas con Liloo integrada

En todas las etapas: cabecera con **«Configurando tu hogar · paso N de 6»**, guardar/reanudar, volver, explicación «por qué te lo pedimos», y barra Liloo persistente con pregunta contextual. Nunca adelantar una barra de progreso ficticia.

### W1 · Objetivo y prioridades
**Título:** «¿Qué te gustaría mejorar en tu hogar?»  
**Subtítulo:** «Elige tu prioridad. Podrás cambiarla cuando quieras».  
Cinco tarjetas con icono, frase y resultado esperado: Ahorro inteligente, Máximo confort, Equilibrado, Tranquilidad, Autoconsumo y sostenibilidad. Selección principal única y secundaria opcional; Equilibrado sugerido por defecto, confirmado por la persona.

**Respuesta de Liloo:** «Perfecto. Priorizaré tu confort y buscaré oportunidades de ahorro que no lo perjudiquen. Antes de actuar te explicaré mis límites».  
**CTA:** «Continuar y descubrir mi casa».

### W2 · Descubrimiento de conectividad (L1)
**Título:** «Vamos a conocer los dispositivos de tu casa».  
Explicar consentimiento, alcance y duración variable. Botón explícito «Autorizar análisis». Antes de aceptarlo no se escanea. Mostrar cuatro eventos de proceso: encontrar red y servicios; identificar puentes; consultar hijos; comparar con catálogo. Registro visible en tarjetas progresivas: **detectado / identificado / conectado / autorizado / operativo**. Interfaz no presenta equipos sin matcher fiable como integrados. 
**Estados:** escaneo con hallazgos en directo, pausado, cancelado, error parcial, red VLAN no accesible, sensor dormido, dispositivo offline y repetición. Puede continuarse con resultados parciales, señalando limitaciones.  
**Pregunta Liloo:** «¿Por qué no aparece mi sensor de la planta de arriba?» → explicación sin inventar.

### W3 · Así es tu vivienda
**Título:** «Esto es lo que he encontrado».  
Resumen verificable: equipos encontrados; identificados; conectables; operativos; que requieren autorización. Listado con nombre, ubicación sugerida, capacidad, verticales vinculadas y estado. Confirmación de habitaciones y corrección manual, sin forzar conocimientos de fabricante. Los no compatibles quedan informados, no desaparecen.  
**CTA:** «Confirmar mi inventario».  
**Liloo:** «He encontrado un equipo de climatización, pero todavía necesito permiso para gobernarlo».

### W4 · Tus oportunidades
**Título:** «Estas son las mejoras que encajan contigo».  
Tarjetas de **plantillas por capacidad**, no por marca: por ejemplo «Aprovecha energía solar para climatizar», «Mantén confort al llegar» y «Resumen de seguridad al salir». Cada una indica: problema que resuelve, equipo necesario, estimación cuando sea justificable, estado de preparación, límites, permisos pendientes, ver detalle/simular, activar después.  
**Estados:** lista, necesita integración, falta dato, no compatible. No ofrecer como ejecutable una plantilla sin capacidades operativas y autorización.  
**CTA:** «Elegir recomendaciones».

### W5 · Permisos, límites y autonomía
**Título:** «Tú decides hasta dónde puedo llegar».  
Controles comprensibles: temperaturas preferidas y mínimos/máximos seguros, horarios, habitaciones prioritarias, acceso de miembros, privacidad de vídeo y permiso por tipo de acción. Tres modos:
1. Observar y explicar.
2. Proponer y pedir confirmación.
3. Ejecutar solo acciones previamente permitidas y dentro de límites específicos.

Mostrar un resumen transparente: «Liloo podrá consultar X; para cambiar Y te preguntará; nunca hará Z sin permiso». Diferenciar autorización de datos, visualización de cámaras y control físico. Revocación fácil. 
**CTA:** «Confirmar límites».

### W6 · Tu hogar está listo para empezar
**Título:** «Ya podemos empezar a conocer mejor tu casa».  
Resumen real: perfil, verticales con equipos operativos, pendientes, propuestas seleccionadas, autonomía elegida y próximos pasos. La activación del modo autónomo nunca puede darse por supuesta; requiere permisos vigentes.  
**CTA principal:** «Ir a Mi hogar».  
**Liloo:** «Desde aquí puedes preguntarme cualquier cosa sobre tu casa, por voz o por escrito».

**Regla de reanudación:** si se interrumpe el Wizard, almacenar estado mínimo seguro y continuar en el último paso confirmado; no repetir permisos automáticamente ni considerarlos aceptados por haber vuelto a abrir la página.

## 5. Dashboard después del Wizard

### Home / Mi hogar — composición priorizada

**A. Cabecera persistente.** Logo Kaenori, nombre de vivienda, perfil actual (con opción de cambiar), acceso a privacidad y configuración. Indicar si se está mostrando información actual o últimos datos recibidos. Mostrar conexión local/nube solo cuando sea pertinente.

**B. Estado de la vivienda.** Un mensaje principal humano:
- «Todo lo que puedo comprobar está en orden» **solo si la cobertura y los datos permiten esa afirmación**.
- «2 elementos requieren tu atención».
- «No tengo datos recientes del sensor del estudio».
Evitar afirmar que todo el hogar es seguro por carecer de cámaras o sensores.

**C. Panorama a primera vista (cuatro tarjetas funcionales).**
1. **Energía ahora:** consumo actual, fotovoltaica y red, cuando estén medidos; diferencia entre kW instantáneos y kWh de un período.
2. **Clima y confort:** temperatura por zonas, consigna, modo, fuente de lectura y última actualización.
3. **Seguridad y accesos:** eventos y sensores que realmente estén disponibles; cámaras nunca abiertas automáticamente sin permisos.
4. **Recomendaciones Liloo:** oportunidades y preguntas con siguiente acción clara, marcando «propuesta», «pendiente» o «ejecutada».

**D. Ámbitos y habitaciones.** Tarjetas de cinco verticales con situación y acceso profundo; sección de habitaciones para tareas cotidianas («salón», «dormitorios»). Las verticales sin equipos útiles no monopolizan espacio.

**E. Control rápido seguro.** Escenas configuradas y acciones reversibles; si no hay permiso, CTA de solicitud; confirmación para decisiones sensibles. Siempre indicar si la orden se ha enviado y si se ha verificado.

**F. Actividad y explicaciones.** «Qué ha ocurrido», «Qué ha propuesto Liloo», «Qué está pendiente», «Por qué se tomó una decisión», con filtro y fechas. No simular resultados.

### Cada vertical — estructura homogénea
- Resumen del ámbito, estado y tendencia.
- Equipos y capacidades disponibles por habitación.
- Objetivos y límites aplicables a ese ámbito.
- Recomendaciones/plantillas relacionadas.
- Registro de acciones recientes y motivo.
- Compositor Liloo con **contexto de la pantalla** (p.ej. «¿Cuánta electricidad gastó el cargador ayer?»).
- Si un equipo pertenece a varias verticales, mostrarlo en varias **vistas** con un único estado/identidad de fondo.

### Ejemplos de consulta durante navegación
| Contexto | Mensaje del cliente | Experiencia esperada |
|---|---|---|
| Inicio | «¿Cómo está la casa?» | Síntesis de lecturas disponibles y carencias, sin certificar lo no monitorizado |
| Energía | «¿Cuánto gastamos ayer?» | Cifra real con unidad y período, o «no tengo datos» |
| Clima | «¿A qué temperatura está el salón?» | Lectura del sensor y tiempo de actualización, sin generar una automatización |
| Seguridad | «Enséñame la cámara de entrada» | Confirmar permiso/rol, estado del vídeo, abrir vista solo con autorización |
| Jardín | «¿Podemos desplazar la depuradora a horas solares?» | Propuesta basada en capacidades y límites, pendiente de permiso si hace falta |
| Multimedia | «Pon música en el salón» | Selección de equipo/cuenta y confirmación de la acción, o motivo concreto de imposibilidad |

## 6. Liloo omnipresente: diseño detallado de texto y voz

**Componente base:** logotipo Liloo, título «Pregúntale a Liloo», campo «Escribe o habla sobre tu casa…», botón micrófono con nombre accesible, botón enviar, opción «Ampliar conversación», indicador de estado. El componente está **anclado visualmente** en Wizard y dashboard; incluso cuando se navega, no se pierde el contenido en curso.

**Modos:**
- **Reposo:** campo editable y micrófono apagado, ejemplos de consultas según pantalla.
- **Escritura:** enviar con botón/Enter; Shift+Enter salto de línea; respuesta contextual y fuente/estado.
- **Escucha (por activación explícita):** borde e icono de micrófono claramente activos, texto «Escuchando…», control «Detener» y transcripción en vivo si el motor existe. Mostrar política de uso de audio y motor local/remoto según realidad.
- **Interpretación:** texto «Entendiendo tu petición…»; opción cancelar.
- **Acción sensible:** resumen de acción y confirmación independiente; no ejecutar por simple transcripción ambigua.
- **Resultado:** «He cambiado la consigna a 22 °C y el termostato lo ha confirmado» **solo después de verificación**; si no hay comprobación, expresar «orden enviada, sin confirmación».
- **Error/offline:** respuesta informativa con recuperación; para acciones que requieren nube mostrar limitación sin ocultar el resto del hogar local.
- **Privacidad:** no procesar ni abrir cámaras o micrófonos sin finalidad/permiso; no suponer identificación del interlocutor solo por voz.

**Comportamiento visual del icono:** rombos y sol compatibles con el logo oficial; movimiento sutil al escuchar/responder, pero *reduced motion* elimina animación. El logo representa a la misma Liloo en todos los contextos: no sustituir con iconos de especialistas.

**Al abrir chat ampliado:** historial de conversación autorizado, acciones propuestas, adjuntos si se soportan, resultados verificables y botón «Volver a la casa». El panel debe dejar accesible el estado clave (o un cierre claro en móvil).

**Estado del contexto:** navegación a «Seguridad» informa a Liloo de la sección para orientar respuestas, pero **no concede permisos** ni fuerza que todas las preguntas pertenezcan a esa sección.

## 7. Layout responsive

| Superficie | Navegación | Compositor Liloo | Orden visual |
|---|---|---|---|
| Escritorio ≥1200 px | Sidebar persistente + contenido central | Barra anclada al final de la vista con chat expandible lateral | Resumen → energía/clima → oportunidades → verticales |
| Tablet 768–1199 px | Sidebar colapsable o navegación superior | Barra inferior fija + panel desplegable | Dos columnas, controles táctiles |
| Móvil <768 px | Barra inferior compacta y menú | Compositor sobre área segura; teclado no oculta enviar | Una columna; alertas y estado antes de métricas avanzadas |
| Panel de pared | Navegación simplificada y tipografía grande | Micrófono **no siempre activo**; pulsar para hablar | Estado, escenas, clima y conversación |

**Accesibilidad:** foco visible, contraste AA a verificar con tokens reales, textos alternativos, navegación teclado, soporte lectores pantalla, áreas táctiles ≥44 px, errores en texto, gráficos con resumen textual y estados accesibles. Evitar que las animaciones indiquen en exclusiva el estado.

## 8. Reglas de información y fiabilidad

Cada tarjeta de datos que dependa de un sensor debe poder indicar **fuente, unidad, marca temporal, calidad y estado**. Los datos ausentes se muestran como «no disponible» y jamás como cero. Las cifras de ahorro requieren baseline y método explícitos; el perfil Ahorro no garantiza una cantidad. En alertas distinguir hechos, inferencias y recomendaciones. En cámaras no generar previsualizaciones antes de autorización. Para acciones diferenciar «propuesta», «autorizada», «enviada», «confirmada» y «fallida».

## 9. Contratos de producto que necesitará ingeniería

Estos nombres son **conceptuales**, no APIs existentes certificadas:
- `HomeSnapshot`: vivienda, perfil activo, fecha de actualización, resumen de cobertura.
- `GoalPreference`: prioridad principal/secundaria y límites por zona/vertical.
- `DeviceInventoryItem`: identidad única, ubicación, procedencia, capabilities, integración y permisos.
- `VerticalProjection`: referencia a capacidades del inventario, no copia del estado.
- `TemplateSuggestion`: prerrequisitos, beneficio esperado, datos, permisos, simulación, explicación, estado.
- `ActionProposal` / `ActionExecutionReceipt`: operación, impacto, autorizaciones, verificación y auditoría.
- `LilooConversation`: mensajes y contexto de la pantalla, aislados por hogar y persona, sujetos a retención configurable.
- `VoiceSession`: consentimiento, estado micrófono, transcripción, motor disponible y control de cancelación.

**Importante:** el diseño visual puede usar fixtures/datos de demostración; toda vista debe etiquetarlos como ejemplo. Ninguna pantalla de diseño es prueba de que el hardware real ofrezca esa capacidad.

## 10. Caminos de usuario y errores obligatorios

1. **Nueva vivienda, sin dispositivos:** elegir objetivo → escanear con consentimiento → no encontrar equipos → pantalla útil y posibilidad de conectar manualmente.
2. **Vivienda con dispositivos mixtos:** algunos operativos, otros requieren cuenta o bridge → inventario explicativo → proponer solo plantillas viables.
3. **Perfil Confort con sistema de clima operativo:** configurar preferencias → plan sugerido → permiso específico → ejecución verificada.
4. **Cámaras encontradas pero no autorizadas:** mostrar existencia/pendiente; no previsualizar, no abrir ni indexar imágenes.
5. **Red con dispositivos que duermen/VLAN:** mostrar estado «detección incompleta» y acciones de recuperación.
6. **Se cambia el objetivo después del alta:** recalcular recomendaciones, no revocar ni ampliar permisos automáticamente; explicar diferencias.
7. **El usuario interrumpe a Liloo:** detener escucha/proceso antes de ejecutar cualquier acción no confirmada.
8. **La orden falla:** aviso comprensible, estado no falseado y acción de reintento cuando corresponda.

## 11. Validación y criterios de aceptación del diseño

- 100 % de pantallas de Wizard y dashboard muestran entrada **texto + micrófono** de Liloo, salvo estados explícitos de bloqueo técnico donde la limitación se informa.
- La misma conversación y nombre Liloo siguen visibles al navegar por las verticales.
- El perfil escogido cambia **prioridades y propuestas**, no crea permisos ni anula seguridad.
- Equipos duplicados entre verticales son solo referencias a una entidad canónica.
- El usuario distingue datos medidos y demostración; los datos sin fuente o sin permiso no aparentan estar disponibles.
- Una tarea de consulta (temperatura) no exige abrir un bot diferente ni llamar obligatoriamente a razonamiento costoso.
- Se pueden pausar/revocar permisos y explicar acciones, resultados y fallos.
- Los prototipos deben pasar revisión de usabilidad, pruebas de accesibilidad y pruebas de seguridad antes de desarrollo de producto.

## 12. Orden de construcción recomendado (pendiente de mandato)

**P0 — diseño:** validar navegación, tokens, accesibilidad, flujo de las 6 etapas y compositor Liloo; pruebas de usuario con fixtures.  
**P1 — contratos:** inventario/capacidades, objetivos, proyecciones verticales, plantillas, permisos, estado de acciones.  
**P2 — experiencia en staging:** conectar las pantallas al backend de pruebas solo lectura; observar fallos; nada de actuadores reales por defecto.  
**P3 — voz y acciones reales:** motor autorizado, permisos por persona, Safety Gate, verificación y pilotaje acotado con consentimiento.  
**P4 — refinamiento:** rendimiento, progresive disclosure, métricas de comprensión y uso real.

**Este documento propone el diseño UX/UI, no autoriza desplegar ni actuar sobre un dispositivo.** La arquitectura de especialistas Liloo sigue como evaluación abierta, no es una dependencia para mostrar el compositor persistente.

---
**Control documental:** UX/UI v0.1. Componente 3 del paquete de tres documentos Kaenori, 2026-11-03.