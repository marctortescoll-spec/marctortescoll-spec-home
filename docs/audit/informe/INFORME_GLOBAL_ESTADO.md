# KAENORI — AUDITORÍA GLOBAL DE PREPARACIÓN DE PRODUCTO V1.3
## Informe Global de Estado · dirigido al propietario

**Fecha:** 2026-11-04 · **Fuente canónica:** `home/integration/connectors-20261022 @ 549700b9` ·
**Naturaleza:** solo lectura (A0). Sin ejecutar pruebas, sin desplegar, sin modificar código.

---

## 1. Resumen ejecutivo

Kaenori tiene **dos mitades asimétricas**: una **mitad de gobierno/lectura bien construida en código**
(Core + permisos + kill-switch + discovery + EMS shadow + Wizard), y una **mitad de recuperación y
experiencia que solo existe en papel** (CCR, Override, tarjeta de Oportunidades, dashboard narrativo).
Además, **no hay evidencia de una instalación real ejecutada**. Distancia a V1.3: **media, con un
bloqueo de seguridad físico que impide declarar preparación**.

## 2. Qué está realmente construido (🟢 codificado)

Core (6 puertos) · política deny-by-default/fail-closed · kill-switch persistente · secretos efímeros ·
descubrimiento pasivo/activo + catálogo + dedupe · 9 conectores solo-lectura · Wizard (máquina de
estados) · onboarding conversacional · Liloo Lite · cerebro de experiencia (sesiones/memoria/voz) ·
EMS Shadow + matemática · receta de fábrica + first-boot + backup.

## 3. Qué está parcial (🟡)

Kill-switch (**solo bloquea órdenes nuevas**; no recupera) · egress jail (**declarativo, sin
cortafuegos real**) · instalación reproducible (**no ejecutada en hierro**) · CI + 203 tests (**no
ejecutados aquí**).

## 4. Qué falta (⚪) y qué está bloqueado (🔴)

- ⚪ **CCR (Clase A/B + safe_stop)** — solo en Anexo A.
- ⚪ **Override** (control manual prevalece) — solo en Anexo A.
- ⚪ **Oportunidades** (5 momentos + evidencia A/B/C) — solo en Anexo A.
- ⚪ **Proyección 12 categorías → 5 verticales** — solo en V1.3 §5.
- ⚪ **Dashboard "cuenta la historia"** + continuidad por usuario — solo en Anexo D.
- 🔴 **Instalación real en nodo** — manifiesto HOME0001 dice "NO ejecutado".

## 5. Riesgos (priorizados)

1. **Seguridad física:** sin CCR, una orden ya enviada no se revierte si el dispositivo se desconecta.
2. **Colisión kill-switch ↔ CCR** (detectada por Argon): el kill-switch fail-closed bloquearía también
   la orden de retorno a estado seguro; el `safe_stop` necesita un canal privilegiado.
3. **Sin aislamiento de red efectivo** (egress sin enforcement).
4. **Convivencia:** sin Override, la automatización puede "pelear" con la intervención humana.
5. **Duplicación de código** (`home_ems` vs `home_discovery_source/ems.py`).

## 6. Los cuatro recorridos de cliente

| Recorrido | Estado | Cadena crítica | Primer bloqueo real |
|---|---|---|---|
| 1. Primera instalación | 🟡 parcial | factory+first-boot (cod) → identidad (cod) → discovery (cod) → lectura (cod) → dashboard (parcial) | **instalación real no verificada** |
| 2. Uso cotidiano | 🟡 parcial | Liloo Lite (cod) + webui (cod) → dashboard narrativo (no) | **dashboard "historia" no implementado** |
| 3. Oportunidad (prueba+eval) | 🔴 bloqueado | EMS shadow (cod) → simular (parcial) → autorizar (parcial) → probar (no: CCR) → evaluar (no) | **CCR / safe_stop** |
| 4. Convivencia | 🟡 parcial | policy deny-by-default (cod) + principal/rol (cod) → Override (no) | **Override** |

## 7. Camino crítico al primer piloto energético

1. **Resolver la colisión kill-switch ↔ CCR** y codificar `safe_stop` (reversión física con bypass
   privilegiado).
2. **Verificar instalación real + egress enforcement** (entorno seguro antes de lógica de control) — o,
   alternativamente, **CCR primero** (desacuerdo GPT-5 vs Argon registrado en el acta).
3. **Codificar Override**.
4. **Codificar la tarjeta de Oportunidades** (5 momentos + A/B/C) sobre EMS shadow.
5. **Verificar el recorrido 3 completo** (una oportunidad energética real) antes de cualquier actuación.

## 8. Dictamen

**NO PREPARADO para producción.** **Preparado para continuar** la validación técnica con un plan de
ingeniería priorizado (abajo). La auditoría documental (áreas A–F + recorridos) está **completa**; falta
la auditoría de ejecución (tests/instalación real), que requiere un mandato posterior con entorno
controlado.

---

*Informe de auditoría (A0). No autoriza desarrollo, despliegue ni actuación sobre equipos reales.*
