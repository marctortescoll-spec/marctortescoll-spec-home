# ACTA DEL CONCILIO — Ronda 30 (revisión propuesta resiliencia DNS) · 2026-11-04

**Concilio:** GPT-5 + Argon (rol 07-auditor) · **A1/A2 · propuesta (sin autorizar cambios)**.

## Objeto

Revisión de la propuesta para eliminar el SPOF de DNS (AdGuard en HOME0001).

## Veredicto: **APRUEBA-CON-CAMBIOS (propuesta refinada)**

- GPT-5: aprueba-con-cambios — endurecer dnsmasq (bind-interfaces, domain-needed, bogus-priv, firewall),
  pruebas de conmutación.
- Argon: aprueba-con-cambios — dos correcciones clave:
  1. `no-dhcp` **no existe** en dnsmasq (error de sintaxis).
  2. Un secundario **sin filtrado** causaría **fuga de anuncios (20–50 %)** porque los clientes
     consultan ambos DNS en paralelo → alternativa superior: **segundo AdGuard Home en Xeon** con la
     misma lista de filtros.

**Propuesta refinada (incorporada):** segundo AdGuard Home en Xeon (`192.168.1.34`, misma lista de
filtros, upstream ISP/público, nunca a `192.168.1.196`, bind-interfaces, sin exponer 53 en VPN/WAN).

## Votos

| IA | Voto |
|---|---|
| GPT-5 | aprueba-con-cambios (endurecimiento incorporado) |
| Argon | aprueba-con-cambios (sintaxis + fuga corregidas) |

## Estado

- **Propuesta validada** con las correcciones. Queda preparada la **solicitud A3-DNS única** para
  implementar (segundo AdGuard en Xeon + DHCP del router + pruebas de conmutación + reversión).
- **Prueba C (reinicio) sigue pendiente** de autorización independiente.

*Acta A1/A2. No autoriza modificar router/DNS/AdGuard ni reiniciar HOME0001.*
