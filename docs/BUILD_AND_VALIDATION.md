# Verificación del build y validación en equipos reales

Este documento registra las comprobaciones de construcción y las pruebas de humo realizadas en equipos reales que fueron reportadas para la versión v1.0.0 de Windows x86. Separa las verificaciones de compilación de las pruebas de ejecución e integración, e identifica qué evidencia no fue reproducida de forma independiente.

## 1. Construcción y empaquetado

La sesión de build proporcionada por el mantenedor en Antigravity reportó los siguientes resultados:

| Comprobación | Resultado reportado | Qué demuestra |
| --- | --- | --- |
| Sintaxis JavaScript: `node --check` sobre archivos de `src/` | Sin errores | Los archivos JavaScript revisados pudieron analizarse sintácticamente |
| `npm run package:x86` | Código de salida 0 | Finalizó la tarea de empaquetado x86 |
| Salida portable | `dist/Home Assistant Companion-win32-ia32/Home Assistant Companion.exe` | Se generó una carpeta portable para x86 |
| Prueba del puente nativo: `ha-sys-bridge.exe sensors` | Devolvió un payload JSON de telemetría | El puente arrancó y devolvió datos de sensores en el equipo de build |
| Arranque de la aplicación empaquetada | El proceso permaneció activo durante una observación de 3 segundos | El ejecutable inició; se trata únicamente de una prueba breve de arranque |

La transcripción del build reportó los siguientes valores de cabecera PE:

| Ejecutable | Arquitectura | MinOS reportado | Subsystem reportado |
| --- | --- | --- | --- |
| `Home Assistant Companion.exe` | x86 (32 bits) | 5.1 | 5.1 |
| `ha-sys-bridge.exe` | x86 (32 bits) | 4.0 | 4.0 |

La sesión de generación del instalador también reportó un instalador NSIS x86 construido con Electron Builder, incluyendo `ha-sys-bridge.exe` y `ha-icon.png` en el directorio de recursos desempaquetados, selector de carpeta de instalación, accesos directos al Menú Inicio y Escritorio, y desinstalador.

### Interpretación y límites

- Estos resultados se transcriben de la sesión de build proporcionada por el mantenedor; esta actualización documental no los volvió a ejecutar de forma independiente.
- Una comprobación de sintaxis o una tarea de empaquetado exitosa no demuestra que todas las funciones se comporten correctamente en ejecución.
- La arquitectura PE y los campos de versión son evidencia técnica útil, pero no prueban compatibilidad con todas las versiones de Windows, controladores, dispositivos o combinaciones de dependencias.
- Una observación de tres segundos demuestra únicamente que el proceso inició y permaneció presente durante ese intervalo.
- La prueba del puente demuestra que devolvió telemetría en el equipo de build; no prueba que todos los sensores funcionen en todas las versiones de Windows.
- La sesión original indicó que OAuth2 y la conexión WebSocket con Home Assistant todavía debían probarse contra un servidor real en esa etapa. La validación posterior en equipos reales se registra por separado a continuación.

## 2. Artefacto del instalador

El registro de la release v1.0.0 identifica:

- Archivo: `Home-Assistant-Companion-Setup-1.0.0-win-x86.exe`
- Arquitectura objetivo: x86 (32 bits)
- Empaquetado: instalador NSIS x86
- Tamaño: 64.973.268 bytes (aproximadamente 61,96 MiB)
- SHA-256 publicado: `44655171B66C6AF01D5F9D889D26839B61DEFA28F5753E62915ABE0E3FE582EC`

Verificá el instalador descargado antes de ejecutarlo:

```powershell
Get-FileHash .\Home-Assistant-Companion-Setup-1.0.0-win-x86.exe -Algorithm SHA256
```

El hash calculado localmente debe coincidir con el publicado en GitHub Releases. Este repositorio no afirma que el instalador tenga firma Authenticode; si se publica una versión firmada, verificá su firma de manera independiente.

## 3. Validación en equipos reales

El mantenedor confirmó que el Companion publicado fue instalado en los siguientes equipos y accedió a la instancia de Home Assistant configurada:

| Equipo | Sistema operativo | Arquitectura del SO | Resultado |
| --- | --- | --- | --- |
| PC | Windows 11 | x64 (64 bits) | Instalado; acceso a Home Assistant confirmado |
| Tablet | Windows 8 | x86 (32 bits) | Instalado; acceso a Home Assistant confirmado |

Estas pruebas de humo en equipos reales fortalecen la evidencia de compatibilidad. Confirman la instalación y el acceso a Home Assistant en esos dos entornos específicos.

Por sí solas no demuestran que se hayan probado todos los sensores, acciones de notificación, comandos remotos, escenarios de reconexión, actualización o desinstalación en ambos equipos. Los números exactos de compilación de Windows y los registros de pruebas por funcionalidad todavía no se adjuntaron a este repositorio público.

## 4. Matriz de compatibilidad

| Sistema operativo anfitrión | Arquitectura | Estado de evidencia |
| --- | --- | --- |
| Windows 8 | x86 | Prueba real: instalación y acceso a Home Assistant confirmados en una tablet |
| Windows 11 | x64 | Prueba real: instalación y acceso a Home Assistant confirmados en un PC |
| Windows 8 | x64 | Todavía no documentado |
| Windows 8.1 | x86 / x64 | Todavía no documentado |
| Windows 10 | x86 / x64 | Todavía no documentado |

Windows 11 se ofrece como sistema anfitrión de 64 bits; la aplicación x86 se ejecuta como proceso de 32 bits sobre ese sistema. No describir Windows 11 como un sistema operativo de 32 bits.

## 5. Alcance de las pruebas de integración con Home Assistant

La sesión original de build dejó explícitamente para una etapa posterior la validación de OAuth2 y WebSocket contra una instancia real de Home Assistant. Las pruebas de campo posteriores confirmaron el acceso a Home Assistant desde la tablet Windows 8 x86 y el PC Windows 11 x64.

Las siguientes pruebas detalladas permanecen como tareas separadas hasta que se ejecuten y documenten individualmente:

- Inicio de sesión OAuth2, renovación y cierre de sesión.
- Registro del dispositivo y webhook mediante `mobile_app`.
- Frecuencia de actualización de sensores y comportamiento al habilitarlos o deshabilitarlos.
- Entrega de notificaciones mientras WebSocket está conectado.
- Procesamiento de eventos de botones de acción.
- Reconexión WebSocket después de perder la red o reiniciar Home Assistant.
- Cada uno de los comandos remotos documentados.
- Cambio entre URL interna/externa y comportamiento según SSID.

## 6. Procedencia y reproducibilidad del build

El repositorio público distribuye intencionalmente documentación e instaladores compilados, no el código fuente de la aplicación. Por lo tanto:

- El árbol de código fuente y la revisión exacta utilizada para v1.0.0 no están disponibles en este repositorio.
- La ruta local del espacio de trabajo de build no se publica.
- Los resultados anteriores son evidencia reportada por el mantenedor, no una compilación reproducible mediante CI público.
- No se afirma que exista un workflow público de CI que reconstruya el artefacto publicado.
- Para futuras versiones se recomienda conservar un registro privado de build con revisión del código fuente, versiones de herramientas, archivo de dependencias, logs, hash del artefacto y evidencia de pruebas.

Esta distinción mantiene el límite de distribución del código fuente y, al mismo tiempo, deja transparente la evidencia disponible.
