---
title: Prompt Engineering – Guía Práctica para Equipos Técnicos
author: Germán Aliprandi
theme: geist
transition: slide-left
colorSchema: light
canvasWidth: 1280

defaults:
  layout: default
  transition: fade
  
seoMeta:
  ogTitle: "Prompt Engineering – Guía Práctica para Equipos Técnicos"
  ogDescription: "Esta guía presenta técnicas y frameworks para optimizar la interacción con Modelos de Lenguaje (LLMs) en entornos de desarrollo."
  ogUrl: "https://galiprandi.github.io/prompt-engineering/"
---


# Prompt Engineering

### Guía Práctica

Esta guía presenta técnicas y frameworks para optimizar la interacción con Modelos de Lenguaje (LLMs) en entornos de desarrollo.

✍️ Autor: [Germán Aliprandi](https://galiprandi.github.io/me/)

⚖️ Licencia: [MIT](https://opensource.org/licenses/MIT)


---


# Agenda

**1. Fundamentos y Principios Clave**
   - Impacto del prompt, anatomía de un LLM y buenas prácticas.

**2. Técnicas de Prompting Esenciales**
   - Zero-shot, Few-shot, Chain-of-Thought, RAG y más.

**3. Framework CRTR para Escalabilidad**
   - Estructura, beneficios y ejemplos prácticos.

**4. Recursos y Siguientes Pasos**
   - Plantillas, lecturas y herramientas útiles.



---

# El Prompt como Vector de Optimización

La calidad del prompt es el principal mecanismo de control sobre el rendimiento, el coste y la fiabilidad de un sistema basado en LLMs.

### Beneficios

**1. Reducción de Ambigüedad**
Mejora precisión y consistencia de las respuestas.

**2. Eficiencia de Recursos**
Menor consumo de tokens → menor coste y latencia.

**3. Mitigación de Riesgos**
Primera línea de defensa contra alucinaciones y *outputs* inesperados.

**4. Estandarización y Escalabilidad**
Permite crear prompts reutilizables, versionados y fáciles de mantener.


---

# Cómo “piensa” un LLM

Un LLM no “entiende” el lenguaje; es un motor de predicción que sigue un proceso matemático para generar el siguiente token más probable. Tu prompt es el punto de partida de este ciclo:

**1. Tokenización**  
El prompt se descompone en piezas (tokens).  
`"Resume este texto"` → `["Resume", "este", "texto"]`

**2. Embeddings (Vectores Semánticos)**  
Cada token se convierte en un vector numérico que captura su significado y relación con otros.

**3. Capas de Atención (Self-Attention)**  
El modelo pondera la importancia de cada token del contexto para decidir dónde "enfocar" su cálculo.

**4. Predicción (Distribución de Probabilidad)**  
Calcula la probabilidad de cada palabra posible en su vocabulario para ser el siguiente token.

**5. Generación y Bucle**  
Elige el token más probable, lo añade a la secuencia y repite todo el proceso hasta generar la respuesta completa.

<hr/>

🧠 **Tu prompt es el director de orquesta:** cada palabra que añades o ajustas es una palanca para dirigir la <strong>atención</strong> del modelo y, por tanto, el resultado final.

---

## ¿Cómo influye el prompt en la inferencia?

El prompt es la palanca que ajusta el motor de inferencia del LLM en tiempo real. Así es como cada palabra que escribes moldea el resultado:

**1. Condicionamiento del Contexto:**
Cada token de tu prompt se convierte en un vector que establece el punto de partida para el mecanismo de atención.

**2. Dirección del Foco de Atención:**
Al añadir instrucciones, ejemplos o un rol, le das "pistas" al modelo sobre qué tokens son más importantes, guiando su foco.

**3. Modificación de la Probabilidad:**
El resultado de la atención altera la distribución de probabilidad para el siguiente token. Un prompt específico "afila" esta distribución, haciendo que la respuesta deseada sea mucho más probable.

**4. Control sin Re-entrenamiento:**
Logras controlar la salida del modelo de forma precisa **sin necesidad de modificar sus pesos internos**, todo ocurre durante la inferencia.


---

# Buen Prompt vs Mal Prompt

No todos los prompts son iguales. Pequeños cambios pueden transformar la calidad de las respuestas.

| Prompt flojo | Prompt afinado |
|--------------|----------------|
| “Resume este texto.” | “Eres editor técnico. Resume en ≤150 palabras, sin jerga, en español neutro, destacando argumentos clave y conclusiones.” |

✅ **Un buen prompt define rol, contexto, límites y tono.**

❌ **Un prompt vago deja demasiadas decisiones al modelo.**

---

# Técnica 1: Zero-shot / Few-shot

### Zero-shot
✅ El modelo responde usando solo su conocimiento general, sin ejemplos específicos.  

### Few-shot
✅ Incluye 1-5 ejemplos para mostrar al modelo el estilo, formato o nivel de detalle deseado.  

**Ejemplo Few-shot**

```text
Eres Chat Bot de soporte.

Ejemplo →
Usuario: ¿Diferencia entre `interface` y `type` en TypeScript?
Bot: Tanto `interface` como `type` definen formas, pero…

Nueva pregunta →
Usuario: ¿Para qué sirve `Partial<T>`?
Bot:
```

---

# Técnica 2: Chain-of-Thought (CoT)

- “Piensa paso a paso” obliga al modelo a explicitar su razonamiento.  
- Mejora precisión en tareas complejas o con varios pasos.  
- Variantes avanzadas: *Self-Consistency*, *Tree of Thoughts*.

**Ejemplo**

```text
Pregunta: ¿Cuál es la complejidad media de `quickSort`?  
Razoná paso a paso y, al final, responde en una sola línea.
```

---

# Técnica 3: Role / Persona

- Define quién “habla”: mentor, abogado, tester, etc.  
- Establece contexto y tono coherente.  
- Mejora consistencia y relevancia de la respuesta.

**Ejemplo**

```text
Eres mentor senior de TypeScript.  
Explica a un junior por qué conviene usar `unknown` en lugar de `any`.
```

---

# Técnica 4: Retrieval-Augmented Generation (RAG)

- Integra fragmentos de documentos externos para respuestas precisas y actualizadas.  
- Ideal para FAQs, bases de conocimiento y documentación interna.  
- Reduce alucinaciones al apoyar respuestas en datos verificables.

**Ejemplo**

```text
Contexto:  
• docs/api-auth.md (líneas 10-30)  
• README.md (líneas 55-60)  

Pregunta: ¿Cómo cambio el token de refresh?
```

---

# Técnica 5: Multimodal & Tool-augmented

- Combina texto, imágenes y llamadas a funciones (`function calling`).  
- Útil para ejecutar código, analizar diagramas o integrar herramientas externas.  
- Amplía las capacidades del LLM más allá del texto plano.

**Ejemplo**

```json
{
  "role": "system",
  "content": "Tienes la función runTests()"
}
```

---

# Framework CRTR – Definición

El framework CRTR es una metodología sistemática para estructurar prompts de manera clara y escalable.  
Divide el prompt en cuatro bloques esenciales que ayudan a reducir ambigüedad y facilitar la reutilización:  
Contexto (qué sabe la IA), Rol (quién responde), Tarea (qué debe hacer) y Resultado (cómo se presenta la salida).  
Esto permite crear prompts mantenibles y auditables en equipos técnicos.

| Bloque          | Contenido                  | Pregunta clave              |
|-----------------|----------------------------|----------------------------|
| **C – Context** | Información relevante      | ¿Qué debe saber la IA?     |
| **R – Role**    | Identidad o perspectiva    | ¿Quién está respondiendo?  |
| **T – Task**    | Acción solicitada          | ¿Qué debe hacer la IA?     |
| **R – Result**  | Formato y limitaciones     | ¿Cómo quiero la salida?    |

---

## Beneficios del Framework CRTR

- 🎯 Reduce ambigüedad, mejorando la calidad de las respuestas.  
- 🔄 Facilita la creación de plantillas reutilizables por todo el equipo.  
- 📚 Permite versionar y auditar prompts de forma sencilla.  
- 🚀 Escala bien en proyectos complejos con múltiples casos de uso.
- 🐛 Facilita el debugging de prompts: Si una respuesta es incorrecta, puedes aislar el problema. ¿Falló el Contexto? ¿El Rol es ambiguo? ¿La Tarea es imprecisa?

Estas ventajas hacen que CRTR sea ideal para equipos técnicos que buscan mantener consistencia y eficiencia en sus interacciones con LLMs.

---

# CRTR – Ejemplo 1

Este ejemplo muestra cómo estructurar un prompt para redactar documentación técnica clara y precisa.  
Definimos el contexto para situar al modelo, el rol para darle la perspectiva adecuada, la tarea específica y el formato esperado para el resultado.  
Esto garantiza respuestas consistentes y alineadas con el objetivo.

```text
Context: Manual “API-Pagos v2.3”.  
Role: Redactor técnico senior.  
Task: Redacta la sección “Autenticación”.  
Result: Markdown, ≤200 palabras, diagrama ASCII.
```

---

# CRTR – Ejemplo 2

Este prompt está diseñado para comunicar resultados de un sprint de forma clara y motivadora.  
Se especifica el contexto del proyecto, el rol del emisor y la tarea de crear un anuncio con un tono apropiado, limitando la extensión para mantenerlo conciso y efectivo.

```text
Context: Sprint 5 finalizado; hitos: OAuth, 95% tests verdes.  
Role: Product Owner.  
Task: Publicar anuncio interno de 4 párrafos.  
Result: ≤180 palabras, tono motivador, emojis moderados.
```

---

# CRTR – Ejemplo 3

Este prompt estructura la descripción de un Pull Request para facilitar la revisión y auditoría.  
Incluye contexto técnico, rol del revisor, tarea concreta y formato detallado, ayudando a que la información crítica sea clara y organizada.

```text
Context: Migración de “user-auth” a TS 5.5 estricto.  
Role: Revisor/a Front-End principal.  
Task: Redactar descripción de Pull Request incluyendo riesgos y pruebas.  
Result: Markdown con secciones: Contexto, Cambios, Pruebas, Checklist QA.
```

---

# CRTR – Recursos

- Plantilla rápida:

```text
Context: …  
Role: …  
Task: …  
Result: …
````

* [LearnPrompting – Prompt Structure & Key Parts](https://learnprompting.org/docs/intro/prompt-structure)
  Guía completa para entender y construir prompts efectivos.

* [DAIR AI – Prompt Engineering Guide](https://www.dair.ai/guide/prompt-engineering)
  Recurso detallado con técnicas y mejores prácticas para prompt engineering.

* [OpenAI Cookbook – Prompt Best Practices](https://github.com/openai/openai-cookbook/blob/main/examples/Prompt_Best_Practices.ipynb)
  Colección de ejemplos y consejos oficiales para mejorar la interacción con modelos OpenAI.

* [ChatGPT Prompt Engineering for Developers (deeplearning.ai)](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/)
  Curso práctico para aprender técnicas de prompting aplicado a desarrolladores.

---

# Muchas gracias

### Germán Aliprandi

📧 galiprandi@gmail.com

🔗 [linkedin.com/in/galiprandi](https://linkedin.com/in/galiprandi)
