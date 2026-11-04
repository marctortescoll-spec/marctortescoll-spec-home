# ACTA DEL CONCILIO — Ronda 29 (auditoría Etapa 2 A+B: HOME0001) · 2026-11-04

**Concilio:** GPT-5 + Argon (rol 07-auditor, evidencia de ejecución) · **A3-E2 (opción B)**.

## Objeto

Dictamen independiente de las Pruebas A (reinicio de servicios 1 a 1) y B (caída de proceso) sobre
HOME0001 @ `018c745`.

## Veredicto: **APRUEBA ETAPA 2 (A+B)** — 2/2

- GPT-5: aprueba — recuperación 5/5 + deny-by-default conservado tras `kill -9`; riesgos operativos
  menores no bloqueantes.
- Argon: aprueba (con nota de NTP) — valida la resiliencia y postura fail-safe; exigía auditar la
  sincronización NTP.

**Nota NTP resuelta:** el nodo sincroniza con `3.debian.pool.ntp.org` (stratum 2, leap normal, root
distance 24 ms); la fecha real es `2026-11-04` (correcta). El `synchronized: no` era transitorio; el
reloj está correcto y NTP funcionando.

## Recomendaciones (no bloqueantes)

- GPT-5: medir MTTR/enforcement, evitar restart storms, validar caducidad TLS, prueba puntual de
  restauración aislada, rotación de passphrase del backup.
- Argon: la **Prueba C (reinicio)** difiere correctamente por el **acoplamiento SPOF** de AdGuard (DNS)
  en el mismo nodo; antes de Prueba C, configurar **DNS secundario/redundante** (router/DHCP o réplica).

## Votos

| IA | Voto |
|---|---|
| GPT-5 | aprueba Etapa 2 (A+B) |
| Argon | aprueba Etapa 2 (A+B) con nota NTP (resuelta) |

## Estado

- **Etapa 2 (A+B) COMPLETADA con éxito.** HOME0001 resiste reinicios de servicio y caídas de proceso,
  conservando identidad, permisos y deny-by-default, con respaldo externo cifrado verificado.
- **Prueba C (reinicio completo)** pendiente de autorización independiente + DNS redundante.
- **Etapa 3 no autorizada.**

*Acta A3-E2. No autoriza Etapa 3 ni Prueba C.*
