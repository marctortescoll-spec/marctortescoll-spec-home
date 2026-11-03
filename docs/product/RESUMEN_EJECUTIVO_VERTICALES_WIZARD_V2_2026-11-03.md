# RESUMEN EJECUTIVO — Verticales de producto y Wizard de incorporación
## Kaenori HOME00X · Autonomy · v2

**Fecha:** 2026-11-04 · **Owner:** Marc · **Estado:** pendiente de tu revisión (decidir cambios uno a uno).
**Alcance:** documento de producto (A0). No autoriza desarrollo, escaneo, despliegue ni cambios de ADR.

> **Cómo leer este documento:** las secciones 1–6 describen el diseño **consolidado** (wizard
> funcional v1.0). La sección 7 contiene **12 mejoras pendientes de tu decisión**. Las frases del
> cuerpo marcadas con `[M#]` dependen de esa mejora; si rechazas el punto, esa frase se revisa.

### Invariantes de gobierno (no negociables)

- **Core v1.0 congelado** (6 puertos); **un conector no toca a otro**.
- **Home Assistant = adaptador de integraciones, no autoridad.**
- **descubrir ≠ controlar** · **deny-by-default** · **fail-closed** · **local-first**.
- **Sin secretos/PII** en prompts, logs ni Git; credenciales en el conector seguro.
- **Life Safety** (incendio/evacuación) y límites técnicos **siempre por encima** de objetivos y rutinas.

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
> de inicio, **manteniendo el aislamiento de conectores y sin elevar a HA a autoridad de control**
> (el control se ejerce bajo los 6 puertos congelados de Core v1.0; HA actúa como traductor).

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
- `[M8]` El objetivo secundario solo aplica donde el principal sea neutro, o como desempate entre
  planes de igual rendimiento (nunca por encima de Life Safety ni límites técnicos).

---

## 5. Recorrido del Wizard (6 etapas)

1. **Tu objetivo** — 5 tarjetas de valor; salida `HomeGoals`, sin permisos. `[M8]`
2. **Descubramos tu vivienda** — consentimiento de escaneo; progreso en vivo; detección en 2 fases
   (red/bridges → hijos); máquina de estados `detectado → identificado → conectado → autorizado → operativo`.
3. **Entendamos tu casa** — unificar identidades, matcher↔catálogo, agrupar por habitaciones, inventario, asignar verticales.
4. **Mejoras que proponemos** — motor = objetivos + capacidades operativas + contexto + seguridad; plantillas.
5. **Decide hasta dónde puedo llegar** — límites; autonomía (observar / proponer-pedir / ejecutar con permisos).
6. **Empecemos a aprender** — activación progresiva; línea base antes de afirmar ahorro; registro y local-first.

`[M1]` En la etapa 4 las plantillas se evalúan sobre capacidades en **lectura verificada**; el paso a
`autorizado → operativo` ocurre en la etapa 5. **"Recomendar" no implica capacidad de ejecución.**
`[M1][M10]` Estados visibles por dispositivo: `operativo / lectura verificada / control verificado /
degradado / suspendido / revocado`, con causa y próximo reintento.

---

## 6. Reglas de gobierno (resumen)

- **Autoridad local:** Kaenori Core (6 puertos, congelado). Home Assistant = adaptador, no autoridad.
- **Prioridad** `[M7]`: Seguridad vital (incendio/evacuación) y límites técnicos **> acción manual > objetivos > rutinas**.
- **Permisos** `[M4]`: explícitos, minimizados, auditables, revocables (vivienda/usuario/capacidad);
  scopes lectura / acción puntual / automatización acotada.
- **Privacidad** `[M9]`: no usar imágenes/vídeo/sensores fuera del permiso autorizado; no inferir
  estancias ni perfilar antes del consentimiento.
- **Conflicto con HA** `[M3]`: advertir y pasar a **observación** ante conflicto de confort/ahorro;
  forzar **estado seguro (fail-closed)** si el conflicto compromete Life Safety o límites técnicos.
- **Local-first** `[M10]`: la vivienda sigue funcionando sin Internet/BDM/LLM remoto.

---

## 7. El concilio: veredicto y 12 modificaciones (pendientes de tu decisión)

**Veredicto unánime (GPT-5 + Argon + orquestador):** el concepto es **sólido**, pero necesita
**cambios** antes de formalizar ADR/implementación.

| # | Ref | Área | Cambio propuesto | ¿Bloqueante si se rechaza? |
|---|---|---|---|---|
| 1 | A | Wizard (etapas 4–5) | Etapa 4 muestra lo **verificable**; el permiso de **actuar** se pide en etapa 5. Añadir estado "verificado" + revocar/suspender. | **Sí** (deny-by-default) |
| 2 | B | Gobierno/Core | El wizard **propone**; `ems` **planifica**; Core+seguridad son la **puerta** de cada orden. | **Sí** (Core v1.0) |
| 3 | C | Gobierno/HA | Al conectar HA, **detectar/avisar** automatizaciones competidoras; observación o fail-closed según el caso. | **Sí** (fail-closed/seguridad) |
| 4 | D | Permisos | Permisos finos: **lectura / acción puntual / automatización acotada**, revocables. | **Sí** (deny-by-default) |
| 5 | E | Wizard (etapa 3) | Pedir **contexto no descubrible** (tarifa, potencia, código postal). | No (producto) |
| 6 | F | Wizard (etapa 3) | **Deduplicar** por MAC/serial y aprovechar **zonas de HA** como borrador editable. | No (calidad) |
| 7 | G | Gobierno | Lista de **protecciones** (anti-legionela, anti-atasco…) y prioridad fija. | **Sí** (Life Safety) |
| 8 | H | Perfiles | "Tranquilidad" = objetivo transversal (no = vertical Seguridad) + regla de desempate. | No (claridad) |
| 9 | I | Privacidad | No **inferir estancias/perfilar** antes del consentimiento; declarar qué se escanea/guarda. | **Sí** (privacidad) |
| 10 | J | Gobierno | Definir **qué funciona sin internet** y hacer el wizard **reanudable**. | No (local-first) |
| 11 | K | Permisos | Objetivos y límites por **hogar/usuario/zona**. | No (multi-usuario) |
| 12 | L | Wizard (etapa 4) | **Simular** exige datos mínimos y rangos; sin datos, solo explicación. Añadir KPIs. | No (precisión) |

> Los puntos **1, 2, 3, 4, 7 y 9** protegen invariantes (deny-by-default, Core v1.0 congelado,
> fail-closed, Life Safety, privacidad). Rechazarlos exige redefinir la regla correspondiente, no
> solo "saltársela".

---

## 8. Decisión pendiente

Marca por número: **aceptar / cambiar / quitar**. Con eso se redacta la **v1.1** del diseño funcional.

```
1  [ ] aceptar  [ ] cambiar  [ ] quitar   (bloqueante)
2  [ ] aceptar  [ ] cambiar  [ ] quitar   (bloqueante)
3  [ ] aceptar  [ ] cambiar  [ ] quitar   (bloqueante)
4  [ ] aceptar  [ ] cambiar  [ ] quitar   (bloqueante)
5  [ ] aceptar  [ ] cambiar  [ ] quitar
6  [ ] aceptar  [ ] cambiar  [ ] quitar
7  [ ] aceptar  [ ] cambiar  [ ] quitar   (bloqueante)
8  [ ] aceptar  [ ] cambiar  [ ] quitar
9  [ ] aceptar  [ ] cambiar  [ ] quitar   (bloqueante)
10 [ ] aceptar  [ ] cambiar  [ ] quitar
11 [ ] aceptar  [ ] cambiar  [ ] quitar
12 [ ] aceptar  [ ] cambiar  [ ] quitar
```

*Fin del resumen v2. Documento exportable y autocontenido.*
