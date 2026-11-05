# MANDATO CONSOLIDADO — Primera integración real de extremo a extremo
## Kaenori HOME00X · Experience V1.3 · Consenso Concilio R32 · 2026-11-05

> Concilio: DeepSeek (orquestador/ejecutor) + GPT-5 (arquitectura/producto) + Argon (validación independiente).
> Estado: **propuesta consolidada, lista para ejecutar** (sin despliegues ni A3 hasta autorización del OWNER).

---

## A · Diagnóstico actual (auditoría real de GitHub + HOME0001)

**Construido y probado (suite 2609 tests verdes):** Core v1.0 (6 puertos) · Discovery mDNS/SSDP/Modbus/AirPlay (fail-soft) · Catálogo homologado `manufacturers.json` · Adaptador HA solo-lectura (`ems_shadow_ingest`, `ha_integration_catalog`) · EMS shadow + oportunidad (fuentes marcadas) · Onboarding/Wizard (11 pantallas, verticales 12→5) · Experience/PWA · Liloo Lite (NLU ES/CA) · Seguridad (safe-stop, deny-by-default) · persistencia `memory.db`.

**HOME0001 hoy (verificado):** desplegado `fix-live` (= `644cd4e` + fixes P0), 5 servicios activos, salud `ok`. **Home Assistant: 26 entidades reales**, destacando el sistema **EMS V2C**: `sensor.ems_v2c_potencia_red`=636 W, `_casa`=927 W, `_fv`=291 W, `_carga`=0 W (cambiantes), `binary_sensor.ems_v2c_disponible`=on; también `light.luces_salon`=off y `weather.forecast_casa`=fog. **Discovery mDNS/SSDP: inventario VACÍO** (`platforms: []`).

**Desconexión real (qué impide el E2E):**
1. Discovery (mDNS/SSDP) y EMS-shadow (HA) son **dos caminos separados sin puente**; el discovery hoy no ve nada, HA sí tiene lecturas.
2. **No hay fuente única** de identidad/estado: inventario discovery vs entidades HA vs catálogo vs `memory.db`.
3. El **dashboard** no muestra la lectura real del dispositivo.
4. **Liloo** no está cableada al estado real (responde con resumen, no con lectura viva + fuente + antigüedad).
5. Persistencia sin validar con dispositivo real.

**Conclusión:** no falta escribir componentes desde cero; falta **cerrar el puente** HA→identidad→lectura→dashboard→Liloo y **validarlo con un dispositivo real**. El camino de datos real hoy es: `HA /api/states` → `ems_shadow_ingest()` → oportunidad EMS (ya probado).

---

## B · Arquitectura de integración (reutilizando lo existente)

```
[HA — infraestructura interna, solo lectura]
   └─ GET /api/states  ──▶  ems_shadow_ingest() / ha_integration_catalog()
            │                     │ conserva last_updated / last_changed NATIVOS de HA
            ▼                     ▼
   [Fuente de verdad: memory.db (SQLite file-backed, B0)]
      · devices (1 dispositivo HA = N entidades)  clave: ha://device/<device_id>
      · readings (value, unit, age_ms, provenance=home_assistant, first/last_seen)
      · identities (manufacturer/model/category ← manufacturers.json)
            │
            ├──▶ GET /api/…/devices · GET /api/…/device/<id>/reading
            │         ▼
            │    [Dashboard app.js] — valor vivo + badge "fuente: HA · hace Xs"
            │
            └──▶ [Liloo NLU — 2 intenciones: device_info, device_status]
                      └─ respuesta: {identidad, estado, lectura real, fuente, antigüedad}
```

**Reglas de consenso:**
- **Una sola fuente de verdad** = `memory.db`. El discovery mDNS/SSDP **solo enriquece** (fabricante/modelo) y **nunca crea identidad** si ya existe `ha://…`. No se crea inventario paralelo ni nuevos buses/colas.
- **Identidad por dispositivo, no por entidad**: el V2C es **1 dispositivo** con **5 entidades** (`_red`,`_casa`,`_fv`,`_carga`,`_disponible`); `seed_device` agrupa las entidades bajo `ha://device/<device_id>`. (Corrección de Argon, aceptada.)
- **Antigüedad real**: se usa `last_updated`/`last_changed` **nativo de HA** (no el reloj de ingesta de Kaenori). Si `age > TTL` → `no_disponible` (no se repite el último valor).
- **Estados distinguibles (UI + Liloo):** `detectado` · `identificado` · `compatible` · `incorporado` · `lectura_real_verificada` · `no_disponible`. Nunca "disponible"=integrado ni "conectado"=lectura verificada.

---

## C · Primera integración seleccionada

**Piloto: sistema EMS V2C (cargador V2C + solar), vía Home Assistant, solo lectura.**

- **1 dispositivo lógico, 5 métricas reales y cambiantes** (verificado hoy: 636/927/291/0 W; valor que varía minuto a minuto).
- **Read-only**, encaja en la vertical **Energía y Clima** (ya construida: `home_ems`, EMS Shadow, oportunidad).
- **Homologación** vía `manufacturers.json` (si el fabricante concreto no está, se añade **una** entrada V2C genérica; no se crea un adaptador nuevo).
- **Descartado** `light.luces_salon` como piloto principal: estado demasiado estático (on/off) para demostrar lectura real variable.

---

## D · Hoja de ruta ejecutable (bloques que entregan resultado comprobable)

**B0 — Esquema + procedencia + TTL (base).** `memory.db` SQLite file-backed con esquema mínimo: `devices` (id `ha://device/<device_id>`, `entity_id`→device, alias histórico), `readings` (value, unit, `age_ms`, `provenance`, `first/last_seen`, timestamps monótonos), `identities` (manufacturer/model/category). TTL por entidad. *(Añadido por GPT-5.)*
- Prueba: reescritura idempotente; reabrir no duplica ni resetea `incorporado`.

**B1 — Puente HA→identidad de dispositivo.** `seed_device` agrupa las entidades HA por `device_id`/`unique_id` y crea **1** dispositivo V2C con sus 5 entidades; inyecta el catálogo **en el contrato exacto que ya consume la pantalla de selección del wizard** (sin tocar el frontend).
- Prueba: el V2C aparece **una vez** (no 5 clones); fabricante/categoría del catálogo.

**B2 — Lectura real + antigüedad.** Exponer `reading(device_id)` con `{value, unit, age_ms (de last_updated de HA), provenance}`. Mapear `unavailable`/`unknown` → `no_disponible` (nunca `0 W` falso).
- Prueba: `potencia_red` devuelve 6xx W con `source=home_assistant` + antigüedad del payload de HA; al apagar HA → `no_disponible` en <30 s.

**B3 — Dashboard con estado real.** El V2C aparece con sus 4 potencias + badge `fuente: HA · hace Xs` (o `no_disponible`).
- Prueba: dashboard y API devuelven **el mismo valor** en el mismo instante.

**B4 — Liloo responde con el estado real.** Dos intenciones cerradas: `device_info` («¿qué sabes de este dispositivo?» → fabricante/modelo/fuente/estado de incorporación) y `device_status` («¿cuál es su estado actual?» → potencia + fuente + antigüedad). Sin paráfrasis creativas en piloto.
- Prueba: la respuesta cita `636 W · fuente home_assistant · hace N s`; un texto LLM sin lectura NO aprueba.

**B5 — Persistencia y reentrada.** Cerrar y reabrir conserva dispositivo, identidad y última lectura; reiniciar backend no duplica ni resetea; `entity_id` renombrado se resuelve por `unique_id`/`device_id`.
- Prueba: tras reabrir, el V2C sigue listado y la lectura se refresca.

**B6 — Prueba E2E de "Delta y Corte" (documentada, sin mock).**
1. Incorporar V2C desde la PWA; pedir estado a Liloo → anotar W₁ y antigüedad T₁.
2. Esperar >30 s a que varíe el consumo; volver a preguntar → W₂ ≠ W₁, y W₂ coincide **exactamente** con un `curl` directo a `HA /api/states` en ese segundo.
3. Reiniciar el backend Kaenori y recargar la PWA (sin pasar por el wizard) → persiste.
4. Forzar `unavailable` en HA (o cortar el puente) → Liloo dice "no disponible" con la antigüedad del último dato válido, sin inventar cifras.

---

## E · Riesgos y soluciones (máx. 5)

1. **HA caído o token revocado (401/403).** → `no_disponible` para lecturas, **sin inventar**; el puente avisa de fallo de datos sin borrar el dispositivo persistido. (El requisito comercial de auto-gestión del token HA queda como trabajo separado.)
2. **Discovery mDNS/SSDP vacío (como hoy).** → el camino de lectura es HA; el discovery solo enriquece fabricante/modelo. No se fía la integración del discovery.
3. **Renombre de `entity_id` en HA.** → resolver por `device_id`/`unique_id` y mantener alias histórico; la identidad no se parte.
4. **Liloo "alucina" estado sin lectura.** → la intención de estado consulta la lectura viva y cita fuente + antigüedad; sin lectura → "no disponible", nunca un valor inventado.
5. **Falsa frescura (lectura congelada).** → se persiste `last_updated`/`last_changed` **nativo de HA**; `age > TTL` ⇒ `no_disponible`.

---

## F · Validación del Concilio

- **DeepSeek (orquestador):** propuso piloto V2C, `memory.db` como fuente única y bloques B1–B6. **Reutiliza lo existente**, sin nuevos adaptadores ni buses.
- **GPT-5 (arquitectura/producto):** aprueba la reutilización y el piloto V2C; **añade B0** (esquema SQLite + procedencia + TTL), la resolución de renombres por `device_id`/`unique_id`, throttling, unidades/signo, y 7 pruebas de aceptación (incluidas parar HA y renombrar entidad).
- **Argon (validación independiente):** **corrige dos puntos técnicos** — (a) la clave debe ser **agregado de dispositivo** (`ha://device/<device_id>` → N entidades), no `entity_id` (evita 5 clones del V2C); (b) la antigüedad debe usar **`last_updated` nativo de HA**, no el reloj de ingesta. **Añade la Prueba del Delta y Corte** en B6 y el mapeo explícito de `unavailable`/`unknown` → `no_disponible`.

**Discrepancia resuelta:** GPT-5 mencionó `ha://<entity_id>`; Argon demostró que eso clonaría las 5 entidades del V2C como 5 "dispositivos". **Se adopta la clave por dispositivo** (`device_id` → N `entity_id`). Resto: consenso pleno en piloto (V2C), fuente única (`memory.db`) y prueba final sin mock.

---

## G · Resultado final esperado

El OWNER considera **superada la primera integración real** cuando: (1) el sistema EMS V2C aparece en Kaenori como **un único dispositivo** identificado (fabricante/categoría del catálogo); (2) Liloo responde «¿qué sabes de este dispositivo?» con su identidad/capacidades y «¿cuál es su estado actual?» con una **lectura real citando fuente (`home_assistant`) y antigüedad**; (3) el dashboard muestra el **mismo valor en el mismo instante**; (4) tras cerrar y reabrir Kaenori **todo se conserva**; y (5) la **Prueba del Delta y Corte** demuestra que el valor cambia con la realidad y coincide con HA. Sin mocks, sin intervención manual del cliente.
