# LIVEPS4 · Guía de uso

LIVEPS4 te permite ver y jugar tu PS4 desde tu Android por Remote Play. Para transmitir, usas la captura de pantalla de **TikTok** o **YouTube**, y LIVEPS4 muestra el juego en pantalla.

---

## 1. Requisitos

- Android 10 o superior.
- PS4 con exploit **GoldHEN** y **Apollo Save Tool** instalado.
- PS4 y teléfono en **la misma red Wi-Fi**.
- Un **perfil secundario** en la PS4, que usarás solo para Remote Play.
- Tu **cuenta principal** de PS4, con tus partidas guardadas.
- El archivo **`libchiaki-jni.so`** (el motor de Remote Play). Ver la sección 2.
- La app de **TikTok** o **YouTube** instalada.
- *(Opcional)* La app **PrismLive**, si quieres transmitir a varias plataformas a la vez. Ver la sección 6.

---

## 2. Motor libchiaki-jni.so (obligatorio)

LIVEPS4 necesita el archivo `libchiaki-jni.so` para conectarse a la PS4. No viene dentro de la app.

**Primera vez:**

1. Guarda `libchiaki-jni.so` en **Descargas**. No lo cambies de nombre.
2. Abre LIVEPS4. Si no encuentra el motor, muestra **"Selecciona libchiaki-jni.so en Descargas"** y abre el selector.
3. Elige el archivo desde **Descargas**.
4. Debe aparecer **Motor PS4LIVE ACTIVO** en la pantalla principal.

**Cuándo vuelve a pedirlo:** después de desinstalar la app o borrar sus datos.

**Si no funciona:** "Motor PS4LIVE con error" o "Chiaki FAIL" significa que el archivo no es el correcto o está incompleto. Vuelve a elegirlo.

---

## 3. Preparar la PS4 (una sola vez)

### 3.1 Activar el perfil secundario sin conexión a PSN

1. Abre **Apollo Save Tool** en la PS4.
2. Ve a **User Tools → Activate PS4 Accounts**.
3. Selecciona tu perfil secundario y presiona **R2** para generar un ID aleatorio. Confirma.
4. Reinicia la PS4 y vuelve a cargar **GoldHEN**.
5. Si al iniciar sesión el nombre del perfil aparece en cursiva, la activación funcionó.

### 3.2 Obtener el Account ID

1. Entra a **Apollo Save Tool → User Tools → Activate PS4 Accounts**.
2. Anota el **Account ID** de 16 caracteres que aparece en **Console details**. Es **hexadecimal** (ejemplo: `0000000000000001`).

**No confundas los campos:** el **User ID** (8 caracteres) y el **Console PSID** (32 caracteres) **no** son el Account ID.

**Sobre los valores de todos unos (`1111111111111111`):** no identifican al perfil de forma confiable. Usa el Account ID del perfil que activaste con R2.

### 3.3 Activar Remote Play y generar el PIN

1. En la PS4: **Ajustes → Ajustes de conexión de Remote Play → Agregar dispositivo**.
2. La PS4 genera un **PIN de 8 dígitos**. Es temporal: pídelo justo antes de emparejar.

### 3.4 Consultar la IP de la PS4

Con el perfil secundario: **Ajustes → Red → Ver estado de la conexión**. Anota la **Dirección IP** (ejemplo: `192.168.1.50`).

---

## 4. Configurar LIVEPS4 (una sola vez)

| Campo | Qué poner |
|---|---|
| **IP de la PS4** | La Dirección IP del paso 3.4. |
| **PIN de registro (8 dígitos)** | El PIN del paso 3.3. |
| **PSN Account ID (Base64)** | El Account ID convertido a Base64 de 8 bytes (ver abajo). |
| **PSN Online ID (opcional)** | Tu nombre de usuario de PSN. |

### Convertir el Account ID a Base64

LIVEPS4 solo acepta el valor si al decodificarlo da **exactamente 8 bytes**. Si no, lo ignora y el registro muestra `account_id=MISSING`.

Como el Account ID es hexadecimal, en una terminal con Python:

```python
import base64
h = "0000000000000001"   # tu Account ID de 16 caracteres de Apollo
print(base64.b64encode(bytes.fromhex(h)).decode())
```

Si ya te funciona el valor que tienes, **no lo cambies**. Usa el script solo si sale `MISSING` o el emparejamiento falla.

---

## 5. Emparejar y conectar

1. Con el perfil secundario iniciado en la PS4, llena la IP, el PIN, el Account ID y el Online ID en LIVEPS4.
2. Toca **Emparejar**.
3. Espera a ver **Emparejado ✔ llaves guardadas**. No vuelves a hacerlo, salvo que desvincules la consola.
4. Toca **INICIAR REMOTE PLAY** para ver la PS4 en tu teléfono.

Si aparece **PS4 no respondió**, **Conexión rechazada** o **Tiempo agotado**, revisa que la PS4 esté encendida, la IP, la red Wi-Fi y que el PIN no haya expirado.

---

## 6. Transmitir con TikTok o YouTube

LIVEPS4 no transmite por sí mismo. Lo que transmites es lo que se ve en tu pantalla, y la app de TikTok o YouTube hace la captura.

### TikTok

1. Conecta la PS4 en LIVEPS4 antes de empezar, para tener la imagen lista.
2. En TikTok toca **+** en la parte inferior y desliza hasta **LIVE**.
3. Elige **Juegos móviles** (Mobile Gaming) y la orientación **Horizontal**.
4. Escribe el título, elige la categoría y la calidad.
5. Toca **Emitir LIVE** y acepta el permiso de captura con **Comenzar ahora**.
6. Durante la cuenta regresiva, regresa a LIVEPS4 y toca **INICIAR REMOTE PLAY** si aún no lo hiciste.
7. Gira el teléfono a horizontal y deja LIVEPS4 en primer plano durante todo el directo.

### YouTube

1. Conecta la PS4 en LIVEPS4 para que el juego se vea.
2. Toca **DIRECTO → MODO YOUTUBE APP**. La app te explica los pasos.
3. En YouTube: **+ → En vivo → Screencast**.
4. Acepta el permiso de captura e inicia la transmisión en YouTube.
5. Regresa a LIVEPS4 con el botón de recientes y juega.

### PrismLive (multistream: varias plataformas a la vez)

PrismLive es una app aparte que puede transmitir a **YouTube y otras plataformas al mismo tiempo**. LIVEPS4 no la controla: solo muestra el juego, y PrismLive hace la captura y la transmisión.

1. Abre PrismLive e inicia sesión con tu cuenta de Google. Así vinculas YouTube sin copiar claves.
2. En la pestaña **LIVE**, elige **Pantalla** y toca **Ready**.
3. Toca **Go Live** y acepta el permiso de captura con **Comenzar ahora**.
4. Durante la cuenta regresiva, cambia a LIVEPS4, conecta la PS4 y deja el juego en pantalla.
5. Gira el teléfono a horizontal. El directo sigue en PrismLive, que muestra el estado y el audio.

**Audio en PrismLive:** tiene su propio **Mezclador de audio**, con un deslizador para el audio de la aplicación (el juego) y otro para el micrófono. Ajusta ahí el volumen de cada fuente.

**Plataformas:** PrismLive transmite a varias plataformas a la vez, pero **TikTok no está disponible** en PrismLive para cuentas normales. Para TikTok sigue usando la captura de pantalla de TikTok, como se describe arriba.

**Costos:** PrismLive tiene una suscripción después de la prueba gratuita. Revisa el precio y las condiciones dentro de la app antes de activarla.

**Importante:** lo que se ve en pantalla es lo que se transmite. Si cambias de app o abres la pantalla de inicio, tu audiencia también lo verá.

**Audio:** el sonido de la consola se mantiene en tu teléfono mientras la transmisión graba tu voz. El audio lo maneja TikTok o YouTube, no LIVEPS4.

---

## 7. Modo limpio (ocultar lo que no quieres que se vea)

- Toca **LIMPIO** en la barra superior.
- Se ocultan los controles de la app y las barras del sistema (hora, batería, señal y barra de navegación).
- Para volver a mostrar los controles, toca el botón **⚙️** (abajo a la derecha). Tocar otras partes de la pantalla **no** los muestra, para evitar que aparezcan por error en el directo.

La zona de la cámara frontal también se dibuja, así no queda una franja negra en la captura.

---

## 8. Botones de la barra superior

| Botón | Qué hace |
|---|---|
| **CAM** | Muestra u oculta la cámara sobre el juego. |
| **LIMPIO** | Oculta controles y barras del sistema. |
| **DIRECTO** | Abre el panel con **MODO YOUTUBE APP**. |
| **SALIR** | Cierra la sesión con la PS4 y regresa a la pantalla principal. |

---

## 9. Ajustes

Toca el engranaje en la pantalla principal.

**Video del Remote Play**

- **Resolución:** 360p, 540p, 720p o 1080p. Por defecto 720p.
- **FPS:** 30 o 60. Afecta lo que ves en tu teléfono.
- **Bitrate del juego:** calidad del video de Remote Play. Más alto = mejor imagen, pero necesita mejor red.
- **Codec H.265:** solo para PS5.

**Sesión y estabilidad**

- **Reconectar automáticamente al caer.**
- **Mantener pantalla encendida.**
- **Log detallado (diagnóstico).** Actívalo solo si necesitas revisar un problema.
- **Vibración (rumble) en el teléfono.**

**Controles táctiles**

- **Intercambiar ✕/○ y □/△ (mandos 8BitDo).**

**Datos y diagnóstico**

- **Exportar ajustes (JSON):** guarda tus ajustes en Descargas.
- **Generar reporte de diagnóstico:** crea un archivo `.txt` en Descargas con datos del teléfono, configuración y errores recientes. No incluye claves.
- **Restablecer ajustes:** vuelve a los valores de fábrica. Tus consolas no se borran.
- **Créditos y licencia:** ver la sección 12.

**Nota:** la sección **DIRECTO** de Ajustes (bitrate del directo, 720p y 1080p del directo, y 60 fps del directo) ya no se usa con el flujo de TikTok y YouTube. No afecta la transmisión.

---

## 10. Batería y cierres

- Un consumo alto es normal mientras transmites: LIVEPS4 decodifica el video de la PS4 y tu teléfono transmite por la app de TikTok o YouTube. Con 1080p a 60 fps el consumo es mayor.
- **No pongas LIVEPS4 en reposo** mientras transmites. Lo que sí conviene es que el sistema no la limite:
  1. Ve a **Ajustes → Aplicaciones → LIVEPS4 → Batería**.
  2. Elige **Sin restricciones**.
  3. Si tu teléfono tiene un administrador de batería o cuidado del dispositivo, agrega LIVEPS4 a las apps que no se cierran.
- Si LIVEPS4 se cierra sola, revisa si el reporte de diagnóstico tiene una línea `CRASH`. Ese es el error exacto.

---

## 11. Si algo falla

| Mensaje o problema | Qué hacer |
|---|---|
| **Motor PS4LIVE con error / Chiaki FAIL** | Vuelve a elegir `libchiaki-jni.so` desde Descargas. |
| **Sin motor** | Elige el archivo desde Descargas. |
| **`account_id=MISSING`** | Revisa el Account ID con el script de la sección 4. |
| **PS4 no respondió / Conexión rechazada / Tiempo agotado** | Revisa que la PS4 esté encendida, la IP, la red Wi-Fi y que el PIN no haya expirado. Genera un PIN nuevo. |
| **Sin permiso de captura** | Vuelve a iniciar en TikTok o YouTube y acepta el permiso. |
| **El directo se congela** | Baja el bitrate del juego en Ajustes. |
| **Barras negras arriba y abajo en el TV** | Es la proporción de tu pantalla (20:9) frente al 16:9 de la captura. Ver la sección 13. |

**Diagnóstico:** si nada funciona, en **Ajustes → Datos y diagnóstico** toca **Generar reporte de diagnóstico** y mándalo al desarrollador.

---

## 12. Licencia y créditos

- El motor `libchiaki-jni.so` está basado en **Chiaki** y se distribuye bajo la licencia **AGPL-3.0**.
- La app se distribuye junto con el código fuente. Si la compartes o la publicas, revisa esa licencia.
- Para ver los créditos dentro de la app: **Ajustes → Datos y diagnóstico → Créditos y licencia**.
- No subas `libchiaki-jni.so` a repositorios públicos sin revisar la licencia.

---

## 13. Limitaciones conocidas

- La captura la hace TikTok o YouTube. LIVEPS4 no controla la proporción ni el audio de esas apps.
- Las barras negras arriba y abajo en el modo horizontal vienen de la proporción 20:9 de tu teléfono frente al 16:9 de la transmisión. Quitarlas requiere recortar la imagen en el lado de la transmisión.
- Si deslizas desde el borde superior o inferior, las barras del sistema pueden aparecer en la captura. Evita hacerlo durante el directo.
- El aviso de "compartiendo pantalla" lo pone Android y no se puede ocultar desde la app.
