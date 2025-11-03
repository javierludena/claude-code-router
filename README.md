![](blog/images/claude-code-router-img.png)

# Claude Code Router - Guía Rápida en Español 🇪🇸

> Herramienta para enrutar peticiones de Claude Code a diferentes modelos LLM y personalizar solicitudes.

[![Discord](https://img.shields.io/badge/Discord-%235865F2.svg?&logo=discord&logoColor=white)](https://discord.gg/rdftVMaUcS)
[![Licencia](https://img.shields.io/github/license/musistudio/claude-code-router)](https://github.com/musistudio/claude-code-router/blob/main/LICENSE)

## ✨ Características

### Ventajas de Claude Code
- **Compatible con cualquier IDE**: Funciona en Visual Studio, VSCode, IntelliJ, etc. (se ejecuta desde terminal)
- **Más eficiente que Cline**: Llamadas optimizadas = **menor coste** en tokens
- **Sin dependencia de extensiones**: No necesitas instalar plugins en tu IDE

### Funcionalidades del Router
- **Enrutamiento Inteligente**: Asigna automáticamente el modelo óptimo según la tarea
- **Multi-Proveedor**: Soporta OpenAI, DeepSeek, Ollama, Gemini, OpenRouter y proxies compatibles
- **Cambio Dinámico**: Cambia modelos sobre la marcha con `/model provider,modelo`
- **UI Web**: Interfaz gráfica con `ccr ui` para configuración y monitoreo
- **Optimización de Costos**: Usa Haiku para tareas simples, Sonnet para código, Gemini para contextos largos

---

## 🚀 Instalación Rápida

### 0. Requisitos Previos: Instalar Node.js (si no lo tienes)

**Windows:**
1. Descarga e instala [NVM for Windows](https://github.com/coreybutler/nvm-windows/releases)
2. Instala la última versión de Node.js:
```bash
nvm install latest
nvm use latest
```

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

**Para proxy OpenAI compatible (ejemplo con Altia mycopilotgold):**
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
    "longContextThreshold": 24000
  }
}
```

**Para proxy OpenAI compatible (ejemplo con Altia mycopilotsilver):**
```
añadir aqui alguien cuando realice la configuracion y PR
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
- **`longContextThreshold`**: Umbral de tokens para activar longContext (default: 24000)

### Ver Configuración de Contexto

Para revisar cuánto contexto tiene configurado tu Claude Code:

```bash
# Ver configuración actual
cat ~/.claude/config.json

# Abrir UI del router para ver logs y uso
ccr ui
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

### Modelo de Largo Contexto

**Importante:** El router cuenta tokens por **petición individual**, no el contexto total acumulado.

**¿Cómo se activa?**
Cuando una sola petición supera 24k tokens, cambia automáticamente a Gemini (longContext).

**Ejemplo para probarlo:**
```bash
ccr code "Lee completamente el archivo package-lock.json y analízalo"
```

**Forzar manualmente:**
```bash
/model altia,mycopilotgold-gemini-2.5-pro
```

**Ajustar umbral:**
```json
{
  "Router": {
    "longContextThreshold": 30000  // Activar antes
  }
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
