# Historial de Cambios

## [Unreleased]

### Documentación y calidad
- README renovado con alcance del producto, compatibilidad de arquitectura y límites de notificaciones locales.
- Aclaración de que el proyecto es independiente y utiliza la integración oficial `mobile_app`.
- Guía de seguridad ampliada para proteger tokens OAuth2 y el identificador de webhook.
- Nuevo checklist de calidad para pruebas de compatibilidad, instalación, integración, seguridad y publicación.
- Registro de validación práctica en Windows 11 x64 (PC) y Windows 8 x86 (tablet), ambos instalados y conectados a Home Assistant; alcance limitado a smoke test.
- Nueva documentación de verificación del build: `node --check`, `npm run package:x86`, cabeceras PE x86, respuesta JSON del puente de sensores y observación de arranque de 3 segundos, con límites de evidencia explícitos.
- Separación entre resultados reportados del proceso de build, validaciones reales de campo y pruebas de integración aún pendientes.

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
