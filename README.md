# Aptia
### Encuentra el repuesto que le queda a tu moto

**Autor:** Orlando Bermúdez
**Curso:** Inteligencia Artificial: Generación de Prompts — Comisión #96165
**Entrega:** #3 (final) — IA: Entretejiendo Imaginación y Algoritmos

---

## Índice

1. [Resumen](#1-resumen)
2. [Introducción](#2-introducción)
3. [Objetivos](#3-objetivos)
4. [Metodología](#4-metodología)
5. [Herramientas y tecnologías](#5-herramientas-y-tecnologías)
6. [Implementación](#6-implementación)
7. [Resultados](#7-resultados)
8. [Conclusiones](#8-conclusiones)
9. [Referencias](#9-referencias)
10. [Cómo ejecutar el proyecto](#10-cómo-ejecutar-el-proyecto)

---

## 1. Resumen

Aptia es una propuesta de plataforma móvil/web que resuelve un problema cotidiano de los motociclistas de Valledupar (Cesar, Colombia): la falta de conocimiento técnico confiable sobre qué repuestos son homologables entre marcas y modelos, y qué aceite o llantas conviene usar según el tipo de uso. Este proyecto final demuestra, mediante una Jupyter Notebook, cómo la inteligencia artificial generativa —un modelo de texto-texto (Claude, de Anthropic) y un modelo de texto-imagen (generación manual con herramienta gratuita)— puede resolver este problema con una arquitectura de prompts optimizada en costos, salida estructurada y técnicas de *fast prompting*, sentando las bases funcionales de la aplicación sin necesidad de construir manualmente una base de datos técnica ni un equipo de diseño.

## 2. Introducción

### 2.1 Nombre del proyecto

**Aptia** — plataforma de homologación de repuestos y mantenimiento para motociclistas de Valledupar.

### 2.2 Presentación del problema

En Valledupar la motocicleta es el medio de transporte y motor económico predominante de buena parte de la población: mensajería, mototaxismo, movilidad diaria de trabajadores y estudiantes, y un ecosistema creciente de talleres y almacenes de repuestos. Sin embargo, la mayoría de los propietarios **carece de conocimiento técnico** sobre homologación de repuestos entre modelos/marcas, y sobre qué aceite o llantas usar según su tipo de conducción.

Es una problemática relevante porque:

- **Impacto económico directo:** comprar un repuesto no homologado, o pagar de más existiendo una alternativa homologada, genera un sobrecosto recurrente para familias que dependen de la moto para trabajar.
- **Asimetría de información:** el usuario depende del criterio del vendedor o mecánico, cuyo interés comercial no siempre está alineado con el del cliente.
- **Seguridad vial:** un repuesto mal homologado o un mantenimiento inadecuado puede derivar en fallas mecánicas y riesgos de accidente.
- **Tamaño del mercado:** la motorización acelerada del Cesar hace que el problema afecte a una porción creciente de la población, con potencial de escalar a otras ciudades intermedias de Colombia.

### 2.3 Desarrollo de la propuesta de solución

Aptia usa modelos de lenguaje como motor de conocimiento —en vez de construir manualmente una base de datos técnica— y un modelo de texto-imagen para la identidad visual y las piezas de marketing. La solución se resuelve en dos etapas:

- **Etapa 1 (texto-texto, implementada en la notebook):** tres funciones con una única consulta a la API cada una — ficha de homologación de repuestos, recomendación de aceite/llantas, y chatbot de consultas frecuentes.
- **Etapa 2 (texto-imagen, generación manual):** logo de marca, mockup de la app y pieza publicitaria, generados con una herramienta gratuita (ver [sección 6.2](#62-etapa-2--identidad-visual-texto-imagen)), ya que DALL·E dejó de ser gratuita y este proyecto no requiere integrarla por API.

### 2.4 Justificación de la viabilidad del proyecto

El proyecto es técnicamente viable porque no requiere infraestructura propia de IA (se apoya en la API de Anthropic, de bajo costo por consulta), el contenido se genera bajo demanda y se cachea, el alcance inicial está acotado a las marcas/categorías más comunes en Valledupar con una arquitectura de prompts parametrizada que permite escalar sin rediseño, y la identidad visual se resolvió sin costo mediante una herramienta gratuita de generación de imágenes.

## 3. Objetivos

- Identificar una problemática real y plantear una solución viable usando generación de prompts, tanto en un modelo de texto-texto como en uno de texto-imagen.
- Diseñar una solución factible de implementar con los recursos y el tiempo disponibles del curso.
- Optimizar el uso de prompts: minimizar la cantidad de consultas a la API y maximizar la consistencia y utilidad de cada respuesta mediante técnicas de *fast prompting*.
- Demostrar, en una Jupyter Notebook funcional, la implementación real de la solución propuesta.
- Evaluar si la solución final —después de aplicar las mejoras de la Entrega 2— efectivamente resuelve el problema planteado.

## 4. Metodología

El proyecto se desarrolló de forma incremental a lo largo de las tres entregas del curso:

1. **Entrega 1:** definición del problema, la propuesta de solución y el diseño inicial de los prompts (en texto libre, sin estructura de datos).
2. **Entrega 2:** implementación de los prompts en una notebook funcional, aplicando técnicas de *fast prompting* (role prompting, salida estructurada en JSON, few-shot, parametrización, *guardrails*, control de temperatura) y una capa de optimización de costos (caché en memoria, una sola consulta por función).
3. **Entrega 3 (esta):** se completa la propuesta con la generación real de las piezas visuales (logo, mockup, pieza publicitaria) usando una herramienta gratuita de texto-imagen, y se documentan los resultados obtenidos al ejecutar la notebook, evaluando si la solución cumple lo propuesto.

En cada etapa se fraccionó el problema en partes simples (una función por capacidad de la app), se probó cada prompt de forma aislada antes de integrarlo, y se documentaron las decisiones de diseño y sus justificaciones directamente en el código y en este README.

## 5. Herramientas y tecnologías

| Herramienta | Uso en el proyecto |
|---|---|
| **Python 3 + Jupyter Notebook** | Entorno de desarrollo y demostración de la POC. |
| **Anthropic API (Claude)** | Modelo de texto-texto usado para resolver las tres capacidades de Aptia. |
| **NightCafe** (gratuita) | Modelo de texto-imagen, usado de forma manual (sin integración por API) para generar el logo, el mockup y la pieza publicitaria. |
| **pandas** | Estructurar y visualizar en tablas las respuestas JSON del modelo. |
| **python-dotenv** | Manejo seguro de la API key. |

### Técnicas de *fast prompting* utilizadas (texto-texto)

- **Role prompting:** cada función define explícitamente el rol del modelo (mecánico experto, asesor técnico, asistente virtual).
- **Salida estructurada (JSON):** se exige al modelo responder en un esquema JSON fijo, parseable sin expresiones regulares frágiles.
- **Few-shot prompting:** el chatbot incluye ejemplos de pregunta-respuesta para fijar tono y formato.
- **Parametrización:** las listas de marcas y categorías de repuesto son variables, no texto fijo, para poder escalar sin rediseñar el prompt.
- **Guardrails explícitos:** el modelo debe declarar su nivel de confianza en vez de inventar una homologación cuando no tiene certeza.
- **Control de temperatura:** baja (0.2) en las funciones técnicas, más alta (0.6) en el chatbot.

Estas técnicas se justifican porque el dominio (recomendaciones técnicas que un usuario sin conocimiento de mecánica va a seguir) exige priorizar confiabilidad y parseabilidad por sobre creatividad.

### Sobre la generación de imágenes

Siguiendo la recomendación de la consigna, no se integró DALL·E por API (dejó de ser gratuita). En su lugar, los tres prompts de texto-imagen se escribieron para usarse **directamente en una herramienta gratuita** (NightCafe), y el resultado se pegó como imagen en este repositorio — ver [sección 6.2](#62-etapa-2--identidad-visual-texto-imagen).

## 6. Implementación

### 6.1 Etapa 1 — Texto-texto (notebook)

El código completo, comentado e indentado, está en:

📓 [`Aptia_FastPrompting_POC.ipynb`](./Aptia_FastPrompting_POC.ipynb)

Contiene: configuración segura de la API key, una función central con caché para minimizar llamadas, y las tres funciones de Aptia (homologación, mantenimiento, chatbot), cada una con una única consulta a la API por invocación.

### 6.2 Etapa 2 — Identidad visual (texto-imagen)

Los tres prompts usados, junto con la imagen generada para cada uno, están documentados en:

🎨 [`prompts_imagenes.md`](./prompts_imagenes.md)

Las tres imágenes generadas con NightCafe están en la carpeta [`/imagenes`](./imagenes) y embebidas en ese mismo archivo.

## 7. Resultados

*(A continuación, un resultado de referencia obtenido en una prueba manual del Prompt 1 durante el desarrollo del proyecto, que ilustra el formato de salida esperado de la notebook.)*

Al ejecutar `ficha_homologacion("AKT NKD 125", marcas_colombia, categorias_base)`, el modelo devolvió una tabla estructurada (JSON → `DataFrame`) con 5 categorías de repuesto, cada una con su nivel de confianza declarado explícitamente (alta/media/baja), en una única consulta a la API. Esto confirma que:

- La solución **sí llega al resultado esperado**: una respuesta estructurada, lista para integrarse a una futura app, sin texto libre que requiera post-procesamiento manual.
- El *guardrail* de confianza funciona como se diseñó: el modelo distingue entre piezas altamente estandarizadas (ej. bujías) y piezas donde la homologación depende de medidas específicas (ej. kit de arrastre), en vez de responder con el mismo nivel de certeza en todos los casos.
- El costo por consulta se mantiene predecible: 1 llamada = 1 resultado completo, sin llamadas encadenadas.

## 8. Conclusiones

- El proyecto logra los objetivos propuestos: se identificó un problema real y local, se diseñó una solución basada en generación de prompts (texto-texto y texto-imagen), y se demostró su viabilidad técnica en una notebook funcional.
- Las técnicas de *fast prompting* aplicadas en la Entrega 2 (salida estructurada, *guardrails*, parametrización) resultaron ser la mejora más significativa respecto a la Entrega 1: pasar de texto libre a JSON estructurado es lo que hace que Aptia sea, en principio, integrable a una app real y no solo una demostración conceptual.
- La decisión de no integrar DALL·E por API y usar en su lugar una herramienta gratuita de forma manual no afecta la viabilidad del proyecto: la identidad visual es un insumo que se genera una vez y se reutiliza, a diferencia de las consultas de texto que ocurren por cada usuario.
- Como trabajo futuro, el propio diseño parametrizado del Prompt 1 permite escalar a más marcas colombianas (TVS, Hero, Kawasaki, KTM) y más categorías de repuesto (válvulas, cadenilla, kit de empaques, balineras) sin rediseñar la lógica, tal como se documentó en la Preentrega 1.

## 9. Referencias

- Documentación oficial de la API de Anthropic (Claude): https://docs.claude.com
- NightCafe Studio (generación de imágenes gratuita): https://creator.nightcafe.studio
- Repositorio de recursos recomendados del curso (Coderhouse).

## 10. Cómo ejecutar el proyecto

```bash
git clone https://github.com/Nandhobj/Entrega-Final-Inteligencia-artificial-Generaci-n-de-Prompts.git
cd Entrega-Final-Inteligencia-artificial-Generaci-n-de-Prompts
pip install -r requirements.txt
jupyter notebook Aptia_FastPrompting_POC.ipynb
```

Se debe contar con una API key de Anthropic (obtenida en [console.anthropic.com](https://console.anthropic.com)), que la notebook solicitará de forma segura (no se guarda en el código ni en el repositorio).
