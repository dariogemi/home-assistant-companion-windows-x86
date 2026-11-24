# Home Assistant Companion para Windows (x86)

Cliente de escritorio de 32 bits para integrar equipos Windows con Home Assistant mediante la integración oficial `mobile_app`.

El proyecto apunta a recuperar PCs y tablets x86 que todavía pueden cumplir una función útil como paneles y terminales de automatización. La aplicación incorpora un motor Chromium y un puente nativo .NET Framework 4.0, sin requerir una instalación externa de WebView2.

> **Distribución:** este repositorio publica documentación e instaladores compilados en [GitHub Releases](https://github.com/dariogemi/home-assistant-companion-windows-x86/releases). No publica el código fuente.
>
> **Proyecto independiente:** no es un producto oficial de Home Assistant ni está afiliado, respaldado o mantenido por Nabu Casa. Utiliza la integración oficial `mobile_app` de Home Assistant.

## Descarga

Descarga la versión estable desde [Releases](https://github.com/dariogemi/home-assistant-companion-windows-x86/releases/latest).

- Instalador: `Home-Assistant-Companion-Setup-1.0.0-win-x86.exe`
- Arquitectura del instalador: x86 (32 bits).
- Tamaño publicado: aproximadamente 62 MB.
- SHA-256 publicado: `44655171B66C6AF01D5F9D889D26839B61DEFA28F5753E62915ABE0E3FE582EC`

Verifica el hash antes de ejecutar el instalador:

```powershell
Get-FileHash .\Home-Assistant-Companion-Setup-1.0.0-win-x86.exe -Algorithm SHA256
```

El valor calculado debe coincidir exactamente con el SHA-256 publicado en la release.

## Compatibilidad y validación

El instalador es x86. En Windows de 64 bits se ejecuta como aplicación de 32 bits mediante la capa de compatibilidad del sistema operativo.

| Sistema operativo anfitrión | Arquitectura del SO | Resultado observado |
| --- | --- | --- |
| Windows 8 | x86 (32 bits) | Instalado; acceso a la instancia Home Assistant confirmado en tablet |
| Windows 11 | x64 (64 bits) | Instalado; acceso a la instancia Home Assistant confirmado en PC |
| Windows 8.1 | x86 / x64 | Pendiente de prueba documentada |
| Windows 8.1 | x64 | Pendiente de prueba documentada |
| Windows 10 | x86 / x64 | Pendiente de prueba documentada |

**Alcance de la evidencia:** las dos validaciones confirmadas corresponden a instalaciones reales en el entorno del mantenedor y acreditan instalación y conectividad con Home Assistant. No constituyen por sí solas una certificación completa de todos los sensores, notificaciones, comandos, ciclos de actualización ni de todas las ediciones de Windows. Los demás escenarios deben probarse y registrarse por separado.

**Importante:** Windows 11 no tiene una edición x86 de 32 bits. La aplicación puede ser x86; el sistema operativo anfitrión debe ser x64.

### Requisitos

- Home Assistant Core con la integración `mobile_app` habilitada.
- Conectividad IP entre el equipo Windows y la instancia de Home Assistant.
- No requiere una instalación externa de WebView2.
- El instalador incluye el runtime y el puente nativo x86 declarados por el proyecto.

## Funcionalidades

- **Onboarding:** descubrimiento mDNS, URL manual y autenticación OAuth2.
- **Integración frontend:** puente `window.externalApp` / `externalBus`, menú de aplicación complementaria, actualización de sesión y sincronización de tema.
- **Múltiples servidores:** selección de instancia y conmutación entre URL interna y externa según SSID Wi-Fi.
- **Sensores del PC:** 15 entidades/sensores declarados para CPU, memoria, disco, batería, ventana activa, inactividad, audio, Wi-Fi, IP, monitores, cámara, micrófono y uptime. La disponibilidad de algunos sensores depende de Windows y del hardware.
- **Notificaciones locales:** recepción mediante el canal WebSocket de `mobile_app`, con botones de acción.
- **Comandos remotos opt-in:** bloqueo, suspensión, apagado, reinicio, pantalla, volumen, multimedia, TTS y navegación.
- **Acceso rápido:** bandeja del sistema, atajos globales, Acciones Rápidas y Home Assistant Assist.

Las funciones de notificación local dependen de que la aplicación mantenga una conexión WebSocket autenticada y activa con Home Assistant. No equivalen a una entrega push garantizada cuando la aplicación está cerrada o desconectada.

## Documentación

- [Guía de usuario y configuración](docs/USER_GUIDE.md)
- [Sensores, notificaciones y comandos](docs/SENSORS_AND_COMMANDS.md)
- [Seguridad, privacidad y troubleshooting](docs/SECURITY_AND_PRIVACY.md)
- [Checklist de calidad para releases](docs/RELEASE_CHECKLIST.md)
- [Historial de cambios](CHANGELOG.md)

## Seguridad y privacidad

La aplicación interactúa con la instancia Home Assistant configurada por el usuario. La documentación describe el almacenamiento local de credenciales y la posibilidad de habilitar certificados autofirmados por servidor.

El archivo de configuración debe tratarse como información sensible: puede contener tokens OAuth2 y el identificador de webhook, que Home Assistant utiliza como credencial para las llamadas webhook. No compartas archivos de configuración ni diagnósticos sin revisar y redactar secretos.

Antes de utilizar comandos remotos, revisá la opción de habilitación y limitá quién puede invocar las automatizaciones correspondientes. Más detalles en [Seguridad y privacidad](docs/SECURITY_AND_PRIVACY.md).

## Estado del proyecto

- Versión publicada: **1.0.0**
- Canal: instalador binario y documentación.
- Código fuente: no publicado en este repositorio.
- Validación práctica documentada: Windows 11 x64 y Windows 8 x86, con acceso a Home Assistant.
- Incidencias y sugerencias: [GitHub Issues](https://github.com/dariogemi/home-assistant-companion-windows-x86/issues).
