![](blog/images/claude-code-router-img.png)

# Claude Code Router (CCR)

> Enruta peticiones de Claude Code a diferentes modelos LLM según la tarea. Compatible con Altia MyCopilot, OpenRouter y más.

[![Discord](https://img.shields.io/badge/Discord-%235865F2.svg?&logo=discord&logoColor=white)](https://discord.gg/rdftVMaUcS)
[![Licencia](https://img.shields.io/github/license/musistudio/claude-code-router)](https://github.com/musistudio/claude-code-router/blob/main/LICENSE)

---

## ✨ Características

### Ventajas de Claude Code
- **Compatible con cualquier IDE**: Funciona en Visual Studio, VSCode, IntelliJ, etc. (se ejecuta desde terminal)
- **Más eficiente que Cline**: Llamadas optimizadas = **menor coste** en tokens
- **Sin dependencia de extensiones**: No necesitas instalar plugins en tu IDE

### Funcionalidades del Router
- **Enrutamiento Inteligente**: Asigna automáticamente el modelo óptimo según la tarea
- **Multi-Proveedor**: Soporta OpenAI, DeepSeek, Ollama, OpenRouter y proxies compatibles
- **Cambio Dinámico**: Cambia modelos sobre la marcha con `/model provider,modelo`
- **UI Web**: Interfaz gráfica con `ccr ui` para configuración y monitoreo
- **Optimización de Costos**: Usa Haiku para tareas simples, Sonnet para código complejo

---

## Instalación

### 1. Instalar Claude Code

Abre **CMD** y ejecuta:

```cmd
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

Añade al PATH de Windows:
```
C:\Users\TU_USUARIO\.local\bin
```

<img width="1558" height="713" alt="image" src="https://github.com/user-attachments/assets/62fca9c4-f7de-41af-af68-2db0b8ed1761" />

Verifica:
```cmd
claude --version
```

---

### 2. Instalar Node.js con NVM

Descarga [NVM for Windows](https://github.com/coreybutler/nvm-windows/releases) e instala Node.js:

```powershell
nvm install latest
nvm use latest
node -v
```

---

### 3. Instalar Claude Code Router

```bash
git clone https://github.com/javierludena/claude-code-router
cd claude-code-router
npm install
npm run build
npm link
```

Verifica:
```cmd
ccr -v
```

---

### 4. Crear configuración

El router busca la config en:
- **Windows:** `C:\Users\TU_USUARIO\.claude-code-router\config.json`
- **Linux/Mac:** `~/.claude-code-router/config.json`

Crea la carpeta automáticamente con:
```cmd
ccr start
```

Luego edita el `config.json` según tu licencia:

**MyCopilot Gold:**
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
        "mycopilotgold-claude-haiku-4.5"
      ]
    }
  ],
  "Router": {
    "default": "altia,mycopilotgold-claude-haiku-4.5",
    "background": "altia,mycopilotgold-claude-haiku-4.5",
    "think": "altia,mycopilotgold-anthropic-claude-sonnet-4.5",
    "longContext": "altia,mycopilotgold-claude-haiku-4.5",
    "longContextThreshold": 999999
  }
}
```

**MyCopilot Silver:**
```json
{
  "LOG": true,
  "PORT": 3456,
  "Providers": [
    {
      "name": "altia",
      "api_base_url": "https://llmproxy.altia.es/v1/chat/completions",
      "api_key": "TU_API_KEY_AQUI",
      "models": ["mycopilotsilver-claude-haiku-4.5"]
    }
  ],
  "Router": {
    "default": "altia,mycopilotsilver-claude-haiku-4.5",
    "background": "altia,mycopilotsilver-claude-haiku-4.5",
    "think": "altia,mycopilotsilver-claude-haiku-4.5",
    "longContext": "altia,mycopilotsilver-claude-haiku-4.5",
    "longContextThreshold": 999999
  }
}
```

**MyCopilot Bronze:**
```json
{
  "LOG": true,
  "PORT": 3456,
  "Providers": [
    {
      "name": "altia",
      "api_base_url": "https://llmproxy.altia.es/v1/chat/completions",
      "api_key": "TU_API_KEY_AQUI",
      "models": ["mycopilotbronze-gemini-2.5-flash"]
    }
  ],
  "Router": {
    "default": "altia,mycopilotbronze-gemini-2.5-flash",
    "background": "altia,mycopilotbronze-gemini-2.5-flash",
    "think": "altia,mycopilotbronze-gemini-2.5-flash",
    "longContext": "altia,mycopilotbronze-gemini-2.5-flash",
    "longContextThreshold": 999999
  }
}
```

**OpenRouter:**
```json
{
  "LOG": true,
  "PORT": 3456,
  "Providers": [
    {
      "name": "openrouter",
      "api_base_url": "https://openrouter.ai/api/v1/chat/completions",
      "api_key": "TU_API_KEY_AQUI",
      "models": [
        "anthropic/claude-sonnet-4.5",
        "anthropic/claude-haiku-4.5",
        "anthropic/claude-opus-4.1"
      ]
    }
  ],
  "Router": {
    "default": "openrouter,anthropic/claude-haiku-4.5",
    "background": "openrouter,anthropic/claude-haiku-4.5",
    "think": "openrouter,anthropic/claude-opus-4.1",
    "longContext": "openrouter,anthropic/claude-haiku-4.5",
    "longContextThreshold": 999999
  }
}
```

> **Tu API Key de Altia:** Generala en [llmproxy.altia.es](https://llmproxy.altia.es) con tus credenciales de Altia.

---

### 5. Arrancar y usar

Necesitas **2 terminales**:

**Terminal 1** — Arranca el proxy local (mantenlo abierto):
```cmd
ccr start
```
> El router queda activo en local escuchando peticiones. No cierres esta terminal.

**Terminal 2** — Ejecuta Claude Code apuntando al proxy:
```cmd
ccr code
```
> Abre Claude Code conectado al llmproxy de Altia (o el proveedor configurado).

---

## Comandos

| Comando | Función |
|---|---|
| `ccr start` | Arrancar el proxy local |
| `ccr stop` | Parar el proxy |
| `ccr restart` | Reiniciar |
| `ccr status` | Ver si está corriendo |
| `ccr code` | Abrir Claude Code con el router |
| `ccr ui` | Interfaz web de monitoreo |
| `ccr model` | Selector interactivo de modelo |
| `claude` | Claude Code original (suscripción Claude Pro) |

Cambio de modelo durante una sesión:
```bash
/model altia,mycopilotgold-anthropic-claude-sonnet-4.5
/model openrouter,anthropic/claude-haiku-4.5
```

---

## 💡 Cómo Usar Claude Code

### Atajos de Teclado

- **`Ctrl + Enter`**: Nueva línea para escribir más instrucciones
- **`Shift + Tab`**: Iterar entre los 3 modos:
  - **Auto-aceptar**: Ejecuta cambios automáticamente
  - **Plan**: Modo planificación (usa el modelo `think`)
  - **Normal**: Requiere aprobación manual para cada cambio

### Comandos Útiles

```bash
./init                    # Inicializar proyecto con CLAUDE.md
/model provider,modelo    # Cambiar modelo manualmente
/clear                    # Limpiar historial completo
/compact                  # Compactar historial (mantiene resumen)
```

#### /clear vs /compact

- **`/clear`**: Borra todo el historial de conversación. Empieza completamente fresco.
- **`/compact`**: Compacta el historial manteniendo un resumen, reduce tokens pero conserva contexto.

> **💡 Buena práctica:** No tengas miedo de usar `/clear` frecuentemente. Aunque parece drástico, **da mejores resultados** porque evita confusión de contextos antiguos. Úsalo al terminar cada tarea para empezar limpio.

### Comando /context

Muestra información sobre el uso de contexto actual:

```bash
/context
```

Verás algo como:
```
⛁ ⛁ ⛁ ⛁ ⛁  claude-sonnet-4-5 · 82k/200k tokens (41%)
⛁ System prompt: 2.1k tokens
⛁ Messages: 22.1k tokens
⛶ Free space: 118k (59.2%)
```

---

## Recursos

- **Documentación oficial Claude Code:** [claude.ai/code](https://claude.ai/code)
- **Cursos oficiales de Claude:** [anthropic.skilljar.com](https://anthropic.skilljar.com/)
- **Portal de licencias Altia:** [llmproxy.altia.es](https://llmproxy.altia.es)
- **Documentación avanzada (inglés):** [musistudio/claude-code-router](https://github.com/musistudio/claude-code-router)

---

## ❤️ Créditos

**Proyecto original:** [Claude Code Router](https://github.com/musistudio/claude-code-router) por [@musistudio](https://github.com/musistudio)

Este fork es una adaptación con guía en español.

---

## Precios de Modelos

### Claude Haiku 4.5
| | Precio |
|---|---|
| Context Window | 200,000 tokens |
| Input | $1.00 / M tokens |
| Cache writes | $1.25 / M tokens |
| Cache reads | $0.10 / M tokens |
| Output | $5.00 / M tokens |

### Claude Sonnet 4.5
| | Precio |
|---|---|
| Context Window | 200,000 tokens |
| Input | $3.00 / M tokens |
| Cache writes | $3.75 / M tokens |
| Cache reads | $0.30 / M tokens |
| Output | $15.00 / M tokens |

### Claude Opus 4.1
| | Precio |
|---|---|
| Context Window | 200,000 tokens |
| Input | $15.00 / M tokens |
| Cache writes | $18.75 / M tokens |
| Cache reads | $1.50 / M tokens |
| Output | $75.00 / M tokens |

### Gemini 2.5 Flash (Bronze)
| | Precio |
|---|---|
| Context Window | 1,048,576 tokens |
| Input | $0.30 / M tokens |
| Cache writes | — |
| Cache reads | $0.075 / M tokens |
| Output | $2.50 / M tokens |

> **Optimización:** Usa Haiku para tareas generales/ligeras, Sonnet para código complejo en modo Plan. Gemini Flash es el más económico pero con menor calidad autónoma para desarrollo.
