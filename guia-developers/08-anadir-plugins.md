# 8. Añadir plugins

[← Anterior: Marketplace y Plugins](07-marketplace-plugins.md) | [Siguiente: MCP →](09-mcp.md)

---

## Flujo general de instalación

```bash
# Copilot CLI
copilot
> /plugin marketplace add <owner>/<repo>
> /plugin install <nombre-plugin>@<marketplace>
> /plugin list   # Verificar

# Claude Code (mismos comandos)
claude
> /plugin marketplace add <owner>/<repo>
> /plugin install <nombre-plugin>@<marketplace>
> /plugin list
```

---

## Ejemplo 1: Plugin de GitHub Actions

Este plugin proporciona capacidades operativas completas para gestionar pipelines de CI/CD directamente desde el CLI, sin necesidad de navegar a la interfaz web de GitHub.

### Capacidades

- **Monitoreo de workflows:** ver el estado de los runs en tiempo real
- **Gestión de ejecuciones:** relanzar pipelines fallidos, cancelar ejecuciones en curso
- **Logs y diagnóstico:** acceder a los logs de fallos directamente en el terminal
- **Gestión de secretos:** listar y actualizar secretos de repositorio/organización
- **Análisis de rendimiento:** identificar los steps más lentos de un workflow

### Instalación

```bash
# Copilot CLI
copilot
> /plugin marketplace add github/awesome-copilot
> /plugin install github-actions@awesome-copilot

# Claude Code (mismos comandos)
claude
> /plugin marketplace add github/awesome-copilot
> /plugin install github-actions@awesome-copilot
```

### Verificar que está activo

```bash
> /plugin list
# github-actions  ✓  (github/awesome-copilot)
```

---

### Casos de uso

#### Caso 1: Diagnosticar un pipeline fallido

```
Prompt:
> El último workflow de CI está fallando. Muéstrame qué step ha fallado
  y por qué, sin que yo tenga que abrir el navegador.

Respuesta esperada:
El agente consulta la API de GitHub Actions, identifica el workflow run más reciente,
localiza el step fallido y muestra los logs relevantes directamente en el terminal:

  ✗ Run tests (step 3/6)
  Error: ENOENT: no such file or directory, open 'coverage/lcov.info'
  
  Causa probable: el script de tests no genera el reporte de cobertura
  cuando falla antes del step de coverage. Línea 47 en jest.config.ts:
  el directorio `coverage/` no existe porque los tests se abortaron.

  Sugerencia: añade `--coverage --coverageReporters=lcov` a jest y
  asegúrate de que la carpeta coverage/ se crea aunque haya fallos.
```

#### Caso 2: Relanzar pipelines fallidos en múltiples ramas

```
Prompt:
> Los últimos tres PRs tienen el pipeline de lint fallando desde las 10:00.
  Es un falso positivo — relanza todos.

Respuesta esperada:
El agente identifica los runs de lint de los 3 PRs más recientes y los relanza:

  Relanzando workflows fallidos...
  ✓ PR #142 — lint — relanzado (run ID: 8821034)
  ✓ PR #139 — lint — relanzado (run ID: 8820891)
  ✓ PR #137 — lint — relanzado (run ID: 8820744)
  
  3 workflows relanzados. Estado actual: en cola.
```

#### Caso 3: Identificar los steps más lentos del pipeline

```
Prompt:
> El pipeline de CI tarda demasiado. ¿Cuáles son los 3 steps más lentos
  de los últimos 10 runs del workflow main.yml?

Respuesta esperada:
El agente analiza los últimos 10 runs y calcula el tiempo promedio por step:

  Steps más lentos (promedio de los últimos 10 runs):
  1. Install dependencies    ⏱ 4m 23s  → Considera cachear node_modules
  2. Run e2e tests           ⏱ 3m 47s  → Paraleliza con sharding
  3. Build Docker image      ⏱ 2m 15s  → Usa multi-stage build con caché

  Optimización estimada posible: ~8 minutos menos por run.
```

#### Caso 4: Actualizar un secreto de repositorio

```
Prompt:
> Actualiza el secreto DATAVERSE_URL en el repositorio CustomerPortal
  con el valor https://org123.crm.dynamics.com

Respuesta esperada:
El agente actualiza el secreto via la API de GitHub:

  Actualizando secreto DATAVERSE_URL en CustomerPortal...
  ✓ Secreto actualizado correctamente.
  
  Nota: los workflows en ejecución actualmente no verán el nuevo valor
  hasta la próxima ejecución.
```

---

## Ejemplo 2: Plugin de Dataverse

Instalación de la conexión MCP de Dataverse con el skill `mcp-configure`, que guía la configuración de forma interactiva.

### Instalación

```bash
# Copilot CLI
copilot
> /plugin marketplace add github/awesome-copilot
> /plugin install dataverse@awesome-copilot

# Claude Code
claude
> /plugin marketplace add github/awesome-copilot
> /plugin install dataverse@awesome-copilot
```

### Configurar la conexión MCP de forma interactiva

```bash
# Copilot CLI
> /dataverse:mcp-configure

# Claude Code
> /dataverse:mcp-configure
```

El skill `mcp-configure` te guía a través de:
1. Selección de tu entorno Dataverse
2. Elección del endpoint (`/api/mcp` o `/api/mcp_preview`)
3. Generación automática del fichero de configuración MCP

### Configuración manual del MCP (sin plugin)

Si prefieres configurar manualmente, edita el fichero de configuración:

```json
// ~/.copilot/mcp-config.json  (Copilot CLI, global)
// .mcp/copilot/mcp.json       (Copilot CLI, por proyecto)
// ~/.claude.json               (Claude Code, global)
// .mcp.json                    (Claude Code, por proyecto)
{
  "mcpServers": {
    "DataverseMcp": {
      "type": "http",
      "url": "https://tuorganizacion.crm.dynamics.com/api/mcp"
    }
  }
}
```

Puedes obtener la URL en `make.powerapps.com` → Configuración → Detalles de sesión → Dirección URL de la instancia.

Para más detalles sobre MCP, ver [sección 9](09-mcp.md).

---

## Ejemplos de plugins para otros stacks

```bash
# Azure / .NET
> /plugin marketplace add microsoft/skills
# Instala skills por tecnología:
# > /plugin install azure-dotnet@skills
# > /plugin install azure-typescript@skills

# React / Frontend
> /plugin marketplace add github/awesome-copilot
> /plugin marketplace add anthropics/skills

# Docker / DevOps
> /plugin marketplace add github/awesome-copilot
# Contiene skills para Docker, GitHub Actions, Kubernetes
```

---

## Troubleshooting

### El plugin no aparece tras la instalación

```bash
> /plugin list   # Verifica que está instalado
copilot          # Reinicia la sesión si es necesario
```

### El skill `mcp-configure` no se activa

Asegúrate de usar el prefijo del namespace del plugin:

```bash
> /dataverse:mcp-configure   # ✅ correcto
> /mcp-configure              # ❌ no funciona sin prefijo
```

### Error de autenticación en el MCP de Dataverse

Verifica que el MCP de Copilot de Microsoft está habilitado en tu entorno:
`admin.powerplatform.microsoft.com` → Entorno → Configuración → Características → MCP

---

[← Anterior: Marketplace y Plugins](07-marketplace-plugins.md) | [Siguiente: MCP →](09-mcp.md)
