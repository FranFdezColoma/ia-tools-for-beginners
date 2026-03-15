# 5. Custom Instructions

[← Anterior: Primeros pasos](04-primeros-pasos.md) | [Siguiente: Agents, Skills y Custom Agents →](06-agents-skills-instructions.md)

---

## ¿Qué son las Custom Instructions?

Son ficheros Markdown que le dicen al agente **cómo comportarse en tu proyecto específico**: stack tecnológico, convenciones de naming, restricciones de código, idioma de respuesta, estructura del proyecto.

Se **cargan automáticamente al inicio de cada sesión**. No hace falta referenciarlo ni recordarlo.

---

## Ubicación de los ficheros

| | GitHub Copilot CLI | Claude Code |
|---|---|---|
| **Proyecto (repositorio)** | `.github/copilot-instructions.md` | `CLAUDE.md` (raíz del proyecto) |
| **Global (todos los proyectos)** | `~/.copilot/instructions.md` | `~/.claude/CLAUDE.md` |
| **VS Code agent mode** | `.github/copilot-instructions.md` | — |

> **Compatibilidad:** El fichero `.github/copilot-instructions.md` es compatible con Copilot CLI, Claude Code y VS Code agent mode. Claude Code también lo lee, pero da prioridad a `CLAUDE.md` si ambos existen.

---

## Crearlo: `/init` vs. manual

### Opción A — `/init` (automático)

```bash
# Copilot CLI
copilot
> /init
# Genera .github/copilot-instructions.md

# Claude Code
claude
> /init
# Genera CLAUDE.md en la raíz del proyecto
```

**Úsalo cuando** el proyecto ya tiene una estructura clara y naming conventions aplicadas de forma consistente. El agente infiere los patrones correctamente.

### Opción B — Manual

```bash
# Para Copilot CLI
mkdir -p .github
# Crea .github/copilot-instructions.md

# Para Claude Code
# Crea CLAUDE.md en la raíz del proyecto
# O crea .github/copilot-instructions.md (compatible con ambas)
```

**Úsalo cuando** el proyecto es nuevo, está en migración, o las convenciones no se aplican de forma consistente.

---

## Estructura recomendada

```markdown
## Stack tecnológico
Describe las tecnologías principales del proyecto.

## Convenciones de naming
Lista las reglas de naming específicas del proyecto.

## Restricciones
Qué no debe hacer el agente (instalar dependencias sin consultar,
usar patrones deprecated, romper compatibilidad con X, etc.).

## Idioma
En qué idioma responder y en qué idioma escribir el código/comentarios.

## Estructura del proyecto
Descripción breve de las carpetas principales y su propósito.
```

---

## Ejemplo: proyecto TypeScript full-stack

```markdown
## Stack tecnológico
- Frontend: React 18 + TypeScript + Vite
- Backend: Node.js + Express + TypeScript
- Base de datos: PostgreSQL + Prisma ORM
- Tests: Vitest (unit) + Playwright (e2e)
- CI/CD: GitHub Actions

## Convenciones de naming
- Componentes React: PascalCase (ej: UserProfile.tsx)
- Servicios y utilidades: camelCase (ej: authService.ts)
- Constantes: UPPER_SNAKE_CASE
- Tablas de base de datos: snake_case (ej: user_sessions)

## Restricciones
- No uses `any` en TypeScript sin comentario de justificación
- No instales dependencias npm sin consultarlo primero
- Los componentes de UI no deben contener lógica de negocio
- Toda función asíncrona debe tener manejo de errores explícito

## Idioma
- Código y comentarios técnicos: inglés
- Respuestas al desarrollador: español

## Estructura del proyecto
- /src/client  → Código del frontend (React)
- /src/server  → Código del backend (Express)
- /src/shared  → Tipos y utilidades compartidos
- /prisma      → Esquema y migraciones de base de datos
```

---

## Ejemplo: Power Platform (referencia del equipo)

```markdown
## Stack tecnológico
- Power Platform: PCF Components, Power Pages, Model-Driven Apps
- Dataverse como capa de datos principal
- TypeScript + React para componentes PCF
- PowerShell para scripts de automatización y despliegue
- Visual Studio 2022 como IDE principal

## Convenciones de naming
- Prefijo del publisher: `src_`
- Componentes PCF: PascalCase con prefijo (ej: `src_CustomerCard`)
- Tablas Dataverse custom: snake_case con prefijo (ej: `src_customer_profile`)
- Ficheros TypeScript: camelCase (ej: `customerService.ts`)
- Constantes: UPPER_SNAKE_CASE

## Restricciones
- No uses `any` en TypeScript sin justificación explícita
- Los componentes PCF deben implementar siempre `IInputs` e `IOutputs`
- No instales dependencias npm sin consultarlo primero
- Mantén compatibilidad con Unified Interface

## Idioma
- Código y comentarios técnicos: inglés
- Respuestas al desarrollador: español

## Estructura del proyecto
- /src       → Código fuente de componentes PCF
- /scripts   → Scripts PowerShell de CI/CD
- /docs      → Documentación técnica
- /solutions → Ficheros de solución exportados (.zip)
```

---

## Ejemplo: proyecto .NET / C#

```markdown
## Stack tecnológico
- ASP.NET Core 8 (Web API)
- Entity Framework Core + SQL Server
- xUnit para tests
- Docker para contenerización

## Convenciones de naming
- Clases y métodos: PascalCase
- Variables locales y parámetros: camelCase
- Interfaces: prefijo I (ej: IUserRepository)

## Restricciones
- Usa el patrón Repository para el acceso a datos
- No mezcles lógica de negocio en los controladores
- Usa record types para DTOs inmutables

## Idioma
- Código y comentarios: inglés
- Respuestas al desarrollador: español
```

---

## Buenas prácticas

- **Sé específico:** "No uses `any`" es mejor que "escribe TypeScript limpio".
- **Evita lo obvio:** No escribas "escribe código limpio" — el agente ya lo hace.
- **Limita a 4–8 KB:** Ficheros muy largos consumen ventana de contexto sin aportar valor proporcional.
- **Versiona el fichero:** Al estar en `.github/`, es parte del repositorio y puede revisarse en PRs.
- **Actualízalo cuando cambie el proyecto:** Si migras el stack o cambias las conventions, actualiza el fichero.

---

[← Anterior: Primeros pasos](04-primeros-pasos.md) | [Siguiente: Agents, Skills y Custom Agents →](06-agents-skills-instructions.md)
