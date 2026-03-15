# 1. ¿Qué es GitHub Copilot CLI y Claude Code?

[← Índice](README.md) | [Siguiente: Elección del modelo →](02-eleccion-modelo.md)

---

## GitHub Copilot CLI

GitHub Copilot CLI es un **agente de IA nativo del terminal** que alcanzó disponibilidad general el **25 de febrero de 2026**. Disponible para todas las suscripciones de pago de GitHub Copilot:

| Plan | Precio |
|------|--------|
| Copilot Pro | $10/mes |
| Copilot Pro+ | $39/mes |
| Copilot Business | $19/usuario/mes |
| Copilot Enterprise | $39/usuario/mes |

> Los estudiantes y profesores verificados acceden a **Copilot Pro de forma gratuita** a través de [GitHub Education](https://education.github.com/pack).

### ¿En qué se diferencia del Copilot de VS Code?

| | Copilot en VS Code | Copilot CLI |
|---|---|---|
| Tipo | Asistente reactivo | Agente autónomo |
| Actúa cuando | Tú escribes | Tú describes una tarea |
| Edita ficheros | No (solo sugiere) | Sí, múltiples a la vez |
| Ejecuta comandos | No | Sí (shell, tests, builds) |
| Recuerda el contexto | Solo en la sesión de chat | Entre sesiones (memoria del repo) |

```bash
# Copilot en VS Code: te sugiere mientras escribes
# Copilot CLI: hace el trabajo completo
copilot
> Refactoriza el módulo de autenticación para separar la lógica de negocio de la UI,
> ejecuta los tests y muéstrame un diff antes de aplicar los cambios.
```

---

## Claude Code

[Claude Code](https://docs.anthropic.com/claude-code) (Anthropic) es el competidor directo: también es un agente nativo del terminal con memoria de repositorio entre sesiones y soporte del mismo estándar de Agent Skills. Requiere una suscripción activa de Anthropic.

```bash
# Claude Code: mismo tipo de tarea
claude
> Refactoriza el módulo de autenticación para separar la lógica de negocio de la UI,
> ejecuta los tests y muéstrame un diff antes de aplicar los cambios.
```

---

## Modos de ejecución

Ambas herramientas ofrecen un modo de trabajo cauteloso (revisión previa) y uno autónomo.

### Plan Mode

El agente analiza la petición, hace preguntas de aclaración y genera un plan de implementación para tu revisión. Solo ejecuta tras tu aprobación.

**Cuándo usarlo:** cambios que afectan a múltiples ficheros, decisiones de arquitectura, cualquier tarea donde quieras supervisar cada paso.

| | GitHub Copilot CLI | Claude Code |
|---|---|---|
| Activación | `Shift+Tab` | `Shift+Tab` |
| Alternativa | Describe el plan antes de pedir código | Incluye "primero planifica" en el prompt |

```bash
# Copilot CLI — Plan Mode activo (Shift+Tab)
> Migra la capa de acceso a datos de REST a GraphQL
# Respuesta:
# Plan de implementación:
# 1. Analizar los endpoints REST actuales en /src/api/
# 2. Identificar las consultas candidatas a migración
# 3. Instalar las dependencias de Apollo Client
# 4. Crear el esquema GraphQL equivalente
# 5. Actualizar los componentes que consumen la API
# 6. Ejecutar los tests de integración
# ¿Apruebas este plan? (s/n)

# Claude Code — misma petición
> Antes de hacer nada, planifica cómo migrarías la capa de acceso a datos de REST a GraphQL.
# Claude responde con un plan estructurado antes de ejecutar ningún cambio.
```

### Autopilot Mode

Para tareas bien definidas — tests unitarios, refactorizaciones, arreglos de CI — el agente trabaja de forma completamente autónoma sin pausas.

**Cuándo usarlo:** generar tests, renombrar variables, aplicar conventions, corregir errores de compilación.

| | GitHub Copilot CLI | Claude Code |
|---|---|---|
| Activación | `Shift+Tab` (alterna) | Activo por defecto |
| Comportamiento | Ejecuta sin pedir confirmación | Ejecuta sin pedir confirmación |

---

## Agentes integrados

Copilot CLI delega automáticamente en sub-agentes especializados según la tarea. Claude Code orquesta sub-agentes de forma similar.

| Agente | Copilot CLI | Claude Code | Función |
|--------|-------------|-------------|---------|
| Explore | ✅ | ✅ | Análisis del código base sin contaminar el contexto |
| Task | ✅ | ✅ | Ejecuta builds, tests y comandos de shell |
| Code Review | ✅ | ✅ | Analiza cambios antes del commit |
| Plan | ✅ | ✅ | Planificación de implementaciones |
| Fleet / Sub-agents paralelos | `/fleet` | Sub-agentes | Ejecutar múltiples tareas en paralelo |

---

## Características clave

### Delegación en segundo plano

```bash
# Copilot CLI — prefija con & para delegar en la nube
& Genera una suite completa de tests unitarios para el módulo src/payments/
# ... haces otras cosas mientras el agente trabaja ...
> /resume   # Retomas cuando esté listo

# Claude Code — no tiene delegación en segundo plano
# Las tareas se ejecutan de forma síncrona en la sesión activa
```

### Memoria del repositorio entre sesiones

Ambas herramientas construyen memoria del repositorio a medida que trabajas: aprenden las convenciones de naming, patrones de código y preferencias del equipo. Las sesiones futuras arrancan ya con ese contexto.

### Comandos de revisión y deshacer

| Acción | Copilot CLI | Claude Code |
|--------|-------------|-------------|
| Ver cambios de la sesión | `/diff` | `/diff` |
| Revisar staged/unstaged | `/review` | `/review` |
| Deshacer cambios | `Esc+Esc` | `Esc+Esc` |

### Extensibilidad MCP

Ambas herramientas soportan servidores MCP para conectarse a servicios externos. Ver [sección 9](09-mcp.md) para más detalles.

---

## Comparativa general

| Característica | GitHub Copilot CLI | Claude Code |
|----------------|-------------------|-------------|
| Integración nativa con GitHub | ✅ Profunda (Issues, PRs, Actions) | Solo vía MCP |
| Plugin Power Platform oficial | `microsoft/power-platform-skills` | Mismo plugin (compatible) |
| Selección de modelos | Claude, GPT, Gemini | Solo modelos Anthropic |
| Ventana de contexto | ~200K | 200K nativa |
| Memoria entre sesiones | ✅ | ✅ |
| Extensibilidad MCP | ✅ | ✅ |
| Delegación en segundo plano (`&`) | ✅ | ❌ No disponible |
| Políticas de empresa | GitHub Org settings | Anthropic Console |
| Coste | Incluido en suscripción Copilot | Suscripción Anthropic separada |

- **Ambos son compatibles con el estándar Agent Skills:** un skill en `.github/skills/` funciona en los dos a la vez.

---

## Referencias

- [Documentación oficial Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)
- [Copilot CLI for Beginners — Quick Start](https://github.com/github/copilot-cli-for-beginners/tree/main/00-quick-start)
- [Documentación Claude Code](https://docs.anthropic.com/claude-code)

---

[← Índice](README.md) | [Siguiente: Elección del modelo →](02-eleccion-modelo.md)
