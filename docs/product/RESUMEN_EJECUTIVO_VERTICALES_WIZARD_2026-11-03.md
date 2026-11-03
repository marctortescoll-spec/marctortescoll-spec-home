# RESUMEN EJECUTIVO — Verticales, Wizard, Dashboard y Liloo
## Kaenori HOME00X · Documento de producto actualizado

**Fecha de origen:** 2026-11-03 · **Revisión:** 1.1 (actualización ejecutiva)  
**Estado:** diseño funcional del Wizard **consolidado como referencia de producto**; UX del Dashboard **propuesta para revisión**; especialización interna de Liloo **en evaluación**; mejoras técnicas A–L **pendientes de aprobación del Owner y de ADR**.  
**Alcance:** A0 (documentación y análisis). Este resumen **no autoriza** código, escaneo, despliegue, modificación de dispositivos ni cambios a contratos congelados de Core.

> **Idea central:** el cliente elige primero qué desea de la casa; Kaenori descubre con permiso qué equipos existen, verifica qué puede leer o gobernar y propone automatizaciones. Al terminar, accede a un **Dashboard de toda la vivienda** en el que **Liloo está siempre presente para conversar por texto o voz**.

---

## 1. Qué ha quedado definido funcionalmente

- **Tres planos distintos:** Conectividad L1 (fundación técnica) → verticales fijas de valor → objetivos y preferencias del cliente.
- **Una sola identidad por dispositivo:** puede colaborar con varias verticales sin duplicar inventario, permisos o estado.
- **Cinco perfiles iniciales** elegidos por el cliente, sin imponer ahorro por defecto.
- **Cinco verticales fijas** que se pueblan según lo encontrado y realmente integrable.
- **Seis etapas del Wizard** antes del Dashboard.
- **Una única autoridad de actuación local:** Kaenori Core; un perfil no concede permisos.
- **Life Safety, protecciones técnicas y consentimiento** prevalecen sobre ahorro, confort y rutinas.
- **Liloo como interlocutora visible y permanente** en Wizard y Dashboard. Su arquitectura de especialistas internos se mantiene abierta, no aprobada.
- **Local-first:** mantener funciones locales que estén verificadas y autorizadas aunque no haya BDM o LLM remoto.

**Matiz importante:** «diseño funcional consolidado» no significa implementación completada ni revisión técnica del Core superada. El dictamen de las otras IAs fue **«sólida con cambios»**.

## 2. Tres planos y secuencia de experiencia

| Plano | Responsabilidad |
|---|---|
| **0 · Conectividad (L1)** | Descubrir red, bridges y dispositivos hijos; identificar, contrastar catálogo, crear inventario canónico y registrar el estado real de integración. Es transversal, no una sexta vertical comercial. |
| **1 · Verticales de producto** | Mostrar áreas de valor de una vivienda, activadas o pobladas con capacidades disponibles; sin inventar verticales nuevas. |
| **2 · Objetivos del cliente** | Aplicar prioridades de ahorro, confort, tranquilidad o sostenibilidad dentro de límites y permisos. |

**Orden de pantallas para el cliente:** preguntar el objetivo → pedir permiso para descubrir → mostrar inventario → recomendar → pedir límites/autorizaciones → empezar a aprender → Dashboard. **La dependencia técnica** sigue empezando por L1. Antes de descubrir no se deben prometer automatizaciones concretas.

## 3. Cinco verticales de la vivienda

| Vertical | Incluye | Recurso compartido posible |
|---|---|---|
| **Energía y Clima** | Tarifa, coste y consumo, solar, baterías, aerotermia, calefacción, refrigeración, ACS, cargadores VE. | Persianas o depuradora como cargas/recursos de optimización. |
| **Seguridad y Accesos** | Alarmas, cámaras, detectores, cerraduras, videoporteros. | Persianas, luces o avisos, según permisos. |
| **Confort y Ambiente** | Iluminación, persianas, estores, escenas, ambiente. | Control solar de persianas junto con Energía. |
| **Media y Entretenimiento** | Altavoces, TV, AV, multiroom. | Escenas de iluminación, conector audiovisual autorizado. |
| **Jardín y Exteriores** | Riego, piscina, depuradora, meteorología y exteriores. | Desplazar riego o depuración a franjas solares cuando sea seguro. |

**Inventario compartido, no duplicado.** Que un dispositivo aparezca en dos vistas no significa dos motores dándole órdenes. Una vertical vacía puede no ocupar espacio principal, pero sus equipos pendientes y posibilidades deben poder consultarse.

**Nota de implementación por verificar:** el dictamen de 03/11 indicaba que Media y Jardín no tenían conector de dominio dedicado y que podrían empezar usando capacidades expuestas por el adaptador de Home Assistant. Esto es una **propuesta**, no una comprobación nueva del código o garantía de control.

## 4. Cinco perfiles que puede escoger el cliente

| Perfil | Qué prioriza | Ejemplo de comportamiento |
|---|---|---|
| **Ahorro inteligente** | Reducir costes | Aprovechar tarifas y excedentes sin vulnerar mínimos de confort. |
| **Máximo confort** | Bienestar | Mantener temperatura y ambiente preferidos; ahorrar cuando no los comprometa. |
| **Equilibrado** | Ahorro + confort | Buscar compromiso, **sugerido** en el alta sin imponerlo. |
| **Tranquilidad** | Supervisión y avisos | Mejor visibilidad y alertas del hogar; es un **objetivo transversal**, no sinónimo de la vertical Seguridad. |
| **Autoconsumo y sostenibilidad** | Aprovechar recursos propios | Favorecer energía local y cargas flexibles. |

Propuesta: **una prioridad principal y una secundaria opcional**, editables más adelante, incluso por estancia o vertical si se valida el modelo multiusuario. **Criterio de desempate pendiente de aprobación:** la prioridad secundaria solo pesa si no perjudica la principal o si dos planes cumplen igual de bien el objetivo principal. Ningún perfil anula Life Safety ni concede permiso de lectura de vídeo o control de actuadores.

## 5. Wizard de seis etapas — recorrido y contratos

| Etapa | Qué ve el cliente | Qué se garantiza / qué queda por detallar |
|---|---|---|
| **W1 · Tu objetivo** | Cinco tarjetas de perfiles y una secundaria opcional. | Solo preferencias iniciales: **sin permisos de control**. |
| **W2 · Descubramos la vivienda** | Alcance, consentimiento específico, progreso por red → bridges → dispositivos hijos. | Distinguir descubierto, identificado, conectado y verificado para lectura. Emparejamientos/credenciales delegados al conector seguro; no guardar secretos en el Wizard. |
| **W3 · Entendamos tu casa** | Equipos, habitaciones, estados y cinco verticales. | Identidad única, importación de zonas de HA solo como borrador editable y con permiso. Solicitar al final datos que la red no descubre (tarifa, potencia, meteorología/ubicación cuando sean necesarios). |
| **W4 · Tus oportunidades** | Plantillas de mejora y explicación de requisitos. | **Recomendar no es ejecutar.** Mostrar «lectura comprobada», «necesita permiso», «solo recomendación», «no compatible»; no presentar una propuesta como operativa antes de W5. |
| **W5 · Tú decides mis límites** | Temperaturas, privacidad, horarios, permisos y autonomía: observar / proponer / ejecutar dentro de límites. | Separar lectura, acción puntual y automatización acotada por usuario, capacidad, zona y duración; autorización/revocación explícitas. |
| **W6 · Empecemos a aprender** | Resumen y acceso al hogar. | Activación gradual, trazabilidad, controles de pausa y mediciones verificables. No afirmar ahorros sin línea base. |

**Estados técnicos a formalizar:** el camino simple «detectado → identificado → conectado → autorizado → operativo» es insuficiente. Separar **lectura verificada** de **control autorizado/ejecución verificada**, y definir «degradado», «suspendido» y «revocado», con causas y reintentos seguros. No adoptar un esquema exacto sin reconciliarlo con los contratos vigentes.

**Privacidad:** el consentimiento de escaneo LAN, nube y puentes se detalla por separado. No perfilar presencia ni inferir rutinas antes de autorización específica. Wizard pausable y reanudable sin reutilizar permisos que no fueron concedidos.

## 6. El Dashboard al terminar el Wizard (nuevo alcance explícito)

El Wizard **no es el producto final**: conduce a **Mi hogar**, el panel desde el que la persona ve y gestiona su vivienda.

**Vista «Mi hogar»:**
1. **Estado global verificable:** qué se ha podido comprobar, qué está pendiente y cuánto tiempo hace de la última lectura; no decir «casa segura» cuando faltan sensores.
2. **Resumen inteligente:** energía actual (kW), consumos acumulados (kWh), solar/red, climatización por zonas, seguridad y accesos según permisos.
3. **Oportunidades de Liloo:** recomendaciones con explicación y estados «propuesta / pendiente de permiso / ejecutada y comprobada».
4. **Cinco verticales:** tarjetas que abren su vista de detalle, junto con navegación transversal por **habitaciones**.
5. **Actividad y confianza:** quién pidió qué, qué decidió Kaenori, qué se envió y qué confirmó el dispositivo.
6. **Ajustes y autonomía:** perfil principal, preferencias, miembros, privacidad, permisos y pausas/revocaciones.

**Navegación:** Inicio / Energía y Clima / Seguridad y Accesos / Confort y Ambiente / Media y Entretenimiento / Jardín y Exteriores / Habitaciones / Actividad / Ajustes.

**Diseño:** apariencia profesional, texto claro, datos con unidad, fuente y hora; escritorio, tableta de pared y móvil. Los datos de un prototipo deben identificarse como simulados. El Dashboard podrá mostrar equipos sin servicio y capacidades pendientes sin confundirlos con equipos operativos.

## 7. Liloo siempre presente — texto y voz

**Decisión de experiencia para el diseño:** Liloo aparece **en todas las pantallas** mediante su identidad (rombos alrededor de un sol) y un **compositor persistente**, similar a una barra de conversación, con campo de texto, botón enviar y micrófono **activado solamente por acción explícita**. Abrir el chat ampliado no debe hacer perder la pantalla ni el contexto.

- «¿A qué temperatura está el salón?» → lectura estructurada, fuente y última actualización; no necesita otro bot.
- «Mírame las cámaras» → disponibilidad y permiso de vídeo antes de mostrar imágenes.
- «¿Podemos optimizar mañana la aerotermia?» → propuesta razonada, con datos/limitaciones y autorización necesaria.
- «Pon música en el salón» → dispositivo multimedia compatible y confirmación del resultado o explicación de por qué no está disponible.

**Experiencia coherente:** el cliente conversa siempre con **una Liloo**. Estados visibles: disponible, escuchando, interpretando, solicitando confirmación, ejecutando, resultado verificado y error. Ni la voz ni el contexto de pantalla conceden privilegios nuevos.

**Arquitectura interna de especialistas:** sigue en **evaluación**, no aprobada. La opción preferida a probar es una Liloo única con selección de herramientas/capacidades de Energía, Seguridad, Confort, Media y Jardín; **no** varios agentes autónomos con identidades o memorias contradictorias. Primero comparar precisión, latencia y coste contra Liloo con herramientas comunes. La presencia permanente de Liloo **no depende** de haber aprobado el router de especialistas.

## 8. Gobierno y frontera con Core — precisión necesaria

**Responsabilidades propuestas para reconciliar con los contratos reales:**

- Discovery identifica; los conectores autorizados leen o actúan según sus contratos.
- Experience/Wizard formula objetivos y **plantillas como intenciones**, no es dueño de la planificación energética.
- EMS conserva el razonamiento y planificación energética que le correspondan; otros ámbitos usarán sus conectores/servicios definidos.
- **Kaenori Core + controles de seguridad vigentes** validan autorización, protecciones y destino antes de permitir acciones físicas.
- HA puede exponer integraciones, pero **sus automatizaciones existentes pueden competir** con Kaenori. Se debe detectar o registrar el origen de control, advertir y pasar a observación ante conflicto no resuelto, en lugar de afirmar autoridad exclusiva sobre toda la instalación.

**Precedencia propuesta:** Life Safety → enclavamientos/límites técnicos → intervención manual (ventana de respeto) → optimización según objetivos autorizados → rutinas. El orden exacto y protecciones por dominio (p. ej., temperatura, funcionamiento hidráulico, dispositivos mecánicos) deben validarse técnicamente. El sistema no debe afirmar una acción como ejecutada hasta contar con verificación.

**No confundir:** un «planificador compartido» como *visión coordinada* de intenciones no implica añadir un séptimo conector o crear una autoridad por encima de Core.

## 9. Dictamen técnico A0 — doce mejoras propuestas, NO aprobadas automáticamente

El dictamen de GPT-5, Argon y el orquestador del 03/11 calificó el diseño como **«sólido con cambios»**. Esta revisión **incorpora los problemas en el resumen y sus recomendaciones como temas a resolver**, pero **no convierte los doce puntos en un ADR ni en una orden de ejecución**.

| # | Revisión propuesta | Prioridad orientativa | Decisión del Owner |
|---|---|---|---|
| 1 | Separar «verificado para recomendar» (W4) de «autorizado para actuar» (W5); revocar y suspender. | **P0** | Pendiente |
| 2 | Mapear Experience, EMS, Security y Core a puertos y contratos reales sin nuevo planificador privilegiado. | **P0** | Pendiente |
| 3 | Identificar automatizaciones de HA/otros orígenes que compitan por un actuador. | **P0** | Pendiente |
| 4 | Permisos por lectura, acción puntual y automatización acotada, por usuario y capacidad. | **P0** | Pendiente |
| 5 | Capturar tarifa, potencia y otros metadatos no descubribles cuando se necesiten. | **P1** | Pendiente |
| 6 | Reconciliar identidad de devices/bridges e importar zonas existentes como borrador. | **P0** | Pendiente |
| 7 | Formalizar Life Safety, límites y enclavamientos específicos y precedencias. | **P0** | Pendiente |
| 8 | Diferenciar Tranquilidad de Seguridad y fijar regla entre perfil principal/secundario. | **P1** | Pendiente |
| 9 | Consentimiento granular y alcance/retención antes de inferir habitaciones o presencia. | **P0** | Pendiente |
| 10 | Degradación offline e incorporación reanudable. | **P1** | Pendiente |
| 11 | Objetivos, permisos y conflictos por hogar/miembro/zona. | **P1** | Pendiente |
| 12 | Criterios para simulación y KPIs (detección, latencia p95, reconexión, ahorro validado). | **P1** | Pendiente |

**El plazo de 14 días de telemetría** planteado por el concilio para simulaciones es **solo un ejemplo**, no un requisito cerrado; ingeniería deberá determinar ventana mínima, calidad, incertidumbre y datos disponibles. Sin datos suficientes, mostrar únicamente una explicación cualitativa.

## 10. Decisiones y siguiente paso

**Se conserva como referencia funcional:** las 5 verticales, los 5 perfiles, los 3 planos y las 6 etapas, más la continuidad hacia el Dashboard con Liloo siempre visible (texto y voz).

**Falta decidir:** A–L / puntos 1–12; modelo de consentimiento y autorización; especificaciones técnicas de estados/puertos; manejo de automatizaciones externas; revisión de privacidad; estrategia real de voz y posibles especialistas internos; pruebas de usabilidad y de accesibilidad.

**Secuencia sugerida (no autorizada por este documento):** (i) Owner revisa los doce puntos y confirma cambios; (ii) equipo redacta revisión funcional/ADR de contratos, permisos y precedencia; (iii) UX prototipa y valida Wizard → Dashboard → Liloo con datos de ejemplo; (iv) pruebas en staging/solo lectura; (v) cualquier control real requiere mandato, copia de seguridad, límites, evidencias y rollback.

## 11. Referencias documentales

Documentos publicados en **marctortescoll-spec/marctortescoll-spec-home**, rama main:

- **Wizard funcional v1.0:** docs/product/KAENORI_WIZARD_FUNCIONAL_V1_2026-11-03.md
- **Liloo, especialización v0.1 (abierta):** docs/product/LILOO_MODELO_ESPECIALIZACION_EVALUACION_V0_1_2026-11-03.md
- **UX/UI: Wizard + Dashboard + Liloo v0.1:** docs/product/KAENORI_UX_WIZARD_DASHBOARD_V0_1_2026-11-03.md

Fuentes de la revisión técnica local: DICTAMEN_CONCILIO_WIZARD_2026-11-03.md, CONCILIO_WIZARD_GPT5.md y CONCILIO_WIZARD_ARGON.md (expediente de diseño, 03/11).

---

**Control documental:** resumen ejecutivo v1.1. Actualiza el resumen previo sin convertir las recomendaciones pendientes en decisiones aprobadas. No autoriza implementación ni despliegue.
