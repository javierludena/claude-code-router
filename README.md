![](blog/images/claude-code-router-img.png)

# Claude Code Router - Guía Rápida en Español 🇪🇸

> Herramienta para enrutar peticiones de Claude Code a diferentes modelos LLM y personalizar solicitudes.

[![Discord](https://img.shields.io/badge/Discord-%235865F2.svg?&logo=discord&logoColor=white)](https://discord.gg/rdftVMaUcS)
[![Licencia](https://img.shields.io/github/license/musistudio/claude-code-router)](https://github.com/musistudio/claude-code-router/blob/main/LICENSE)

## ✨ Características

- **Enrutamiento de Modelos**: Dirige peticiones a diferentes modelos según la tarea (background, razonamiento, contexto largo)
- **Multi-Proveedor**: Soporta OpenAI, DeepSeek, Ollama, Gemini, OpenRouter y proxies compatibles
- **Cambio Dinámico**: Cambia modelos sobre la marcha con `/model provider,modelo`
- **UI Web**: Interfaz gráfica con `ccr ui` para configuración visual
- **Optimización de Costos**: Usa modelos más baratos para tareas simples automáticamente

---

## 🚀 Instalación Rápida

### 1. Instalar Claude Code (si no lo tienes)
```bash
npm install -g @anthropic-ai/claude-code
```

### 2. Instalar Claude Code Router desde este fork

```bash
# Clonar el repositorio
git clone https://github.com/javierludena/claude-code-router.git
cd claude-code-router

# Instalar dependencias
npm install

# Compilar el proyecto
npm run build

# Instalar globalmente
npm link
```

**Verificar instalación:**
```bash
ccr -v
```

### 3. Crear Configuración

Crea el archivo de configuración en `~/.claude-code-router/config.json` (el router lo buscará automáticamente en tu directorio home):

**Windows:** `C:\Users\TU_USUARIO\.claude-code-router\config.json`
**Linux/Mac:** `~/.claude-code-router/config.json`

**Para proxy OpenAI compatible (ejemplo con Altia):**
```json
{
  "LOG": true,
  "PORT": 3456,
  "Providers": [
    {
      "name": "altia",
      "api_base_url": "https://llmproxy.altia.es/v1/chat/completions",
      "api_key": "TU_API_KEY_AQUI",
      "models": [
        "mycopilotgold-anthropic-claude-sonnet-4.5",
        "mycopilotgold-claude-haiku-4.5",
        "mycopilotgold-gemini-2.5-pro"
      ]
    }
  ],
  "Router": {
    "default": "altia,mycopilotgold-anthropic-claude-sonnet-4.5",
    "background": "altia,mycopilotgold-claude-haiku-4.5",
    "think": "altia,mycopilotgold-anthropic-claude-sonnet-4.5",
    "longContext": "altia,mycopilotgold-gemini-2.5-pro",
    "longContextThreshold": 60000
  }
}
```

### 4. Iniciar el Router
```bash
ccr start
```

### 5. Ejecutar Claude Code
```bash
ccr code "tu prompt aquí"
```

---

## 📋 Comandos Útiles

```bash
ccr start        # Iniciar servicio
ccr stop         # Detener servicio
ccr restart      # Reiniciar servicio
ccr status       # Ver estado
ccr ui           # Abrir interfaz web
ccr model        # Selector de modelos interactivo
ccr code         # Ejecutar Claude Code con el router
claude           # Ejecutar Claude Code original que funciona con claude pro (suscripcion)
```

---

## ⚙️ Configuración Detallada

### Estructura del Router

- **`default`**: Modelo para tareas generales
- **`background`**: Modelo para tareas ligeras (usar modelo más barato como Haiku)
- **`think`**: Modelo para razonamiento complejo (Plan Mode)
- **`longContext`**: Modelo para contextos largos >60K tokens (recomendado: Gemini 2.5 Pro)
- **`longContextThreshold`**: Umbral de tokens para activar longContext (default: 60000)

### Ejemplos de Providers

**OpenAI / Proxies compatibles:**
```json
{
  "name": "openai",
  "api_base_url": "https://api.openai.com/v1/chat/completions",
  "api_key": "sk-...",
  "models": ["gpt-4", "gpt-4-turbo"]
}
```

**DeepSeek:**
```json
{
  "name": "deepseek",
  "api_base_url": "https://api.deepseek.com/chat/completions",
  "api_key": "sk-...",
  "models": ["deepseek-chat", "deepseek-reasoner"],
  "transformer": { "use": ["deepseek"] }
}
```

**Ollama (local):**
```json
{
  "name": "ollama",
  "api_base_url": "http://localhost:11434/v1/chat/completions",
  "api_key": "ollama",
  "models": ["qwen2.5-coder:latest"]
}
```

---

## 🔧 Variables de Entorno

Puedes usar variables de entorno para las API keys:

```json
{
  "OPENAI_API_KEY": "$OPENAI_API_KEY",
  "Providers": [
    {
      "api_key": "$OPENAI_API_KEY"
    }
  ]
}
```

---

## 📚 Documentación Completa

Para configuración avanzada, transformers personalizados, GitHub Actions, y más:
- [Documentación original (inglés)](https://github.com/musistudio/claude-code-router)
- [Blog del proyecto](blog/en/project-motivation-and-how-it-works.md)

---

## ❤️ Créditos

**Proyecto original:** [Claude Code Router](https://github.com/musistudio/claude-code-router) por [@musistudio](https://github.com/musistudio)

Este fork es una adaptación con guía en español.
