# Historial de Cambios

## [1.0.0] - 2026-11-19

### Novedades
- Primer lanzamiento oficial de **Home Assistant Companion para Windows (x86 / Windows 8+)**.
- Instalador NSIS 32-bit (`Home-Assistant-Companion-Setup-1.0.0-win-x86.exe`) compatible con Windows 8, 8.1, 10 y 11.
- Asistente de Onboarding con descubrimiento mDNS y flujo OAuth2 embebido.
- Integración nativa con el frontend mediante `window.externalApp` (`externalBus`, `config_screen/show`, `assist/show`, `theme-update`).
- Soporte para múltiples servidores y conmutación automática URL interna/externa por SSID Wi-Fi.
- 15 sensores del sistema Windows en tiempo real mediante puente nativo x86 (.NET 4.0).
- Canal WebSocket para notificaciones push, notificaciones con botones de acción y comandos remotos del PC.
- Panel flotante de Acciones Rápidas (`Ctrl+Shift+Q`), Assist (`Ctrl+Shift+A`) y menú completo en la bandeja del sistema.
