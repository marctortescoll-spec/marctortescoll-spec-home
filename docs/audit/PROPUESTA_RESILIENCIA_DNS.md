# PROPUESTA TÉCNICA — Resiliencia DNS de la vivienda (eliminar SPOF AdGuard)
## Preparación A1/A2 · sin autorizar cambios · 2026-11-04

## 1. Inventario (solo lectura, verificado)

| Elemento | Valor | Dato |
|---|---|---|
| Router / DHCP | `192.168.1.1` | DHCP server (el nodo **no** es DHCP) |
| HOME0001 | `192.168.1.196` | AdGuard DNS en `:53` (TCP+UDP), upstream ISP |
| Xeon (ingeniero) | `192.168.1.34` | **dnsmasq ya instalado**, uptime 7 d 23 h, VPN endpoint (wg0 `10.200.0.1`) |
| DNS propio del nodo | `80.58.61.250/254` (ISP) | el nodo **no** depende de AdGuard |
| VPN | `wg0` 10.200.0.0/24 | Xeon=10.200.0.1 · HOME0001=10.200.0.2 |

**SPOF identificado:** los dispositivos de la casa resuelven vía `192.168.1.196` (AdGuard). Si HOME0001
se apaga, **se pierde DNS** para toda la vivienda.

## 2. Alternativas evaluadas

| Opción | Descripción | Pros | Contras | Veredicto |
|---|---|---|---|---|
| **A. DNS secundario en Xeon** | dnsmasq (ya instalado) en `192.168.1.34`, forwarding a ISP | sin hardware nuevo · dnsmasq presente · Xeon siempre encendido · sin bucle (forward a ISP, no a AdGuard) | pierde ad-blocking si cae HOME0001 (aceptable en fallback) | **RECOMENDADA** |
| B. DNS en el router | usar el DNS del router como secundario | sin máquina nueva | depende del router ISP (muchos no lo permiten); menos control | viable si el router lo soporta |
| C. 2º equipo dedicado | dispositivo solo para DNS | aislamiento total | coste, mantenimiento, otro equipo | descartada (no económica) |

## 3. Recomendación (Opción A refinada — segundo AdGuard en Xeon)

El concilio detectó que un secundario **sin filtrado** (dnsmasq plano) haría que los clientes, al
consultar ambos DNS en paralelo, **filtren ~20–50 % de anuncios menos** (fuga constante). Por tanto:

- **Primario:** `192.168.1.196` — AdGuard Home (filtrado).
- **Secundario:** `192.168.1.34` — **segundo AdGuard Home** (docker en Xeon), **misma lista de filtros**
  (sincronizada con `adguardhome-sync` o copia del `AdGuardHome.yaml`).
- **Router DHCP:** anunciar ambos (`192.168.1.196`, `192.168.1.34`). Ambos filtran → **sin fuga**; si
  cae uno, el otro sigue filtrando y resolviendo.

## 4. Configuración, pruebas y reversión

**Configuración (Xeon, AdGuard en docker):** bind `192.168.1.34:53`, misma `AdGuardHome.yaml` que
HOME0001 (upstream ISP `80.58.61.250/254` + `1.1.1.1` como respaldo; **nunca** `192.168.1.196`),
`bind-interfaces`, sin exponer el puerto 53 en la interfaz VPN/WAN (firewall: permitir solo LAN).

**Pruebas de conmutación (sin apagar HOME0001 al principio):**
1. `dig @192.168.1.34 example.com` → debe resolver vía ISP.
2. `dig @192.168.1.196 example.com` → debe resolver vía AdGuard.
3. Simular caída de AdGuard (parar el contenedor en ventana) → cliente con DNS secundario debe seguir
   resolviendo (sin bucle).
4. Verificar que `192.168.1.34` **no** reenvía a `192.168.1.196` (sin bucle).

**Reversión:** quitar `192.168.1.34` del DHCP del router (volver a anunciar solo `192.168.1.196`) y
detener dnsmasq en el Xeon. Sin cambios destructivos.

## 5. Garantía anti-bucle y anti-corte

- dnsmasq usa `no-resolv` + upstream fijo (ISP/público), **no** consulta a `192.168.1.196` → **sin bucle**.
- Con HOME0001 apagado: los clientes usan el DNS secundario (Xeon) → **siguen resolviendo** (sin
  ad-blocking, con conectividad).
- El Xeon (7 d 23 h uptime) es el fallback estable.

## 6. Solicitud de autorización (al validar)

Una única autorización A3-DNS para: (1) configurar dnsmasq en Xeon; (2) añadir el DNS secundario al
DHCP del router; (3) ejecutar las pruebas de conmutación; (4) revertir si falla.

*Propuesta A1/A2. No autoriza modificar router/DNS/AdGuard ni reiniciar HOME0001.*
