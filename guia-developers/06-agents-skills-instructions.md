# 6. Agents, Skills y Custom Agents

[← Anterior: Custom Instructions](05-custom-instructions.md) | [Siguiente: Marketplace y Plugins →](07-marketplace-plugins.md)

---

> **Tres mecanismos distintos con propósitos distintos:**
>
> | | Instructions | Skills | Custom Agents |
> |---|---|---|---|
> | **Cuándo está activo** | Siempre, toda la sesión | Bajo demanda, por keywords | Solo cuando lo seleccionas |
> | **Para qué** | Contexto base del proyecto | Conocimiento especializado de un dominio | Perfil con herramientas restringidas o flujo multi-etapa |
> | **Activación** | Automática al iniciar | Automática por triggers | Manual (selector de agentes) |
>
> Comprender la diferencia es clave para no sobrecargar el contexto ni duplicar configuración.

---

## Skills

Un skill es una **carpeta con instrucciones especializadas** que el agente carga **bajo demanda** cuando la tarea lo requiere.

### Estructura

```
.github/skills/
  nombre-del-skill/
    SKILL.md              ← Obligatorio: descripción, triggers e instrucciones
    references/           ← Documentación de referencia (opcional)
    sample_codes/         ← Ejemplos de código (opcional)
```

### El fichero `SKILL.md`

```markdown
---
name: nombre-del-skill
description: >
  Descripción de cuándo usar este skill y qué hace.
  Se activa cuando el usuario menciona: keyword1, keyword2
triggers:
  - keyword1
  - keyword2
---

## Instrucciones

Las instrucciones que el agente debe seguir cuando este skill está activo.
```

### Ejemplo 1: Skill para consultas Dataverse

```markdown
---
name: dataverse-query
description: >
  Skill para construir y optimizar consultas FetchXML y OData contra Dataverse.
  Se activa cuando el usuario menciona FetchXML, OData, WebAPI o consultas a Dataverse.
triggers:
  - FetchXML
  - OData
  - Dataverse query
  - WebAPI
  - retrieveMultiple
---

## Instrucciones

1. Prefiere FetchXML para consultas complejas con joins
2. Usa OData/WebAPI para consultas simples desde el cliente
3. Siempre incluye paginación y límite de registros
4. Valida que los nombres de atributos usen el nombre lógico (snake_case con prefijo)
5. Incluye el atributo `statecode` en los filtros para excluir registros inactivos
```

### Ejemplo 2: Skill para despliegues con PAC CLI

```markdown
---
name: pac-cli-deploy
description: >
  Despliegue de soluciones Power Platform usando PAC CLI.
  Se activa cuando el usuario menciona exportar, importar, publicar o soluciones Dataverse.
triggers:
  - pac solution
  - deploy
  - export solution
  - PAC CLI
---

## Instrucciones

Siempre autenticar vía Service Principal.
Ejecutar `pac solution check` antes de cada importación.
Usar `--async` para soluciones de más de 50 MB.
Entornos del proyecto: DEV / TEST / PROD.
```

### Ejemplo 3: Skill para APIs REST

```markdown
---
name: rest-api-conventions
description: >
  Convenciones para el diseño e implementación de endpoints REST.
triggers:
  - endpoint
  - REST
  - API
  - controller
  - route
---

## Instrucciones

1. Usa sustantivos en plural para los recursos (/users, /orders)
2. Los códigos HTTP deben ser semánticamente correctos (201 para create, 204 para delete)
3. Versiona siempre la API en la URL (/api/v1/)
4. Incluye paginación en todos los endpoints de listado
5. Nunca expongas IDs internos de base de datos — usa UUIDs
```

### Dónde colocar los Skills

| Ubicación | Alcance | Compatible con |
|-----------|---------|----------------|
| `.github/skills/` | Solo para este repositorio | Copilot CLI, Claude Code |
| `~/.copilot/skills/` | Global (todos los proyectos) | Copilot CLI |
| `~/.claude/skills/` | Global (todos los proyectos) | Claude Code |

> **Limit recomendado:** instala entre 4 y 8 skills por proyecto. Con demasiados, los metadatos consumen la ventana de contexto (_context rot_).

---

## Custom Agents

Los Custom Agents son **perfiles especializados** que configuran el comportamiento del agente para una tarea concreta: instrucciones específicas, herramientas disponibles restringidas y modelo de IA seleccionado. Se activan con un solo clic o comando.

A diferencia de los Skills (que se cargan automáticamente por keywords), los Custom Agents se **seleccionan explícitamente** para un flujo de trabajo.

### ¿Cuándo usar un Custom Agent?

- Necesitas restricciones de herramientas específicas (ej: solo lectura para un agente de planificación)
- Quieres un modelo de IA distinto para una tarea concreta
- Defines flujos multi-etapa con **handoffs** entre agentes especializados
- Creas perfiles reutilizables para todo el equipo

Para tareas puntuales sin restricciones de herramientas, usa un [prompt file](05-custom-instructions.md) o las instrucciones directamente.

### Formato del fichero — GitHub Copilot CLI (`.agent.md`)

Los Custom Agents se definen en ficheros `.agent.md`:

```markdown
---
name: Nombre del agente
description: Descripción breve mostrada como placeholder en el chat
tools: ['fetch', 'githubRepo', 'search', 'editFiles', 'terminalLastCommand']
agents: ['OtroAgente']      # Sub-agentes permitidos (opcional)
model: 'Claude Sonnet 4.6'  # O array ['Claude Opus 4.6', 'Claude Sonnet 4.6']
user-invocable: true        # Si aparece en el selector (default: true)
handoffs:
  - label: Texto del botón
    agent: nombre-agente-destino
    prompt: Prompt que se envía al agente destino
    send: false             # true = envía automáticamente
---

## Instrucciones del agente

Texto en Markdown con las instrucciones específicas del agente.
Puedes referenciar herramientas con la sintaxis #tool:nombre-herramienta.
```

**Herramientas disponibles (campo `tools`):**

| Tool | Función |
|------|---------|
| `fetch` | Consultar URLs |
| `githubRepo` | Interactuar con GitHub repos |
| `search` | Buscar en el código base |
| `usages` | Buscar usos de símbolos |
| `codebase` | Leer ficheros del proyecto |
| `editFiles` | Editar ficheros |
| `terminalLastCommand` | Leer el último comando de terminal |
| `agent` | Invocar sub-agentes |

### Formato del fichero — Claude Code (`.claude/agents/*.md`)

Claude Code usa el mismo concepto con sintaxis ligeramente distinta:

```markdown
---
name: Nombre del agente
description: Descripción del agente
tools: Read, Grep, Glob, Bash    # String separado por comas
disallowedTools: Write, Edit     # Herramientas bloqueadas
---

## Instrucciones del agente

Instrucciones en Markdown.
```

### Ubicación de los ficheros

| Alcance | Copilot CLI | Claude Code |
|---------|-------------|-------------|
| Repositorio | `.github/agents/*.agent.md` | `.claude/agents/*.md` |
| Global (usuario) | `~/.copilot/agents/` | `~/.claude/agents/` |

> VS Code detecta automáticamente ambos formatos: `.agent.md` en `.github/agents/` y `.md` en `.claude/agents/`.

---

## Handoffs

Los handoffs permiten crear **flujos de trabajo secuenciales** entre agentes con un solo clic. Después de que el agente responde, aparecen botones de handoff que llevan al usuario al siguiente agente con el contexto relevante ya cargado.

**Casos de uso típicos:**
- Planificador → Implementador
- Implementador → Revisor de código
- Escribir tests fallidos → Escribir código que los pasa

```markdown
handoffs:
  - label: "Implementar Plan"   # Texto del botón
    agent: implementacion        # ID del agente destino
    prompt: "Implementa el plan descrito arriba."
    send: false                  # false = pre-rellena el prompt; true = envía automáticamente
    model: "Claude Sonnet 4.6 (copilot)"  # Modelo opcional para el handoff
```

> Claude Code no soporta handoffs con el formato `send: true`. Los botones de handoff son exclusivos de Copilot CLI con el formato `.agent.md`.

---

## Ejemplo 1: Agente Planificador (con handoff a implementación)

**Fichero:** `.github/agents/planner.agent.md`

```markdown
---
name: Planner
description: Genera un plan de implementación detallado antes de escribir código.
tools: ['fetch', 'githubRepo', 'search', 'usages', 'codebase']
model: ['Claude Opus 4.6', 'Claude Sonnet 4.6']
handoffs:
  - label: "▶ Implementar este plan"
    agent: agent
    prompt: "Implementa exactamente el plan descrito arriba, paso a paso."
    send: false
---

# Instrucciones del Planificador

Estás en modo planificación. Tu objetivo es generar un plan de implementación completo.
**No escribas código ni modifiques ficheros.**

El plan debe incluir:

1. **Resumen:** descripción breve del cambio.
2. **Ficheros afectados:** lista de ficheros que se modificarán o crearán.
3. **Pasos de implementación:** lista ordenada de acciones concretas.
4. **Casos de prueba:** tests que deben pasar tras la implementación.
5. **Riesgos y dependencias:** qué podría fallar y qué dependencias hay.

Usa #tool:search para explorar el código base antes de proponer cambios.
Usa #tool:usages para verificar el impacto de cada modificación.
```

**Uso en Copilot CLI:**

```bash
copilot
# Selecciona el agente "Planner" en el selector de agentes
> Añade soporte para múltiples monedas al módulo de pagos

# El agente genera el plan completo (sin tocar código)
# Aparece el botón: [▶ Implementar este plan]
# Al hacer clic, se activa el agente de implementación con el contexto del plan
```

**Equivalente en Claude Code** (`.claude/agents/planner.md`):

```markdown
---
name: Planner
description: Genera un plan de implementación sin escribir código.
tools: Read, Grep, Glob
disallowedTools: Write, Edit, Bash
---

# Instrucciones del Planificador

Estás en modo planificación. No escribas código ni modifiques ficheros.

El plan debe incluir:
1. Resumen del cambio
2. Ficheros afectados
3. Pasos de implementación ordenados
4. Casos de prueba requeridos
5. Riesgos y dependencias
```

---

## Ejemplo 2: Agente Revisor de Seguridad

**Fichero:** `.github/agents/security-reviewer.agent.md`

```markdown
---
name: Security Reviewer
description: Revisa el código en busca de vulnerabilidades de seguridad. Solo lectura.
tools: ['codebase', 'search', 'usages', 'githubRepo']
model: 'Claude Sonnet 4.6'
handoffs:
  - label: "🔧 Corregir vulnerabilidades encontradas"
    agent: agent
    prompt: "Corrige todas las vulnerabilidades de seguridad identificadas en la revisión anterior."
    send: false
---

# Instrucciones del Revisor de Seguridad

Eres un experto en seguridad. **No modifiques ningún fichero.**
Analiza el código en busca de:

- **Inyección:** SQL injection, XSS, command injection
- **Autenticación y autorización:** tokens expuestos, rutas sin protección, privilege escalation
- **Exposición de datos:** secrets en el código, logging de datos sensibles, respuestas de API con datos innecesarios
- **Dependencias:** paquetes con CVEs conocidos
- **Validación de entrada:** datos sin sanitizar del usuario

Para cada vulnerabilidad encontrada, reporta:
- Fichero y línea exacta
- Categoría (OWASP Top 10 si aplica)
- Severidad: 🔴 Crítica / 🟠 Alta / 🟡 Media / 🟢 Baja
- Descripción del problema
- Recomendación de corrección con ejemplo de código
```

**Uso en Copilot CLI:**

```bash
copilot
# Selecciona "Security Reviewer" en el selector
> Revisa el módulo src/auth/ en busca de vulnerabilidades

# El agente analiza el código y genera el informe de seguridad (sin modificar nada)
# Aparece el botón: [🔧 Corregir vulnerabilidades encontradas]
```

**Equivalente en Claude Code** (`.claude/agents/security-reviewer.md`):

```markdown
---
name: Security Reviewer
description: Revisa vulnerabilidades de seguridad. Solo lectura.
tools: Read, Grep, Glob
disallowedTools: Write, Edit, Bash, MultiEdit
---

# Instrucciones del Revisor de Seguridad

No modifiques ningún fichero. Analiza el código en busca de:
- Inyección (SQL, XSS, command injection)
- Autenticación y autorización incorrectas
- Exposición de datos sensibles
- Dependencias con CVEs conocidos

Reporta cada vulnerabilidad con: fichero, línea, severidad y recomendación.
```

---

## Tabla comparativa: Instructions vs Skills vs Custom Agents

| | Custom Instructions | Skills | Custom Agents |
|---|---|---|---|
| **Formato de fichero** | `.github/copilot-instructions.md` (Copilot) / `CLAUDE.md` (Claude) | `SKILL.md` en `.github/skills/<nombre>/` | `.agent.md` en `.github/agents/` (Copilot) / `.md` en `.claude/agents/` (Claude) |
| **Ámbito** | Todo el proyecto, toda la sesión | Por dominio o tarea específica | Por rol o flujo de trabajo concreto |
| **Activación** | Automática al iniciar sesión | Automática por keywords/triggers | Manual (selección explícita) |
| **Restringe herramientas** | ❌ No | ❌ No | ✅ Sí |
| **Fija el modelo de IA** | ❌ No | ❌ No | ✅ Sí (opcional) |
| **Soporta handoffs** | ❌ No | ❌ No | ✅ Sí (Copilot CLI) / ❌ (Claude Code) |
| **Cuándo usar** | Siempre (contexto base del proyecto) | Cuando necesitas conocimiento especializado para una tarea | Cuando necesitas un perfil con restricciones, modelo específico o flujo multi-etapa |

---

## Cómo interactúan los tres conceptos

```
Sesión de Copilot CLI / Claude Code
├── Instructions (.github/copilot-instructions.md o CLAUDE.md)
│   └── Se carga siempre → define el contexto base del proyecto
│
├── Skills (.github/skills/)
│   └── Se cargan bajo demanda según los triggers
│       ├── dataverse-query/   ← activo cuando el usuario menciona "FetchXML"
│       └── pac-cli-deploy/    ← activo cuando el usuario menciona "pac solution"
│
└── Custom Agents (.github/agents/ o .claude/agents/)
    └── Se activan manualmente para un flujo concreto
        ├── Planner/           ← solo herramientas de lectura, genera plan
        └── Security Reviewer/ ← solo herramientas de lectura, analiza seguridad
```

---

## Referencias

- [VS Code Docs: Custom Agents](https://code.visualstudio.com/docs/copilot/customization/custom-agents)
- [VS Code Docs: Agent Skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- [Estándar Agent Skills](https://agentskills.io)
- [Ejemplos de la comunidad (awesome-copilot)](https://github.com/github/awesome-copilot)

---

[← Anterior: Custom Instructions](05-custom-instructions.md) | [Siguiente: Marketplace y Plugins →](07-marketplace-plugins.md)
