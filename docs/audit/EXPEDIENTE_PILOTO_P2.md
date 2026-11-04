# EXPEDIENTE DE PILOTO — P2 · Primera oportunidad energética
## "Aprovechar mejor mi energía solar" · preparación A1 (sin A3)

**Fuente:** Anexo A §14 · Plan Maestro P2 · `home @ 549700b9` · **No autoriza actuación física ni A3.**

---

## 1. Objetivo del piloto

Una vivienda piloto, **un sistema térmico compatible** (aerotermia/ACS), medición fiable, lectura real,
permiso temporal específico, **una estrategia acotada**. Vertical: Energía y Clima (+ Confort).
**Reutilizar EMS y la infraestructura de lectura existentes** antes de nada alternativo.

## 2. Candidato CCR Clase A (selección documental + requisito de verificación)

**Candidato propuesto:** el sistema térmico (aerotermia/ACS) de la vivienda piloto.

**Criterio de selección verificable (obligatorio antes de A3):**
1. El equipo recupera **por sí mismo** un estado seguro al expirar un **TTL/lease nativo** (dead-man).
2. **Evidencia real de que lo hace** (prueba de corte de red observada), **no** el anuncio del fabricante.
3. Si es **Clase B** (sin TTL nativo), queda **excluido** de pruebas automáticas de riesgo (regla
   fail-closed). Para Clase B apto se exige **Watchdog local + heartbeat** antes de cada consigna.

**Estado en código:** los conectores son **solo-lectura** (leen estado); la **actuación** se hará vía
`HomeAssistantActionExecutor` (HA services) una vez habilitada. **No hay CCR Clase A codificada aún** (el
TTL existente es de permisos/secretos, no de recuperación de dispositivo): la verificación TTL es un
requisito de **A3**, no algo que este plan pueda dar por hecho.

## 3. Recorrido end-to-end (simulado) — mapeado a lo ya construido

| Momento | Componente construido (A1) | Estado |
|---|---|---|
| Descubrir | `home_opportunity.lifecycle` (DETECTED) | ✅ |
| Simular | `home_ems/math.py` (`oportunidad_energetica`, consignas) | ✅ |
| Autorizar | `lifecycle` (PERMISSION_PENDING → TRIAL_READY) + `permission.py` (PDP/TTL) | ✅ |
| Probar | `lifecycle` (TRIAL_RUNNING) + `home_security.safe_stop` (CCR Clase B) + `home_ems.override` | ✅ |
| Verificar | `lifecycle` (EVALUATED) + `EvidenceLevel` A/B/C + `can_adopt` (B/C) | ✅ |

**Falta para el piloto real (A3):** actuación habilitada (HA services), instalación real, medición real,
y **verificación CCR Clase A** del dispositivo.

## 4. Criterios de aceptación (Anexo A §15) — mapeados

| Criterio | Cubierto por |
|---|---|
| Sin datos → sin beneficio inventado | `oportunidad_energetica` devuelve `None` sin tarifa |
| Sin permiso → sin actuación | `AuthorizationPolicy` deny-by-default |
| Permiso expirado → rechazo | `permission.py` TTL + `command_expired` |
| Equipo desconectado → no actúa a ciegas | `verification_unavailable/failed` |
| Cambio manual → Override | `home_ems.override` (hold + invalidación) |
| Orden sin confirmación → no verificada | `verification_failed` (enviada-no-confirmada) |
| Prueba vencida → sin órdenes | `safe_stop` timeout → `ARMED_DENY_ALL` |
| Revocación → bloqueo + recuperación | `safe_stop` + kill-switch |
| Sin LLM remoto → protecciones locales | kill-switch/safe-stop locales |

## 5. Pruebas, mediciones, permisos, límites, recuperación, riesgos

- **Pruebas (A3):** corte de red observado (evidencia TTL) · rollback de consigna · restore desde
  backup · reinicio con estado consistente.
- **Mediciones:** producción solar, consumo casa, temperatura ambiente, consigna, estado del actuador,
  tarifa (actual/histórica/inferida/ausente).
- **Permisos:** finalidad · equipo · capacidad · límites · horario · duración · revocación · recuperación;
  lectura/acción/automatización **por separado**; Admin autoriza, Habitante pausa/cancela.
- **Límites:** Life Safety > límites > manual > objetivos > rutinas (`ConflictPolicy`).
- **Recuperación:** kill-switch local (detener nuevas ≠ recuperar) + safe-stop + CCR.

## 6. FMEA resumido (modos de fallo → mitigación)

| Fallo | Efecto | Mitigación |
|---|---|---|
| Pérdida de red durante prueba | consigna sin verificar | Clase A TTL / safe-stop timeout → deny-all |
| Sensor crítico caído | decisión sin dato | detener + política segura (fail-closed) |
| Conflicto de objetivos | consigna incorrecta | `ConflictPolicy` (Life Safety primero) |
| Override manual | prueba inválida | `override` (hold + invalidación) |
| Reinicio con reloj incierto | prueba en estado ambiguo | `suspended-segura` hasta NTP/RTC |

## 7. Puerta A3 (lo que NO hace este expediente)

- Instalación real, actuación física, vivienda real: **requieren mandato A3 con target + rollback**.
- Verificación CCR Clase A (evidencia TTL real): **requiere A3**.

*Expediente A1. No autoriza actuación física, instalación ni producción.*
