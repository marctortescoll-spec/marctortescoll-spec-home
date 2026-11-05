# ACTA DEL CONCILIO — Ronda 32 (mandato primera integración E2E) · 2026-11-05

**Concilio:** DeepSeek (orquestador/ejecutor) + GPT-5 (arquitectura/producto) + Argon (validación) · **A0** (análisis/diseño, sin autoridad).

## Objeto

Elaborar el mandato consolidado para cerrar la primera integración real de extremo a extremo de Kaenori.

## Auditoría previa (DeepSeek, real)

- Suite 2609 tests verde; componentes construidos: Core, Discovery, Catálogo, HA solo-lectura, EMS, Wizard, PWA, Liloo, Seguridad.
- HOME0001: HA con 26 entidades reales (EMS V2C: 636/927/291/0 W cambiantes); discovery mDNS vacío (`platforms: []`).

## Veredicto: **consenso** (propuesta única entregada)

- **Piloto:** sistema EMS V2C vía HA (1 dispositivo lógico, 5 métricas, solo lectura).
- **Arquitectura:** HA → `ems_shadow_ingest` → `memory.db` (fuente única) → dashboard + Liloo.
- **Hoja de ruta:** B0 (esquema+procedencia+TTL) → B6 (prueba Delta y Corte).

## Aportaciones y discrepancia resuelta

| IA | Aportación |
|---|---|
| DeepSeek | Diagnóstico + piloto V2C + bloques B1–B6 + fuente única `memory.db`. |
| GPT-5 | Añade B0 (SQLite file-backed + procedencia + TTL); renombres por `device_id`/`unique_id`; 7 pruebas. |
| Argon | Corrige la clave (agregado de **dispositivo**, no `entity_id`); antigüedad con `last_updated` nativo de HA; Prueba Delta y Corte; `unavailable`→`no_disponible`. |

**Discrepancia:** GPT-5 proponía `ha://<entity_id>`; Argon demostró que clonaría las 5 entidades del V2C como 5 dispositivos. **Resuelto:** clave por dispositivo (`ha://device/<device_id>` → N entidades).

## Resultado

Mandato consolidado en `MANDATO_INTEGRACION_E2E.md` (secciones A–G). **Sin despliegues ni cambios de código hasta autorización del OWNER.**

*Acta A0. El Concilio no concede autoridad: la ejecución requiere autorización del OWNER para A3/despliegues.*
