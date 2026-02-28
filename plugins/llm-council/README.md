# Plugin: Consejo LLM

Orquesta múltiples LLMs de código abierto vía **Fireworks AI** para deliberar sobre consultas, inspirado en el concepto de Consejo LLM de Karpathy.

## ¿Cómo funciona?

El proceso consta de tres fases:

1. **Fase 1 — Respuestas individuales**: Todos los modelos del consejo responden en paralelo a tu consulta
2. **Fase 2 — Evaluación anónima**: Cada modelo evalúa y clasifica las respuestas de sus pares (A, B, C...)
3. **Fase 3 — Síntesis final**: Un modelo "Presidente" integra las clasificaciones y genera una respuesta final

## Requisitos

Necesitas una API key de Fireworks AI:

1. Ve a [https://app.fireworks.ai/](https://app.fireworks.ai/)
2. Crea tu cuenta y obtén tu API key
3. Agrégala a tu shell (`~/.bashrc` o `~/.zshrc`):

```bash
export FIREWORKS_API_KEY=tu_api_key_aqui
```

4. Recarga tu terminal: `source ~/.bashrc`

## Instalación

```bash
/plugin install llm-council@claude-pluyings
```

## Modelos disponibles

| Modelo | Proveedor |
|--------|-----------|
| GLM 5 | Z.ai |
| DeepSeek V3.1 | DeepSeek |
| DeepSeek V3.2 | DeepSeek |
| MiniMax M2.1 | MiniMax |
| Kimi K2.5 | Moonshot |
| Qwen3 235B | Alibaba |
| Llama 4 Maverick | Meta |

## Uso

Al invocar el skill, podrás seleccionar qué modelos forman tu consejo. Claude te guiará en la selección y ejecutará el proceso completo mostrando todas las respuestas individuales, las clasificaciones y la síntesis final.
