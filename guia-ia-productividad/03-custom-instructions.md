# 3. Custom Instructions

[← Anterior: Primeros pasos](02-primeros-pasos-claude.md) | [Siguiente: Skills y Conectores →](04-skills-mcp.md)

---

## Qué son y para qué sirven

Las Custom Instructions son un **prompt de sistema persistente**: un texto que Claude lee antes de cada conversación, sin que tengas que repetirlo. Definen tu rol, el contexto habitual de tu trabajo y las preferencias de formato y tono.

Sin instrucciones personalizadas, Claude responde como asistente genérico. Con ellas, responde como si ya te conociera.

---

## Dónde se configuran

1. Haz clic en tu avatar (esquina superior derecha)
2. **Settings** → **Profile** → **Custom Instructions**
3. Rellena los dos campos:
   - **¿Qué quieres que Claude sepa sobre ti?** — Tu rol, empresa, tecnologías y contexto habitual
   - **¿Cómo quieres que responda Claude?** — Tono, formato, idioma, longitud

Los cambios se aplican a todas las conversaciones nuevas.

> 💡 En cada **Project** puedes añadir instrucciones adicionales específicas de ese dominio. Ver [sección 5](05-projects.md).

---

## Qué incluir y qué evitar

**Incluye:**
- Tu rol y empresa
- Las tecnologías, productos o sectores con los que trabajas
- Preferencias de formato (respuestas cortas vs detalladas, listas vs párrafos)
- Idioma de respuesta
- Contexto habitual de tus peticiones

**Evita:**
- Instrucciones vagas como "sé profesional" o "sé creativo" — Claude ya lo es
- Instrucciones contradictorias ("respuestas cortas" y luego "explica todo en detalle")
- Información sensible (contraseñas, tokens, datos de clientes)
- Instrucciones de más de ~500 palabras — pierden efectividad

---

## Ejemplo 1: Perfil técnico (arquitecto de soluciones Power Platform)

```
¿Qué quieres que Claude sepa sobre ti?
Soy arquitecto de soluciones en una consultora especializada en Power Platform
y Microsoft 365. Trabajo principalmente con Dataverse, Power Pages, PCF y
Power Automate. Los proyectos habituales son para clientes enterprise del
sector servicios financieros, industria y retail.

¿Cómo quieres que responda Claude?
Responde siempre en español. Cuando expliques conceptos técnicos, ve directo
al grano — no incluyas introducción genérica. Usa listas cuando el contenido
lo permita. Si das un ejemplo de código, usa TypeScript o C#. Si hay varias
opciones para resolver algo, dime cuál recomendarías y por qué.
```

**Con estas instrucciones:**
```
Prompt: ¿Cómo gestiono la concurrencia en actualizaciones masivas de Dataverse?

Sin instrucciones: Explicación genérica de Dataverse con introducción de qué es
Con instrucciones: Va directo a las opciones (optimistic concurrency, batch updates,
                   retry policies), con código TypeScript y recomendación concreta.
```

---

## Ejemplo 2: Perfil de negocio (preventa / consultoría)

```
¿Qué quieres que Claude sepa sobre ti?
Trabajo en preventa de soluciones Dynamics 365 (Sales, Customer Service y
Field Service) en una consultora de tamaño medio. Mis propuestas van dirigidas
a directores de operaciones y CIOs de empresas entre 200 y 2000 empleados,
principalmente en España y Latinoamérica. Sector habitual: distribución,
manufacturing y servicios.

¿Cómo quieres que responda Claude?
Responde en español, con tono profesional pero sin jerga técnica excesiva.
Para propuestas y correos, máximo 300 palabras salvo que pida más. Si generas
contenido para cliente, hazlo ejecutivo: orientado a valor de negocio, no a
funcionalidades. Cuando te pida algo para una propuesta, pregúntame siempre
si necesitas datos concretos del cliente antes de empezar.
```

**Con estas instrucciones:**
```
Prompt: Escribe el resumen ejecutivo de una propuesta para un proyecto de
        Dynamics 365 Customer Service para una empresa de distribución.

Sin instrucciones: Texto genérico sobre las funcionalidades de Dynamics 365.
Con instrucciones: Claude pregunta primero por el nombre del cliente, su
                   situación actual y el problema concreto a resolver. Con
                   esa información, genera un resumen ejecutivo orientado a
                   valor de negocio para el director de operaciones.
```

---

## Comparativa con otras herramientas

| | ChatGPT | Gemini | Copilot 365 | Claude.ai |
|---|---|---|---|---|
| **Dónde se configuran** | Settings → Personalization | No tiene instrucciones globales (solo Gems individuales) | Microsoft 365 Copilot Settings | Settings → Profile |
| **Alcance** | Todas las conversaciones | Por Gem creado | Toda la suite M365 | Todas las conversaciones + por Project |
| **Longitud máxima** | ~1500 caracteres | — | ~1000 caracteres | ~2000 caracteres |
| **Instrucciones por proyecto** | Solo en GPTs personalizados | Solo en Gems | No | ✅ En cada Project |

> **Sobre Gemini:** Gemini Advanced no tiene un campo de "instrucciones globales" equivalente. Para personalizar el comportamiento, se crean Gems (asistentes individuales), que son más similares a los Projects que a las custom instructions.

La ventaja clave de Claude.ai es que puedes tener instrucciones **globales** (para ti) e instrucciones **por proyecto** (para un cliente o dominio concreto), activas simultáneamente.

---

[← Anterior: Primeros pasos](02-primeros-pasos-claude.md) | [Siguiente: Skills y Conectores →](04-skills-mcp.md)
