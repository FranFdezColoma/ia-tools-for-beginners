# 4. Skills y Conectores en Claude.ai

[← Anterior: Custom Instructions](03-custom-instructions.md) | [Siguiente: Claude Projects →](05-projects.md)

---

## Skills: módulos de conocimiento especializado

Las **Skills** son paquetes de instrucciones y recursos que Claude carga **automáticamente** cuando la tarea lo requiere. A diferencia de las Custom Instructions (siempre activas), las Skills se activan solo cuando son relevantes — lo que permite tener muchas sin saturar el contexto.

### Tipos de Skills

| Tipo | Descripción | Ejemplos |
|---|---|---|
| **Anthropic** | Pre-construidas por Anthropic | Crear Word, Excel, PowerPoint, PDF |
| **Custom** | Creadas y subidas por el usuario (ZIP) | Plantilla de propuesta, guía de argumentario |
| **Organization** | Distribuidas por el admin del equipo | Skills corporativas para todos los miembros |
| **Partner** | Del Skills Directory (socios de Anthropic) | Notion, Figma, Atlassian |

### Cómo activar las Skills

1. Ve a **Customize** → **Skills** (o [claude.ai/customize/skills](https://claude.ai/customize/skills))
2. Activa las Skills de Anthropic que necesites con el toggle
3. Para Partner Skills: accede al **Skills Directory** desde la misma pantalla
4. Para subir una Custom Skill: haz clic en **Upload skill** y sube el ZIP

> ⚠️ Requiere activar también **"Code execution and file creation"** en Settings → Capabilities.  
> Disponible en Free (limitado), Pro, Max, Team y Enterprise.

---

### Skills de Anthropic: documentos con formato real

Las Skills de Anthropic para Office permiten crear archivos `.docx`, `.xlsx`, `.pptx` y `.pdf` con formato real — no texto plano exportado. Esto es lo que diferencia Claude.ai de ChatGPT o Gemini en este caso de uso.

**Ejemplo — presentación PowerPoint:**

```
Prompt:
Crea una presentación de 10 slides sobre la propuesta de migración a Dynamics 365
para Acme Corp. Incluye: resumen ejecutivo, situación actual, propuesta de valor,
roadmap en 3 fases, tabla de inversión y próximos pasos.

Resultado:
Claude genera un archivo .pptx descargable con diseño consistente, tabla de
inversión formateada y roadmap visual con las fases. No es texto plano:
es una presentación con layout y formato real.
```

**Ejemplo — Excel con análisis ponderado:**

```
Prompt:
Crea un Excel con la evaluación de 5 proveedores para el proyecto CRM.
Columnas: proveedor, precio, puntuación técnica, puntuación funcional,
total (ponderado 40%/30%/30%). Añade formato condicional para destacar al ganador.

Resultado:
Claude genera un .xlsx con fórmulas de ponderación, formato condicional
aplicado y el proveedor ganador resaltado en verde.
```

---

### Custom Skills: cómo crearlas

Una Custom Skill es un ZIP con esta estructura mínima:

```
MiSkill/
├── Skill.md          ← obligatorio
└── recursos/         ← opcional (plantillas, docs de referencia)
```

**Estructura de `Skill.md`:**

```markdown
---
name: Propuesta Comercial Dynamics 365
description: >
  Guía para estructurar propuestas comerciales de implementación de Dynamics 365.
  Incluye secciones recomendadas, tono y argumentario de valor.
version: 1.0
---

## Cuándo usar esta Skill

Cuando el usuario pida crear, revisar o estructurar una propuesta comercial
para Dynamics 365 Sales, Customer Service o Field Service.

## Estructura recomendada

1. Resumen ejecutivo (máx. 1 página)
2. Situación actual y dolor del cliente
3. Propuesta de valor con cifras concretas
4. Solución técnica y roadmap
5. Inversión y ROI estimado
6. Próximos pasos
```

Una vez creado el ZIP, súbelo desde **Customize → Skills → Upload skill**.  
La Skill se activa automáticamente cuando Claude detecta que la tarea es relevante.

---

## Conectores: acceso a servicios externos

Los **Conectores** son el mecanismo oficial de Claude.ai para conectarse a servicios externos. En la versión CLI (Claude Code / Copilot CLI), esto se llama MCP y requiere instalación local. En Claude.ai cloud, los Conectores se configuran desde la interfaz web, sin instalar nada.

| | MCP en CLI | Conector en Claude.ai cloud |
|---|---|---|
| **Configuración** | `npm install` + fichero JSON | Interfaz web, sin instalación |
| **Quién lo usa** | Solo el desarrollador en su máquina | Cualquier usuario con acceso |
| **Requiere técnica** | Sí | No |
| **Ejemplos** | Azure DevOps MCP, Dataverse MCP | Jira, Google Drive, GitHub, Slack |

### Cómo añadir un Conector

1. Ve a **Settings → Connectors** (o [claude.ai/settings/connectors](https://claude.ai/settings/connectors))
2. Elige un conector del catálogo, o haz clic en **Add custom connector** para uno propio
3. Introduce la URL del servidor MCP y completa el flujo OAuth
4. El Conector queda disponible en todas tus conversaciones y Projects

> En **Team** y **Enterprise**, el administrador puede preinstalar Conectores para todo el equipo.

---

### Partner Skills + Conectores: la combinación más potente

Las Partner Skills del Skills Directory están diseñadas para trabajar **junto con** su Conector correspondiente:

- **Skill de Atlassian** + **Conector de Jira** → Claude sabe cómo gestionar tickets *y* tiene acceso real a tu Jira
- **Skill de Figma** + **Conector de Figma** → Claude sabe cómo trabajar con diseños *y* puede leer tus archivos

La diferencia práctica: el Conector da *acceso*, la Skill aporta *conocimiento* sobre cómo usarlo bien.

---

## Ejemplo 1: Conector de Jira + Partner Skill de Atlassian

**Configuración:**
1. Skills Directory → Atlassian → Añadir Skill
2. Settings → Connectors → Jira → Connect (autoriza con OAuth)

**Caso de uso — crear tickets desde notas de reunión:**

```
Prompt:
Estas son mis notas de la reunión de hoy con el equipo técnico de Acme Corp:
[pega las notas]

Identifica todas las acciones acordadas, crea un ticket de Jira por cada una
en el proyecto ACME-CRM y asígnalos al responsable mencionado.

Resultado:
Claude extrae las acciones, crea los tickets con título, descripción, asignado
y prioridad, y confirma con los IDs generados (ej. ACME-412 a ACME-417).
```

**Caso de uso — resumen del sprint:**

```
Prompt:
Dame el estado del sprint actual de ACME-CRM: completados, en progreso
y bloqueados. Para los bloqueados, sugiere cómo desbloquearlos.

Resultado:
Claude consulta Jira en tiempo real y devuelve métricas del sprint con
propuestas concretas para cada bloqueo.
```

---

## Ejemplo 2: Conector de Google Drive + Skill de Word

**Configuración:**
1. Customize → Skills → Activar la Skill de Word (Anthropic)
2. Settings → Connectors → Google Drive → Connect (autoriza OAuth)

**Caso de uso — generar y guardar una propuesta:**

```
Prompt:
Crea una propuesta ejecutiva de 3 páginas para la migración del CRM de
Acme Corp a Dynamics 365 Sales. Incluye roadmap de 2 fases y tabla de
inversión. Guárdala en "Propuestas/2026" de Google Drive como
"Propuesta_AcmeCorp_D365_Mar2026.docx".

Resultado:
Claude usa la Skill de Word para generar el documento con formato profesional
y el Conector de Google Drive para guardarlo directamente en la carpeta indicada.
Sin copiar y pegar: el archivo queda en Drive listo para compartir con el cliente.
```

---

## Resumen: cuándo usar qué

| | Custom Instructions | Skills | Conectores |
|---|---|---|---|
| **Para qué** | Comportamiento general permanente | Procedimientos para tareas concretas | Acceso a sistemas externos |
| **Se activa** | Siempre | Automáticamente, según la tarea | Cuando lo pides explícitamente |
| **Se configura en** | Settings → Custom Instructions | Customize → Skills | Settings → Connectors |
| **Combinable con** | Todo | Conectores (Partner Skills) | Skills (Partner Skills) |

---

[← Anterior: Custom Instructions](03-custom-instructions.md) | [Siguiente: Claude Projects →](05-projects.md)
