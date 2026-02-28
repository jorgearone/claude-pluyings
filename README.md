# Claude Pluyings

Marketplace de código abierto de plugins para Claude Code, pensado para la comunidad hispanohablante.

## Instalación del marketplace

```bash
/plugin marketplace add jorgearone/claude-pluyings
```

## Plugins disponibles

| Plugin | Descripción |
|--------|-------------|
| [image-generator](./plugins/image-generator/) | Genera y edita imágenes con Gemini |
| [llm-council](./plugins/llm-council/) | Orquesta múltiples LLMs vía Fireworks AI |

## Instalar un plugin

```bash
/plugin install <nombre-plugin>@claude-pluyings
```

### Ejemplos

```bash
/plugin install image-generator@claude-pluyings
/plugin install llm-council@claude-pluyings
```

## Contribuir

¿Tienes un plugin que quieras compartir con la comunidad? ¡Los pull requests son bienvenidos!

Estructura mínima de un plugin:

```
plugins/<nombre-plugin>/
├── .claude-plugin/
│   └── plugin.json
├── README.md
└── skills/<nombre-skill>/
    ├── SKILL.md
    └── .env.example
```

## Autor

Creado por [@jorgearone](https://github.com/jorgearone)
