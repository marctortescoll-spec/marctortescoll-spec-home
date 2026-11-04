# FICHA TÉCNICA — Separación de la VPN de ingeniería del producto
## Preparación A1/A2 · sin autorizar cambios · 2026-11-04

## 1. Estado actual (verificado)

- HOME0001 tiene WireGuard `wg0` (`10.200.0.2`) con **endpoint = Xeon `192.168.1.34`** (`10.200.0.1`).
- Es el **canal de gestión** (el manifest marca "VPN = P0 REQUIRED" y "el Mac Mini/Xeon actúa como
  endpoint de gestión" mientras no exista BDM).
- **Riesgo:** el producto del cliente **no debe** depender de la máquina de ingeniería (Xeon/Mac).

## 2. Diseño de producto (a preparar)

- **Conectividad comercial del cliente:** la caja HOME00X se conecta al router del cliente y funciona
  **sin** VPN de ingeniería (local-first). La VPN P0 es **opcional de gestión remota**, con:
  - **Identidad por nodo** (ya existe en el factory: `fingerprint.sh` + claves Ed25519).
  - **Endpoint de gestión propio** (BDM/P1 o servicio del cliente), **no** el Xeon.
- **Separación:** la VPN de ingeniería (para que yo administre el laboratorio) es **distinta** de la
  conectividad comercial del cliente. No se reutiliza el canal del Xeon para el producto.

## 3. Migración propuesta (a ejecutar cuando haya BDM/endpoint propio)

1. Definir el **endpoint de gestión del producto** (BDM/P1).
2. Generar **identidad/claves por nodo** en el factory (ya preparado).
3. Cambiar el peer de `wg0` del Xeon al endpoint de producto.
4. Mantener la VPN de ingeniería **solo** para el laboratorio, aislada del producto.

## 4. Bloqueo actual

No hay **BDM ni endpoint de gestión propio** implementado (P1). Hasta entonces, la VPN de ingeniería se
mantiene **solo para el laboratorio** y **no** forma parte del producto entregable.

*Ficha A1/A2. No autoriza modificar VPN/red ni desplegar.*
