# Guía del Desarrollador: GitHub Copilot CLI y Claude Code

Guía completa para equipos que trabajan con **Power Platform y Dataverse**, aplicable a cualquier stack tecnológico.

> **Versión de referencia:** GitHub Copilot CLI GA (25 de febrero de 2026)

---

## Qué vas a conseguir

Al terminar esta guía serás capaz de:

- Usar un agente de IA en el terminal que planifica, edita ficheros y ejecuta comandos de forma autónoma
- Elegir el modelo adecuado (Haiku / Sonnet / Opus) según la tarea para optimizar coste y velocidad
- Configurar custom instructions, skills y custom agents para tu stack (Power Platform, Dataverse, .NET)
- Instalar plugins del marketplace oficial de Microsoft, GitHub y Anthropic
- Conectar el agente a sistemas externos (Azure DevOps, Dataverse) mediante servidores MCP

---

## Índice

| # | Sección | Descripción |
|---|---------|-------------|
| 1 | [🤖 ¿Qué es GitHub Copilot CLI y Claude Code?](01-que-es.md) | Modos de ejecución, agentes integrados, comparativa entre herramientas |
| 2 | [⚡ Elección del modelo correcto](02-eleccion-modelo.md) | Cuándo usar Haiku, Sonnet u Opus en Copilot CLI y Claude Code |
| 3 | [🛠️ Instalación](03-instalacion.md) | GitHub Copilot CLI y Claude Code paso a paso |
| 4 | [🚀 Primeros pasos y autenticación](04-primeros-pasos.md) | Abrir el CLI, autenticarse, comandos esenciales |
| 5 | [⚙️ Custom Instructions](05-custom-instructions.md) | Ficheros de instrucciones para Copilot CLI y Claude Code |
| 6 | [🧩 Agents, Skills y Custom Agents](06-agents-skills-instructions.md) | Skills, Custom Agents, handoffs y tabla comparativa |
| 7 | [🏪 Marketplace y Plugins](07-marketplace-plugins.md) | Qué son los plugins, repositorios clave y cómo instalarlos |
| 8 | [🔌 Añadir plugins](08-anadir-plugins.md) | Ejemplo GitHub Actions plugin + plugin de Dataverse |
| 9 | [🔗 MCP: instalación, configuración y ejemplos](09-mcp.md) | Qué es MCP, rutas de config, Azure DevOps como ejemplo completo |

---

## ¿Por dónde empezar?

| Si eres... | Ruta recomendada |
|------------|-----------------|
| Nuevo en Copilot CLI / Claude Code | [01](01-que-es.md) → [03](03-instalacion.md) → [04](04-primeros-pasos.md) → [05](05-custom-instructions.md) |
| Desarrollador Power Platform | [01](01-que-es.md) → [05](05-custom-instructions.md) → [07](07-marketplace-plugins.md) → [09](09-mcp.md) |
| Ya instalado, quiero sacar más partido | [05](05-custom-instructions.md) → [06](06-agents-skills-instructions.md) → [07](07-marketplace-plugins.md) |
| Solo me interesa conectar Azure DevOps / Dataverse | [08](08-anadir-plugins.md) → [09](09-mcp.md) |

---

## ¿Por qué esta guía?

GitHub Copilot CLI y Claude Code son **agentes de IA nativos del terminal** que planifican, editan ficheros, ejecutan comandos y recuerdan el contexto del repositorio entre sesiones. No son chatbots: son pair-programmers autónomos.

Esta guía cubre desde la instalación hasta la integración con el IDE, incluyendo el ecosistema completo de skills, plugins y MCP servers — con ejemplos reales para que puedas aplicar cada concepto desde el primer día.

---

[← Volver al índice general](../README.md) | [Empezar: ¿Qué es GitHub Copilot CLI? →](01-que-es.md)
