# Seguridad, Privacidad y Solución de Problemas

## Almacenamiento local y credenciales

- Toda la configuración de servidores, tokens OAuth2 (`access_token` y `refresh_token`), `webhook_id`, sensores habilitados y preferencias se guarda **exclusivamente en el equipo del usuario** dentro de:
  `%APPDATA%\HomeAssistantCompanion\config.json`
- La aplicación **no incluye telemetría de terceros**, analíticas externas ni servidores intermediarios: toda comunicación ocurre directamente entre tu PC y las instancias de Home Assistant que hayas configurado.
- Al cerrar sesión desde Home Assistant o eliminar un servidor, se solicita la revocación del token OAuth2 mediante `/auth/token`.

## Certificados SSL autofirmados

- Por defecto la verificación TLS estricta está **activada**.
- Solo si marcas explícitamente **Permitir certificados SSL autofirmados en red local** en un servidor concreto, la app aceptará certificados autofirmados para esa instancia.

## Comandos remotos

- Si prefieres que Home Assistant no pueda ejecutar acciones de energía, bloqueo, volumen o TTS sobre tu equipo, desactiva **Permitir comandos remotos del sistema** en **Configuración de Companion → Notificaciones y Comandos**.

## Solución de problemas frecuentes

1. **No aparece mi servidor en el escaneo mDNS:**
   - Asegúrate de que tu PC y Home Assistant estén en la misma subred y que el firewall de Windows permita tráfico UDP en el puerto `5353` (mDNS), o usa **Introducir dirección manualmente**.
2. **Los sensores no aparecen en Home Assistant:**
   - Verifica en **Configuración de Companion → Servidores** que figure `Webhook: Registrado` y que la integración **Mobile App** esté habilitada en tu `configuration.yaml` (`default_config:` la incluye por defecto).
   - Pulsa **Actualizar ahora** en la pestaña **Administrar Sensores** y revisa **Registros y Diagnóstico**.
3. **Las notificaciones no llegan:**
   - Confirma que la barra superior muestre estado **Conectado**. Las notificaciones dependen del WebSocket activo hacia tu instancia de Home Assistant.
