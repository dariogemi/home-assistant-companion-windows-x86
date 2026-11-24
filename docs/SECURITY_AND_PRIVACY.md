# Seguridad, Privacidad y Solución de Problemas

Este documento describe el comportamiento y las opciones de seguridad declaradas para la versión publicada. No reemplaza una auditoría del código fuente ni una prueba independiente del instalador.

## Datos sensibles y almacenamiento local

La configuración se almacena localmente en:

`%APPDATA%\HomeAssistantCompanion\config.json`

Según la documentación de la aplicación, el archivo puede contener:

- Tokens OAuth2 (`access_token` y `refresh_token`).
- Identificadores de webhook (`webhook_id`).
- Servidores configurados, sensores habilitados y preferencias.

**Tratá este archivo como un secreto.** La documentación de la API de Home Assistant establece que el `webhook_id` funciona como credencial para las solicitudes webhook. No publiques, adjuntes ni compartas el archivo sin eliminar previamente tokens, webhook IDs, direcciones privadas y otros datos identificatorios.

La documentación de esta release no especifica un mecanismo de cifrado en reposo para estos valores. Hasta que ese mecanismo se verifique en el binario o en el código, no debe asumirse que el archivo está cifrado. Protegé la cuenta de Windows, el perfil de usuario y los permisos NTFS del directorio.

## Comunicación con Home Assistant

- La aplicación se comunica directamente con las instancias configuradas.
- La autenticación inicial utiliza OAuth2.
- Las solicitudes posteriores al webhook usan el `webhook_id`, que debe protegerse como una contraseña.
- Las notificaciones locales requieren una conexión WebSocket autenticada y activa.
- Si la aplicación se desconecta, la entrega por WebSocket local deja de estar disponible. No debe interpretarse como un servicio push en segundo plano garantizado.

Al cerrar sesión o eliminar una instancia, la aplicación declara solicitar la revocación del token OAuth2. Verificá el estado de la sesión desde Home Assistant si el equipo se pierde o queda fuera de tu control.

## Certificados TLS

La verificación TLS estricta se declara activada por defecto.

La opción para permitir certificados autofirmados debe habilitarse únicamente para una instancia concreta y cuando el certificado sea esperado y comprendido. No habilites excepciones de certificado para conexiones a través de redes públicas o no confiables.

Siempre que sea posible, utilizá HTTPS con un certificado válido y mantené Home Assistant y Windows actualizados.

## Comandos remotos

La opción **Permitir comandos remotos del sistema** debe permanecer desactivada si no necesitás controlar el equipo desde Home Assistant.

Si se habilita:

- Restringí el acceso a Home Assistant a usuarios de confianza.
- Revisá las automatizaciones y scripts que puedan enviar comandos.
- Evitá exponer servicios de Home Assistant directamente a Internet.
- Tené presente que apagado, reinicio y suspensión pueden interrumpir tareas en curso.
- No uses notificaciones remotas como único mecanismo de autorización para operaciones sensibles.

## Privacidad

El proyecto declara no utilizar telemetría de terceros ni servidores intermediarios. Las funciones de red descritas requieren comunicación con la instancia Home Assistant configurada.

Los sensores de ventana activa, procesos, cámara, micrófono, red y nombre de usuario pueden revelar información sobre la actividad del equipo. Habilitá únicamente los sensores que realmente necesites y considerá quién puede consultar esas entidades dentro de Home Assistant.

## Solución de problemas

1. **No aparece el servidor en mDNS:** comprobá que ambos equipos estén en la misma red y que el firewall permita mDNS (UDP 5353). Como alternativa, ingresá la URL manualmente.
2. **No aparecen sensores:** comprobá que `mobile_app` esté habilitada, que el registro del dispositivo se haya completado y que los sensores estén habilitados en la aplicación.
3. **No llegan notificaciones:** verificá que la aplicación esté abierta, conectada y que la conexión WebSocket se mantenga activa.
4. **Problemas con HTTPS:** revisá la validez, confianza y nombre del certificado antes de habilitar excepciones.
5. **Comandos remotos no ejecutados:** comprobá la opción de consentimiento local, la conectividad y el formato del comando enviado.

## Alcance de las afirmaciones

Este repositorio distribuye instaladores compilados y documentación, pero no publica el código fuente. Por ese motivo, las afirmaciones sobre cifrado de credenciales, permisos internos, telemetría, validación TLS y comportamiento exacto del binario deben considerarse declaraciones del producto hasta contar con evidencia técnica independiente.

Para cada release se recomienda publicar resultados de pruebas, hash del instalador y una lista de cambios verificables.
