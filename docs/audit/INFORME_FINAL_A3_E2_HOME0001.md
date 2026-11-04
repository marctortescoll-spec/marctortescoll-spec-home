# INFORME FINAL — Etapa 2 (Pruebas A y B) · HOME0001
## Resistencia y recuperación de servicios Kaenori · 2026-11-04

**Resultado: ÉXITO** (Pruebas A y B superadas). La Prueba C (reinicio completo) queda **pendiente** de
autorización independiente.

---

## Preparación previa (verificada)

1. Identidad/versión: `home0001` · `018c745` ✅.
2. Servicios Kaenori = **solo-lectura** (node, catalog, experience, ha-adapter, experience-web): no
   emiten órdenes sobre dispositivos. **No se reinició** HA, AdGuard ni Music Assistant.
3. **Backup fresco**: `/srv/home00x/backups/kaenori-e2-pre-test-20261104T214541Z.tar.gz` (463 MB,
   SHA-256 `dba9deb9…`).
4. **Copia externa cifrada en Xeon** (`/home/marc/kaenori-custody/`): `.tar.gz.gpg` (AES-256),
   descifrado round-trip verificado (SHA coincide). Passphrase en `.passphrase` (0600).

## Prueba A — Recuperación de servicios (reinicio 1 a 1)

| Servicio | Resultado |
|---|---|
| kaenori-node · catalog · experience · ha-adapter · experience-web | **5/5 active** tras reinicio |

Health final: `{"status":"ok","version":"1.1.0","tls":true}` · puertos 8137/8138/8443 escuchando.

## Prueba B — Seguridad ante caída de procesos (SIGKILL)

- kaenori-node (PID 97439) → `kill -9` → **auto-recuperado** (PID 99671, `active`).
- Kill-switch tras crash: `{"active": false}` (**deny-by-default conservado**, sin ventana safe-stop
  abierta).
- Puerto 8137 responde (404 = servicio arriba).

## Conclusión

- Los servicios Kaenori **se recuperan correctamente** tras reinicio y tras caída de proceso.
- La **identidad, permisos y deny-by-default** se conservan (no hay autorizaciones inseguras tras fallos).
- **Datos intactos** (no se tocó `/srv/home00x` ni equipos externos).
- Existe **respaldo externo cifrado y verificado** (Xeon) + rollback local.

## Recomendación

**Continuar a Prueba C (reinicio completo)** solo cuando exista un procedimiento que garantice la
**continuidad del DNS de la vivienda** (AdGuard en 192.168.1.196:53) — requiere autorización A3
independiente.

---

*Informe Etapa 2 (A+B). Requiere dictamen de 07-auditor. La Etapa 3 no está autorizada.*
