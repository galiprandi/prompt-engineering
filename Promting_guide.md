---
title: "Prompt Engineering – Guía práctica para equipos"
author: "Tu Nombre"
date: "5 Jul 2025"
---

# Slide 1 · Título y datos básicos
**Prompt Engineering – Guía práctica para equipos**  
_Germán Aliprandi - galiprandi@gmail.com_

---

# Slide 2 · Agenda
1. ¿Por qué importa el prompt?  
2. Cómo “piensa” un LLM  
3. Buen Prompt vs Mal Prompt  
4–8. Técnicas de prompting  
9–13. Framework **CRTR** y ejemplos  
14–15. Recursos  

---

# Slide 3 · ¿Por qué hablar de prompts?
- ~80 % de la calidad depende del prompt.  
- Menos iteraciones ⇒ menor coste y tiempo.  
- Mitiga alucinaciones y sesgos.  
- Habilidad transversal: Devs, PMs, UX…  

*Visual sugerido*: gráfico “calidad vs iteraciones”.

---

# Slide 4 · Cómo “piensa” un LLM
1. Ventana de contexto  
2. Embeddings → vectores semánticos  
3. Capas de auto-atención  
4. Distribución de probabilidad → token  
5. Bucle hasta fin  

**¿Cómo modifica el prompt esta cadena de inferencia?**  
Cada token del prompt entra en la ventana de contexto y, tras convertirse en vectores, actúa como *clave* y *valor* en la atención. Al ajustar esos tokens (definir rol, dar ejemplos, añadir datos externos) desplazamos el foco de atención y, con ello, la probabilidad de cada token siguiente. Un solo token inicial puede “afilar” o “aplanar” la distribución y cambiar todo el resultado sin tocar los pesos del modelo.

---

# Slide 5 · Buen Prompt vs Mal Prompt
| Prompt flojo | Prompt afinado |
|--------------|----------------|
| “Resume este texto.” | “Eres un editor técnico. Resume en ≤150 palabras, sin jerga, en español neutro, resaltando argumentos clave y una conclusión.” |

---

# Slide 6 · Técnica 1 – Zero-shot / Few-shot
- **Zero-shot**: confía en conocimiento general.  
- **Few-shot**: añade 1-5 demostraciones.

**Ejemplo Few-shot**
```
Eres Chat Bot de soporte.

Ejemplo →
Usuario: ¿Diferencia entre `interface` y `type` en TypeScript?
Bot: Tanto `interface` como `type` definen formas, pero…

Nueva pregunta →
Usuario: ¿Para qué sirve `Partial<T>`?
Bot:

```

---

# Slide 7 · Técnica 2 – Chain-of-Thought (CoT)
- “Piensa paso a paso” fuerza al modelo a exponer la lógica.  
- Variantes: *Self-Consistency*, *Tree of Thoughts*.

**Ejemplo**
```

Pregunta: ¿Cuál es la complejidad de `quickSort` promedio?
Primero razona paso a paso, luego responde en una línea.

```

---

# Slide 8 · Técnica 3 – Role / Persona
- Define “quién habla” (mentor, abogado, tester…).  
- Mejora coherencia y estilo.

**Ejemplo**
```

Eres mentor senior de TypeScript.
Explica a un junior por qué preferir `unknown` sobre `any`.

```

---

# Slide 9 · Técnica 4 – Retrieval-Augmented Generation (RAG)
- Inyecta fragmentos de documentos → info actual y verificable.  
- Ideal para FAQ internas o bases de conocimiento.

**Ejemplo**
```

Contexto:
• docs/api-auth.md (línea 10-30)
• README.md (línea 55-60)

Pregunta: ¿Cómo cambio el token de refresh?

````

---

# Slide 10 · Técnica 5 – Multimodal & Tool-augmented
- Combina texto + imágenes + llamadas a funciones (`function calling`).  
- Casos: ejecución de código, análisis de diagramas.

**Ejemplo**
```json
{
  "role": "system",
  "content": "Tienes la función runTests()"
}
Usuario: “Ejecuta las pruebas unitarias y dime los fallos”.
````

---

# Slide 11 · Framework CRTR – Definición

| Bloque          | ¿Qué contiene?          | Pregunta guía                |
| --------------- | ----------------------- | ---------------------------- |
| **C – Context** | Información de fondo    | ¿Qué debe saber la IA antes? |
| **R – Role**    | Identidad o perspectiva | ¿Quién responde?             |
| **T – Task**    | Acción solicitada       | ¿Qué debe hacer?             |
| **R – Result**  | Formato y límite        | ¿Cómo quiero la salida?      |

**Beneficios**

* Menos ambigüedad → más calidad.
* Plantillas reutilizables por el equipo.
* Facilita auditoría de prompts.

---

# Slide 12 · CRTR – Ejemplo 1 (documentación de servicio interno)

```
Context: Manual “API-Pagos v2.3”.
Role: Redactor técnico senior.
Task: Redacta la sección “Autenticación”.
Result: Markdown, ≤200 palabras, diagrama ASCII.
```

---

# Slide 13 · CRTR – Ejemplo 2 (post Sprint 5)

```
Context: Sprint 5 finalizado; hitos: OAuth, 95 % tests verdes.
Role: Product Owner.
Task: Post de 4 párrafos en #anuncios.
Result: ≤180 palabras, tono motivador, emojis moderados.
```

---

# Slide 14 · CRTR – Ejemplo 3 (PR migración TypeScript estricto)

```
Context: Migración de “user-auth” a TS 5.5 estricto.
Role: Revisor/a principal Front-End.
Task: Descripción de Pull Request con riesgos y pruebas.
Result: Markdown con secciones: Contexto, Cambios, Pruebas, Checklist QA.
```

---

# Slide 15 · CRTR – Recursos

* Plantilla rápida:

```
Context: …  
Role: …  
Task: …  
Result: …  
```

* LearnPrompting – **Prompt Structure & Key Parts**
* DAIR AI – **Prompt Engineering Guide**
* OpenAI Cookbook – **Prompt Best Practices**
* Curso corto — “ChatGPT Prompt Engineering for Developers” (deeplearning.ai)

