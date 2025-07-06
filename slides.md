---
title: "Prompt Engineering – Guía práctica para equipos"
author: "Germán Aliprandi - galiprandi@gmail.com"
date: "3 Jul 2024"
theme: geist
class: text-center
transition: slide-left
mdc: true
---

# Prompt Engineering – Guía práctica para equipos

---

# Agenda

1. ¿Por qué importa el prompt?  
2. Cómo “piensa” un LLM  
3. Buen Prompt vs Mal Prompt  
4–8. Técnicas de prompting  
9–13. Framework **CRTR** y ejemplos  
14–15. Recursos  

---

# ¿Por qué hablar de prompts?

- ~80 % de la calidad depende del prompt.  
- Menos iteraciones ⇒ menor coste y tiempo.  
- Mitiga alucinaciones y sesgos.  
- Habilidad transversal: Devs, PMs, UX…

:::info
Visual sugerido: gráfico “calidad vs iteraciones”.
:::

---

# Cómo “piensa” un LLM

1. Ventana de contexto  
2. Embeddings → vectores semánticos  
3. Capas de auto-atención  
4. Distribución de probabilidad → token  
5. Bucle hasta fin  

---

## ¿Cómo modifica el prompt esta cadena de inferencia?

Cada token del prompt entra en la ventana de contexto y, tras convertirse en vectores, actúa como *clave* y *valor* en la atención.  

Al ajustar esos tokens (definir rol, dar ejemplos, añadir datos externos) desplazamos el foco de atención y, con ello, la probabilidad de cada token siguiente.  

Un solo token inicial puede “afilar” o “aplanar” la distribución y cambiar todo el resultado sin tocar los pesos del modelo.

---

# Buen Prompt vs Mal Prompt

| Prompt flojo | Prompt afinado |
|--------------|----------------|
| “Resume este texto.” | “Eres un editor técnico. Resume en ≤150 palabras, sin jerga, en español neutro, resaltando argumentos clave y una conclusión.” |

---

# Técnica 1 – Zero-shot / Few-shot

- **Zero-shot**: confía en conocimiento general.  
- **Few-shot**: añade 1-5 demostraciones.

**Ejemplo Few-shot**

```text
Eres Chat Bot de soporte.

Ejemplo →
Usuario: ¿Diferencia entre `interface` y `type` en TypeScript?
Bot: Tanto `interface` como `type` definen formas, pero…

Nueva pregunta →
Usuario: ¿Para qué sirve `Partial<T>`?
Bot:
````

---

# Técnica 2 – Chain-of-Thought (CoT)

* “Piensa paso a paso” fuerza al modelo a exponer la lógica.
* Variantes: *Self-Consistency*, *Tree of Thoughts*.

**Ejemplo**

```text
Pregunta: ¿Cuál es la complejidad de `quickSort` promedio?
Primero razona paso a paso, luego responde en una línea.
```

---

# Técnica 3 – Role / Persona

* Define “quién habla” (mentor, abogado, tester…).
* Mejora coherencia y estilo.

**Ejemplo**

```text
Eres mentor senior de TypeScript.
Explica a un junior por qué preferir `unknown` sobre `any`.
```

---

# Técnica 4 – Retrieval-Augmented Generation (RAG)

* Inyecta fragmentos de documentos → info actual y verificable.
* Ideal para FAQ internas o bases de conocimiento.

**Ejemplo**

```text
Contexto:
• docs/api-auth.md (línea 10-30)
• README.md (línea 55-60)

Pregunta: ¿Cómo cambio el token de refresh?
```

---

# Técnica 5 – Multimodal & Tool-augmented

* Combina texto + imágenes + llamadas a funciones (`function calling`).
* Casos: ejecución de código, análisis de diagramas.

**Ejemplo**

```json
{
  "role": "system",
  "content": "Tienes la función runTests()"
}
```

Usuario: “Ejecuta las pruebas unitarias y dime los fallos”.

---

# Framework CRTR – Definición

| Bloque          | ¿Qué contiene?          | Pregunta guía                |
| --------------- | ----------------------- | ---------------------------- |
| **C – Context** | Información de fondo    | ¿Qué debe saber la IA antes? |
| **R – Role**    | Identidad o perspectiva | ¿Quién responde?             |
| **T – Task**    | Acción solicitada       | ¿Qué debe hacer?             |
| **R – Result**  | Formato y límite        | ¿Cómo quiero la salida?      |

---

## Beneficios

* Menos ambigüedad → más calidad.
* Plantillas reutilizables por el equipo.
* Facilita auditoría de prompts.

---

# CRTR – Ejemplo 1

*(documentación de servicio interno)*

```text
Context: Manual “API-Pagos v2.3”.
Role: Redactor técnico senior.
Task: Redacta la sección “Autenticación”.
Result: Markdown, ≤200 palabras, diagrama ASCII.
```

---

# CRTR – Ejemplo 2

*(post Sprint 5)*

```text
Context: Sprint 5 finalizado; hitos: OAuth, 95 % tests verdes.
Role: Product Owner.
Task: Post de 4 párrafos en #anuncios.
Result: ≤180 palabras, tono motivador, emojis moderados.
```

---

# CRTR – Ejemplo 3

*(PR migración TypeScript estricto)*

```text
Context: Migración de “user-auth” a TS 5.5 estricto.
Role: Revisor/a principal Front-End.
Task: Descripción de Pull Request con riesgos y pruebas.
Result: Markdown con secciones: Contexto, Cambios, Pruebas, Checklist QA.
```

---

# CRTR – Recursos

* Plantilla rápida:

```text
Context: …
Role: …
Task: …
Result: …
```

* LearnPrompting – **Prompt Structure & Key Parts**
* DAIR AI – **Prompt Engineering Guide**
* OpenAI Cookbook – **Prompt Best Practices**
* Curso corto — “ChatGPT Prompt Engineering for Developers” (deeplearning.ai)

---

