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

**Para proxy OpenAI compatible (ejemplo con Altia mycopilotsilver):**
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



### 4. Iniciar el Router
```bash
#es necesario tener el terminal abierto o en la misma sesion
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

- **`default`**: Modelo para tareas generales (recomendado: Haiku para ahorro, Sonnet para calidad)
- **`background`**: Modelo para tareas ligeras y automáticas (recomendado: Haiku - menor coste)
- **`think`**: Modelo para razonamiento complejo y Plan Mode (recomendado: Sonnet 4.5)
- **`longContext`**: Modelo para contextos largos >60k tokens (recomendado: Haiku o Sonnet. NO usar Gemini)
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

### Gemini 2.5 Pro
**Pricing:**
- Input: $1.25/1M tokens
- Output: $10.00/1M tokens

### Claude Sonnet 4.5
**Pricing:**
- Input: $3.00/1M tokens
- Output: $15.00/1M tokens

### Claude Haiku 4.5
**Pricing:**
- Input: $1.00/1M tokens
- Output: $5.00/1M tokens

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

**Problemas de Gemini:**
- ❌ **TÚ tienes que buscar** dónde está el error
- ❌ **TÚ tienes que decirle** qué archivos revisar
- ❌ **TÚ tienes que investigar** las líneas problemáticas
- ❌ No busca proactivamente en múltiples ubicaciones
- ❌ No encadena búsquedas automáticamente
- ❌ Te conviertes en su asistente, no al revés

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
