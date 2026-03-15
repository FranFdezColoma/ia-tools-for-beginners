# 4. Primeros pasos y autenticación

[← Anterior: Instalación](03-instalacion.md) | [Siguiente: Custom Instructions →](05-custom-instructions.md)

---

## Flujo de inicio de sesión

### GitHub Copilot CLI

```powershell
# 1. Navega a la raíz del repositorio
cd C:\Proyectos\MiProyecto   # Windows
# cd ~/proyectos/mi-proyecto  # macOS/Linux

# 2. Lanza el CLI
copilot
```

La primera vez te pedirá que confirmes que confías en el directorio:

```
? Do you trust the files in this folder?
  ❯ Trust (for this session)
    Trust (always)
    Don't trust
```

Si es la primera vez o el token ha expirado:

```
> /login
# 1. El CLI muestra un código de un solo uso (ej: ABCD-1234)
# 2. Se abre el navegador en github.com/login/device
# 3. Introduce el código → Authorize
# 4. Vuelve al terminal — ya estás autenticado
```

> El login persiste entre sesiones. Solo necesitas hacerlo una vez hasta que el token expire.

### Claude Code

```bash
# 1. Navega a la raíz del repositorio
cd ~/proyectos/mi-proyecto

# 2. Lanza Claude Code
claude
# La primera vez, el flujo OAuth de Anthropic se inicia automáticamente en el navegador.
# No hace falta ningún comando adicional de login.
```

---

## Verificar que funciona

```bash
# Copilot CLI
> Di hola y dime qué puedes hacer en este proyecto

# Claude Code (misma petición)
> Di hola y dime qué puedes hacer en este proyecto
```

Recibirás un resumen de las capacidades del agente y el contexto que ha detectado del proyecto.

---

## Inicializar el proyecto (recomendado)

> ⚠️ Antes de ejecutar `/init`, lee la [sección 5](05-custom-instructions.md) para elegir entre crearlo automáticamente o manualmente.

```bash
# Copilot CLI
> /init
# El agente analiza la estructura del proyecto y propone .github/copilot-instructions.md

# Claude Code
> /init
# El agente analiza el proyecto y propone CLAUDE.md en la raíz
```

---

## Comandos esenciales

### Navegación y sesión

| Acción | Copilot CLI | Claude Code |
|--------|-------------|-------------|
| Ayuda | `/help` | `/help` |
| Cerrar sesión | `/exit` | `/exit` |
| Iniciar sesión | `/login` | Automático al lanzar |
| Cerrar sesión de cuenta | `/logout` | `/logout` |
| Cambiar modelo | `/model` | `/model` |

### Gestión del proyecto

| Acción | Copilot CLI | Claude Code |
|--------|-------------|-------------|
| Generar instrucciones | `/init` | `/init` |
| Ver cambios de la sesión | `/diff` | `/diff` |
| Revisar staged/unstaged | `/review` | `/review` |
| Deshacer cambios | `Esc+Esc` | `Esc+Esc` |

### Modos y ejecución

| Acción | Copilot CLI | Claude Code |
|--------|-------------|-------------|
| Alternar Plan/Autopilot | `Shift+Tab` | `Shift+Tab` |
| Delegar en segundo plano | `& <prompt>` | ❌ No disponible |
| Retomar sesión delegada | `/resume` | ❌ No disponible |

### Skills y plugins

| Acción | Copilot CLI | Claude Code |
|--------|-------------|-------------|
| Listar skills | `/skills` | `/skills` |
| Añadir marketplace | `/plugin marketplace add <repo>` | `/plugin marketplace add <repo>` |
| Instalar plugin | `/plugin install <plugin>@<marketplace>` | `/plugin install <plugin>@<marketplace>` |

### IDE

| Acción | Copilot CLI | Claude Code |
|--------|-------------|-------------|
| Conectar con VS Code / VS | `/ide` | ❌ No disponible (usa integración nativa de Claude Code) |

---

## Referenciar ficheros en el prompt

```bash
# Copilot CLI
> Analiza @src/auth/loginService.ts y explica qué hace el método refreshToken
> Refactoriza @src/components/UserProfile.tsx para separar la lógica de la UI

# Claude Code (misma sintaxis)
> Analiza @src/auth/loginService.ts y explica qué hace el método refreshToken
```

---

## Flujo de trabajo habitual

```bash
# Copilot CLI
cd ~/proyectos/mi-proyecto
copilot
> Añade validación de email al formulario de registro y genera tests unitarios
# Si Plan Mode está activo: revisa el plan → aprueba → ejecuta
> /diff          # Revisa los cambios
# Esc+Esc        # Si algo no está bien, deshaz

# Claude Code — flujo equivalente
cd ~/proyectos/mi-proyecto
claude
> Añade validación de email al formulario de registro y genera tests unitarios
> /diff
```

---

[← Anterior: Instalación](03-instalacion.md) | [Siguiente: Custom Instructions →](05-custom-instructions.md)
