---
title: 🧠 Prompt Engineering
subtitle: Guía Práctica para Equipos Técnicos
author: Germán Aliprandi
date: Abril 2025
license: Licencia MIT

theme: geist
colorSchema: light
canvasWidth: 1280
transition: fade

defaults:
  layout: cover
  transition: fade

seoMeta:
  title: "Prompt Engineering – Guía Práctica para Equipos Técnicos"
  description: "Esta guía presenta técnicas y frameworks para optimizar la interacción con Modelos de Lenguaje (LLMs) en entornos de desarrollo."
  keywords: "prompt engineering, técnicas de prompt, frameworks de prompt, desarrollo de IA, arquitectura de IA, inteligencia artificial, patrones de diseño, Cencosud Tech"
  ogTitle: "Prompt Engineering – Guía Práctica para Equipos Técnicos"
  ogDescription: "Esta guía presenta técnicas y frameworks para optimizar la interacción con Modelos de Lenguaje (LLMs) en entornos de desarrollo."
  ogUrl: "https://galiprandi.github.io/prompt-engineering"

  twitterCard: "summary_large_image"
  twitterCreator: "@galiprandi"
  canonicalUrl: "https://galiprandi.github.io/prompt-engineering"
---

# {{ $frontmatter.title }}
### {{ $frontmatter.subtitle }}


Esta guía presenta técnicas y frameworks para optimizar la interacción con Modelos de Lenguaje (LLMs) en entornos de desarrollo.

<div class="mt-10 text-l">
<b>{{ $frontmatter.author }}</b> 
<br>
{{ $frontmatter.date }}
<br>
{{ $frontmatter.license }}
</div>

<div class="absolute bottom-8 right-8 text-l bg-black/10 px-4 py-2 rounded-full">
  ➡️ Navega con la flecha derecha del teclado
</div>


---

## Temas a cubrir

**1. Fundamentos y Principios Clave:** Impacto del prompt, anatomía de un LLM y buenas prácticas.

**2. Técnicas de Prompting Esenciales:** Zero-shot, Few-shot, Chain-of-Thought, RAG, Multimodal & Tool-augmented y Incremental Prompting.

**3. Framework CRTR:** Estructura, beneficios y ejemplos prácticos.

**4. Recursos:** Plantillas, lecturas y herramientas útiles.



---

## El Prompt como Vector de Optimización

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

## Cómo “piensa” un LLM

Un LLM no “entiende” el lenguaje; es un motor de predicción que sigue un proceso matemático para generar el siguiente token más probable. Tu prompt es el punto de partida de este ciclo:

**1. Tokenización:** El prompt se descompone en piezas (tokens).  
`"Resume este texto"` → `["Resume", "este", "texto"]`

**2. Embeddings (Vectores Semánticos):** Cada token se convierte en un vector numérico que captura su significado y relación con otros.

**3. Capas de Atención (Self-Attention):** El modelo pondera la importancia de cada token del contexto para decidir dónde "enfocar" su cálculo.

**4. Predicción (Distribución de Probabilidad):** Calcula la probabilidad de cada palabra posible en su vocabulario para ser el siguiente token.

**5. Generación y Bucle:** Elige el token más probable, lo añade a la secuencia y repite todo el proceso hasta generar la respuesta completa.

#### 💡 *Tu prompt es el director de orquesta:* cada palabra que añades o ajustas es una palanca para dirigir la *atención* del modelo y, por tanto, el resultado final.

---

## ¿Cómo influye el prompt en la inferencia?

El prompt es la palanca que ajusta el motor de **inferencia del LLM en tiempo real**. Así es como cada palabra que escribes **moldea el resultado**:

**1. Condicionamiento del Contexto:**
Cada token de tu prompt se convierte en un vector que establece el punto de partida para el mecanismo de atención.

**2. Dirección del Foco de Atención:**
Al añadir instrucciones, ejemplos o un rol, le das "pistas" al modelo sobre qué tokens son más importantes, guiando su foco.

**3. Modificación de la Probabilidad:**
El resultado de la atención altera la distribución de probabilidad para el siguiente token. Un prompt específico "afila" esta distribución, haciendo que la respuesta deseada sea mucho más probable.

**4. Control sin Re-entrenamiento:**
Logras controlar la salida del modelo de forma precisa **sin necesidad de modificar sus pesos internos**, todo ocurre durante la inferencia.


---

## Buen Prompt vs. Mal Prompt

<div class="grid grid-cols-2 gap-8 items-start">

<div>
<h3 class="flex items-center gap-2">✅ Buen Prompt</h3>
<p class="text-sm opacity-75">Define rol, contexto, límites y formato, guiando al modelo hacia un resultado preciso.</p>
<br/>
<div class="p-4 bg-green-50 text-green-800 rounded-lg">
"Eres editor técnico. Resume en ≤150 palabras, sin jerga, en español neutro, destacando argumentos clave y conclusiones."
</div>
</div>

<div>
<h3 class="flex items-center gap-2">❌ Mal Prompt</h3>
<p class="text-sm opacity-75">Deja decisiones críticas al modelo, generando respuestas genéricas o incorrectas.</p>
<br/>
<div class="p-4 bg-red-50 text-red-800 rounded-lg">
"Resume este texto."
</div>
</div>

</div>

---

## Técnica 1: Zero-shot / Few-shot

### Zero-shot

El modelo responde usando solo su conocimiento general, sin ejemplos específicos.

### Few-shot

Incluye 1-5 ejemplos para mostrar al modelo el estilo, formato o nivel de detalle deseado.

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

## Técnica 2: Chain-of-Thought (CoT)


**Beneficios**

✔️ “Piensa paso a paso” obliga al modelo a explicitar su razonamiento.  

✔️ Mejora precisión en tareas complejas o con varios pasos.  

**Variantes avanzadas:** *Self-Consistency* (auto-consistencia), *Tree of Thoughts* (árbol de pensamientos).

**Ejemplo**

```text
Pregunta: ¿Cuál es la complejidad media de `quickSort`?  
Razoná paso a paso y, al final, responde en una sola línea.
```

---

## Técnica 3: Role / Persona

Asignar una identidad y un rol al modelo es clave para guiar su comportamiento y estilo.

**Beneficios**

✔️ Aumenta la relevancia de la respuesta.

✔️ Asegura consistencia y coherencia en el tono.

✔️ Facilita la creación de plantillas reutilizables por todo el equipo.

**Ejemplo**

```text
Eres mentor senior de TypeScript.  
Explica a un junior por qué conviene usar `unknown` en lugar de `any`.
```

---

## Técnica 4: Retrieval-Augmented Generation (RAG)

Integra conocimiento externo en tiempo real para generar respuestas más precisas y actualizadas, basándose en fuentes de datos verificables.

**Beneficios**

✔️ Reduce alucinaciones al basar las respuestas en datos concretos.

✔️ Permite responder sobre información muy reciente o privada.

✔️ Ideal para sistemas de preguntas y respuestas sobre documentación interna.

**Ejemplo**

```text
Contexto:  
• docs/api-auth.md (líneas 10-30)  
• README.md (líneas 55-60)  

Pregunta: ¿Cómo cambio el token de refresh?
```

**Alucinación:** Respuestas generadas por el modelo que **suenan correctas**, pero **son falsas o inventadas**, sin base real en datos o hechos.

---

## Técnica 5: Multimodal & Tool-augmented

Permite al LLM interactuar con herramientas externas (APIs, funciones) y procesar múltiples formatos de entrada, como imágenes y texto.

**Beneficios**

✔️ Combina texto, imágenes y llamadas a funciones (`function calling`).

✔️ Permite ejecutar código, analizar diagramas o integrar herramientas externas.

✔️ Amplía las capacidades del LLM mucho más allá del texto plano.

**Ejemplo**

```json
{
  "role": "system",
  "content": "Tienes la función runTests()"
}
```

---

## Técnica 6: Incremental Prompting

Consiste en dividir un problema complejo en pasos pequeños, usando prompts en secuencia donde cada resultado alimenta al siguiente.

**Beneficios**

✔️ Mejora la precisión en tareas complejas.

✔️ Reduce errores de contexto y aumenta el control del proceso.

✔️ Ideal para procesar textos largos o generar código paso a paso.

**Ejemplo**

```text
# Prompt 1: Traducir
Traduce al inglés: "La inteligencia artificial está transformando las empresas."
> "Artificial intelligence is transforming businesses."

# Prompt 2: Resumir
Resume el texto anterior en una sola frase.
> "AI is changing business."
```

---

## El Framework CRTR

CRTR es un framework para **estructurar prompts** de forma clara, escalable y reutilizable. Se compone de estos cuatro bloques:

| Bloque          | Contenido                  | Pregunta clave              |
|-----------------|----------------------------|----------------------------|
| **C – Context** | Información relevante      | ¿Qué debe saber la IA?     |
| **R – Role**    | Identidad o perspectiva    | ¿Quién está respondiendo?  |
| **T – Task**    | Acción solicitada          | ¿Qué debe hacer la IA?     |
| **R – Result**  | Formato y limitaciones     | ¿Cómo quiero la salida?    |

---

## Beneficios del Framework CRTR

El framework CRTR ofrece ventajas que lo hacen ideal para equipos técnicos que buscan mantener **consistencia y eficiencia en sus interacciones con LLMs**, optimizando la calidad, performance y reduciendo los costos de uso en grandes proyectos.

✔️ Reduce ambigüedad, mejorando la calidad de las respuestas.  

✔️ Facilita la creación de plantillas reutilizables por todo el equipo.  

✔️ Permite versionar y auditar prompts de forma sencilla.  

✔️ Escala bien en proyectos complejos con múltiples casos de uso.

✔️ Facilita el debugging de prompts: Si una respuesta es incorrecta, puedes aislar el problema. ¿Falló el Contexto? ¿El Rol es ambiguo? ¿La Tarea es imprecisa?

---

## Framework CRTR, ejemplo 1

Este ejemplo muestra cómo estructurar un prompt para redactar documentación técnica clara y precisa asegurando respuestas consistentes y alineadas con el objetivo.

**Metodología:**
 
 ✔️ **Contexto:** Definimos el contexto para situar al modelo
 
 ✔️ **Role:** Definimos el rol para darle la perspectiva adecuada
 
 ✔️ **Task:** Definimos la tarea específica
 
 ✔️ **Result:** Definimos el formato esperado para el resultado
 
**Ejemplo:**

```text
Context: Manual “API-Pagos v2.3”.  
Role: Redactor técnico senior.  
Task: Redacta la sección “Autenticación”.  
Result: Markdown, ≤200 palabras, diagrama ASCII.
```

---

## Framework CRTR, ejemplo 2

Este prompt está diseñado para comunicar resultados de un sprint de forma clara y motivadora, manteniendo al equipo informado y alineado.

**Metodología:**
 
✔️ **Contexto:** Especificamos los hitos clave del sprint finalizado.
 
✔️ **Role:** Asignamos el rol de Product Owner para dar una perspectiva de producto.
 
✔️ **Task:** Solicitamos la redacción de un anuncio interno.
 
✔️ **Result:** Definimos un formato conciso, con un tono motivador y uso de emojis.
 
**Ejemplo:**

```text
Context: Sprint 5 finalizado; hitos: OAuth, 95% tests verdes.  
Role: Product Owner.  
Task: Publicar anuncio interno de 4 párrafos.  
Result: ≤180 palabras, tono motivador, emojis moderados.
```

---

## Framework CRTR, ejemplo 3

Este prompt estructura la descripción de un Pull Request para facilitar la revisión y auditoría, asegurando que la información crítica sea clara y organizada.

**Metodología:**
 
✔️ **Contexto:** Aportamos el contexto técnico de la migración.
 
✔️ **Role:** Asignamos el rol de revisor/a para asegurar una perspectiva de calidad.
 
✔️ **Task:** Solicitamos la redacción de la descripción del PR, incluyendo riesgos y pruebas.
 
✔️ **Result:** Exigimos un formato Markdown estructurado en secciones para facilitar la lectura.
 
**Ejemplo:**

```text
Context: Migración de “user-auth” a TS 5.5 estricto.  
Role: Revisor/a Front-End principal.  
Task: Redactar descripción de Pull Request incluyendo riesgos y pruebas.  
Result: Markdown con secciones: Contexto, Cambios, Pruebas, Checklist QA.
```

---

## Recursos y Herramientas

**Plantilla CRTR**

Utiliza plantilla CRTR para empezar a construir prompts efectivos.

```text
Context: …
Role: …
Task: …
Result: …
```

**Lecturas Recomendadas**

📚 [Prompt Engineering Guide](https://learnprompting.org/docs/introduction): introducción del “Prompt Engineering Guide” de Learn Prompting.
<br>
📚 [DAIR AI – Prompt Engineering Guide](https://dair.ai/projects/prompt-engineering/): guía libre y de código abierto sobre prompt engineering.
<br>
📚 [GPT-4.1 Prompting Guide](https://cookbook.openai.com/examples/gpt4-1_prompting_guide): ofrece técnicas avanzadas para aprovechar al máximo la familia GPT‑4.1.
<br>
📚 [Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview): guía completa y práctica para optimizar el rendimiento de modelos Claude.


---
layout: center
class: "text-center"
---

# ¡Gracias!

### Hablemos de IA y desarrollo

**Germán Aliprandi**

<div class="flex justify-center items-center space-x-4 mt-4">
  <a href="https://linkedin.com/in/galiprandi" target="_blank" class="flex items-center space-x-2">
    <span>🔗 LinkedIn</span>
  </a>
  <a href="https://github.com/galiprandi" target="_blank" class="flex items-center space-x-2">
    <carbon-logo-github />  GitHub
  </a>
</div>

<div class="mt-8">
  <p>Escanea para ver esta presentación online</p>
  <img src="https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=https://galiprandi.github.io/prompt-engineering/" alt="QR Code" class="mx-auto" />
</div>
