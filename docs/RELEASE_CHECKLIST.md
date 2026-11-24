# Lista de verificación de calidad de publicaciones

Utilizá esta lista antes de publicar un nuevo instalador. Marcá un punto como completado únicamente cuando exista evidencia para la compilación exacta que se va a publicar.

## 1. Identidad y procedencia del build

- [ ] Registrar la revisión del código fuente y el entorno utilizados para generar el instalador.
- [ ] Registrar versión, arquitectura, tamaño y SHA-256 del instalador.
- [ ] Confirmar que el ejecutable y los componentes incluidos corresponden al objetivo x86 previsto.
- [ ] Analizar el instalador final con herramientas de protección actualizadas.
- [ ] Si hay firma de código, firmar el instalador y verificar la firma en un equipo limpio.
- [ ] Publicar notas de versión con cambios visibles y limitaciones conocidas.

## 2. Compatibilidad del sistema operativo

Ejecutá pruebas en equipos limpios o snapshots. No infieras compatibilidad únicamente a partir de una compilación exitosa.

| Plataforma | Evidencia de prueba de humo registrada |
| --- | --- |
| Windows 8 x86 | Instalado en una tablet; acceso a la instancia configurada de Home Assistant confirmado |
| Windows 11 x64 | Instalado en un PC; acceso a la instancia configurada de Home Assistant confirmado |
| Windows 8 x64 | Todavía no documentado |
| Windows 8.1 x86 / x64 | Todavía no documentado |
| Windows 10 x86 / x64 | Todavía no documentado |

Los dos resultados registrados son pruebas de humo en el entorno del mantenedor, no una certificación completa de compatibilidad. Confirman instalación y conectividad con Home Assistant, pero no implican que se hayan probado todos los sensores, notificaciones, comandos remotos, actualización/desinstalación ni funciones dependientes del hardware.

Para cada plataforma adicional, registrá compilación del SO, arquitectura, resultado de instalación, arranque, onboarding y limitaciones conocidas. Windows 11 no tiene una edición x86 de 32 bits; la aplicación x86 debe ejecutarse sobre un sistema anfitrión x64 compatible.

## 3. Instalación y ciclo de vida

- [ ] Instalar como usuario estándar cuando sea compatible.
- [ ] Verificar selección de carpeta y accesos directos.
- [ ] Verificar primer inicio y onboarding.
- [ ] Verificar actualización sobre una versión anterior.
- [ ] Verificar desinstalación y limpieza de archivos.
- [ ] Verificar inicio automático después de reiniciar Windows.
- [ ] Verificar el comportamiento cuando Home Assistant no está disponible al iniciar la aplicación.

## 4. Integración con Home Assistant

- [ ] Verificar descubrimiento mediante mDNS y entrada manual de URL.
- [ ] Verificar inicio de sesión OAuth2, renovación de token y cierre de sesión.
- [ ] Verificar registro del dispositivo mediante `mobile_app`.
- [ ] Verificar registro del webhook y actualización de sensores.
- [ ] Verificar sincronización del estado de sensores habilitados/deshabilitados.
- [ ] Verificar selección de URL interna/externa y cambio según SSID.
- [ ] Verificar múltiples instancias configuradas de Home Assistant.

## 5. Notificaciones y comandos remotos

- [ ] Verificar reconexión WebSocket después de perder la red y reiniciar Home Assistant.
- [ ] Verificar recepción de notificaciones mientras la aplicación está conectada.
- [ ] Verificar confirmación de notificaciones cuando el canal lo admita.
- [ ] Verificar botones de acción y el evento `mobile_app_notification_action`.
- [ ] Verificar individualmente cada comando remoto documentado.
- [ ] Verificar que los comandos remotos permanezcan deshabilitados salvo consentimiento explícito del usuario.
- [ ] Verificar que la interfaz comunique que las notificaciones locales por WebSocket requieren una conexión activa.

## 6. Seguridad y privacidad

- [ ] Inspeccionar cómo se almacenan los tokens OAuth de acceso y renovación.
- [ ] Inspeccionar cómo se almacena y protege el webhook ID; tratarlo como una credencial.
- [ ] Verificar permisos NTFS del directorio de configuración.
- [ ] Verificar que secretos se redacten en logs, informes de fallos y diagnósticos.
- [ ] Verificar que la validación de certificados TLS sea estricta por defecto.
- [ ] Verificar que las excepciones de certificados se limiten al servidor previsto.
- [ ] Verificar que los comandos remotos del sistema requieran consentimiento explícito.
- [ ] Verificar que los sensores que exponen actividad del usuario puedan deshabilitarse individualmente.
- [ ] Documentar si los secretos locales están cifrados en reposo; no afirmar cifrado sin evidencia.

## 7. Publicación de la release

- [ ] Recalcular SHA-256 del artefacto final publicado.
- [ ] Confirmar que el hash publicado coincide con el archivo descargado.
- [ ] Confirmar nombre, versión y arquitectura del instalador.
- [ ] Incluir resultados de la matriz de pruebas y limitaciones conocidas en las notas de versión.
- [ ] Confirmar que los enlaces de descarga del README apunten a la release prevista.
- [ ] Conservar una copia del instalador exacto y de la evidencia de pruebas para soporte y rollback.

## Registro de evidencia

Para consultar la evidencia de build y las pruebas de campo de v1.0.0, ver [Verificación del build y validación en equipos reales](BUILD_AND_VALIDATION.md). Mantené las comprobaciones de build, las observaciones breves de arranque y las pruebas de integración en equipos reales como categorías de evidencia separadas.

Para cada release, conservá:

- Versión y etiqueta de release.
- Revisión de código fuente/build (o declaración explícita de que el código fuente no se publica).
- Fecha y entorno de compilación.
- SHA-256 y tamaño del artefacto.
- Matriz de sistemas operativos y resultados.
- Comprobaciones de seguridad realizadas y excepciones.
- Problemas conocidos e instrucciones de rollback.

### Validación de campo actual — 2026-11-23

- Windows 11 x64 (PC): Companion instalado y acceso a la instancia configurada de Home Assistant confirmado.
- Windows 8 x86 (tablet): Companion instalado y acceso a la instancia configurada de Home Assistant confirmado.
- Alcance de la evidencia: pruebas de humo en equipos reales reportadas por el mantenedor. Las compilaciones exactas de Windows, el hash verificado en cada equipo, los resultados por funcionalidad y los artefactos reproducibles todavía no se adjuntaron al repositorio.

### Verificación de build — v1.0.0

- [x] Se reportó que la comprobación de sintaxis JavaScript pasó para los archivos revisados de `src/` (`node --check`).
- [x] Se reportó que `npm run package:x86` finalizó con código de salida 0.
- [x] Se reportó arquitectura x86 en las cabeceras PE de la aplicación y el puente nativo.
- [x] Se reportó una respuesta JSON de telemetría al ejecutar `ha-sys-bridge.exe sensors`.
- [x] Se reportó que la aplicación empaquetada permaneció activa durante una observación de arranque de 3 segundos.
- [ ] Reproducir el build de forma independiente y adjuntar logs originales, versiones exactas de herramientas y revisión de código a un registro privado de procedencia.
- [ ] Ejecutar pruebas de integración detalladas contra Home Assistant y conservar evidencia redactada.

Los puntos marcados como completados corresponden a resultados reportados en la sesión de build del mantenedor; no fueron reproducidos de forma independiente por esta actualización documental. Consultá [Verificación del build y validación en equipos reales](BUILD_AND_VALIDATION.md) para conocer la interpretación y sus límites.
