# RESUMEN EJECUTIVO — Verticales de producto y Wizard de incorporación
## Kaenori HOME00X · Autonomy

**Fecha:** 2026-11-03 · **Owner:** Marc · **Estado:** pendiente de tu revisión (Opción B: decidir cambios uno a uno).
**Alcance:** documento de producto (A0). No autoriza desarrollo, escaneo, despliegue ni cambios de ADR.

---

## 1. Qué se pedía (punto de partida)

Replantear las **verticales** de Kaenori y del **Wizard** con nombres más óptimos:

- **Energía** como vertical amplia (calefacción, aerotermia, aire, coche, batería…).
- **Persianas/cortinas/toldos** compartiendo vertical (eficiencia **y** seguridad).
- Presentar **4–5 líneas** de producto al empezar.
- Flujo: **primero conectividad** (Matter/MQTT/…), **luego** organizar por vertical.

---

## 2. Modelo resultante: tres planos

| Plano | Qué es |
|---|---|
| **0 · Conectividad (L1)** | Fundación transversal. Descubrir, detectar puentes, identificar, emparejar con catálogo, inventario canónico. **Es el foco de entrada**, no una vertical comercial. |
| **1 · Verticales fijas** | Líneas de producto que ve el cliente. Se **pueblan/activan** por detección; no se crean dinámicamente. |
| **2 · Objetivos y preferencias** | Definen prioridades dentro de permisos, límites y reglas de seguridad. |

**Regla de oro:** un mismo dispositivo puede participar en varias verticales **sin duplicar su identidad** (inventario y permisos únicos por capacidad).

---

## 3. Verticales de producto (5, fijas)

1. **Energía y Clima** — consumo/coste, tarifas, solar FV, baterías, aerotermia, calefacción, refrigeración, ACS, movilidad eléctrica.
2. **Seguridad y Accesos** — cámaras, alarmas, detectores, cerraduras, videoporteros.
3. **Confort y Ambiente** — persianas, iluminación, escenas, estores, ambientes.
4. **Media y Entretenimiento** — televisores, altavoces, sistemas AV, multiroom.
5. **Jardín y Exteriores** — riego, piscina, depuradora, meteorología, exteriores.

**Compartidos:** persianas = Confort + apoyo a Energía; depuradora = Jardín + carga gestionable por Energía.

> **Nota de arquitectura:** "Media" y "Jardín" aún **no tienen conector dedicado** en el código
> (solo `security, ha-adapter, discovery, ems, liloo, experience`). Se servirían vía `ha-adapter`
> de inicio, sin crear conector nuevo.

---

## 4. Perfiles de cliente (5)

| Perfil | Promesa | Regla orientativa |
|---|---|---|
| **Ahorro inteligente** | Pagar menos sin renunciar a lo esencial | Menor coste con límites de confort |
| **Máximo confort** | La casa se adapta a mis hábitos | Condiciones preferidas; gasto como 2º criterio |
| **Equilibrado** *(sugerencia inicial)* | Buen balance ahorro-bienestar | Compromiso dentro de límites |
| **Tranquilidad** | Sentirme al corriente y protegido | Supervisión y avisos; nunca rebajar seguridad |
| **Autoconsumo y sostenibilidad** | Aprovechar mejor mi energía | Cargas flexibles a producción local |

- Un objetivo principal + opcionalmente uno secundario; editable después del alta.
- El perfil **no concede permisos de control** ni cambia rangos técnicos.

---

## 5. Recorrido del Wizard (6 etapas)

1. **Tu objetivo** — 5 tarjetas de valor; salida `HomeGoals`, sin permisos.
2. **Descubramos tu vivienda** — consentimiento de escaneo; progreso en vivo; detección en 2 fases
   (red/bridges → hijos); máquina de estados `detectado → identificado → conectado → autorizado → operativo`.
3. **Entendamos tu casa** — unificar identidades, matcher↔catálogo, agrupar por habitaciones, inventario, asignar verticales.
4. **Mejoras que proponemos** — motor = objetivos + capacidades operativas + contexto + seguridad; plantillas.
5. **Decide hasta dónde puedo llegar** — límites; autonomía (observar / proponer-pedir / ejecutar con permisos).
6. **Empecemos a aprender** — activación progresiva; línea base antes de afirmar ahorro; registro y local-first.

---

## 6. Reglas de gobierno (resumen)

- **Autoridad local:** Kaenori Core. Home Assistant = adaptador de integraciones, no autoridad.
- **Prioridad:** Seguridad vital (incendio/evacuación) y límites técnicos **> acción manual > objetivos > rutinas**.
- **Permisos:** explícitos, minimizados, auditables, revocables (vivienda/usuario/capacidad).
- **Privacidad:** no usar imágenes/vídeo/sensores fuera del permiso autorizado.
- **Local-first:** la vivienda sigue funcionando sin Internet/BDM/LLM remoto.

---

## 7. El concilio: veredicto y 12 modificaciones (pendientes de tu decisión)

**Veredicto unánime (GPT-5 + Argon + orquestador):** el concepto es **sólido**, pero necesita
**cambios** antes de formalizar ADR/implementación.

Los 12 cambios, en lenguaje de negocio:

| # | Cambio propuesto | Por qué |
|---|---|---|
| 1 | Etapa 4 muestra lo **leíble/verificable**; el permiso de **actuar** se pide en etapa 5. Añadir estado "verificado" + revocar/suspender. | Hoy la etapa 4 promete "listo" antes de pedir permiso. |
| 2 | El wizard **propone**; `ems` **planifica** la energía; Core+seguridad son la **puerta** de cada orden. | Evitar romper el contrato Core congelado. |
| 3 | Al conectar HA, **detectar/avisar** automatizaciones competidoras y registrar origen. | Evitar bucles y falsos "acción manual". |
| 4 | Permisos finos: **lectura / acción puntual / automatización acotada**, revocables. | Falta quién puede qué. |
| 5 | Pedir **contexto no descubrible** (tarifa, potencia, código postal) al final de etapa 3. | La red no descubre tarifas ni meteorología. |
| 6 | **Deduplicar** por MAC/serial y aprovechar **zonas de HA** como borrador editable. | Inventario único fiable. |
| 7 | Lista de **protecciones** (anti-legionela, anti-atasco…) y prioridad fija Seguridad > límites > manual > planificador > rutinas. | Lo que faltaba para que "objetivo→prioridad" sea seguro. |
| 8 | Aclarar que **"Tranquilidad"** es objetivo transversal (no = vertical "Seguridad") + regla de desempate principal/secundario. | Evitar confusión de planos. |
| 9 | No **inferir estancias/perfilar** antes del consentimiento; declarar qué se escanea/guarda. | Privacidad. |
| 10 | Definir **qué funciona sin internet** y hacer el wizard **reanudable**. | Local-first real. |
| 11 | Objetivos y límites por **hogar/usuario/zona**. | Multi-usuario no resuelto. |
| 12 | **Simular** exige datos mínimos (p. ej. 14 días) y rangos; sin datos, solo explicación. Añadir KPIs. | No vender ahorros sin datos. |

---

## 8. Decisión pendiente

Marca por número: **aceptar / cambiar / quitar**. Con eso se redacta la **v1.1** del diseño funcional.

```
1  [ ] aceptar  [ ] cambiar  [ ] quitar
2  [ ] aceptar  [ ] cambiar  [ ] quitar
3  [ ] aceptar  [ ] cambiar  [ ] quitar
4  [ ] aceptar  [ ] cambiar  [ ] quitar
5  [ ] aceptar  [ ] cambiar  [ ] quitar
6  [ ] aceptar  [ ] cambiar  [ ] quitar
7  [ ] aceptar  [ ] cambiar  [ ] quitar
8  [ ] aceptar  [ ] cambiar  [ ] quitar
9  [ ] aceptar  [ ] cambiar  [ ] quitar
10 [ ] aceptar  [ ] cambiar  [ ] quitar
11 [ ] aceptar  [ ] cambiar  [ ] quitar
12 [ ] aceptar  [ ] cambiar  [ ] quitar
```

*Fin del resumen. Documento exportable y autocontenido.*
