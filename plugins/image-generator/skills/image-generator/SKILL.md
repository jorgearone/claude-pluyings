# Skill: Generador de Imágenes con Gemini

Genera y edita imágenes usando el modelo `gemini-3-pro-image-preview` de Google.

## Configuración requerida

SIEMPRE cargar y verificar la API key ANTES de cualquier otra acción usando PowerShell:

```powershell
# Paso 1: intentar desde variable de entorno
if (-not $env:GEMINI_API_KEY) {
    # Paso 2: leer desde archivo (necesario en Claude Desktop - Microsoft Store)
    $pluginsEnv = "$env:APPDATA\Claude\plugins.env"
    if (Test-Path $pluginsEnv) {
        $line = Get-Content $pluginsEnv | Where-Object { $_ -match "^GEMINI_API_KEY=" }
        if ($line) { $env:GEMINI_API_KEY = $line -replace "^GEMINI_API_KEY=", "" }
    }
}
echo $env:GEMINI_API_KEY
```

- Si imprime un valor → PROCEDER normalmente.
- Si sigue vacía → mostrar este mensaje y **detener el proceso**:

```
La GEMINI_API_KEY no está configurada.
Crea el archivo:  %APPDATA%\Claude\plugins.env
Con el contenido: GEMINI_API_KEY=tu_api_key_aqui

Obtén tu key gratuita en: https://aistudio.google.com/
```

## Capacidades

- **Texto a imagen**: Crear imágenes desde descripciones textuales
- **Edición de imágenes**: Modificar imágenes existentes (agregar/quitar elementos, cambiar estilos)
- **Composición multi-imagen**: Combinar elementos de hasta 14 imágenes de referencia
- **Resoluciones**: 1K, 2K, 4K
- **Proporciones**: 1:1, 4:3, 16:9, 9:16, 21:9

## Flujo de trabajo

### Generación de texto a imagen

1. Solicitar al usuario una descripción detallada de la imagen
2. Preguntar resolución y proporción deseada (opcional, default: 1K, 1:1)
3. Construir el payload JSON y escribirlo en un archivo temporal
4. Llamar a la API REST de Gemini via `curl` usando el archivo JSON
5. Extraer la imagen del response y guardarla con un nombre descriptivo
6. Mostrar la ruta del archivo generado al usuario

### Edición de imagen existente

1. Solicitar la ruta de la imagen a editar
2. Codificar la imagen en base64:

   **Linux**
   ```bash
   base64 -w 0 imagen.jpg > imagen_b64.txt
   ```

   **macOS**
   ```bash
   base64 imagen.jpg > imagen_b64.txt
   ```

   **Windows PowerShell**
   ```powershell
   [Convert]::ToBase64String([IO.File]::ReadAllBytes("imagen.jpg")) | Out-File -NoNewline imagen_b64.txt
   ```

3. Construir el payload con la imagen en base64 y las instrucciones de edición
4. Escribir el payload en un archivo JSON temporal (no usar argumentos de línea de comandos por límite de longitud)
5. Llamar a la API y guardar el resultado

## Estructura del payload para la API

```json
{
  "contents": [
    {
      "parts": [
        {
          "text": "DESCRIPCIÓN_O_INSTRUCCIÓN_DE_EDICIÓN"
        }
      ]
    }
  ],
  "generationConfig": {
    "responseModalities": ["IMAGE", "TEXT"]
  }
}
```

Para edición con imagen de referencia, agregar en `parts`:
```json
{
  "inline_data": {
    "mime_type": "image/jpeg",
    "data": "BASE64_DE_LA_IMAGEN"
  }
}
```

## Llamada a la API

**Linux / macOS**
```bash
curl -s -X POST \
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent?key=$GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d @payload.json
```

**Windows CMD**
```cmd
curl -s -X POST ^
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent?key=%GEMINI_API_KEY%" ^
  -H "Content-Type: application/json" ^
  -d @payload.json
```

**Windows PowerShell**
```powershell
curl -s -X POST `
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent?key=$env:GEMINI_API_KEY" `
  -H "Content-Type: application/json" `
  -d @payload.json
```

## Buenas prácticas para prompts

- Ser descriptivo en lugar de usar palabras clave sueltas
- Incluir estilo, humor y detalles de iluminación
- Para imágenes con texto, especificar fuente y posición
- Para composiciones, describir la disposición espacial de los elementos

## Notas importantes

- Las imágenes incluyen marca de agua SynthID automáticamente
- La API no almacena las imágenes generadas
- Siempre guardar los archivos JSON temporales en `/tmp/` (Linux/macOS) o `%TEMP%` (Windows)
- Nunca pasar el payload como argumento de línea de comandos (límite de longitud del shell)
