# 2. Elección del modelo correcto

[← Anterior: ¿Qué es Copilot CLI?](01-que-es.md) | [Siguiente: Instalación →](03-instalacion.md)

---

Elegir bien el modelo es la palanca más directa para optimizar la relación **calidad / coste / velocidad**.

> **Regla de oro:** usa el modelo más barato que resuelva tu tarea correctamente. Sube un nivel solo cuando el modelo actual no dé resultados satisfactorios tras 2 intentos.

---

## Decisión rápida

| Necesito... | Nivel | Copilot CLI | Claude Code |
|-------------|-------|-------------|-------------|
| Preguntar sobre el código, documentar, renombrar | 1 | GPT-4.1 *(gratis)* | Haiku 4.5 |
| Generar tests, refactorizar dentro de un fichero | 2 | Haiku 4.5 | Haiku 4.5 |
| Módulos completos, refactorizaciones multi-fichero | 3 ⭐ | Sonnet 4.6 | Sonnet 4.6 |
| Arquitecturas complejas, bugs difíciles de diagnosticar | 4 | Opus 4.6 | Opus 4.6 |

---

## Cambiar de modelo

```bash
# GitHub Copilot CLI
copilot
> /model   # Muestra lista interactiva de modelos

# Claude Code
claude
> /model   # Muestra lista interactiva (solo modelos Anthropic)
```

El cambio es inmediato y aplica al resto de la sesión.

---

## Modelos disponibles en GitHub Copilot CLI

| Modelo | Multiplicador de crédito | Velocidad |
|--------|--------------------------|-----------|
| **GPT-5 mini / GPT-4.1** | 0x — incluido sin coste adicional | ⚡⚡⚡ |
| **Claude Haiku 4.5** | 0.33x | ⚡⚡⚡ |
| **Claude Sonnet 4.6** | 1x | ⚡⚡ |
| **GPT-5.3-Codex** | 1x | ⚡⚡ |
| **Claude Opus 4.6** | 3x | ⚡ |

> Con los modelos gratuitos (0x) puedes hacer preguntas ilimitadas sin consumir cuota de _premium requests_.

```bash
# Lista interactiva en Copilot CLI
> /model
# ● gpt-4.1              (0x — free)
# ● gpt-5-mini           (0x — free)
# ● claude-haiku-4.5     (0.33x)
# ● claude-sonnet-4.6    (1x  — recomendado ⭐)
# ● gpt-5.3-codex        (1x)
# ● claude-opus-4.6      (3x)
```

---

## Modelos disponibles en Claude Code

Claude Code solo usa modelos Anthropic. El coste se gestiona a través de la suscripción de Anthropic.

| Modelo | Velocidad | Cuándo usarlo |
|--------|-----------|---------------|
| **Claude Haiku 4.5** | ⚡⚡⚡ | Tareas rápidas y sencillas |
| **Claude Sonnet 4.6** | ⚡⚡ | **Uso recomendado por defecto** |
| **Claude Opus 4.6** | ⚡ | Problemas muy complejos |

```bash
# Lista interactiva en Claude Code
> /model
# ● claude-haiku-4.5     (rápido)
# ● claude-sonnet-4.6    (recomendado ⭐)
# ● claude-opus-4.6      (máxima capacidad)
```

> Claude Code no tiene acceso a modelos GPT o Gemini. Si necesitas modelos de OpenAI o Google, usa GitHub Copilot CLI.

---

## Guía de cuándo usar cada nivel

### Nivel 1 — GPT-4.1 / GPT-5 mini (Copilot CLI) / Haiku (ambas)

Ideal para tareas que no requieren razonamiento profundo:

- Responder preguntas concretas sobre el código
- Generar comentarios y documentación inline
- Renombrar variables o aplicar naming conventions
- Formatear datos (CSV, JSON, SQL)

```bash
# Copilot CLI
> /model gpt-4.1
> ¿Qué hace el método calculateDiscount en src/pricing.ts?

# Claude Code
> /model claude-haiku-4.5
> ¿Qué hace el método calculateDiscount en src/pricing.ts?
```

### Nivel 2 — Haiku / Sonnet básico (ambas)

Para tareas directas con algo más de capacidad de razonamiento:

- Generar tests unitarios para funciones sencillas
- Refactorizaciones menores dentro de un solo fichero
- Corrección de errores de linting o compilación

```bash
# Copilot CLI
> /model claude-haiku-4.5
> Escribe tests unitarios para la función validateEmail

# Claude Code (mismo modelo)
> /model claude-haiku-4.5
> Escribe tests unitarios para la función validateEmail
```

### Nivel 3 — Claude Sonnet 4.6 (ambas) — Recomendado por defecto

El equilibrio óptimo entre capacidad y coste para la mayoría de tareas:

- Refactorizaciones complejas que afectan a múltiples ficheros
- Generación de componentes o módulos completos
- Revisión de código y análisis de calidad
- Depuración de lógica de negocio

```bash
# Copilot CLI
> /model claude-sonnet-4.6
> Refactoriza el módulo de autenticación para usar el patrón Repository

# Claude Code (mismo modelo)
> /model claude-sonnet-4.6
> Refactoriza el módulo de autenticación para usar el patrón Repository
```

### Nivel 4 — Claude Opus 4.6 (ambas) — Solo cuando Sonnet no es suficiente

Reserva Opus para los problemas más complejos. Su coste es 3x mayor en Copilot CLI:

- Diseño de arquitecturas de alto nivel con múltiples sistemas
- Resolución de bugs muy difíciles de diagnosticar
- Razonamiento multi-step complejo con muchas restricciones

```bash
# Copilot CLI
> /model claude-opus-4.6
> Diseña la arquitectura de un sistema de procesamiento de eventos en tiempo real

# Claude Code (mismo modelo)
> /model claude-opus-4.6
> Diseña la arquitectura de un sistema de procesamiento de eventos en tiempo real
```

---

## Ejemplos por tipo de proyecto

### Power Platform / Dataverse

| Tarea | Copilot CLI | Claude Code |
|-------|-------------|-------------|
| Generar fórmulas Power Fx sencillas | GPT-4.1 | Haiku 4.5 |
| Queries FetchXML básicas | GPT-4.1 / Haiku 4.5 | Haiku 4.5 |
| Desarrollar un componente PCF completo | Sonnet 4.6 | Sonnet 4.6 |
| Lógica compleja de Power Automate | Sonnet 4.6 | Sonnet 4.6 |
| Diseñar arquitectura multi-entorno Dataverse | Opus 4.6 | Opus 4.6 |

### Backend (.NET, Node.js)

| Tarea | Modelo recomendado (ambas) |
|-------|---------------------------|
| Documentar endpoints | Nivel 1 |
| Generar tests unitarios | Nivel 2 |
| Implementar endpoint con validaciones | Sonnet 4.6 |
| Optimizar queries complejas | Sonnet 4.6 |
| Diseñar sistema de colas distribuido | Opus 4.6 |

---

[← Anterior: ¿Qué es Copilot CLI?](01-que-es.md) | [Siguiente: Instalación →](03-instalacion.md)
