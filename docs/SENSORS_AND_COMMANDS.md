# Sensores, Notificaciones y Comandos Remotos

## 1. Sensores enviados a Home Assistant

Una vez registrado el equipo mediante la integración `mobile_app`, Home Assistant creará un dispositivo para tu PC con los siguientes sensores configurables (cada uno puede activarse o desactivarse individualmente en **Administrar Sensores**):

| Identificador (`unique_id`) | Tipo | Unidad | Atributos adicionales |
| --- | --- | --- | --- |
| `cpu_usage` | `sensor` | `%` | — |
| `memory_usage` | `sensor` | `%` | `used_mb`, `total_mb` |
| `disk_usage` | `sensor` | `%` | `drive`, `free_gb`, `total_gb` |
| `battery_level` *(si hay batería)* | `sensor` | `%` | `is_charging`, `plugged_in` |
| `battery_charging` *(si hay batería)* | `binary_sensor` | — | — |
| `active_window` | `sensor` | — | `process` |
| `idle_time` | `sensor` | `s` | — |
| `audio_volume` | `sensor` | `%` | `muted` |
| `audio_muted` | `binary_sensor` | — | — |
| `wifi_ssid` | `sensor` | — | `signal_pct` |
| `ip_address` | `sensor` | — | — |
| `display_count` | `sensor` | — | `primary_resolution`, `screensaver_active` |
| `mic_in_use` *(Windows 10/11)* | `binary_sensor` | — | — |
| `webcam_in_use` *(Windows 10/11)* | `binary_sensor` | — | — |
| `uptime` | `sensor` | `min` | `os_version`, `machine_name`, `user_name` |

> **Nota sobre Windows 8 / 8.1:** Los sensores `mic_in_use` y `webcam_in_use` utilizan `CapabilityAccessManager` introducido en Windows 10; en Windows 8 y 8.1 devuelven `false` de forma segura sin generar errores.

---

## 2. Notificaciones Push y Botones de Acción

La aplicación se suscribe directamente al canal `mobile_app/push_notification_channel` mediante el WebSocket de Home Assistant, por lo que no requiere Firebase ni servicios externos.

### Ejemplo de notificación con botones de acción

```yaml
action: notify.mobile_app_mi_pc
data:
  title: "Puerta principal abierta"
  message: "La puerta lleva más de 5 minutos abierta."
  data:
    actions:
      - action: "LOCK_DOOR"
        title: "Cerrar cerradura"
      - action: "SILENCE_ALERT"
        title: "Ignorar"
```

Al pulsar un botón en Windows, la aplicación envía el evento `mobile_app_notification_action` a Home Assistant con `action: "LOCK_DOOR"` para que puedas disparar tus automatizaciones.

---

## 3. Comandos Remotos desde Home Assistant

Si está activada la opción **Permitir comandos remotos del sistema** en **Notificaciones y Comandos**, puedes controlar el PC enviando un mensaje `notify.mobile_app_<dispositivo>` con los siguientes valores en `message`:

| Comando (`message`) | Parámetros en `data` | Acción en Windows |
| --- | --- | --- |
| `command_lock` | — | Bloquea la sesión de Windows (`LockWorkStation`) |
| `command_sleep` | — | Suspende el equipo |
| `command_shutdown` | — | Apaga el equipo (con espera de 5 s) |
| `command_reboot` | — | Reinicia el equipo (con espera de 5 s) |
| `command_display_off` | — | Apaga los monitores |
| `command_display_on` | — | Despierta los monitores |
| `command_volume_level` | `command: 0..100` | Ajusta el volumen maestro |
| `command_media` | `media_command: play_pause \| next \| previous \| stop` | Envía tecla multimedia global |
| `command_tts` | `tts_text: "Texto a leer"` | Reproduce voz sintetizada (SAPI) |
| `command_webview` | `command: "/lovelace/camaras"` o URL | Abre la ventana y navega a esa ruta |
| `command_update_sensors` | — | Fuerza el envío inmediato de sensores |
| `clear_notification` | — | Oculta el toast activo |

### Ejemplo YAML para hablar por los altavoces del PC

```yaml
action: notify.mobile_app_mi_pc
data:
  message: "command_tts"
  data:
    tts_text: "Alguien ha llamado al timbre de la entrada."
```
