# Home Assistant Companion para Windows (x86 / Windows 8+)

Cliente de escritorio **32-bit (x86)** para **Windows 8, 8.1, 10 y 11** que ofrece la experiencia completa de **Home Assistant Companion**, integrando tu equipo Windows como dispositivo oficial `mobile_app` en Home Assistant.

> **Nota de distribución:** Este repositorio público contiene exclusivamente la documentación oficial y el **instalador binario compilado** en [GitHub Releases](https://github.com/dariogemi/home-assistant-companion-windows-x86/releases/latest). No incluye código fuente.

---

## Descarga e Instalación

1. Descarga el instalador desde la última versión publicada:
   - **[Home-Assistant-Companion-Setup-1.0.0-win-x86.exe](https://github.com/dariogemi/home-assistant-companion-windows-x86/releases/latest)** *(Instalador Windows NSIS 32-bit)*
2. Ejecuta el instalador y sigue el asistente (permite elegir carpeta de instalación, acceso directo en Escritorio y Menú Inicio).
3. Abre **Home Assistant Companion** y completa el asistente inicial (*Onboarding*).

### Requisitos del sistema

| Requisito | Detalle |
| --- | --- |
| **Sistema operativo** | Windows 8, Windows 8.1, Windows 10 o Windows 11 (ediciones 32-bit x86 y 64-bit x64) |
| **Arquitectura** | `x86` (32-bit) con motor Chromium 108 integrado (sin WebView2 externo) |
| **Home Assistant** | Home Assistant Core 2023.x o superior con integración `mobile_app` activa |
| **Dependencias adicionales** | Ninguna (incluye su propio runtime y puente nativo .NET 4.0 x86 integrado) |

---

## Características principales

- **Onboarding oficial:** Descubrimiento automático en red local mediante mDNS (`_home-assistant._tcp.local.`), entrada manual de URL, soporte opcional para certificados SSL autofirmados en LAN y autenticación OAuth2 segura.
- **Integración nativa con la UI de Home Assistant:** Implementa el puente `window.externalApp` / `externalBus`, habilitando automáticamente el menú **Configuración → Aplicación complementaria** dentro de Home Assistant, refresco automático de sesión y sincronización de tema claro/oscuro.
- **Múltiples servidores y cambio inteligente de red:** Alterna entre varias instancias de Home Assistant y conmuta automáticamente entre URL interna y externa según el **SSID Wi-Fi** conectado.
- **15 sensores del PC en tiempo real:** CPU, Memoria RAM, Disco principal, Batería y carga, Ventana y proceso activo, Tiempo inactivo, Volumen y silencio de audio, SSID Wi-Fi, IP local, Monitores conectados, Micrófono/Cámara en uso y Tiempo encendido.
- **Notificaciones Push WebSocket en tiempo real:** Recibe avisos de `notify.mobile_app_*` sin depender de servicios en la nube de terceros, con soporte para **botones de acción** (`mobile_app_notification_action`).
- **Comandos remotos del sistema:** Bloquear sesión, suspender, apagar, reiniciar, apagar/encender pantalla, ajustar volumen, control multimedia, Text-to-Speech (TTS) y navegación remota.
- **Acciones Rápidas, Assist y Bandeja del Sistema:** Atajos globales (`Ctrl+Shift+H`, `Ctrl+Shift+Q`, `Ctrl+Shift+A`), ejecución de entidades fijadas desde la bandeja de Windows y chat directo con **Home Assistant Assist**.

---

## Documentación

- [Guía de Usuario y Configuración](docs/USER_GUIDE.md)
- [Referencia de Sensores, Notificaciones y Comandos Remotos](docs/SENSORS_AND_COMMANDS.md)
- [Seguridad, Privacidad y Solución de Problemas](docs/SECURITY_AND_PRIVACY.md)
- [Historial de Cambios](CHANGELOG.md)

---

## Verificación de integridad (SHA-256)

| Archivo | Tamaño | SHA-256 |
| --- | --- | --- |
| `Home-Assistant-Companion-Setup-1.0.0-win-x86.exe` | 64,973,268 bytes (~61.96 MB) | `44655171B66C6AF01D5F9D889D26839B61DEFA28F5753E62915ABE0E3FE582EC` |

```powershell
Get-FileHash .\Home-Assistant-Companion-Setup-1.0.0-win-x86.exe -Algorithm SHA256
```
