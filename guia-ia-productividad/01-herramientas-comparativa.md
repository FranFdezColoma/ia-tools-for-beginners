# 1. Herramientas de IA para productividad: comparativa

[← Índice](README.md) | [Siguiente: Primeros pasos con Claude.ai →](02-primeros-pasos-claude.md)

---

## Las cuatro herramientas principales

| Herramienta | Quién la hace | Acceso |
|-------------|---------------|--------|
| 🤖 **ChatGPT** | OpenAI | [chat.openai.com](https://chat.openai.com) |
| 🔍 **Gemini** | Google | [gemini.google.com](https://gemini.google.com) |
| 💼 **Microsoft Copilot 365** | Microsoft | Integrado en Office, Teams y Outlook |
| ✨ **Claude.ai** | Anthropic | [claude.ai](https://claude.ai) |

---

## Qué puede hacer cada una

Todas permiten hacer preguntas, redactar textos, resumir documentos y generar código básico. La diferencia real está en **hasta dónde puede llegar cada herramienta**.

| Capacidad | ChatGPT | Gemini | Copilot 365 | Claude.ai |
|-----------|---------|--------|-------------|-----------|
| Conversación y razonamiento | ✅ | ✅ | ✅ | ✅ |
| Análisis de documentos | ✅ | ✅ | ✅ | ✅ |
| Fuentes de conocimiento personalizadas | ✅ GPTs | ✅ Gems | ✅ SharePoint/Graph | ✅ Projects |
| Instrucciones personalizadas | ✅ | ⚠️ Solo por Gem | ✅ | ✅ |
| Búsqueda web en tiempo real | ✅ | ✅ | ✅ | ✅ |
| **Integraciones con sistemas externos (MCPs)** | ⚠️ Solo en GPTs personalizados | ❌ | ⚠️ Solo vía Graph API | ✅ Nativo |
| **Proyectos individuales organizados** | ✅ Projects | ✅ Gems | ✅ | ✅ Projects |
| **Proyectos de equipo compartidos** | ⚠️ Solo GPTs en Team | ❌ | ⚠️ Solo vía SharePoint | ✅ Teams/Enterprise |

> **Nota sobre las ⚠️:** ChatGPT puede conectarse a APIs externas a través de "Actions" en GPTs personalizados, pero requiere configuración técnica y no está disponible en la interfaz estándar de chat. Las integraciones de Claude.ai se añaden desde la interfaz sin ningún conocimiento técnico.

La diferencia clave no es que las demás herramientas no puedan hacer nada de esto, sino que en Claude.ai está **disponible de forma nativa, directa y sin configuración técnica** desde la interfaz de chat.

---

## Por qué Claude.ai destaca frente al resto

ChatGPT, Gemini y Copilot **responden**. Claude.ai también puede **actuar**: conectarse a sistemas externos sin configuración técnica, generar ficheros reales con formato y encadenar múltiples pasos en una sola conversación.

---

### 📄 Ejemplo 1: Generar un documento Word con formato profesional

**Con ChatGPT / Gemini / Copilot:**
```
Prompt:  Redacta una propuesta ejecutiva para la implantación de Dynamics 365 Sales
         para un cliente del sector retail con 500 empleados.

Resultado: Texto en Markdown o texto plano. Para obtener un .docx hay que
           copiar el contenido a Word y formatear manualmente.
```

**Con Claude.ai (integración Google Drive):**
```
Prompt:  Genera una propuesta ejecutiva para Retail Corp (500 empleados, 12 tiendas,
         sin CRM actual). Guárdala como documento Word en mi carpeta "Propuestas"
         de Google Drive con portada, índice, situación actual, propuesta de valor,
         roadmap de 3 fases y tabla de inversión.

Resultado: Claude redacta el documento completo y lo guarda directamente en
           Google Drive como .docx. Sin copiar, sin pegar, sin formatear.
```

---

### 📊 Ejemplo 2: Crear una presentación PowerPoint

**Con ChatGPT / Gemini / Copilot:**
```
Prompt:  Crea una presentación de 8 diapositivas sobre los beneficios de migrar
         a la nube con Microsoft Azure.

Resultado: Texto estructurado con títulos y viñetas. Hay que crear la presentación
           manualmente en PowerPoint.
```

**Con Claude.ai (integración Google Slides):**
```
Prompt:  Crea una presentación de 8 diapositivas sobre los beneficios de migrar
         a Azure. Diseño limpio, diapositiva de agenda al principio y de próximos
         pasos al final. Guárdala en mi Google Drive.

Resultado: Claude genera la presentación con estructura y contenido, y la guarda
           en Google Drive lista para editar.
```

---

### 🔧 Ejemplo 3: Ejecutar acciones sobre sistemas externos (Azure DevOps)

**Con ChatGPT / Gemini / Copilot:** Pueden hablar *sobre* Azure DevOps, pero no conectarse a él desde el chat estándar.

**Con Claude.ai (integración Azure DevOps):**
```
Prompt:  Revisa las pipelines del proyecto CustomerPortal que han fallado hoy,
         identifica la causa del fallo más reciente y crea un bug en el backlog
         con el título y la descripción del error.

Resultado: Claude consulta Azure DevOps en tiempo real, identifica el fallo
           (ej: error de compilación en index.ts:47), crea el bug con ID #4821,
           lo asigna al responsable y confirma con la URL del work item.
```

---

### 🔄 Ejemplo 4: Flujo automatizado en una sola conversación

**Con ChatGPT / Gemini / Copilot:** Cada paso requiere cambiar de herramienta o copiar y pegar manualmente.

**Con Claude.ai (varias integraciones activas en un Project):**
```
Prompt:  Prepara la propuesta para el cliente Retail Corp que tengo en el CRM.
         Consulta sus datos y el historial de la oportunidad, redacta una propuesta
         ejecutiva personalizada, añade un roadmap de 3 fases y guárdala en la
         carpeta del cliente en SharePoint.

Resultado: Claude consulta el CRM, lee el historial de la oportunidad, genera
           la propuesta con los datos reales del cliente (no datos genéricos)
           y la guarda en SharePoint. Todo en una sola conversación.
```

---

## Cuándo usar cada herramienta

| Situación | Herramienta recomendada |
|-----------|------------------------|
| Tienes Microsoft 365 y usas Outlook / Teams / Word diariamente | 💼 Copilot 365 primero |
| Necesitas búsqueda web integrada con el ecosistema Google | 🔍 Gemini |
| Quieres el modelo más conocido con mayor ecosistema de plugins | 🤖 ChatGPT |
| Necesitas generar documentos reales, conectarte a sistemas o crear agentes de equipo | ✨ **Claude.ai** |
| Tu equipo trabaja con Power Platform / Azure y quiere automatizar flujos de trabajo | ✨ **Claude.ai** |

---

[← Índice](README.md) | [Siguiente: Primeros pasos con Claude.ai →](02-primeros-pasos-claude.md)
