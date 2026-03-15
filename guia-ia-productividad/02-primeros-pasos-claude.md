# 2. Primeros pasos con Claude.ai

[← Anterior: Herramientas y comparativa](01-herramientas-comparativa.md) | [Siguiente: Custom Instructions →](03-custom-instructions.md)

---

## Plan a elegir

Accede a [claude.ai](https://claude.ai) y regístrate con email, Google o Apple.

| Plan | Precio | Lo esencial |
|------|--------|-------------|
| **Free** | Gratis | Acceso básico con límite de mensajes diario. Sin Projects ni Conectores. |
| **Pro** | ~$20/mes | Sin límites prácticos, todos los modelos, Projects e integraciones. |
| **Team** | ~$25/usuario/mes | Todo lo de Pro + Projects compartidos, administración centralizada. |
| **Enterprise** | Precio negociado | Todo lo de Team + SSO, seguridad avanzada, SLA, datos no usados para entrenamiento. |

> Para Projects compartidos y Conectores necesitas al menos **Pro** (individual) o **Team** (equipo).

---

## Anatomía de un buen prompt

El error más común es escribir prompts demasiado cortos. Claude necesita contexto para dar resultados útiles. La fórmula es:

```
[Rol] + [Contexto] + [Tarea] + [Formato / Restricciones]
```

**Ejemplo — mal prompt vs. buen prompt:**

| Mal prompt | Buen prompt |
|------------|-------------|
| "Escribe una propuesta para el cliente" | "Actúa como consultor senior de Dynamics 365. El cliente es Retail Corp (500 empleados, 12 tiendas, sin CRM actual). Redacta un resumen ejecutivo de 1 página que argumente por qué Dynamics 365 Sales mejora su conversión. Tono directivo, sin jerga técnica." |
| "Dame ideas para la reunión de mañana" | "Mañana tengo un kickoff con un cliente nuevo del sector seguros que quiere modernizar su gestión de siniestros. Dame 5 preguntas de discovery que no debería olvidar hacerle, organizadas por área (negocio, operaciones, IT)." |

**Los cuatro elementos:**

- **Rol** — quién eres o cómo debe actuar Claude: _"Actúa como consultor de preventa..."_, _"Eres un experto en..."_
- **Contexto** — información relevante: sector del cliente, tamaño, situación actual, objetivo del proyecto
- **Tarea** — qué quieres exactamente: redactar, resumir, analizar, comparar, estructurar
- **Formato / Restricciones** — extensión, secciones, tono, audiencia, idioma, qué evitar

Cuanto más contexto des, menos iteraciones necesitarás.

---

## Cómo iterar sin empezar de cero

La primera respuesta raramente es la definitiva. Claude mejora con feedback directo dentro de la misma conversación:

| Qué quieres ajustar | Cómo pedirlo |
|---------------------|--------------|
| Más conciso | _"Reduce a la mitad, sin perder los puntos clave"_ |
| Más detalle en una sección | _"Desarrolla más el apartado de roadmap"_ |
| Cambiar el tono | _"Hazlo más directo y menos corporativo"_ |
| Añadir algo que falta | _"Añade una sección de riesgos al final"_ |
| Cambiar el formato | _"Conviértelo en una tabla comparativa"_ |
| Reformatear para Word | _"Adapta el texto para copiarlo a Word sin perder la estructura"_ |

No necesitas volver a escribir el contexto. Claude mantiene la conversación activa y ajusta sobre lo que ya generó.

---

## Cómo adjuntar documentos

Puedes arrastrar PDFs, Word, Excel, imágenes o texto plano directamente al campo de conversación.

**Casos de uso habituales:**

```
Prompt: [adjuntas una RFP de 40 páginas]
Resume los requisitos técnicos y funcionales en una tabla.
Identifica qué requisitos son obligatorios y cuáles son deseables.
```

```
Prompt: [adjuntas una propuesta anterior tuya]
Analiza esta propuesta y dime qué argumentos de valor son débiles
y cómo los reforzarías sabiendo que el cliente es del sector industrial.
```

```
Prompt: [adjuntas un contrato en PDF]
Identifica las cláusulas de penalización, los plazos críticos
y cualquier condición inusual que debería revisar un abogado.
```

> ⚠️ Claude no recuerda adjuntos entre conversaciones distintas. Si vas a trabajar con documentos de forma recurrente, usa un **Project** y súbelos a la base de conocimiento — así estarán siempre disponibles sin tener que adjuntarlos cada vez.

---

## Cuándo empezar una conversación nueva

Claude mantiene el contexto de toda la conversación activa. Esto es útil para iterar, pero el contexto se acumula y puede degradar la calidad de las respuestas si la conversación se vuelve muy larga o mezcla temas distintos.

**Empieza una conversación nueva cuando:**
- Cambias de tema o de proyecto completamente
- La conversación tiene más de 20-30 mensajes y notas que las respuestas se alejan del objetivo
- Quieres probar un enfoque completamente diferente sin que Claude se vea influido por el hilo anterior

**Continúa en la misma conversación cuando:**
- Estás iterando sobre el mismo documento o tarea
- Quieres que Claude recuerde el contexto del cliente o proyecto que ya explicaste
- Estás haciendo un flujo de trabajo encadenado (analizar → redactar → ajustar)

---

## Errores comunes

| Error | Cómo evitarlo |
|-------|---------------|
| Prompts demasiado cortos | Usa siempre: rol + contexto + tarea + formato |
| Aceptar la primera respuesta sin revisar | Revisa siempre cifras, fechas y datos del cliente — Claude puede equivocarse |
| Tratar a Claude como un buscador | Para datos en tiempo real, conecta un Conector o usa la búsqueda web |
| Empezar de cero si la respuesta no es perfecta | Responde con instrucciones directas: "Más conciso", "Añade un ejemplo" |
| Pegar texto largo sin instrucción | Acompaña siempre el adjunto con una petición clara |
| Mezclar muchos temas en una conversación | Empieza conversaciones nuevas por proyecto o tarea principal |

---

[← Anterior: Herramientas y comparativa](01-herramientas-comparativa.md) | [Siguiente: Custom Instructions →](03-custom-instructions.md)
