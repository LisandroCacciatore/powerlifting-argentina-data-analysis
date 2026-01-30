## Capas de análisis para datos deportivos de Powerlifting

-- Introducción --

Este proyecto no organiza su análisis únicamente por queries o visualizaciones, sino por capas de análisis.

Las capas representan niveles de complejidad conceptual, no técnica.
Cada capa responde a un tipo de pregunta distinto y se apoya en las anteriores.

Este enfoque permite:
- avanzar de forma progresiva,
- evitar conclusiones prematuras,
- separar descripción de interpretación,
- construir análisis reutilizables y comparables

Las capas no reemplazan a las fases del proyecto (raw, limpieza, analítica).
Las fases describen el estado de la data.
Las capas describen cómo se piensa el análisis.

## 🟦 Capa 1 — Perfil Base del País
-- Propósito --
Describir el ecosistema del powerlifting en un país sin emitir juicios de rendimiento.
Es una capa descriptiva, no comparativa.

-- Preguntas que responde --
- ¿Cuántos levantadores hay registrados?
- ¿Cuál es la distribución por sexo?
- ¿En qué tipo de eventos compiten?
- ¿Qué equipamiento es más frecuente?
- ¿Cómo se reparte la participación por categorías?

-- Ejemplos de métricas --
- Total de levantadores
- Total de mujeres / hombres
- Participación por evento (SBD, B, D, etc.)
- Participación por equipamiento
- Distribución por categorías de peso

-- Qué NO hace esta capa --
- No rankea
- No compara países
- No evalúa calidad del rendimiento
- Por qué es importante

Sin esta capa, cualquier análisis posterior pierde contexto.
Permite entender el tamaño, la composición y la diversidad del ecosistema antes de hablar de resultados.

## 🟦 Capa 2 — Distribución del Rendimiento
-- Propósito --
Entender cómo se distribuye el rendimiento dentro del país.
No busca al “mejor”, sino cómo se reparte el rendimiento.

-- Preguntas que responde --
- ¿Cómo se distribuyen los Totales?
- ¿Cuánto concentra el top 10?
- ¿La mayoría del rendimiento está en pocos o muchos atletas?
- ¿Qué pasa por lift (Squat, Bench, Deadlift)?
- ¿Como se componen los totales?
- ¿Como se componen los totales del Top 10 por categoria de Edad?
- ¿Como se componen los totales del Top 10 por categoria de Peso?

-- Ejemplos de análisis -- 
- Percentiles de TotalKg
- Top 10 vs resto (% del total)
- Distribución por lift
- Diferencias entre categorías

-- Valor agregado --
Esta capa permite hablar de:
- concentración,
- desigualdad de rendimiento,
- profundidad competitiva, 

Sin necesidad de comparar con otros países.

## 🟦 Capa 3 — Contexto Deportivo
-- Propósito --
Agregar variables contextuales que expliquen por qué el rendimiento se distribuye como se distribuye.

-- Variables típicas --
- Peso corporal
- Categoría de peso
- Edad / AgeClass
- Equipamiento
- Tipo de evento
- Condición tested / untested

-- Preguntas que responde --
- ¿Qué categorías son mas fuertes a nivel relativo?
- ¿Cómo influye el equipamiento?
- ¿Qué perfiles concentran los mejores totales?
- ¿Dónde aparecen outliers y por qué?

-- Importante --
Esta capa no compara países.
Solo profundiza dentro del mismo ecosistema.

## 🟦 Capa 4 — Comparación Regional (Sudamérica)
-- Propósito --
Comparar países sin caer en rankings simplistas.
El foco no es “quién es mejor”, sino cómo se estructura cada país.

-- Preguntas que responde --
- ¿Cómo se posiciona Argentina en la región?
- ¿Tiene más participación o más elite?
- ¿En qué lifts destaca o queda rezagada?
- ¿Qué países tienen mayor profundidad competitiva?

-- Enfoque --
- Comparaciones relativas
- Contexto antes que absolutos

Lectura estructural, no de podio

## 🟦 Capa 5 — Producto / IA (Futuro)
-- Propósito --
Usar las capas anteriores como base para productos educativos, dashboards avanzados o asistentes analíticos.

-- Ejemplos --
- “¿Dónde estoy parado respecto a mi país?”
- “¿Qué perfil tiene un levantador promedio?”
- “¿Qué patrón de progresión es más común?”
- “¿Qué lift define más el Total en mi categoría?”

-- Condición --
Esta capa solo existe si las anteriores están bien construidas.
No se puede automatizar criterio que no fue definido antes.

## -- Cierre --
Pensar el análisis en capas permite:
- Evitar sobreinterpretación
- Construir conocimiento transferible
- Escalar el proyecto sin rehacerlo
- Conectar datos con decisiones reales de entrenamiento

Este documento funciona como mapa conceptual del proyecto, independiente de la tecnología utilizada.
