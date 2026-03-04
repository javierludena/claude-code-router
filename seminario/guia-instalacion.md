# Guía de Instalación: Claude Code + CCR para Altia
**Windows — Instalación nativa**

---

## Paso 1 — Instalar Claude Code

Abre **CMD** (no PowerShell) y ejecuta:

```cmd
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

Esto instala Claude Code de forma nativa en Windows. Al terminar verifica:

```cmd
claude --version
```

---

## Paso 2 — Instalar Node.js con NVM (necesario para el Router)

Descarga e instala **NVM for Windows** desde:
`https://github.com/coreybutler/nvm-windows/releases`

Luego en CMD:

```cmd
nvm install latest
nvm use latest
node -v
```

---

## Paso 3 — Instalar Claude Code Router (CCR)

```cmd
git clone https://github.com/javierludena/claude-code-router.git
cd claude-code-router
npm install
npm run build
npm link
```

Verifica que está instalado:

```cmd
ccr -v
```

---

## Paso 4 — Crear el archivo de configuración

El router busca la configuración en:
```
C:\Users\TU_USUARIO\.claude-code-router\config.json
```

Crea la carpeta si no existe:

```cmd
mkdir %USERPROFILE%\.claude-code-router
```

---

## Paso 5 — Configurar según tu licencia

Crea el archivo `config.json` en la carpeta del paso anterior con el contenido de tu licencia:

### MyCopilot Gold

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

### MyCopilot Silver

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

> **Tu API Key:** Generala en `https://llmproxy.altia.es` con tus credenciales de Altia.

---

## Paso 6 — Arrancar y verificar

```cmd
ccr start
ccr status
ccr code
```

Si todo está bien, `ccr code` abre Claude Code conectado al proxy de Altia.

---

## Uso diario

| Comando | Función |
|---|---|
| `ccr start` | Arrancar el proxy local |
| `ccr stop` | Parar el proxy |
| `ccr code` | Abrir Claude Code |
| `ccr status` | Ver si está corriendo |

---

## Recursos

- **Documentación oficial Claude Code:** `https://claude.ai/code`
- **Cursos oficiales de Claude:** `https://anthropic.skilljar.com/`
- **Portal de licencias Altia:** `https://llmproxy.altia.es`
- **Repositorio CCR corporativo:** `https://github.com/javierludena/claude-code-router`
