# 5. Claude Projects

[← Anterior: Skills y Conectores](04-skills-mcp.md) | [← Volver al índice](README.md)

---

## Qué es un Project

Un Project es un **agente especializado persistente** que combina en un solo espacio:

- **Fuentes de conocimiento** — documentos, PDFs, URLs, bases de conocimiento internas
- **Instrucciones específicas** — cómo debe comportarse el agente para ese dominio
- **Conectores** — conexiones a herramientas externas activas en el Project
- **Acceso de equipo** — todos los miembros ven el mismo agente con la misma configuración

La diferencia con una conversación normal es que **el conocimiento y la configuración persisten**. Abres el Project mañana y el agente sigue sabiendo lo que sabe y comportándose como se configuró.

> Disponible a partir del **plan Pro** para uso individual y **Team/Enterprise** para uso compartido.

---

## Cómo configurar un Project

**1. Crear** — Panel izquierdo → **Projects** → **New Project** → añade nombre y descripción.

**2. Conocimiento** — Pestaña **Knowledge** → sube PDFs, Word, txt o añade URLs. Claude indexa el contenido y lo usa en todas las conversaciones del Project.

Qué subir:
- Documentación del producto o servicio
- Propuestas anteriores de calidad
- Plantillas de documentos
- Arquitecturas de referencia o restricciones técnicas

**3. Instrucciones** — Pestaña **Instructions** → escribe cómo debe comportarse el agente en este dominio. Se combinan con tus instrucciones globales (ver [sección 3](03-custom-instructions.md)).

**4. Conectores** — Pestaña **Integrations** → activa las conexiones relevantes (Google Drive, Jira, etc.).

**5. Verificar antes de compartir** — Haz al menos 3 pruebas: una pregunta de conocimiento, una petición de generación de contenido y una acción sobre un sistema externo (si hay integraciones configuradas).

---

## 🏷️ Caso de uso 1: Project de preventa de Dynamics 365

**Contexto:** Equipo de preventa que trabaja propuestas para clientes que evalúan Dynamics 365 Sales y Customer Service.

**Configuración del Project:**
- **Conocimiento:** documentación oficial de Dynamics 365, 10 propuestas ganadas en el último año, plantilla de propuesta ejecutiva del equipo, fichas de casos de éxito por sector
- **Instrucciones:** "Eres un consultor senior de preventa de Dynamics 365. Cuando generes propuestas, usa siempre datos del cliente si te los proporciono. El tono es ejecutivo, orientado a valor de negocio. Incluye siempre un apartado de ROI estimado y casos de éxito del mismo sector."
- **MCPs:** Google Drive (para guardar propuestas), CRM interno (para consultar datos de la oportunidad)

**Ejemplo de uso:**

```
Prompt:
El cliente es Retail Corp, 800 empleados, 15 tiendas en España, actualmente
sin CRM. Su director de ventas quiere mejorar el seguimiento de oportunidades
y el forecasting. Genera la propuesta ejecutiva completa y guárdala en Drive.

Resultado:
El agente usa la plantilla del equipo, incluye casos de éxito del sector retail
de las propuestas anteriores, calcula un ROI estimado para 800 usuarios y guarda
el documento en Drive con el nombre y la estructura acordada por el equipo.
No es texto genérico: es una propuesta con los argumentos y la estructura que
el equipo ha validado que funciona.
```

---

## 🏗️ Caso de uso 2: Project de solutioning técnico

**Contexto:** Equipo de arquitectura que diseña soluciones técnicas para proyectos Power Platform.

**Configuración del Project:**
- **Conocimiento:** arquitecturas de referencia del equipo, restricciones técnicas habituales de clientes enterprise, guías de buenas prácticas de Dataverse y Power Pages, registro de decisiones técnicas de proyectos anteriores
- **Instrucciones:** "Eres un arquitecto senior de Power Platform. Cuando propongas arquitecturas, considera siempre: límites de Dataverse (llamadas API, almacenamiento), compatibilidad con Unified Interface, requisitos de ALM (DEV/TEST/PROD) y seguridad basada en roles. Si la solución implica PCF, indica el impacto de rendimiento."
- **MCPs:** Azure DevOps (para consultar backlog técnico y crear work items)

**Ejemplo de uso:**

```
Prompt:
El cliente necesita un portal de autoservicio para sus 500 clientes B2B donde
puedan consultar pedidos, facturas y abrir incidencias. Tienen Dynamics 365
Customer Service y Azure AD. ¿Qué arquitectura recomiendas?

Resultado:
El agente propone una arquitectura con Power Pages como portal (justificando
la elección frente a un desarrollo custom), conexión nativa a Dataverse,
autenticación via Azure AD B2C, y señala las restricciones de API de Dataverse
para 500 usuarios concurrentes con la recomendación de caché. Incluye un
diagrama en texto y los próximos pasos técnicos para el proyecto.
```

---

## 👥 Caso de uso 3: Project de recruiting

**Contexto:** Equipo de RRHH que gestiona procesos de selección para perfiles técnicos y de negocio.

**Configuración del Project:**
- **Conocimiento:** perfiles de puesto abiertos, criterios de evaluación por posición, plantilla de informe de candidato, competencias clave por nivel (junior/senior/lead)
- **Instrucciones:** "Eres un recruiter experto en perfiles tecnológicos y de consultoría. Cuando evalúes candidatos, sé objetivo y estructura el informe por competencias. Nunca incluyas valoraciones subjetivas sin evidencia concreta de la entrevista. El idioma del informe es siempre español."
- **MCPs:** Google Drive (para guardar informes de candidatos)

**Ejemplo de uso:**

```
Prompt:
Estas son mis notas de la entrevista con Ana García para el puesto de
consultora senior de Dynamics 365:

- 6 años de experiencia en CRM
- Ha liderado 3 proyectos de Dynamics 365 Sales en clientes enterprise
- Conoce Power Automate pero no tiene experiencia con PCF
- Comunicación muy clara, buena gestión del cliente
- Disponibilidad: 1 mes de preaviso
- Pretensión: 55.000€

Genera el informe de candidato y guárdalo en Drive en la carpeta del proceso.

Resultado:
El agente genera un informe estructurado con: resumen ejecutivo, evaluación
por competencias técnicas (con evidencias concretas de las notas), evaluación
de soft skills, brechas respecto al perfil y recomendación de contratación
con justificación. Lo guarda en Drive en la carpeta del proceso.
```

---

## Ventajas frente al uso individual

| | Sin Projects (chat individual) | Con Projects (equipo) |
|---|---|---|
| **Conocimiento compartido** | Cada persona sube los mismos documentos por separado | Todos acceden al mismo repositorio de conocimiento |
| **Consistencia** | Cada miembro obtiene respuestas distintas según cómo pregunta | Todos trabajan con el mismo agente configurado igual |
| **Calidad del output** | Variable — depende de cómo esté configurado el contexto | Uniforme — el Project garantiza que se usen las plantillas y el tono del equipo |
| **Mantenimiento** | Imposible — el conocimiento está en conversaciones dispersas | Centralizado — el administrador actualiza el Project y todos se benefician |
| **Onboarding** | Nuevo miembro tarda en aprender a usar la IA correctamente | Nuevo miembro usa el Project desde el primer día con el mismo nivel que el resto |

---

[← Anterior: Skills y Conectores](04-skills-mcp.md) | [← Volver al índice](README.md)
