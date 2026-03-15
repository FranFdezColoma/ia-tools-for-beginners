# 9. MCP: instalación, configuración y ejemplos

[← Anterior: Añadir plugins](08-anadir-plugins.md) | [← Volver al índice](README.md)

---

## ¿Qué es MCP y en qué se diferencia de un plugin?

**Model Context Protocol (MCP)** es un protocolo abierto que permite a los agentes de IA conectarse a **servicios externos** en tiempo real: bases de datos, APIs, herramientas de CI/CD, sistemas de gestión de contenido, etc.

| | Plugin | MCP Server |
|---|---|---|
| **Qué aporta** | Skills, agents e instructions | Conexión en tiempo real a un servicio externo |
| **Qué puede hacer** | Mejorar cómo razona el agente sobre un dominio | Consultar y ejecutar operaciones contra el servicio |
| **Ejemplo** | Plugin de Dataverse → instrucciones para escribir FetchXML | MCP de Dataverse → leer el esquema real de tu entorno |
| **Requiere servidor** | No | Sí (proceso o HTTP server que implementa MCP) |

> Un plugin y un MCP son complementarios: el plugin de Dataverse instala un skill que ayuda al agente a **configurar** el servidor MCP de Dataverse.

---

## Instalación y configuración paso a paso

### Paso 1: Instalar el servidor MCP

Los servidores MCP se instalan como paquetes npm o ejecutables. El proceso es el mismo para Copilot CLI y Claude Code.

```bash
# Ejemplo con el servidor MCP de Azure DevOps
npm install -g @azure-devops/mcp-server

# Verificar que está disponible
azure-devops-mcp --version
```

### Paso 2: Registrar el servidor en el fichero de configuración

#### GitHub Copilot CLI

```
Configuración global (todos los proyectos):
  ~/.copilot/mcp-config.json

Configuración por proyecto (solo este repositorio):
  .mcp/copilot/mcp.json
```

```json
{
  "mcpServers": {
    "NombreServidor": {
      "type": "stdio",
      "command": "nombre-ejecutable",
      "args": ["--arg1", "valor1"],
      "env": {
        "TOKEN": "tu-token-aquí"
      }
    }
  }
}
```

Para servidores HTTP (como el MCP de Dataverse):

```json
{
  "mcpServers": {
    "DataverseMcp": {
      "type": "http",
      "url": "https://tuorganizacion.crm.dynamics.com/api/mcp"
    }
  }
}
```

#### Claude Code

```
Configuración global (todos los proyectos):
  ~/.claude.json

Configuración por proyecto (solo este repositorio):
  .mcp.json  (raíz del proyecto)
```

```json
{
  "mcpServers": {
    "NombreServidor": {
      "command": "nombre-ejecutable",
      "args": ["--arg1", "valor1"],
      "env": {
        "TOKEN": "tu-token-aquí"
      }
    }
  }
}
```

> Claude Code no requiere el campo `"type"` en la configuración; infiere el tipo automáticamente.

### Paso 3: Verificar que el servidor está activo

```bash
# Copilot CLI — reinicia la sesión y verifica
copilot
> /mcp list
# NombreServidor  ✓  connected

# Claude Code — reinicia y verifica
claude
> /mcp list
# NombreServidor  ✓  connected
```

Si el servidor no aparece como conectado:
1. Verifica que el ejecutable está en tu PATH
2. Comprueba que el fichero de configuración tiene JSON válido
3. Revisa los logs de error en el terminal donde lanzaste el CLI

---

## Ejemplo completo: MCP de Azure DevOps

Azure DevOps MCP permite al agente consultar proyectos, pipelines, work items y sprints directamente, sin salir del terminal.

### Instalación del servidor

```bash
npm install -g @azure-devops/mcp-server
```

### Configuración del fichero

Sustituye `TU_ORGANIZACION` por el nombre de tu organización en Azure DevOps y `TU_PAT_TOKEN` por un [Personal Access Token](https://learn.microsoft.com/es-es/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) con permisos de lectura y escritura sobre Work Items, Build y Code.

**GitHub Copilot CLI** — `~/.copilot/mcp-config.json` (global) o `.mcp/copilot/mcp.json` (proyecto):

```json
{
  "mcpServers": {
    "AzureDevOps": {
      "type": "stdio",
      "command": "azure-devops-mcp",
      "args": ["--organization", "https://dev.azure.com/TU_ORGANIZACION"],
      "env": {
        "AZURE_DEVOPS_PAT": "TU_PAT_TOKEN"
      }
    }
  }
}
```

**Claude Code** — `~/.claude.json` (global) o `.mcp.json` (proyecto):

```json
{
  "mcpServers": {
    "AzureDevOps": {
      "command": "azure-devops-mcp",
      "args": ["--organization", "https://dev.azure.com/TU_ORGANIZACION"],
      "env": {
        "AZURE_DEVOPS_PAT": "TU_PAT_TOKEN"
      }
    }
  }
}
```

Reinicia el CLI para aplicar los cambios:

```bash
copilot   # o: claude
> /mcp list
# AzureDevOps  ✓  connected
```

---

### Prompts de ejemplo con Azure DevOps MCP

#### Prompt 1: Listar proyectos de la organización

```
Prompt:
> ¿Qué proyectos tenemos en la organización de Azure DevOps?

Respuesta esperada:
El agente consulta la API de Azure DevOps y devuelve:

  Proyectos en https://dev.azure.com/TU_ORGANIZACION:
  
  • CustomerPortal       — Git, Agile  (activo)
  • InternalTools        — Git, Scrum  (activo)
  • DataverseMigration   — Git, Scrum  (archivado)
  • PowerPlatformALM     — Git, Agile  (activo)
  
  4 proyectos encontrados (3 activos, 1 archivado).
```

#### Prompt 2: Ver pipelines fallidas

```
Prompt:
> Muéstrame los pipelines que han fallado en el proyecto CustomerPortal
  en las últimas 24 horas y el motivo del fallo.

Respuesta esperada:
El agente consulta los runs de build y release:

  Pipelines fallidos en CustomerPortal (últimas 24h):
  
  ✗ CI-Build #2341      — Fallido hace 3h
    Causa: Error de compilación en src/PCF/CustomerCard/index.ts:47
    "Property 'getFormattedValue' does not exist on type 'EntityReference'"
  
  ✗ Deploy-TEST #891    — Fallido hace 1h
    Causa: pac solution import falló — la solución contiene componentes
    con dependencias no resueltas (MissingDependency: src_CustomerCard)
  
  Acción sugerida: corrige el error de TypeScript en CustomerCard y
  asegúrate de incluir las dependencias en la solución antes de importar.
```

#### Prompt 3: Crear un work item

```
Prompt:
> Crea un bug en el proyecto CustomerPortal con título "El componente PCF
  src_CustomerCard no muestra el campo telefono en modo mobile" asignado
  a mí mismo, con prioridad 2 e incluido en el sprint actual.

Respuesta esperada:
El agente crea el work item via la API:

  ✓ Bug creado correctamente:
  
  ID:          #4821
  Título:      El componente PCF src_CustomerCard no muestra el campo
               telefono en modo mobile
  Asignado a:  tu.nombre@empresa.com
  Prioridad:   2
  Sprint:      Sprint 14 (CustomerPortal)
  Estado:      Active
  
  URL: https://dev.azure.com/TU_ORGANIZACION/CustomerPortal/_workitems/edit/4821
```

#### Prompt 4: Ver el estado de un sprint

```
Prompt:
> Dame un resumen del sprint actual del proyecto PowerPlatformALM:
  cuántos work items hay, qué porcentaje está completado y qué tareas
  están bloqueadas.

Respuesta esperada:
El agente consulta el sprint activo y sus work items:

  Sprint 8 — PowerPlatformALM (14 Mar – 28 Mar 2026)
  ─────────────────────────────────────────────────
  Total work items:    23
  Completados (Done):  14  (61%)
  En progreso:          6  (26%)
  Sin empezar:          2  (9%)
  Bloqueados:           1  (4%)

  Work items bloqueados:
  • #4756 — Migrar solución a entorno TEST
    Bloqueado por: entorno TEST sin licencias asignadas
    Asignado a: desarrollador@empresa.com

  Velocidad estimada: 18 puntos / sprint
  Puntos completados este sprint: 21 / 34
  Riesgo de no completar: ⚠️ Moderado
```

---

## Integración con VS Code e IDEs

### VS Code

VS Code soporta MCP de forma nativa. No necesitas ningún plugin adicional.

```
1. Abre VS Code
2. Ctrl+Shift+P → "MCP: Add Server"
3. Selecciona HTTP o Server-Sent Events
4. Introduce la URL del servidor MCP
5. Activa el modo agente con Ctrl+Alt+I
```

VS Code genera automáticamente la entrada de configuración del servidor MCP.

### Visual Studio 2022 / 2026

Usa la extensión de código abierto **[CopilotCliIde](https://github.com/sailro/CopilotCliIde)**.

```bash
# 1. Instala el .vsix desde la página de releases
# 2. Abre tu solución en Visual Studio
# 3. Lanza Copilot CLI en la carpeta de la solución
copilot
> /ide
# ✓ Connected to Visual Studio (CopilotCliIde)
# El agente ahora ve tu IDE en tiempo real
```

Capacidades disponibles con `/ide`:

| Capacidad | Descripción |
|-----------|-------------|
| Consultar la solución | Lee la estructura de proyectos y referencias |
| Ver selección | Accede al código seleccionado en tiempo real |
| Proponer diffs | Muestra cambios con botones Aceptar/Rechazar en VS |
| Leer diagnósticos | Accede a los errores de la lista de errores del IDE |

> Claude Code no tiene comando `/ide`. Su integración con VS Code se hace directamente desde la extensión de VS Code para Claude Code.

---

## Tabla comparativa: Copilot CLI vs Claude Code

| | GitHub Copilot CLI | Claude Code |
|---|---|---|
| **Instalación del servidor** | `npm install -g <servidor>` | `npm install -g <servidor>` |
| **Config global** | `~/.copilot/mcp-config.json` | `~/.claude.json` |
| **Config por proyecto** | `.mcp/copilot/mcp.json` | `.mcp.json` |
| **Campo `"type"` en config** | Requerido (`"stdio"` o `"http"`) | No requerido (se infiere) |
| **Comando de verificación** | `/mcp list` | `/mcp list` |
| **Integración IDE** | `/ide` (VS Code y Visual Studio) | Solo VS Code (extensión nativa) |
| **MCP incluido por defecto** | GitHub MCP integrado | No tiene MCP predeterminado |
| **Limitación conocida** | Algunas herramientas MCP requieren reinicio del CLI | Los servidores MCP HTTP no son compatibles con todas las versiones |

---

## Referencias

- [Microsoft Learn — MCP Dataverse en VS Code y Copilot CLI](https://learn.microsoft.com/es-es/power-apps/maker/data-platform/data-platform-mcp-vscode)
- [CopilotCliIde — repositorio oficial](https://github.com/sailro/CopilotCliIde)
- [Model Context Protocol — especificación](https://modelcontextprotocol.io)
- [Azure DevOps — Personal Access Tokens](https://learn.microsoft.com/es-es/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate)

---

[← Anterior: Añadir plugins](08-anadir-plugins.md) | [← Volver al índice](README.md)
