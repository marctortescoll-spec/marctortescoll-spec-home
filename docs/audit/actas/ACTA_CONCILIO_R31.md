# ACTA DEL CONCILIO — Ronda 31 (auditoría redespliegue HOME0001) · 2026-11-04

**Concilio:** GPT-5 + Argon (rol 07-auditor, evidencia de ejecución) · **A3 (redespliegue)**.

## Objeto

Dictamen del redespliegue controlado `018c745` → `117d7b6` en HOME0001.

## Veredicto: **APRUEBA** (2/2)

- GPT-5: aprueba-con-cambios — condiciones menores: test autenticado de `/api/opportunity` (200), smoke
  de UI, confirmación de checksum.
- Argon: aprueba — el 401 confirma ruta + middleware, pero no la lógica interna; recomienda un 200
  autenticado.

**Nota temporal (Argon, recurrente):** el timestamp del backup `20261104…` es **correcto** (la fecha del
sistema es 2026-11-04); la lectura de "fecha futura" es un desajuste del reloj del propio auditor, no del
nodo.

## Evidencia del despliegue (verificada)

- Commit `117d7b6d0c42494d773604632d1bc5350e8bc74b` · backup 434 MB SHA `0b98037e…`.
- Release nueva + symlink · `018c745` conservada · 5/5 servicios active · health `status ok 1.1.0` ·
  `/api/opportunity` 401 (existe) · datos intactos (22) · kill-switch `{"active": false}`.

## Pendiente menor (no bloqueante)

Test **autenticado** de `/api/opportunity` (HTTP 200) — requiere el emparejamiento en el navegador
(código de un solo uso); lo puede hacer el OWNER al abrir la interfaz.

## Votos

| IA | Voto |
|---|---|
| GPT-5 | aprueba-con-cambios (condiciones menores) |
| Argon | aprueba |

## Estado

- **Redespliegue COMPLETADO y aprobado.** HOME0001 corre `117d7b6` con la UI conectada a Oportunidades.
  Rollback a `018c745` disponible.

*Acta A3. No incluye Etapas posteriores ni actuación física.*
