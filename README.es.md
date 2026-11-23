*[English](README.md) · **Español***

# Plugin de Ulanzi D200X / D200H para OpenDeck

Usa un controlador **Ulanzi D200X** o **D200H** con
[OpenDeck](https://github.com/nekename/OpenDeck), el software libre y gratuito
para Stream Deck, en **Windows** o **Linux**.

Es un *plugin de dispositivo*: hace que el deck de Ulanzi aparezca en OpenDeck
como cualquier Stream Deck, así que puedes poner cualquier acción de OpenDeck,
venga del plugin que venga, en sus teclas, ruletas y botones laterales. No
necesitas Ulanzi Studio.

> Es un fork de [edubox/opendeck-ulanzi-d200x](https://github.com/edubox/opendeck-ulanzi-d200x),
> que soporta la D200X en Linux. Este fork añade soporte para **Windows** y para
> la **D200H**. Gracias al autor original por descifrar el protocolo.
> Tiene su propio identificador de plugin, `io.github.kastro5.ulanzi-d200` (desde la 1.4.0; la 1.3.x usaba `dev.kastro5.ulanzi-d200`).

## Contenido

- [Hardware soportado](#hardware-soportado)
- [Instalación en Windows](#instalación-en-windows)
- [Instalación en Linux](#instalación-en-linux)
- [Elegir el modelo](#elegir-el-modelo)
- [Usar el deck en OpenDeck](#usar-el-deck-en-opendeck)
- [Limitaciones conocidas](#limitaciones-conocidas)
- [Resolución de problemas](#resolución-de-problemas)
- [Cómo funciona](#cómo-funciona)
- [Créditos y licencia](#créditos-y-licencia)

## Hardware soportado

| Modelo | Pantallas | Ruletas | Botones laterales | Estado |
|---|---|---|---|---|
| **D200X** | 13 teclas + 1 pantalla ancha | 3 (giro + pulsación) | 2 | soportado, probado en Windows 11 (teclas, ruletas, botones laterales) y en el original con Linux |
| **D200H** (deck dock) | 13 teclas + 1 pantalla ancha | — | — | soportado, probado en Windows 11 |

Los dos modelos usan el mismo ID USB (`2207:0019`) y el mismo protocolo. No se
pueden distinguir por USB, así que tienes que decirle al plugin cuál tienes (ver
[Elegir el modelo](#elegir-el-modelo)).

| Sistema | Estado |
|---|---|
| Windows 10 / 11 | soportado (este fork) |
| Linux (OpenDeck nativo o Flatpak) | soportado (original) |
| macOS | sin probar |

## Instalación en Windows

### Instalación rápida (descarga)

No necesitas Python.

1. **Cierra Ulanzi Studio** (`UlanziDeck.exe`, en la bandeja del sistema) y
   desactiva su arranque automático. Controla el mismo dispositivo y se pelearía
   con OpenDeck por él.
2. Instala [OpenDeck](https://github.com/nekename/OpenDeck/releases) y ábrelo
   una vez.
3. Desde la [última versión](https://github.com/kastro5/opendeck-ulanzi-d200/releases/latest),
   descarga el zip de tu modelo: `opendeck-ulanzi-d200h-…zip` o
   `opendeck-ulanzi-d200x-…zip`.
4. Descomprímelo en `%APPDATA%\opendeck\plugins\` (pega esa ruta en la barra de
   direcciones del Explorador). Debe quedar
   `%APPDATA%\opendeck\plugins\io.github.kastro5.ulanzi-d200.sdPlugin\d200x_plugin.exe`.
5. Cierra OpenDeck desde la bandeja y vuelve a abrirlo.

**¿Actualizas desde una versión anterior a la 1.4.0?** La carpeta del plugin
pasó a ser `io.github.kastro5.ulanzi-d200.sdPlugin` (era
`dev.kastro5.ulanzi-d200.sdPlugin` en la 1.3.x y `com.ulanzi.d200x.sdPlugin`
antes). Borra la carpeta antigua (o quita el plugin antiguo en la pestaña Plugins
de OpenDeck), porque los dos reclamarían el deck. Los ajustes de modelo y de
pantalla ancha se recuperan solos, y tus teclas se quedan como estaban; solo la
acción **Wide screen**, si la tenías puesta, hay que volver a ponerla.

Si Windows SmartScreen o tu antivirus marcan `d200x_plugin.exe`, es porque se
trata de una compilación de PyInstaller sin firmar. Si prefieres no fiarte de un
exe descargado, compílalo tú mismo como se explica abajo.

### Compilar desde el código

#### 1. Requisitos

- **OpenDeck** para Windows ([descargas](https://github.com/nekename/OpenDeck/releases)).
  Ábrelo una vez para que cree su carpeta de configuración.
- **Python 3.10 o posterior**, que solo se usa para *compilar* el plugin.
  Descárgalo de [python.org](https://www.python.org/downloads/) o ejecuta
  `winget install Python.Python.3.13`. El plugin compilado es un `.exe`
  autónomo, así que OpenDeck no necesita Python para ejecutarlo.
- **Cierra Ulanzi Studio** (`UlanziDeck.exe`, en la bandeja del sistema) y
  desactiva su arranque automático. Controla el mismo dispositivo y se pelearía
  con OpenDeck por él.

No hace falta ningún driver: Windows deja que cualquier usuario abra el deck.

#### 2. Compilar e instalar

Clona o descarga este repositorio, abre PowerShell en su carpeta y ejecuta:

```powershell
# Si tienes una D200H:
powershell -ExecutionPolicy Bypass -File .\install.ps1 -Model d200h

# Si tienes una D200X:
powershell -ExecutionPolicy Bypass -File .\install.ps1 -Model d200x
```

El script:

1. Crea un entorno de Python privado en `.venv` e instala en él `hidapi`,
   `pillow`, `websockets`, `psutil` y `pyinstaller`. No toca el Python de tu
   sistema.
2. Compila `dist\d200x_plugin.exe`.
3. Copia el plugin a `%APPDATA%\opendeck\plugins\io.github.kastro5.ulanzi-d200.sdPlugin`.
4. Te avisa si Ulanzi Studio u otro plugin de D200 está en marcha.

#### 3. Reinicia OpenDeck

Cierra OpenDeck desde el icono de la bandeja y vuelve a abrirlo. El deck aparece
en el selector de dispositivos de arriba a la izquierda como **Ulanzi D200H** o
**Ulanzi D200X**.

#### Otras opciones del instalador

```powershell
.\install.ps1 -SkipBuild    # reinstala la última compilación sin recompilar
.\install.ps1 -Uninstall    # quita el plugin de OpenDeck
```

`-Model` se recuerda, así que las reinstalaciones lo mantienen salvo que lo
vuelvas a indicar.

## Instalación en Linux

### Instalación rápida (descarga)

No necesitas Python: el zip lleva una compilación de Linux autónoma.

1. De la [última versión](https://github.com/kastro5/opendeck-ulanzi-d200/releases/latest),
   descarga el zip de tu modelo y descomprímelo en la carpeta de plugins de
   OpenDeck: `~/.config/opendeck/plugins/`, o
   `~/.var/app/me.amankhanna.opendeck/config/opendeck/plugins/` si usas el
   Flatpak.
2. **Añade la regla udev.** Por defecto `/dev/hidraw*` solo es accesible para
   root, así que el plugin no ve el deck hasta que la regla le da acceso. La
   regla está en la carpeta del plugin:

   ```sh
   sudo cp ~/.config/opendeck/plugins/io.github.kastro5.ulanzi-d200.sdPlugin/60-ulanzi-d200x.rules /etc/udev/rules.d/
   sudo udevadm control --reload-rules && sudo udevadm trigger
   # después desenchufa y vuelve a enchufar el deck
   ```

3. Reinicia OpenDeck.

### Desde el código fuente

Python 3 con tres paquetes:

```sh
# Arch / CachyOS
sudo pacman -S python-hidapi python-pillow python-websockets
# Debian / Ubuntu
sudo apt install python3-hid python3-pil python3-websockets
```

Si OpenDeck es la versión Flatpak, instálalos **en el sistema anfitrión**. El
plugin se relanza allí con `flatpak-spawn --host`.

Después añade la regla udev e instala:

```sh
sudo cp 60-ulanzi-d200x.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules && sudo udevadm trigger
# después desenchufa y vuelve a enchufar el deck

./install.sh             # copia al directorio de plugins de OpenDeck
./install.sh --link      # enlace simbólico, para desarrollo
./install.sh --uninstall
```

Después reinicia OpenDeck.

### Notas

La regla usa `TAG+="uaccess"`, que da acceso al usuario con sesión iniciada, en
vez del `MODE="0666"` que sugieren muchas guías y que deja el dispositivo
escribible por todos. Sin systemd-logind, añade `GROUP="input"` a la regla y
únete a ese grupo. `install.sh` comprueba que el nodo del dispositivo sea
escribible y te avisa si no lo es.

El zip de la versión ya trae el modelo elegido. Con `install.sh`, elígelo desde OpenDeck,
como se explica a continuación.

## Elegir el modelo

El plugin necesita saber si controla una D200X (con ruletas y botones laterales)
o una D200H (solo teclas). Hay dos formas de indicarlo:

- **Al instalar (Windows):** `install.ps1 -Model d200h` o `-Model d200x`. Esto
  escribe `defaults.json` junto al plugin. Sin él, el plugin supone que es una
  D200X.
- **Desde OpenDeck:**
  1. Busca **Wide screen** en la categoría *Ulanzi D200* de la lista de acciones
     de la derecha.
  2. **Arrástrala** a una tecla. Hacer clic en la lista no hace nada.
  3. Haz clic en esa tecla de la cuadrícula. Sus ajustes aparecen debajo de la
     cuadrícula.
  4. Elige el modelo en **Modelo**.
  5. **Reinicia OpenDeck.** El plugin cambia al momento, pero el editor de
     OpenDeck se queda en el dispositivo anterior y muestra una cuadrícula vacía
     hasta que lo reinicias.

El modelo elegido en OpenDeck tiene prioridad sobre el del instalador. Cada
modelo es un dispositivo distinto en OpenDeck (`ulanzi-d200x` / `ulanzi-d200h`)
con sus propios perfiles, así que al cambiar no se conserva la configuración de
las teclas.

## Usar el deck en OpenDeck

El deck aparece como una cuadrícula de 5 × 3:

```
┌────┬────┬────┬────┬────┐
│  0 │  1 │  2 │  3 │  4 │
├────┼────┼────┼────┼────┤
│  5 │  6 │  7 │  8 │  9 │
├────┼────┼────┼────┴────┤
│ 10 │ 11 │ 12 │13(ancha)│   ← el hueco 14 es la mitad derecha de 13 y no hace nada
└────┴────┴────┴─────────┘
  Solo D200X:  ◯ ruleta 1   ◯ ruleta 2   ◯ ruleta 3     ▭ lateral 1   ▭ lateral 2
```

| En el deck | En OpenDeck | Notas |
|---|---|---|
| 13 pantallas cuadradas | teclas 0–12 | iconos escalados a la pantalla de 196×196 |
| la pantalla ancha (abajo a la derecha) | **tecla 13** | pon aquí su acción, no en la 14 |
| — | tecla 14 | sin uso, es la mitad derecha de la pantalla ancha |
| 3 ruletas (D200X) | encoders 1–3 | giro y pulsación |
| 2 botones laterales (D200X) | touchpoints 1–2 | solo botones, sin pantalla |

### La pantalla ancha

Por defecto la pantalla ancha muestra el **reloj** del propio deck. Para
cambiarlo, arrastra la acción **Wide screen** sobre ella, haz clic y elige un
modo:

| Modo | Qué muestra la pantalla |
|---|---|
| **Reloj** (por defecto) | el reloj integrado en el dispositivo |
| **Reloj digital** | un reloj digital grande, como el que ofrece Ulanzi Studio |
| **Reloj digital con fecha** / **con día de la semana** / **con fecha y día de la semana** | lo mismo, con la fecha y/o el día de la semana debajo |
| **Monitor del sistema** | indicadores de CPU, RAM y GPU que el plugin actualiza cada 2 s |
| **Apagada** | negro |

El ajuste es global: se aplica aunque la acción no esté en la pantalla ancha. Si
pones **cualquier otra** acción en la tecla 13, la pantalla ancha muestra su
icono, con bandas negras para que no se deforme. En ambos casos, pulsar la
pantalla ancha dispara la tecla 13.

Los relojes digitales escriben la fecha y el día de la semana en el idioma de
OpenDeck (inglés, alemán o español; si no, en inglés).

El monitor del sistema obtiene CPU y RAM del sistema operativo. La carga de la
GPU sale de `nvidia-smi` en tarjetas NVIDIA, o de `gpu_busy_percent` en tarjetas
AMD con Linux. Con otras GPU el indicador marca 0.

**Imagen de fondo.** Pon una imagen llamada `wide_background.png` en la carpeta
del plugin y reinicia OpenDeck: el reloj y los indicadores se dibujan encima.
Sirve cualquier tamaño; se escala para cubrir la pantalla.

**Imágenes de otros programas.** OpenDeck no da a un plugin ninguna forma de
mostrar algo un momento en la pantalla ancha (la imagen propia de una ruleta
nunca llega al deck), así que el plugin vigila un archivo. Escribe un PNG en
`%TEMP%\d200x-wide-overlay.png` (Windows) o `$TMPDIR/d200x-wide-overlay.png`
(Linux, normalmente `/tmp`), o en la ruta de `D200X_OVERLAY_FILE`, y se muestra
en la pantalla ancha durante 2,5 s tras la última escritura; después vuelve el
reloj. Una imagen cuadrada se centra sobre el fondo; una al menos 1,5 veces más
ancha que alta llena la pantalla (dibújala a 458×196, el tamaño real del
panel). Escribe a un nombre temporal y renómbralo, para que nunca se lea un
archivo a medias. Otro plugin puede usarlo para mostrar el valor de una ruleta
mientras se gira, o una lista corta.

**Tiempo de inactividad.** Cada pulsación de tecla y cada evento de ruleta
actualiza `%TEMP%\d200x-last-input` (o `D200X_ACTIVITY_FILE`), así que otros
programas pueden saber cuánto tiempo lleva el deck sin tocarse, por ejemplo para
volver a la página principal.

### Borde de las teclas

Las imágenes que llenan todo su cuadrado llegan justo hasta el borde de la
pantalla de la tecla. Los ajustes de la acción **Wide screen** tienen también
**Borde de las teclas** (ninguno, fino, medio, ancho): un borde negro de 0, 4, 8
o 12 píxeles alrededor de la imagen de cada tecla, salvo la pantalla ancha. Es
global, como el modo.

### Brillo

El ajuste de brillo de OpenDeck y la acción **Device Brightness** se pasan
directamente al deck.

## Limitaciones conocidas

- **La pantalla ancha no puede ser una sola tecla grande en el editor de
  OpenDeck.** OpenDeck dibuja todos los dispositivos como una cuadrícula
  uniforme de teclas cuadradas, y un plugin no puede fusionar celdas. Así que la
  pantalla ancha es la tecla 13, con una tecla 14 vacía al lado. En el deck es
  una sola pantalla. Arreglarlo requiere cambiar OpenDeck.
  [El PR #359 de OpenDeck](https://github.com/nekename/OpenDeck/pull/359)
  propone que los plugins describan su distribución exacta, y este plugin ya la
  envía: la tecla 13 como una sola tecla doble y los botones laterales a media
  altura. OpenDeck sin modificar la ignora. Una versión con #359 debería dibujar
  el deck correctamente sin actualizar el plugin.
- **Los botones laterales de la D200X tienen su propia fila** en el editor, a
  tamaño completo, en vez de ir junto a las ruletas. Es la misma limitación de
  diseño de OpenDeck.
- **Los iconos se envían en paquetes.** El firmware solo acepta iconos en un
  paquete ZIP (ver [Cómo funciona](#cómo-funciona)), así que el deck se
  actualiza como mucho cada 0,5 s, por rápido que cambie una tecla.
- **Solo un programa puede controlar el deck.** Cierra Ulanzi Studio y no
  instales un segundo plugin de D200, como `com.glmagalhaes.ulanzi.d200.sdPlugin`.

## Resolución de problemas

**El deck no aparece en OpenDeck**

- *Windows:* asegúrate de que Ulanzi Studio esté cerrado del todo (mira la
  bandeja) y reinicia OpenDeck. Busca `Deck connected` en el log del plugin (ver
  abajo).
- *Linux:* casi siempre es la regla udev. Instálala y después desenchufa y
  vuelve a enchufar el deck.
- Comprueba que el plugin esté en la carpeta de plugins de OpenDeck
  (`%APPDATA%\opendeck\plugins` en Windows, `~/.config/opendeck/plugins` en
  Linux) y que hayas reiniciado OpenDeck tras instalarlo.

**La cuadrícula está vacía tras cambiar el modelo.** Reinicia OpenDeck. Su
editor no sigue a un dispositivo que se cambia mientras está abierto.

**Distribución equivocada (ruletas en una D200H o ninguna en una D200X).**
Indica el modelo: ver [Elegir el modelo](#elegir-el-modelo).

**Las pantallas no cambian.** Busca `Pushed icon bundle` en el log. Si aparece
pero el deck no se actualiza, otro programa está hablando con el deck.

**El indicador de GPU se queda en 0.** Tu GPU no es NVIDIA (ni AMD en Linux).
Ver [La pantalla ancha](#la-pantalla-ancha).

### Logs

| Plataforma | Log del plugin |
|---|---|
| Windows | `%LOCALAPPDATA%\opendeck\logs\plugins\io.github.kastro5.ulanzi-d200.sdPlugin.log` |
| Linux (nativo) | `~/.local/share/opendeck/logs/plugins/io.github.kastro5.ulanzi-d200.sdPlugin.log` |
| Linux (Flatpak) | `~/.var/app/me.amankhanna.opendeck/data/opendeck/logs/plugins/io.github.kastro5.ulanzi-d200.sdPlugin.log` |

Define la variable de entorno `D200X_DEBUG=1` antes de abrir OpenDeck para
obtener logs detallados.

## Cómo funciona

Todo el plugin es un único archivo de Python,
[`d200x_plugin.py`](io.github.kastro5.ulanzi-d200.sdPlugin/d200x_plugin.py). OpenDeck lo
arranca y se comunica con él por websocket usando el protocolo de plugins de
Stream Deck. El plugin habla con el deck por HID USB en bruto.

**Imágenes.** OpenDeck dibuja cada tecla como un JPEG y lo envía con `setImage`.
El firmware del deck no acepta imágenes por tecla: solo acepta un **paquete
ZIP** con un `manifest.json` y todos los iconos, enviado en paquetes de 1024
bytes. Así que el plugin:

1. escala cada imagen a 196×196 y la guarda en caché;
2. junta los cambios de 0,35 s en un solo paquete y envía como mucho un paquete
   cada 0,5 s (`D200X_MIN_INTERVAL` lo cambia);
3. mete en el paquete solo las teclas cuyo icono ha cambiado: una tecla nombrada
   en el paquete se redibuja y una que se queda fuera conserva su imagen, así que
   una tecla en vivo no hace parpadear a las demás;
4. rellena el ZIP hasta que ningún byte `0x00` o `0x7c` caiga en el límite de un
   paquete, para esquivar un fallo de lectura del firmware.

Tras una reconexión o un reinicio del firmware, el siguiente paquete vuelve a
llevar todas las teclas.

Mientras el firmware dibuja él mismo la pantalla ancha (reloj o monitor), esa
celda se deja fuera del paquete para que no parpadee en cada actualización.

**Entrada.** Las pulsaciones de teclas, los giros y las pulsaciones de las
ruletas llegan como reports HID de entrada. El hilo HID las espera con una
lectura bloqueante que expira cada 100 ms, así que un deck en reposo apenas
consume CPU. Se reenvían a OpenDeck como
`keyDown`/`keyUp` y `encoderChange`/`encoderDown`/`encoderUp`. Una segunda
pulsación de la misma tecla en menos de 0,8 s se descarta, con su liberación,
para que una tecla que cambia de página pulsada dos veces no dispare también la
tecla que ocupa su sitio en la página nueva (`D200X_REPRESS_GUARD` fija el
tiempo, `0` lo desactiva).

**Señal de vida.** El firmware vuelve a su logo de arranque si el host pasa
menos de un minuto sin enviar nada. Siempre que el plugin lleva 5 s sin
escribir, reenvía el brillo actual, lo que mantiene el deck activo sin
redibujar nada.

**Reconexiones.** El deck reinicia brevemente su conexión USB mientras
descomprime un paquete grande. El plugin vuelve a abrir el dispositivo sin
avisar y solo le dice a OpenDeck que el deck ha desaparecido tras 30 s de
ausencia. Al reconectar reenvía los iconos que tiene en caché.

### Qué cambió el port a Windows

- **Byte de report ID.** El descriptor HID del deck no usa report IDs y sus
  reports son de 1024 bytes. Linux (hidraw) escribe los 1024 bytes tal cual.
  Windows exige un byte inicial de report ID, así que cada escritura se envía
  como `0x00` + 1024 bytes. Sin él, hidapi toma la cabecera `0x7c` como ID y los
  datos se desplazan un byte. Las lecturas no cambian, porque Windows quita el
  ID.
- **Elección de interfaz.** El deck tiene dos interfaces HID: la 0 es el canal
  del fabricante y la 1 es un teclado de arranque que Windows bloquea. El plugin
  abre explícitamente la interfaz 0.
- **Estadísticas del sistema.** En Windows no existe `/proc`, así que la CPU y la
  RAM salen de `psutil`.
- **Empaquetado.** OpenDeck en Windows ejecuta directamente el `CodePathWin`
  del plugin, así que el plugin se distribuye como un `.exe` de consola
  compilado con PyInstaller. Su stderr va al log de plugins de OpenDeck.

### Referencia del protocolo

Cada paquete mide 1024 bytes: cabecera `7c 7c`, un comando de 2 bytes en
big-endian, la longitud de los datos en 4 bytes little-endian y después los
datos.

| Comando | Valor | Uso |
|---|---|---|
| `SET_BUTTONS` | `0x0001` | paquete ZIP con iconos y manifest |
| `SET_SMALL_WINDOW` | `0x0006` | pantalla ancha: `modo\|cpu\|ram\|HH:MM:SS\|gpu`; modo `0` estadísticas, `1` reloj, `2` imagen |
| `SET_BRIGHTNESS` | `0x000a` | `0`–`100`, en ASCII |
| `SET_LABEL_STYLE` | `0x000b` | fuente y color de las etiquetas (los títulos se ocultan; OpenDeck los dibuja en la imagen) |
| `IN_BUTTON` | `0x0101` / `0x0102` | entrada desde el deck |

Índices de hardware de la entrada: pantallas `0`–`13`, botones laterales
`15`/`16`, ruletas `17`/`18`/`19`. Byte de evento de ruleta: `0` soltar,
`1` pulsar, `2` girar a la izquierda, `3` girar a la derecha.

## Estructura del repositorio

| Ruta | Qué es |
|---|---|
| `io.github.kastro5.ulanzi-d200.sdPlugin/` | el plugin: `d200x_plugin.py`, `manifest.json`, iconos, panel de ajustes |
| `install.ps1` | script de compilación e instalación para Windows |
| `requirements-build.txt` | dependencias de compilación fijadas, compartidas por `install.ps1` y la CI |
| `scripts/package_release.py` | empaqueta el exe en los zips de cada modelo |
| `.github/workflows/release.yml` | compila y publica las versiones |
| `install.sh` | script de instalación para Linux |
| `60-ulanzi-d200x.rules` | regla udev para Linux |
| `CHANGELOG.es.md` | notas de versión |

## Publicar una versión

Las versiones las compila GitHub Actions, no el PC de quien desarrolla:

1. Sube `Version` en `io.github.kastro5.ulanzi-d200.sdPlugin/manifest.json` y añade una
   entrada `## <versión>` a `CHANGELOG.md` (y a `CHANGELOG.es.md`). La entrada
   en inglés se usa como notas de la versión.
2. Haz commit, crea la etiqueta y súbela:
   `git tag -a v1.2.3 -m "v1.2.3" && git push origin main v1.2.3`

El [workflow Release](.github/workflows/release.yml) compila el exe en Windows,
comprueba que arranca, empaqueta los dos zips y publica la versión. Rechaza una
etiqueta que no coincida con la versión del manifest. Para probar una
compilación sin publicar nada, lanza el workflow a mano desde la pestaña Actions
y descarga los zips de sus artefactos.

## Créditos y licencia

- Plugin original y trabajo sobre el protocolo:
  [edubox/opendeck-ulanzi-d200x](https://github.com/edubox/opendeck-ulanzi-d200x).
- Port a Windows y soporte para la D200H: este fork.
- [OpenDeck](https://github.com/nekename/OpenDeck), de nekename.

Licencia **GNU GPL v3.0**, la misma que el original. Ver [LICENSE](LICENSE). Sin
relación con Ulanzi.
