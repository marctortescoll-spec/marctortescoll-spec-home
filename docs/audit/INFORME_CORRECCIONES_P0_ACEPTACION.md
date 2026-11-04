# INFORME — Correcciones P0 de aceptación real (emparejamiento + descubrimiento)
## Preparación A1/A2 · commit `08d26ea` · 2026-11-04

**Suite: 2609 tests en verde.**

## Problema 1 — Emparejamiento (copiar/pegar)

**Causa raíz identificada:** el código de emparejamiento solo se normalizaba con `strip()` (recorte de
espacios en los extremos). Al **pegar** un código podían colarse caracteres invisibles (espacio no
separable, ancho cero, salto de línea, guiones) que el `strip()` no eliminaba, y el canje fallaba hasta
editar manualmente.

**Corrección:** nueva `_normalize_pairing_code()` que **elimina todo espacio (incluido NBSP y saltos),
guiones y caracteres invisibles**, y pasa a mayúsculas — en **servidor** (`auth.py`) y **cliente**
(`app.js`). Test automático de 7 casos (pegar con espacios, guiones, saltos, NBSP, ancho cero).

## Problema 2 — Descubrimiento (Error 501)

**Causa:** dos factores:
1. **Caché PWA obsoleta**: el service worker servía `app.js`/`setup.js` antiguos (versión `v26`) que
   apuntaban a rutas/comportamiento ya corregidos. **Bump a `v27`** para forzar la recarga.
2. **Escaneo no "fail-soft"**: `scan_for_devices()` fallaba entero si una fuente (p. ej. AirPlay) fallaba.
   **Corrección:** cada fuente (sync / airplay / catálogo) se aísla con try/except y los resultados
   válidos de las demás se conservan, anotando el error en `scan_errors`.

## Problema 3 — Recuperación de la experiencia

- El backend ya distinguía 401/403/405/500 (no 501); con la corrección de caché + fail-soft, el cliente
  recibe un error **útil** en vez de "Revisa la conexión" genérico.
- El escaneo parcial permite **continuar sin perder progreso** (criterio exigido).

## Pendiente para verlo en HOME0001

Re-despliegue de `08d26ea` (requiere **grant A3** como el anterior). Mismo procedimiento no destructivo:
nueva release + symlink + reinicio de los 5 servicios Kaenori.

---

*Informe A1/A2. No autoriza despliegue.*
