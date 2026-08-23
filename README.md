# Aptia
### Encuentra el repuesto que le queda a tu moto

**Autor:** Orlando Bermúdez
**Curso:** Inteligencia Artificial: Generación de Prompts — Comisión #96165
**Entrega:** #2 — Fast Prompting en Acción: Desentrañando la Magia

---

## Índice

1. [Introducción](#1-introducción)
2. [Objetivos](#2-objetivos)
3. [Metodología](#3-metodología)
4. [Herramientas y tecnologías](#4-herramientas-y-tecnologías)
5. [Implementación](#5-implementación)
6. [Análisis de costos](#6-análisis-de-costos)
7. [Cómo ejecutar la notebook](#7-cómo-ejecutar-la-notebook)

---

## 1. Introducción

### 1.1 Nombre del proyecto

**Aptia** — plataforma móvil/web que ayuda a los propietarios de motocicletas en Valledupar (Cesar, Colombia) a resolver, con inteligencia artificial, las dudas técnicas que hoy resuelven a ciegas o dependiendo exclusivamente del criterio comercial de un tercero.

### 1.2 Presentación del problema

En Valledupar la motocicleta se ha convertido en el medio de transporte y el motor económico predominante de buena parte de la población: mensajería, mototaxismo, movilidad diaria de trabajadores y estudiantes, y un ecosistema creciente de talleres y almacenes de repuestos. Sin embargo, la mayoría de los propietarios de motocicletas **carece de conocimiento técnico** sobre un aspecto crítico: la **homologación de repuestos** entre modelos y marcas, así como el tipo de aceite o de llanta más adecuado según su uso real.

Esto es relevante porque:

- **Impacto económico directo:** comprar un repuesto no homologado, o pagar de más por una pieza original existiendo una alternativa homologada, genera un sobrecosto recurrente para familias que dependen de la moto para trabajar.
- **Asimetría de información:** el usuario depende del criterio del vendedor o del mecánico, cuyo interés comercial no siempre está alineado con el del cliente.
- **Seguridad vial y mecánica:** un repuesto mal homologado o un mantenimiento inadecuado puede derivar en fallas mecánicas y riesgos de accidente.
- **Tamaño del mercado:** la motorización acelerada del Cesar hace que el problema afecte a una porción creciente de la población, con potencial de escalar a otras ciudades intermedias de Colombia.

### 1.3 Desarrollo de la propuesta de solución

Aptia usa modelos de lenguaje (IA generativa de texto-texto) como motor de conocimiento: en lugar de construir manualmente una base de datos técnica —tarea costosa y lenta—, la aplicación genera y estructura ese conocimiento mediante prompts diseñados con técnicas de *fast prompting*, y modelos de texto-imagen para la identidad visual y las piezas de marketing.

La solución se organiza en tres capacidades principales, cada una resuelta con **una única consulta a la API** por interacción de usuario (ver [sección 6](#6-análisis-de-costos)):

1. **Ficha de homologación de repuestos**, parametrizada por marca, modelo y categoría de repuesto.
2. **Recomendación de aceite y llantas**, personalizada según uso y kilometraje.
3. **Chatbot de consultas frecuentes**, con tono cercano y siempre orientado a no reemplazar a un mecánico certificado.

### 1.4 Justificación de la viabilidad del proyecto

El proyecto es técnicamente viable porque:

- No requiere infraestructura propia de IA: se apoya en la API de Anthropic (Claude), de acceso inmediato y de bajo costo por consulta.
- El contenido técnico (fichas, recomendaciones) se genera bajo demanda y se puede cachear, evitando construir y mantener manualmente una base de datos.
- El alcance inicial (MVP) se acota deliberadamente a las marcas y categorías de repuesto más comunes en Valledupar, con una arquitectura de prompts parametrizada que permite escalar a más marcas y categorías (válvulas, cadenilla, kit de empaques, balineras, etc.) sin rediseñar la lógica.
- El modelo de negocio freemium con publicidad, con una futura capa de pauta paga para marcas y talleres, es sostenible desde el inicio porque el costo marginal por consulta a la API es bajo y decreciente gracias al cacheo de respuestas frecuentes.

---

## 2. Objetivos

- Demostrar la comprensión de los principios y técnicas detrás de *fast prompting* (prompting por rol, ejemplos *few-shot*, salida estructurada, parametrización y *guardrails* contra alucinaciones), aplicados a un caso real.
- Experimentar con distintas configuraciones de prompts para optimizar la eficacia y la consistencia de las respuestas del modelo.
- Preparar una demostración funcional y reproducible en un Jupyter Notebook, con celdas ordenadas, indentadas y comentadas.
- Analizar si las técnicas de *fast prompting* aprendidas mejoran la propuesta planteada en la Preentrega 1, y documentar esas mejoras.
- Optimizar el número de consultas a la API, evaluando el costo de cada función y evitando llamadas redundantes o innecesarias.

---

## 3. Metodología

El proyecto se desarrolló de forma incremental, fraccionando el problema en partes simples de programar y validar por separado:

1. **Revisión de la Preentrega 1:** se relevaron los tres prompts de texto-texto ya diseñados (homologación, aceite/llantas, chatbot) y se identificaron oportunidades de mejora a la luz de las técnicas de *fast prompting* vistas en el curso (por ejemplo, pasar de una respuesta en texto libre a una respuesta en **JSON estructurado**, más fácil de integrar a una futura app real).
2. **Rediseño de cada prompt** aplicando, según el caso: *role prompting* (definir explícitamente el rol del modelo), *few-shot prompting* (ejemplos de formato esperado), *salida estructurada* (JSON con esquema fijo) y *guardrails* explícitos (instrucción de declarar incertidumbre en vez de inventar datos).
3. **Implementación en Python**, encapsulando cada prompt en una función independiente y testeable, con una única llamada a la API por función.
4. **Capa de optimización de costos:** se agregó una caché en memoria que evita repetir una consulta idéntica a la API, y se diseñó cada función para resolver su tarea en **una sola consulta**, evitando cadenas de llamadas innecesarias.
5. **Prueba y demostración en la notebook**, incluyendo una celda interactiva que permite ingresar parámetros sin modificar el código (a través de `input()`), y una celda de análisis de costos que cuenta las consultas realizadas durante la sesión.
6. **Reflexión final** sobre qué cambió respecto a la Preentrega 1 gracias a estas técnicas.

---

## 4. Herramientas y tecnologías

| Herramienta | Uso en el proyecto |
|---|---|
| **Python 3 + Jupyter Notebook** | Entorno de desarrollo y demostración de la POC. |
| **Anthropic API (Claude)** | Modelo de lenguaje usado para resolver las tres capacidades de texto-texto de Aptia. |
| **pandas** | Estructurar y visualizar en tablas las respuestas JSON del modelo (ej. ficha de homologación). |
| **python-dotenv** | Manejo seguro de la API key, sin exponerla en el código. |

### Técnicas de *fast prompting* utilizadas

- **Role prompting (prompt de sistema):** cada función define explícitamente el rol del modelo (mecánico experto, asesor técnico, asistente virtual), lo que reduce respuestas genéricas o fuera de contexto.
- **Salida estructurada (structured output / JSON):** en lugar de pedir texto libre, se le exige al modelo responder únicamente en JSON con un esquema fijo, lo que permite parsear la respuesta programáticamente sin depender de expresiones regulares frágiles.
- **Few-shot prompting:** el prompt del chatbot incluye 1-2 ejemplos de pregunta-respuesta para fijar el tono y el largo esperado de la respuesta.
- **Parametrización de prompts:** las listas de marcas y categorías de repuesto son variables, no texto fijo, lo que permite escalar el proyecto (ver Preentrega 1, sección 2.5) sin rediseñar el prompt.
- **Guardrails explícitos:** se instruye al modelo a declarar `"confianza": "baja"` o un campo `"advertencia"` en vez de inventar una homologación cuando no tiene certeza, reduciendo el riesgo de alucinaciones en un dominio donde un dato erróneo tiene consecuencias reales (seguridad vial).
- **Control de temperatura:** se usa una temperatura baja (0.2–0.3) en las funciones técnicas (homologación, mantenimiento) para priorizar consistencia sobre creatividad, y una temperatura algo mayor (0.6) en el chatbot para un tono más natural.

Estas técnicas se justifican porque el dominio del proyecto (recomendaciones técnicas que un usuario sin conocimiento de mecánica va a seguir) exige **priorizar la confiabilidad y la parseabilidad de la respuesta por sobre la creatividad del modelo**.

---

## 5. Implementación

El desarrollo completo, con código comentado e indentado, está en:

📓 [`Aptia_FastPrompting_POC.ipynb`](./Aptia_FastPrompting_POC.ipynb)

La notebook está organizada en las siguientes secciones:

1. Instalación e importación de librerías.
2. Configuración segura de la API key.
3. Función central `consultar_claude()` con caché en memoria (evita llamadas repetidas idénticas).
4. Función 1 — Ficha de homologación de repuestos (salida JSON → tabla `pandas`).
5. Función 2 — Recomendación de aceite y llantas (salida JSON).
6. Función 3 — Chatbot de consultas frecuentes (few-shot).
7. Celda interactiva (`input()`) para probar el sistema sin tocar el código.
8. Análisis de costos: conteo de llamadas realizadas durante la sesión.
9. Conclusión: mejoras respecto a la Preentrega 1.

---

## 6. Análisis de costos

Cada una de las tres funciones principales realiza **una única consulta a la API por invocación** (0 llamadas encadenadas, 0 llamadas de "verificación" adicionales). Además:

- Se implementó una **caché en memoria** (`diccionario` con clave = función + parámetros) que evita volver a consultar la API si el usuario repite exactamente la misma consulta en la misma sesión.
- El `max_tokens` de cada función se limitó al mínimo necesario según el tipo de respuesta esperada (ficha estructurada vs. respuesta corta de chatbot), para evitar pagar por tokens de salida innecesarios.
- La notebook incluye un contador global de llamadas (`contador_llamadas_api`) que se muestra al final de la sesión, permitiendo verificar en todo momento cuántas consultas reales se hicieron.

Este diseño responde directamente a la recomendación de la consigna: *"si la app funciona pero hace consultas innecesarias, el proyecto no es rentable"*.

---

## 7. Cómo ejecutar la notebook

```bash
git clone https://github.com/Nandhobj/Entrega-Final-Inteligencia-artificial-Generaci-n-de-Prompts.git
cd Entrega-Final-Inteligencia-artificial-Generaci-n-de-Prompts
pip install -r requirements.txt
jupyter notebook Aptia_FastPrompting_POC.ipynb
```

Se debe contar con una API key de Anthropic (obtenida en [console.anthropic.com](https://console.anthropic.com)), que la notebook solicitará de forma segura (no se guarda en el código ni en el repositorio).
