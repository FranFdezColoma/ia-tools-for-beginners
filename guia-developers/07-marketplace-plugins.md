# 7. Marketplace y Plugins

[← Anterior: Agents, Skills y Custom Agents](06-agents-skills-instructions.md) | [Siguiente: Añadir plugins →](08-anadir-plugins.md)

---

## ¿Qué es un Plugin?

Un plugin es un **bundle instalable** que agrupa en una sola unidad:

- 🤖 **Agents** especializados para un dominio
- 🛠️ **Skills** para tareas específicas
- 🔧 **Tools** (herramientas externas, como servidores MCP)
- 📚 **Instructions** adicionales del dominio

```
plugin: power-platform-dataverse
├── Skills/
│   ├── dataverse-query/        ← Consultas FetchXML y OData
│   ├── dataverse-crud/         ← Operaciones CRUD
│   └── dataverse-schema/       ← Exploración del esquema
├── Tools/
│   └── mcp-server-config.json  ← Configuración del servidor MCP
└── Instructions/
    └── dataverse-guidelines.md ← Buenas prácticas del equipo
```

**Diferencia con un Skill suelto:**

| | Skill suelto | Plugin |
|---|---|---|
| **Instalación** | Manual (copias la carpeta a `.github/skills/`) | Automática (un comando instala todo) |
| **Contenido** | Un solo skill | Múltiples skills + tools + instructions |
| **Mantenimiento** | Manual | Automático (el marketplace gestiona versiones) |
| **Cuándo usarlo** | Skills muy específicos del proyecto | Dominios completos (Dataverse, Azure, etc.) |

---

## ¿Qué es el Marketplace?

El marketplace de Agent Skills es un **ecosistema de repositorios públicos en GitHub** donde Microsoft, Anthropic, GitHub y la comunidad publican colecciones de skills, plugins e instrucciones listas para usar.

El estándar [agentskills.io](https://agentskills.io) garantiza compatibilidad: un skill publicado en cualquier marketplace funciona en **GitHub Copilot CLI, Claude Code y VS Code agent mode** indistintamente.

---

## Cómo instalar un plugin

```bash
# Copilot CLI
copilot
> /plugin marketplace add <owner>/<repo>    # Añadir el marketplace
> /plugin install <nombre-plugin>@<marketplace>  # Instalar el plugin
> /plugin list                              # Verificar instalación

# Claude Code (mismos comandos)
claude
> /plugin marketplace add <owner>/<repo>
> /plugin install <nombre-plugin>@<marketplace>
> /plugin list
```

### Gestionar plugins instalados

```bash
> /plugin list                    # Ver plugins instalados
> /plugin marketplace list        # Ver marketplaces disponibles
> /plugin uninstall <nombre>      # Desinstalar un plugin
```

---

## Repositorios principales

| Repositorio | Mantenido por | Qué cubre |
|-------------|---------------|-----------|
| [microsoft/power-platform-skills](https://github.com/microsoft/power-platform-skills) | Microsoft (oficial) | Power Pages, Model-Driven Apps |
| [microsoft/skills](https://github.com/microsoft/skills) | Microsoft (oficial) | 132 skills del Azure SDK (.NET, Python, TypeScript, Java) |
| [github/awesome-copilot](https://github.com/github/awesome-copilot) | Comunidad (GitHub) | 14.6k ⭐ — Skills para todos los stacks, MCP de Dataverse |
| [anthropics/skills](https://github.com/anthropics/skills) | Anthropic (oficial) | 69k ⭐ — Documentos, diseño frontend, creador de skills |

---

## Detalle de cada repositorio

### `microsoft/power-platform-skills`

El repositorio oficial de Microsoft para Power Platform:

- **Power Pages**: creación y despliegue de Code Sites (SPAs) con React, Angular, Vue o Astro via PAC CLI
- **Power Apps generative pages** (próximamente): React + TypeScript + Fluent UI para Model-Driven Apps

> ⚠️ **Nota:** No existe plugin oficial para PCF en este repositorio. El soporte para PCF está disponible como instrucciones en `github/awesome-copilot`.

```bash
> /plugin marketplace add microsoft/power-platform-skills
> /plugin install power-pages@power-platform-skills
```

### `microsoft/skills`

132 skills para el Azure SDK en cuatro lenguajes. Útil para desarrollo con .NET, Python, TypeScript y Java.

```bash
> /plugin marketplace add microsoft/skills
```

### `github/awesome-copilot`

Repositorio de referencia de la comunidad:

- Instructions para todos los stacks (React, Vue, Django, FastAPI, Rust, Go, etc.)
- Colecciones de skills curadas por tecnología
- Plugin de Dataverse con el skill `mcp-configure`
- Skills de productividad transversales (Git, Docker, CI/CD)

```bash
> /plugin marketplace add github/awesome-copilot
> /plugin install dataverse@awesome-copilot
```

### `anthropics/skills`

Repositorio de referencia de Anthropic:

- Análisis y transformación de documentos
- Diseño y maquetación frontend
- Skill para **crear nuevos skills** (útil para equipos que quieren publicar los suyos)

```bash
> /plugin marketplace add anthropics/skills
```

---

## Estrategia por perfil

### Desarrollador Power Platform / Dataverse

```bash
> /plugin marketplace add microsoft/power-platform-skills
> /plugin marketplace add github/awesome-copilot
> /plugin install power-pages@power-platform-skills
> /plugin install dataverse@awesome-copilot
```

### Desarrollador .NET / Azure

```bash
> /plugin marketplace add microsoft/skills
> /plugin marketplace add github/awesome-copilot
```

### Desarrollador frontend (React / TypeScript)

```bash
> /plugin marketplace add github/awesome-copilot
> /plugin marketplace add anthropics/skills
```

### Desarrollador full-stack (Node.js / Python)

```bash
> /plugin marketplace add github/awesome-copilot
> /plugin marketplace add anthropics/skills
```

---

## Compatibilidad

Los plugins del estándar Agent Skills son compatibles con:

- ✅ GitHub Copilot CLI
- ✅ Claude Code
- ✅ VS Code en modo agente

Un plugin instalado una vez funciona con cualquiera de las tres herramientas.

**Límite recomendado: 4–8 skills activos por proyecto.** Con demasiados skills, los metadatos consumen ventana de contexto que debería estar disponible para tu código (_context rot_).

---

## Crear un skill personalizado para tu equipo

Si ningún skill del marketplace cubre tus necesidades, crea el tuyo:

```bash
mkdir -p .github/skills/mi-workflow-personalizado
# Crea .github/skills/mi-workflow-personalizado/SKILL.md
# Ver sección 6 para la estructura del fichero SKILL.md
```

El skill estará disponible automáticamente en la próxima sesión y es compartido por todo el equipo al estar en `.github/`.

---

## Referencias

- [agentskills.io](https://agentskills.io) — Especificación del estándar
- [microsoft/power-platform-skills](https://github.com/microsoft/power-platform-skills)
- [github/awesome-copilot](https://github.com/github/awesome-copilot)
- [anthropics/skills](https://github.com/anthropics/skills)
- [microsoft/skills](https://github.com/microsoft/skills)

---

[← Anterior: Agents, Skills y Custom Agents](06-agents-skills-instructions.md) | [Siguiente: Añadir plugins →](08-anadir-plugins.md)
