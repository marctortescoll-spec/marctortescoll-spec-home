# RESUMEN EJECUTIVO — Fase 1 (Áreas A + B)
## Auditoría Global de Preparación de Producto V1.3 · para el propietario

**Fecha:** 2026-11-04 · **Fuente:** `home/integration/connectors-20261022 @ 549700b9` · **A0** (solo
lectura).

---

## Qué hemos encontrado (en una frase)

**El "cerebro" y las "reglas" están bien construidos en código; lo que falta es la "red de seguridad
física" y la prueba de que la caja se instala de verdad.**

## Lo que está realmente construido (con código y commit)

- **Core con sus 6 puertos** (autorización, lectura, acción, identidad, auditoría, discovery) — sí está
  codificado (`core/kaenori_core/runtime/ports.py`).
- **Política deny-by-default + fail-closed + kill-switch** — sí está codificada y es correcta
  (`home_security/policy.py`, `killswitch.py`).
- **Receta de fábrica** (instalación + primer arranque con identidad + backup/restore + tests offline)
  — sí existe (`deployment/factory/`), en versión **0.1.0**.

## Lo que NO está en el código (solo en el papel de V1.3)

1. **La recuperación física (CCR).** Hoy el **kill-switch solo bloquea órdenes nuevas**; **no existe**
   un mecanismo que devuelva un equipo ya modificado a su estado anterior. Es el **riesgo de seguridad
   nº 1**: si un dispositivo se desconecta tras recibir una orden, no está garantizado que se recupere.
2. **El "Override"** (que tu intervención manual prevalezca y anule la evaluación) — está especificado,
   **no codificado**.
3. **El egress jail** (aislar la caja de Internet) es **declarativo, no real**: el propio código dice
   que "no se aplica ningún cortafuegos".

## Lo que NO se puede verificar desde aquí

- **Ninguna instalación real.** El manifiesto de HOME0001 dice explícitamente "NO ejecutado". El
  t520/laboratorio queda **NO VERIFICADO**.

## Distancia real a Experience V1.3

La **mitad de gobierno** (Core, permisos, kill-switch de parada) está implementada. La **mitad de
recuperación y de experiencia real instalada** no lo está. **No está preparado para producción.**

## Camino crítico (lo primero, segundo y tercero)

1. **Implementar y verificar la CCR** (Clase A/B + recuperación segura por dispositivo) — bloquea
   cualquier actuación segura.
2. **Implementar el Override** (tu control manual siempre prevalece).
3. **Verificar una instalación real de la caja** (instalar, arrancar, reiniciar, restaurar) en un
   entorno controlado.

## Dictamen preliminar del concilio

**NO PREPARADO** para pasar a la siguiente fase como producto; **sí preparado** para continuar la
auditoría documental (faltan las áreas C, D, E y F y los cuatro recorridos de cliente). El dictamen
independiente de GPT-5 y Argon sobre esta Fase 1 se emite a continuación.

*Documento de auditoría (A0). No autoriza desarrollo, despliegue ni actuación sobre equipos.*
