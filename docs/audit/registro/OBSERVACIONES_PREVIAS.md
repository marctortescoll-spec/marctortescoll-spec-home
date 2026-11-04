# Observaciones previas (mandato §3) — Fase 1
## Registro de discrepancias detectadas en la línea base

**Fecha:** 2026-11-04 · **Naturaleza:** observación (A0). No se modifica `main`; se registra en la rama
de auditoría.

| # | Observación | Explicación |
|---|---|---|
| 1 | **SHAs documentales `695266c` vs `fc1dac1`** | Ambos en `main` de `marctortescoll-spec-home`. `695266c` = publicación de la aprobación OWNER de V1.3; `fc1dac1` = registro posterior de la ronda 11 (organización de la auditoría). Es un avance normal, no una contradicción; `fc1dac1` es el tip vigente. |
| 2 | **`kaenori-governance` local `09b61430` vs remoto `ce71449`** | El `main` local está **adelantado 5 commits** respecto a `origin/main` (trabajo sin publicar en el workspace). Requiere revisión/reconciliación en un mandato posterior. |
| 3 | **Contradicción editorial del Anexo B** | El Anexo B contiene una sección "Aprobación del propietario" (aprobada) y otra "Cuestiones pendientes de aprobación" que todavía dice que los 12 puntos están "pendientes". Residual editorial a corregir **en la rama de auditoría**, no en `main`. |
| 4 | **Sin protección efectiva confirmada en la rama `audit/global-v1.3`** | La rama se creó, pero no hay *branch protection* configurada (requiere GitHub web/API). Se registra como pendiente de gobernanza. |

*Registro A0. No autoriza modificar `main` ni el código.*
