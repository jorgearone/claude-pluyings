# Skill: Consejo LLM con Fireworks AI

Orquesta múltiples LLMs de código abierto para deliberar sobre consultas del usuario usando el concepto de Consejo LLM.

## Configuración requerida

Verificar que el usuario tenga configurada la variable de entorno `FIREWORKS_API_KEY`. Usar el comando según el sistema operativo:

**Linux / macOS (bash)**
```bash
echo $FIREWORKS_API_KEY
```

**Windows CMD**
```cmd
echo %FIREWORKS_API_KEY%
```

**Windows PowerShell**
```powershell
echo $env:FIREWORKS_API_KEY
```

Si no está configurada, indicar al usuario que siga las instrucciones del README.

## Modelos disponibles en Fireworks AI

| ID del modelo | Nombre | Proveedor |
|--------------|--------|-----------|
| `accounts/fireworks/models/glm-5` | GLM 5 | Z.ai |
| `accounts/fireworks/models/deepseek-v3p1` | DeepSeek V3.1 | DeepSeek |
| `accounts/fireworks/models/deepseek-v3` | DeepSeek V3.2 | DeepSeek |
| `accounts/fireworks/models/minimax-m2p1` | MiniMax M2.1 | MiniMax |
| `accounts/fireworks/models/kimi-k2p5` | Kimi K2.5 | Moonshot |
| `accounts/fireworks/models/qwen3-235b` | Qwen3 235B | Alibaba |
| `accounts/fireworks/models/llama-4-maverick` | Llama 4 Maverick | Meta |

## Flujo de trabajo

### IMPORTANTE: Siempre usar AskUserQuestion para selección de modelos

SIEMPRE usar `AskUserQuestion` para que el usuario elija qué modelos forman su consejo antes de ejecutar cualquier llamada.

### Fase 1 — Respuestas individuales

Para cada modelo seleccionado:
1. Enviar la consulta del usuario al modelo vía API de Fireworks AI
2. Guardar la respuesta RAW completa en un archivo JSON en el directorio de sesión
3. NUNCA resumir ni truncar las respuestas de la API

Directorio de sesión: `./council-session-<timestamp>/`

Estructura de archivos:
```
council-session-<timestamp>/
├── query.txt
├── response_modelo1.json
├── response_modelo2.json
├── ...
├── rankings_modelo1.json
├── rankings_modelo2.json
├── ...
└── synthesis_final.md
```

### Fase 2 — Evaluación anónima

1. Asignar letras anónimas a cada respuesta (A, B, C...)
2. Pedir a cada modelo que evalúe y clasifique las respuestas de sus pares
3. Guardar cada evaluación en un archivo JSON separado
4. Mostrar las clasificaciones completas al usuario

### Fase 3 — Síntesis del Presidente

1. Usar el modelo con mejor clasificación promedio como "Presidente" (o Qwen3 235B por defecto)
2. Enviar todas las respuestas y clasificaciones al Presidente
3. El Presidente genera la síntesis final integrando los mejores elementos
4. Guardar y mostrar la síntesis completa

## Llamada a la API de Fireworks

**Linux / macOS**
```bash
curl -s -X POST \
  "https://api.fireworks.ai/inference/v1/chat/completions" \
  -H "Authorization: Bearer $FIREWORKS_API_KEY" \
  -H "Content-Type: application/json" \
  -d @payload.json
```

**Windows CMD**
```cmd
curl -s -X POST ^
  "https://api.fireworks.ai/inference/v1/chat/completions" ^
  -H "Authorization: Bearer %FIREWORKS_API_KEY%" ^
  -H "Content-Type: application/json" ^
  -d @payload.json
```

**Windows PowerShell**
```powershell
curl -s -X POST `
  "https://api.fireworks.ai/inference/v1/chat/completions" `
  -H "Authorization: Bearer $env:FIREWORKS_API_KEY" `
  -H "Content-Type: application/json" `
  -d @payload.json
```

Ejemplo de payload:
```json
{
  "model": "accounts/fireworks/models/deepseek-v3",
  "messages": [
    {"role": "user", "content": "CONSULTA_DEL_USUARIO"}
  ],
  "max_tokens": 4096
}
```

## Transparencia total

- Mostrar TODAS las respuestas individuales sin modificar
- Mostrar TODAS las clasificaciones completas
- Mostrar métricas de tokens y latencia por modelo
- Nunca ocultar ni resumir respuestas de la API

## Notas importantes

- Escribir todos los payloads en archivos JSON, no como argumentos de línea de comandos
- Registrar uso de tokens y latencia para cada llamada
- Si un modelo falla, continuar con los demás e informar al usuario
- El directorio de sesión permite revisar todo el proceso posteriormente
