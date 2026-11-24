# Guía de Usuario y Configuración

## 1. Primer inicio (Onboarding)

1. Al abrir **Home Assistant Companion** por primera vez verás la pantalla de bienvenida. Haz clic en **Continuar**.
2. La aplicación escaneará tu red local usando mDNS (`_home-assistant._tcp.local.`) y direcciones locales estándar (`homeassistant.local:8123`).
3. Si tu servidor no aparece o usas una dirección personalizada:
   - Haz clic en **Introducir dirección manualmente**.
   - Ingresa tu URL interna (por ejemplo, `http://homeassistant.local:8123` o `https://homeassistant.example:8123`).
   - Opcionalmente agrega una **URL Externa** para acceder fuera de tu red local.
   - Si usas HTTPS con certificado autofirmado en red local, marca **Permitir certificados SSL autofirmados en red local**.
4. Inicia sesión con tu usuario de Home Assistant en la ventana OAuth2.
5. Elige si deseas activar **Sensores del PC**, **Notificaciones Push** e **Inicio automático con Windows**, y pulsa **Finalizar y abrir panel**.

---

## 2. Acceso a la configuración de Companion

Puedes abrir el panel nativo **Configuración de la aplicación Companion** de tres maneras:

1. Dentro de la propia interfaz de Home Assistant: **Configuración → Aplicación complementaria**.
2. Desde el icono de engranaje en la barra superior de la ventana Companion.
3. Haciendo clic derecho en el icono de Home Assistant en la **bandeja del sistema** → **Configuración de Companion**.

---

## 3. Múltiples servidores y cambio por Wi-Fi (SSID)

En **Servidores** puedes añadir tantas instancias como necesites (por ejemplo, *Casa* y *Oficina*):

- Configura **URL Interna** y **URL Externa**.
- En **SSIDs Wi-Fi de Casa**, escribe los nombres de tus redes Wi-Fi locales separados por comas (ej: `MiRedCasa, MiRedCasa_5G`).
- Cuando el equipo detecta que está conectado a alguno de esos SSIDs, priorizará automáticamente la **URL Interna**; fuera de esas redes cambiará a la **URL Externa**.

---

## 4. Acciones Rápidas, Assist y Atajos Globales

| Atajo predeterminado | Acción |
| --- | --- |
| `Ctrl + Shift + H` | Mostrar u ocultar la ventana principal de Home Assistant |
| `Ctrl + Shift + Q` | Abrir el panel flotante de **Acciones Rápidas** |
| `Ctrl + Shift + A` | Abrir el diálogo rápido de **Assist** |

Todos los atajos se pueden personalizar o desactivar desde **General y Apariencia → Atajos de Teclado Globales**.

Para fijar entidades en **Acciones Rápidas** (y en el submenú de la bandeja del sistema):
1. Abre **Configuración de Companion → Acciones Rápidas / Widgets**.
2. Pulsa **Cargar entidades de HA**.
3. Activa las luces, interruptores, scripts, escenas o cerraduras que quieras tener siempre a mano.
