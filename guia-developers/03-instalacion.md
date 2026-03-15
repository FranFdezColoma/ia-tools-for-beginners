# 3. Instalación

[← Anterior: Elección del modelo](02-eleccion-modelo.md) | [Siguiente: Primeros pasos →](04-primeros-pasos.md)

---

## Prerrequisitos (ambas herramientas)

- **Node.js v18 o superior** (recomendado v22+)
- **npm v9 o superior**

```powershell
node --version   # Debe ser v18.x.x o superior
npm --version    # Debe ser v9.x.x o superior
```

Si no tienes Node.js: [nodejs.org](https://nodejs.org) o usa [nvm-windows](https://github.com/coreybutler/nvm-windows) (Windows) / [nvm](https://github.com/nvm-sh/nvm) (macOS/Linux).

---

## GitHub Copilot CLI

**Requisito adicional:** cuenta de GitHub con suscripción activa a Copilot (Pro, Business o Enterprise).

### Instalación

```bash
# Todos los sistemas (recomendado)
npm install -g @github/copilot

# macOS / Linux (Homebrew — se actualiza con brew upgrade)
brew install copilot-cli

# Windows (WinGet — se actualiza con winget upgrade)
winget install GitHub.Copilot

# macOS / Linux (script oficial)
curl -fsSL https://gh.io/copilot-install | bash
```

> **GitHub Codespaces:** Copilot CLI viene preinstalado. No necesitas instalarlo.

### Verificar la instalación

```bash
copilot --version
```

Si el comando no se reconoce tras instalar con npm, añade el directorio global de npm a tu PATH:

```powershell
# Windows
npm config get prefix
# Añade la ruta mostrada + \node_modules\.bin a tu variable de entorno PATH

# macOS/Linux — añadir al .zshrc o .bashrc
export PATH="$(npm config get prefix)/bin:$PATH"
```

---

## Claude Code

**Requisito adicional:** suscripción activa de Anthropic.

### Instalación

```bash
# Todos los sistemas (npm)
npm install -g @anthropic-ai/claude-code

# macOS / Linux (Homebrew)
brew install claude-code
```

### Verificar la instalación

```bash
claude --version
```

Al lanzar `claude` por primera vez, el flujo de autenticación OAuth se inicia automáticamente en el navegador.

---

## Comparativa de instalación

| | GitHub Copilot CLI | Claude Code |
|---|---|---|
| Comando npm | `npm install -g @github/copilot` | `npm install -g @anthropic-ai/claude-code` |
| Comando para lanzar | `copilot` | `claude` |
| Verificar versión | `copilot --version` | `claude --version` |
| Autenticación | GitHub OAuth (`/login`) | Anthropic OAuth (automática al lanzar) |
| Prerequisito de cuenta | GitHub Copilot (Pro/Business/Enterprise) | Suscripción Anthropic |

---

## Resolución de problemas comunes

### "command not found" tras instalar con npm

```bash
# Si falló brew, prueba npm:
npm install -g @github/copilot

# O el script oficial:
curl -fsSL https://gh.io/copilot-install | bash
```

### "You don't have access to GitHub Copilot"

1. Verifica tu suscripción en [github.com/settings/copilot](https://github.com/settings/copilot)
2. Si usas cuenta de trabajo, comprueba que tu organización permite el acceso al CLI

### Token expirado

```bash
# Copilot CLI
copilot
> /login

# Claude Code — ejecuta de nuevo y sigue el flujo OAuth
claude
```

---

[← Anterior: Elección del modelo](02-eleccion-modelo.md) | [Siguiente: Primeros pasos →](04-primeros-pasos.md)
