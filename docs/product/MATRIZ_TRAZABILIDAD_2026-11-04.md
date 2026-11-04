# Anexo C — Matriz de trazabilidad
## Capacidad → módulo → código → test → evidencia → estado · Anexo de "Kaenori Experience v1.2"

**Naturaleza:** herramienta de paso del diseño a la ingeniería (A0). **El estado es el documentado**
(skills + estado actual), **no una auditoría de ejecución**: la columna código/test/evidencia se
completa en el paso de **validación** (contraste con el repositorio real). No inventa verificaciones.

**Leyenda de estado:** 🟢 implementado · 🟡 parcial · ⚪ pendiente · 🔴 bloqueado · ❓ por contrastar.

---

## Bloque 1 — Conectividad y descubrimiento (Plano 0)

| Capacidad prometida | Módulo | Código | Test | Evidencia | Estado |
|---|---|---|---|---|---|
| Escaneo pasivo (mDNS/SSDP) + activo (Modbus/Matter/HomeKit/Tuya) | discovery | ❓ | ❓ | 31 dispositivos en laboratorio | 🟢 |
| Catálogo canónico + inventario (servicio 8138) | catalog | ❓ | ❓ | catálogo en disco | 🟢 |
| Identificación F1/F2/F4 | discovery | ❓ | ❓ | HomeKit/Matter/Tuya | 🟢 |
| Zigbee (F3) | discovery | ❓ | ❓ | pincho mudo | 🔴 |
| WiFi/ESP | discovery | ❓ | ❓ | sin empezar | ⚪ |

## Bloque 2 — Energía y Clima (EMS)

| Capacidad prometida | Módulo | Código | Test | Evidencia | Estado |
|---|---|---|---|---|---|
| Lectura V2C (coche) en vivo | ems / ha-adapter | ❓ | ❓ | `/state/energy/*` | 🟢 |
| Matemática tarifa→precio→consigna | ems | ❓ | 13 tests | portada HOME0000 | 🟢 |
| Shadow Mode (propone sin ejecutar) | ems | ❓ | ❓ | `/ems/status` | 🟢 |
| SolarEdge + Aquarea (fuentes reales) | ems | ❓ | ❓ | pendiente | ⚪ |
| Actuación (Shadow → ejecutar) | ems | ❓ | ❓ | declarada, lazo no cableado | 🟡 |

## Bloque 3 — Liloo / Voz (y conversación)

| Capacidad prometida | Módulo | Código | Test | Evidencia | Estado |
|---|---|---|---|---|---|
| Liloo Lite (NLU ES/CA determinista) | liloo | ❓ | ❓ | comandos → Core | 🟢 |
| Voz en navegador (ASR/TTS) | liloo | ❓ | ❓ | micrófono navegador | 🟢 |
| Liloo Pro (DeepSeek) | liloo | ❓ | ❓ | clave en secreto | 🟢 |
| Altavoz físico (ESP32-S3) | liloo/voz | ❓ | ❓ | bloqueo firmware | 🔴 |

## Bloque 4 — Core y seguridad (transversal)

| Capacidad prometida | Módulo | Código | Test | Evidencia | Estado |
|---|---|---|---|---|---|
| Core v1.0 (contrato + 6 puertos) | core | ❓ | ❓ | freeze v1.0 | 🟢 |
| AuthorizationPolicy deny-by-default | security | ❓ | 14 tests | — | 🟢 |
| Kill-switch persistente | security | ❓ | ❓ | — | 🟢 |
| Custodia age + egress jail (enforcement real) | security | ❓ | ❓ | declarado, sin cgroups/iptables | 🟡 |
| ha-adapter lectura / escritura | ha-adapter | ❓ | ❓ | lectura viva · escritura declarada | 🟢/🟡 |

## Bloque 5 — Experiencia y Wizard

| Capacidad prometida | Módulo | Código | Test | Evidencia | Estado |
|---|---|---|---|---|---|
| Wizard (discovery → matcher → confirmación → PIN/credenciales → vault) | experience | ❓ | 1320 tests | integrado | 🟢 |
| Catálogo homologado multi-vertical | home_catalog | ❓ | ❓ | — | 🟢 |
| Sinergias (escenarios home/away/…) | home_synergy | ❓ | ❓ | motor de escenarios | 🟡 |
| Primer arranque / empaquetado / recuperación de credenciales | experiencia/infra | ❓ | ❓ | pendientes (informe 02/11) | ⚪ |

## Bloque 6 — Oportunidades (Anexo A, requisitos v1.2)

| Capacidad prometida | Módulo | Código | Test | Evidencia | Estado |
|---|---|---|---|---|---|
| Tarjeta de Oportunidades (5 momentos) | experience/liloo | ❓ | ❓ | solo diseño | ⚪ |
| CCR (Clase A/B) con evidencia de reversión | catálogo/conectores | ❓ | ❓ | no implementado | ⚪ |
| Override (intervención manual invalida) | core/security | ❓ | ❓ | no implementado | ⚪ |
| Kill-switch local (detener vs recuperar) | security/core | ❓ | ❓ | parcial (kill-switch existe, distinción nueva) | 🟡 |
| FMEA por oportunidad | experience | ❓ | ❓ | no implementado | ⚪ |

---

**Nota:** los `❓` son el objeto exacto del **paso 2 de validación** ("contrastar cada capacidad prometida
con el código real"). Esta matriz no sustituye esa auditoría; la estructura.

*Documento A0. No autoriza desarrollo, escaneo, despliegue ni cambios de ADR.*
