# Plugin: Generador de Imágenes

Genera y edita imágenes usando el modelo **Gemini** (`gemini-3-pro-image-preview`) de Google.

## Requisitos

Necesitas una API key gratuita de Google AI Studio:

1. Ve a [https://aistudio.google.com/](https://aistudio.google.com/)
2. Crea una API key
3. Agrégala a tu shell (`~/.bashrc` o `~/.zshrc`):

```bash
export GEMINI_API_KEY=tu_api_key_aqui
```

4. Recarga tu terminal: `source ~/.bashrc`

## Instalación

```bash
/plugin install image-generator@claude-pluyings
```

## Capacidades

- **Texto a imagen** — Crea imágenes desde descripciones en texto
- **Edición de imágenes** — Agrega/elimina elementos, transferencia de estilo
- **Composición multi-imagen** — Combina elementos de hasta 14 imágenes de referencia
- **Resoluciones** — 1K a 4K, múltiples proporciones (1:1, 16:9, 21:9, etc.)

## Uso

Una vez instalado, simplemente describe lo que quieres crear en tu conversación con Claude Code.

**Ejemplos:**
- "Genera una imagen fotorrealista de una ciudad futurista al atardecer"
- "Crea un logo minimalista para una startup de tecnología"
- "Edita esta imagen agregando un cielo más dramático"

## Notas

- Las imágenes generadas incluyen una marca de agua SynthID
- La API no retiene las imágenes generadas
