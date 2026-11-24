# LIVEPS4 · Guía de uso

LIVEPS4 te permite ver y jugar tu PS4 desde tu Android, y transmitir ese juego en vivo a TikTok o YouTube.

---

## 1. Requisitos

- Android 10 o superior (para el audio del juego).
- PS4 con exploit **GoldHEN** (necesario para Apollo Save Tool).
- **Apollo Save Tool** instalado en la PS4.
- PS4 y teléfono en **la misma red Wi-Fi**.
- Un **perfil secundario** en la PS4, que usarás solo para Remote Play.
- Tu **cuenta principal** de PS4, con tus partidas guardadas.
- Cuenta de TikTok o YouTube lista para transmitir.

---

## 2. Preparar la PS4 (una sola vez)

### 2.1 Activar el perfil secundario sin conexión a PSN

1. Abre **Apollo Save Tool** en la PS4.
2. Ve a **User Tools → Activate PS4 Accounts**.
3. Selecciona tu perfil secundario y presiona **R2** para generar un ID aleatorio. Confirma.
4. Reinicia la PS4 y vuelve a cargar **GoldHEN**.
5. Al iniciar sesión, si el nombre del perfil aparece en cursiva, la activación funcionó.

### 2.2 Obtener el Account ID

1. Vuelve a entrar a **Apollo Save Tool → User Tools → Activate PS4 Accounts**.
2. Anota el **Account ID** de 16 caracteres que aparece en **Console details**. Es **hexadecimal** (ejemplo: `0000000000000001`).

**No confundas los campos:** el **User ID** (8 caracteres, ejemplo `1d4b651b`) y el **Console PSID** (32 caracteres) **no** son el Account ID. Solo usa el campo **Account ID**.

**Sobre los valores de todos unos (`1111111111111111`):** si tu perfil muestra solo unos, ese valor no identifica al perfil de forma confiable. Usa el Account ID del perfil que activaste con R2 en el paso 2.1.

Si Apollo lo muestra como **número decimal** en vez de hexadecimal, usa la versión decimal del script de la sección 3.

### 2.3 Activar Remote Play y generar el PIN

1. En la PS4: **Configuración → Configuración de conexión del Uso a distancia**.
2. Marca **Activar Uso a distancia**.
3. Selecciona **Agregar dispositivo**. Aparece un **PIN de 8 dígitos**.
4. Deja esa pantalla abierta. El PIN dura unos minutos.

### 2.4 Consultar la IP de la PS4

1. Con el perfil secundario, ve a **Configuración → Red → Ver estado de la conexión**.
2. Anota la **Dirección IP** (ejemplo: `192.168.1.50`).

---

## 3. Configurar LIVEPS4 (una sola vez)

Completa estos datos y guárdalos:

| Campo | Qué poner |
|---|---|
| **IP de la PS4** | La Dirección IP del paso 2.4. |
| **PIN de registro (8 dígitos)** | El PIN del paso 2.3. |
| **PSN Account ID (Base64)** | El ID del paso 2.2 convertido a Base64 de 8 bytes (ver abajo). |
| **PSN Online ID (opcional)** | Tu nombre de usuario de PSN. |

### Convertir el Account ID a Base64

LIVEPS4 decodifica el valor en Base64 y solo lo acepta si da **exactamente 8 bytes**. Si no, lo ignora y el log muestra `account_id=MISSING`.

**Si Apollo muestra 16 caracteres hexadecimales (el caso normal):**

```python
import base64
h = "0000000000000001"   # tu Account ID de 16 caracteres hex de Apollo
print(base64.b64encode(bytes.fromhex(h)).decode())
```

Ejemplos verificados: `0000000000000001` → `AAAAAAAAAAE=` y `1111111111111111` → `ERERERERERE=`.

**Solo si Apollo muestra un número decimal de 16 dígitos (no es tu caso):**

```python
import base64, struct
n = 1234567890123456     # tu número de 16 dígitos
print("LE", base64.b64encode(struct.pack("<Q", n)).decode())
print("BE", base64.b64encode(struct.pack(">Q", n)).decode())
```

Si ya te funciona el valor que tienes en la app, **no lo cambies**. Estos scripts son solo para cuando sale `account_id=MISSING` o el emparejamiento falla. No uses convertidores en línea ni chatbots para esto.

---

## 4. Flujo completo para transmitir a TikTok

Sigue este orden. El primer paso es obligatorio.

**Paso 1. Inicia la PS4 con el perfil secundario.**
Ese perfil es el que usa Remote Play. Si entras con tu cuenta principal primero, no funciona como debe.

**Paso 2. Deja LIVEPS4 lista.**
Abre la app y verifica que la IP, el PIN, el Account ID y el Online ID estén guardados.

**Paso 3. Abre TikTok e inicia el directo.**
- Sin cerrar LIVEPS4 (puede quedar en segundo plano), abre la app de **TikTok**.
- Toca **+** en la parte inferior y desliza hasta **LIVE**.
- En las opciones superiores de LIVE, elige **Juegos móviles** (Mobile Gaming).
- Elige la orientación **Horizontal**.
- Escribe el título, elige la categoría del juego y ajusta la calidad de video según tu internet.
- Toca **Emitir LIVE**. TikTok pedirá permiso de captura de pantalla; acéptalo con **Comenzar ahora**.

**Paso 4. Regresa a LIVEPS4 durante la cuenta regresiva.**
Aparece una cuenta regresiva (unos 25 segundos). En ese tiempo:
- Toca **Emparejar**.
- Toca **INICIAR REMOTE PLAY**.

**Paso 5. Gira el teléfono a horizontal.**
La transmisión de TikTok arranca sola y lo que se ve en pantalla se transmite.

**Paso 6. Enciende el control.**
Al activarse Remote Play, el control se desconecta. Enciéndelo de nuevo.

**Paso 7. Inicia sesión con tu cuenta principal.**
Así accedes a tus partidas guardadas y puedes jugar.

**Importante:** LIVEPS4 debe quedar **en primer plano** durante todo el directo. Lo que se ve en pantalla es lo que se transmite. Si cambias de app, tu audiencia también lo verá.

---

## 5. Transmitir a YouTube (alternativa)

1. Conecta la PS4 en LIVEPS4 para que el juego se vea.
2. Toca **DIRECTO → MODO YOUTUBE APP**. La app te explica los pasos.
3. Toca **ABRIR YOUTUBE**. En YouTube: **+ → En vivo → Screencast**.
4. Inicia la transmisión en YouTube.
5. Regresa a LIVEPS4 con el botón de recientes y juega.

También puedes transmitir directo desde LIVEPS4 con una **Stream Key** de YouTube o Twitch: pégala en el panel de DIRECTO y toca **INICIAR EN VIVO (RTMP)**.

---

## 6. Modo limpio (ocultar lo que no quieres que se vea)

- Toca **LIMPIO** en la barra superior.
- Se ocultan los controles de la app y las barras del sistema (hora, batería, señal y barra de navegación).
- Para volver a mostrar los controles, toca el botón **⚙️** (abajo a la derecha). Tocar otras partes de la pantalla **no** los muestra.

Durante un directo, las barras se ocultan automáticamente. Mientras el directo está activo, **No molestar** silencia las notificaciones y al terminar restaura tu configuración anterior.

**Permiso de No molestar:** la primera vez que inicies un directo, la app te lleva a los ajustes para activar LIVEPS4 en "No molestar". Actívalo, regresa a la app y vuelve a iniciar.

---

## 7. Botones de la barra superior

| Botón | Qué hace |
|---|---|
| **CAM** | Muestra u oculta la cámara sobre el juego. |
| **LIMPIO** | Oculta controles y barras del sistema. |
| **DIRECTO** | Abre o cierra el panel de transmisión. |
| **SALIR** | Cierra la sesión con la PS4 y regresa a la pantalla principal. |

---

## 8. Ajustes

Toca el engranaje en la pantalla principal.

- **Resolución**: 720p (si no, 480p) u otra.
- **Bitrate del juego** y **Bitrate del directo**: calidad de video. Más alto = mejor imagen, pero necesita mejor red.
- **FPS**: cuadros por segundo.
- **Audio del juego en el directo (Android 10+)**: si está **activado**, el directo lleva el sonido del juego. Si está **desactivado**, usa el micrófono (no recomendado para juegos).
- **Mantener pantalla encendida**: evita que el teléfono se apague durante la sesión.
- **Reconectar automáticamente al caer**: intenta volver a conectar si se corta la sesión.
- **Vibración (rumble) en el teléfono**: vibra cuando el juego lo pide.
- **Intercambiar ✕/○ y □/△ (mandos 8BitDo)**: para mandos 8BitDo.
- **Exportar ajustes (JSON)**: guarda tus ajustes en Descargas.
- **Restablecer ajustes**: vuelve a los valores de fábrica. Tus consolas y claves **no** se borran.

---

## 9. Si algo falla

| Mensaje o problema | Qué hacer |
|---|---|
| **Sin permiso de captura** | Vuelve a iniciar y acepta el permiso de captura. |
| **El sistema negó la captura de pantalla** | Cierra la app, ábrela y vuelve a intentar. |
| **El audio del juego requiere Android 10 o superior** | Tu teléfono no soporta el audio del juego. Desactívalo en Ajustes. |
| **No se pudo capturar el audio del juego** | La app de Remote Play puede estar bloqueando la captura. Prueba con un video de YouTube para comprobar si el audio sale. |
| **PS4 no respondió / Conexión rechazada / Tiempo agotado** | Revisa que la PS4 esté encendida, la IP, la red Wi-Fi y que el PIN no haya expirado. Genera un PIN nuevo. |
| **Sin llaves: Empareja primero** | Empareja la PS4 (paso 4 del flujo). |
| **`account_id=MISSING` en el log** | Revisa el Account ID con los scripts de la sección 3. |
| **El directo se congela** | Baja el bitrate del directo en Ajustes. |
| **No aparece la hora ni la batería en el directo** | Es lo esperado: el modo limpio oculta las barras del sistema. |

**Diagnóstico:** si nada funciona, genera un reporte desde la app y compártelo con el desarrollador. Se guarda en Descargas como `ps4live_diagnostico_....txt`.

---

## 10. Limitaciones conocidas

- Solo se transmite lo que se ve en pantalla.
- Si deslizas desde el borde superior o inferior, las barras pueden aparecer en el directo. Evita hacerlo.
- El audio del juego depende de que la app de Remote Play lo permita.
- El aviso de "compartiendo pantalla" lo pone Android y no se puede ocultar desde la app.
