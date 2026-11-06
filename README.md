# stt-tts — versiones publicadas

Transcripción de audio y síntesis de voz **en tu ordenador**, conectadas a Claude (Claude Code y Claude Desktop). El código vive en un repositorio privado de Centic; aquí solo se publican los programas y los instaladores.

## Instalar o actualizar

**macOS (Apple Silicon) y Linux:**

```sh
curl -fsSL https://github.com/CenticMurcia/stt-tts-releases/releases/latest/download/install.sh | sh
```

**Windows (PowerShell):**

```powershell
irm https://github.com/CenticMurcia/stt-tts-releases/releases/latest/download/install.ps1 | iex
```

El instalador descarga el programa, comprueba que ha llegado íntegro (SHA-256), descarga los modelos (~0,8 GB) y lo conecta con Claude. Una vez instalado, se actualiza con `stt-tts actualizar`.

## Cómo se usa

### Qué necesitas

- Un Mac con Apple Silicon (M1 o posterior), o un PC con Windows o Linux de 64 bits. Con tarjeta gráfica (Vulkan) va mucho más rápido; sin ella también funciona.
- [Claude Code](https://claude.com/claude-code) o [Claude Desktop](https://claude.ai/download). El instalador conecta stt-tts con los que tengas instalados. Si instalas Claude Desktop después, ejecuta `stt-tts configurar` y reinícialo.
- Unos 1,5 GB libres para el programa y los modelos.

Todo se procesa en tu ordenador: el audio y el texto no se envían a ningún servidor. Solo se usa internet para descargar los modelos y las voces la primera vez, y para consultar el catálogo de voces.

| Medido en un MacBook M1 Pro | Reunión de 1 h |
|---|---|
| Transcripción | ~8 min |
| Transcripción + quién habla | ~16 min |

### Con Claude

Deja tus audios en la carpeta **`~/Grabaciones`** (se crea al instalar) y pídele a Claude lo que necesites:

- «Transcribe la última grabación»
- «Transcribe `reunion-lunes.m4a`; somos 4 personas y quiero saber quién dijo qué»
- «Hazme un resumen con los acuerdos de la reunión de ayer»
- «Lee este texto en voz alta» · «Léelo con voz de mujer» · «Léelo en francés»

Claude te irá diciendo el progreso y el tiempo que falta, y al terminar recibirás una notificación del sistema.

Dónde quedan los resultados:

| Qué | Dónde |
|---|---|
| Transcripciones (`.txt` con marcas de tiempo, `.srt` de subtítulos, `.json`) | `~/Grabaciones/transcripciones/` |
| Voz generada (`.wav`) | `~/Grabaciones/tts/` |

**Importante:**

- **No adjuntes el audio al chat**: Claude no puede pasárselo al programa. Déjalo en `~/Grabaciones` o dile a Claude dónde está (por ejemplo, `~/Downloads/reunion.m4a`).
- **Pide «quién habla» solo cuando lo necesites**: duplica el tiempo de proceso. Si sabes cuántas personas hablaron, díselo a Claude: acierta mucho más.
- En Mac, si el audio está en Descargas, Documentos o Escritorio, macOS pedirá permiso la primera vez. Con `~/Grabaciones` no hace falta.

Formatos de audio: mp3, m4a, mp4, aac, wav, flac, ogg, mkv/webm, aiff, caf.

### Idiomas y voces

- **Transcripción:** detecta el idioma sola. También puedes indicarlo («está en portugués»). Desde la 0.1.4 admite los ~99 idiomas de Whisper; antes, castellano e inglés.
- **Lectura en voz alta:** vienen incluidas estas voces:

| Voz | Idioma |
|---|---|
| `es_ES-davefx-medium` | Castellano, masculina (por defecto) |
| `es_ES-sharvard-medium` | Castellano, hablante `M` (masculina) o `F` (**femenina**) |
| `en_US-lessac-medium` | Inglés, femenina (por defecto en inglés) |
| `en_US-amy-medium` | Inglés, femenina |

  Desde la 0.1.4 hay además **unas 180 voces en más de 40 idiomas** (catalán, euskera, francés, alemán, italiano, portugués, español de México y Argentina…). Son las voces [Piper](https://github.com/rhasspy/piper). Pregúntale a Claude «¿qué voces hay en catalán?» o usa `stt-tts voces`. Cada voz se descarga (20–120 MB) la primera vez que se usa.

#### Descargar voces nuevas (desde la 0.1.4)

No hace falta descargar nada a mano: **la primera vez que usas una voz, se descarga sola** (20–120 MB, tarda unos segundos) y queda instalada para siempre.

- **Con Claude:** pídela por idioma o por nombre, por ejemplo «léelo en catalán», «léelo con la voz `es_AR-daniela-high`» o «¿qué voces hay en italiano?». Claude elige la voz y se descarga al generar el audio.
- **Desde la terminal**, para verlas o descargarlas antes de usarlas (por ejemplo, si después vas a estar sin conexión):

```sh
stt-tts voces                                   # idiomas disponibles y voces ya instaladas
stt-tts voces --idioma it                       # todas las voces en italiano (✅ = instalada)
stt-tts voces --descargar it_IT-paola-medium    # descargar una voz ahora
stt-tts hablar "Ciao a tutti" --voz it_IT-paola-medium
```

La lista de voces disponibles se consulta en internet como mucho una vez al día. `stt-tts voces --actualizar` la consulta en el momento. Las voces se guardan junto a los modelos y se borran con `stt-tts desinstalar --borrar-modelos`.

### Desde la terminal

```sh
stt-tts transcribir reunion.m4a                       # el idioma se detecta solo
stt-tts transcribir reunion.m4a --hablantes --num-hablantes 4
stt-tts transcribir entrevista.mp3 --idioma pt
stt-tts hablar "Hola equipo"                          # crea un .wav en ~/Grabaciones/tts
stt-tts hablar "Hola equipo" --voz es_ES-sharvard --hablante F   # voz femenina
stt-tts hablar "Bonjour à tous" --idioma fr           # (0.1.4) voz por defecto en francés
stt-tts voces --idioma ca                             # (0.1.4) voces en catalán
stt-tts --help                                        # todos los comandos
```

### Mantenimiento

```sh
stt-tts estado          # ¿está en marcha? versión, GPU, trabajos en curso
stt-tts actualizar      # instala la última versión y reinicia el servidor
stt-tts configurar      # repite la configuración (p. ej. tras instalar Claude Desktop)
stt-tts desinstalar     # quita todo menos tus grabaciones (--borrar-modelos libera ~0,8 GB)
```

### Problemas frecuentes

| Problema | Solución |
|---|---|
| Claude no ve las herramientas de stt-tts | `stt-tts estado`. Si no responde, `stt-tts configurar`. En Claude Desktop, reinícialo. |
| «no existe el fichero» | Comprueba la ruta o mueve el audio a `~/Grabaciones`. |
| Transcripción muy lenta | `stt-tts estado` muestra la GPU. Si dice `cpu`, el equipo no tiene Vulkan o es la versión sin GPU. |
| Una frase se repite muchas veces seguidas | Fallo conocido de Whisper en tramos con ruido, música o silencios largos. Lo que se dijo en ese tramo se pierde; el resto no se ve afectado. |
| El puerto 8765 está ocupado | Usa otro: `stt-tts configurar --puerto 8790`. |
| Ver qué ha pasado | Log: `~/Library/Application Support/stt-tts/servidor.log` (Mac), `~/.local/share/stt-tts/servidor.log` (Linux), `%LOCALAPPDATA%\stt-tts\servidor.log` (Windows). |

## Ficheros de cada versión

| Fichero | Para |
|---|---|
| `stt-tts-macos-arm64.gz` | Mac con Apple Silicon (M1 o posterior) |
| `stt-tts-linux-x64-vulkan.gz` / `-cpu.gz` | Linux x86_64 con GPU (Vulkan) / sin GPU |
| `stt-tts-windows-x64-vulkan.gz` / `-cpu.gz` | Windows x64 con GPU (Vulkan) / sin GPU |
| `SHA256SUMS` | Sumas de comprobación |
| `install.sh`, `install.ps1` | Instaladores |
