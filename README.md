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
- **Multi-Proveedor**: Soporta OpenAI, DeepSeek, Ollama, OpenRouter y proxies compatibles (Gemini no recomendado para desarrollo)
- **Cambio Dinámico**: Cambia modelos sobre la marcha con `/model provider,modelo`
- **UI Web**: Interfaz gráfica con `ccr ui` para configuración y monitoreo
- **Optimización de Costos**: Usa Haiku para tareas simples, Sonnet para código complejo

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
#Permite todos los scripts pero avisa sobre scripts descargados:
Set-ExecutionPolicy Unrestricted -Scope CurrentUser

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

El router busca automáticamente el archivo `config.json` en:
- **Windows:** `C:\Users\TU_USUARIO\.claude-code-router\config.json`
- **Linux/Mac:** `~/.claude-code-router/config.json`

#### 📁 Archivos de Configuración Disponibles:

- **`config.altia.example.json`** - Solo Altia mycopilotgold
- **`config.openrouter.example.json`** - Solo OpenRouter
- **`config.example.json`** - Ambos proveedores (puedes cambiar entre ellos con `/model`)

---

### Ejemplos de Configuración Individual por Proveedor:

**Altia mycopilotgold (Sonnet + Haiku):**
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

**Altia mycopilotsilver (solo Haiku):**
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
        "mycopilotsilver-claude-haiku-4.5"
      ]
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

**OpenRouter (múltiples modelos):**
```json
{
  "LOG": true,
  "LOG_LEVEL": "info",
  "PORT": 3456,
  "HOST": "127.0.0.1",
  "APIKEY": "mi-clave-router-local",
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

**Altia + OpenRouter (configuración dual):**
```json
{
  "LOG": true,
  "PORT": 3456,
  "Providers": [
    {
      "name": "altia",
      "api_base_url": "https://llmproxy.altia.es/v1/chat/completions",
      "api_key": "TU_API_KEY_ALTIA",
      "models": [
        "mycopilotgold-anthropic-claude-sonnet-4.5",
        "mycopilotgold-claude-haiku-4.5",
        "mycopilotsilver-claude-haiku-4.5"
      ]
    },
    {
      "name": "openrouter",
      "api_base_url": "https://openrouter.ai/api/v1/chat/completions",
      "api_key": "TU_API_KEY_OPENROUTER",
      "models": [
        "anthropic/claude-sonnet-4.5",
        "anthropic/claude-haiku-4.5",
        "anthropic/claude-opus-4.1"
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





### 4. Iniciar el Router
```bash
#es necesario tener el terminal abierto o en la misma sesion
ccr start
```

### 5. Ejecutar Claude Code
```bash
ccr code
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

## 🔄 Cambiar Entre Proveedores

### Opción 1: Cambio Dinámico con `/model` (Recomendado)

Durante una sesión de Claude Code, cambia de proveedor/modelo sin reiniciar:

```bash
# Comenzar sesión con Altia (predeterminado)
ccr code "analiza este código"

# Dentro de la sesión, cambiar a OpenRouter (Claude Sonnet)
/model openrouter,anthropic/claude-3.5-sonnet

# Cambiar a OpenRouter (Claude Haiku - más económico)
/model openrouter,anthropic/claude-3-haiku

# Cambiar de nuevo a Altia
/model altia,mycopilotgold-claude-haiku-4.5

# Ver todos los modelos disponibles
/model
```

### Opción 2: Selector Interactivo

Abre un menú interactivo para elegir proveedor y modelo:

```bash
ccr model
```

Verás un menú como:
```
? Select a provider:
❯ altia
  openrouter
  
? Select a model for altia:
❯ mycopilotgold-anthropic-claude-sonnet-4.5
  mycopilotgold-claude-haiku-4.5
```

### Opción 3: Cambiar de Proveedor Completo

Si tienes configuraciones separadas, puedes cambiar fácilmente entre ellas:

```powershell
# Cambiar a configuración de Altia
copy config.altia.example.json "$env:USERPROFILE\.claude-code-router\config.json"
ccr restart

# Cambiar a configuración de OpenRouter
copy config.openrouter.example.json "$env:USERPROFILE\.claude-code-router\config.json"
ccr restart

# Cambiar a configuración dual (ambos proveedores)
copy config.example.json "$env:USERPROFILE\.claude-code-router\config.json"
ccr restart
```

**💡 Tip:** Guarda tus archivos de configuración con tus API keys en una carpeta segura para poder cambiar rápidamente:

```powershell
# Guardar configuraciones con API keys
copy "$env:USERPROFILE\.claude-code-router\config.json" config.altia.mykeys.json
copy "$env:USERPROFILE\.claude-code-router\config.json" config.openrouter.mykeys.json

# Cambiar entre ellas cuando necesites
copy config.altia.mykeys.json "$env:USERPROFILE\.claude-code-router\config.json"
ccr restart
```

---

---

## ⚙️ Configuración Detallada

### Estructura del Router

- **`default`**: Modelo para tareas generales (recomendado: Haiku para ahorro, Sonnet para calidad)
- **`background`**: Modelo para tareas ligeras y automáticas (recomendado: Haiku - menor coste)
- **`think`**: Modelo para razonamiento complejo y Plan Mode (recomendado: Sonnet 4.5)
- **`longContext`**: Modelo para contextos largos >60k tokens (No tengo claro cual recomendar)
- **`longContextThreshold`**: Umbral de tokens para activar longContext (recomendado: 999999 para desactivar cambio automático)

### Ver Configuración de Contexto

Para revisar cuánto contexto tiene configurado tu Claude Code:

```bash
# Ver configuración actual
cat ~/.claude/config.json

# Abrir UI del router para ver logs y uso
ccr ui
```

---

## 💰 Precios de Modelos

### Claude-Haiku 4.5

Context Window: 200,000 tokens\
Input price: $1.00/million tokens\
Cache writes price: $1.25/million tokens\
Cache reads price: $0.10/million tokens\
Output price: $5.00/million tokens

#### Gemini 2.5 pro

Context Window: 1,048,576 tokens\
Input price: $1.25/million tokens\
Cache writes price: $1.63/million tokens\
Cache reads price: $0.13/million tokens\
Output price: $10.00/million tokens

### Claude-Sonnet 4.5

Context Window: 200,000 tokens\
Input price: $3.00/million tokens\
Cache writes price: $3.75/million tokens\
Cache reads price: $0.30/million tokens\
Output price: $15.00/million tokens

### Claude Opus 4.1

Context Window: 200,000 tokens\
Input price: $15.00/million tokens\
Cache writes price: $18.75/million tokens\
Cache reads price: $1.50/million tokens\
Output price: $75.00/million tokens


> **💡 Optimización de costos:** Usa Haiku para tareas ligeras y generales, Sonnet para tareas complejas en modo Plan (think).

### ⚠️ NO SE RECOMIENDA Gemini 2.5 Pro para Desarrollo

**Importante:** Aunque Gemini maneja contextos muy largos (hasta 2M tokens), **su calidad de trabajo autónomo es significativamente inferior** a Claude para desarrollo de software.

**Problema real con Gemini - Ejemplo de conversación:**
```
Usuario: "Modifica el método ImportFotoAlbaran"

Gemini:
● Read(archivo.cs) → Error: File too large
● Search(pattern: "ImportFotoAlbaran") → Found 0 lines
  ⎿ Interrumpido

Usuario TIENE QUE intervenir y hacer el trabajo de investigación:
> "El error está en Altia.ControlTower.eURD.Web\App_Start\AutoMapperConfig.cs
   línea 271, posiblemente te falte en el DTO añadir el método"
  ⎿ Read AutoMapperConfig.cs (42 lines)

<<<<<<< HEAD
Usuario TIENE QUE seguir guiando:
> "También tienes que comprobar el fichero de mapping de automapper
   en altia.controltower.portal.web/automapper/automapperconfig.cs
   en la zona de expediciones. Gracias por la ayuda."

Gemini: "Soy Gemini, un modelo de lenguaje grande, entrenado por Google."
```

**Con Claude Sonnet/Haiku (trabajo autónomo correcto):**
```
● Read(archivo.cs) → Error: File too large
● Search(pattern: "ImportFotoAlbaran", Controllers) → Found 56 lines
● Search(pattern: "ImportFotoAlbaran", AutoMapperConfig) → Found
● Search(pattern: "LOGF_ExpeditionsFiles", ControllersApi) → Found 141 lines
● Search(pattern: "DTO definitions", Core) → Found 127 lines
● TodoWrite: [5 pasos de modificación]
● Edit(AutoMapperConfig.cs) → ✅
● Edit(DTO.cs) → ✅
```

**Conclusión:** Gemini puede leer mucho contexto, pero **no lo procesa eficientemente para trabajo autónomo**. Claude Haiku/Sonnet son mucho mejores para desarrollo.

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

**Recomendación:** Mantén `longContextThreshold` en 999999 para **desactivar** el cambio automático de modelo. Claude Haiku y Sonnet manejan bien archivos grandes usando búsquedas y lectura por secciones.

**Si necesitas contextos extremadamente largos:**
- Claude Sonnet maneja hasta 200k tokens eficientemente
- Usa comandos como Search y Read con offset/limit para archivos muy grandes
- NO uses Gemini (ver advertencia en sección de Precios)

**Configuración recomendada:**
```json
{
  "Router": {
    "longContext": "altia,mycopilotgold-claude-haiku-4.5",
    "longContextThreshold": 999999  // Nunca cambia automáticamente
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
