# CIERRE — Ejecución del rediseño de onboarding (verticales + gateways) · R34
## Kaenori HOME00X · 2026-11-05 · código `e68f32e` · suite 2660 tests verde

**Resultado: los 7 bloques del mandato R34 están construidos, probados y listos para desplegar (A1/A2, sin desplegar).**

## Qué se ha construido (B0–B5)

| Bloque | Qué hace | Prueba |
|---|---|---|
| **B0** | Mapper determinista `categoría cruda → 5 verticales` (audio→media, energy→clima, camera→seguridad…) + desambiguación por nombre/fabricante + override manual | 8 tests |
| **B1** | Añadidos **MQTT** y **Smart Life/Tuya** a `ecosystems_status` (gateways con estado) | test ecosystems |
| **B2** | **Deduplicación** por MAC/UUID + prioridad `Gateway > Local > Cloud` (sin duplicados hub↔mDNS) | 6 tests |
| **B3** | `verticals_report()`: informe + narrativa ("he encontrado X en clima…") + `GET /api/verticals` | test |
| **B4** | **Menú izquierdo de verticales** en el wizard + filtro por vertical (brand-tokens) | JS + tests |
| **B5** | Cierre "he encontrado las verticales… pincha cada una" integrado en la narrativa | test |

## Cómo se ve (para que lo imagines)

- **Izquierda**: el acompañante de Liloo ahora muestra la lista de **verticales descubiertas** (Todas · Energía Y Clima · Seguridad · …) con su conteo.
- **Derecha**: al pulsar "Buscar equipos", el panel muestra el **informe** ("Escaneando la red… he encontrado 23 equipos… gateways detectados…") y los equipos filtrables por vertical.

## Límites respetados

Solo lectura · sin desplegar en HOME0001 (requiere grant A3) · colores/logo ya definidos aplicados · `es`+`ca`.

---

*Cierre A1/A2. El despliegue en HOME0001 queda pendiente de autorización A3 del OWNER.*
