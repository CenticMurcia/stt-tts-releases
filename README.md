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

## Usarlo

Deja tus audios en `~/Grabaciones` y pídele a Claude, por ejemplo: «Transcribe la última grabación; somos 4 personas». Las transcripciones aparecen en `~/Grabaciones/transcripciones/`.

Desde la terminal: `stt-tts transcribir reunion.m4a`, `stt-tts hablar "Hola"`, `stt-tts estado`, `stt-tts --help`.

## Ficheros de cada versión

| Fichero | Para |
|---|---|
| `stt-tts-macos-arm64.gz` | Mac con Apple Silicon (M1 o posterior) |
| `stt-tts-linux-x64-vulkan.gz` / `-cpu.gz` | Linux x86_64 con GPU (Vulkan) / sin GPU |
| `stt-tts-windows-x64-vulkan.gz` / `-cpu.gz` | Windows x64 con GPU (Vulkan) / sin GPU |
| `SHA256SUMS` | Sumas de comprobación |
| `install.sh`, `install.ps1` | Instaladores |
