# Capa 01 — Column Selection Criteria

## Proyecto: Powerlifting Argentina Data Analysis

---

## 1. Propósito del documento

Este documento define los **criterios conceptuales de selección de columnas** que conforman la **Capa 01 — Análisis Descriptivo Base** del proyecto *Powerlifting Argentina Data Analysis*.

Su objetivo es establecer, **antes de cualquier implementación técnica**, qué atributos del dataset de **OpenPowerlifting** son considerados:

* relevantes desde un punto de vista descriptivo,
* semánticamente claros,
* comunicables a deportistas y entrenadores,
* y coherentes para un primer análisis exploratorio.

Del mismo modo, explicita qué columnas **quedan fuera de alcance** o se **postergan deliberadamente** para capas posteriores.

Este documento funciona como **gobierno semántico del dato**:

* toda query,
* toda vista,
* todo dashboard

dependen explícitamente de estas decisiones.

### Qué decisiones habilita

* Qué columnas **existen** en la Capa 01.
* Qué columnas **no existen**, aun estando disponibles en el dataset original.
* Qué columnas se **postergan conscientemente** para capas posteriores.

### Qué decisiones NO toma

Este documento **no define**:

* métricas,
* joins,
* queries,
* visualizaciones,
* lógica técnica de implementación.

Estas decisiones se abordan en documentos posteriores.

> **Nota de alcance**
> El usuario final de esta capa son deportistas y entrenadores afines al powerlifting. La prioridad está puesta en comprensión, trazabilidad y coherencia conceptual por sobre precisión extrema o sofisticación estadística.

---

## 2. Dataset de origen

El proyecto utiliza como fuente el dataset público de **OpenPowerlifting**, distribuido en formato CSV.

Este dataset consolida resultados históricos de competencias de powerlifting reportadas por múltiples federaciones, países y períodos temporales.

### Consideraciones generales sobre la fuente

* Nivel de confianza asumido: **medio–alto**.
* Rango temporal: **varias décadas**, con fuerte heterogeneidad histórica.
* Dataset **no homogéneo**, con:

  * campos obligatorios y opcionales,
  * reporting desigual entre federaciones,
  * cambios de criterios a lo largo del tiempo.

Estas características hacen necesario un **recorte conceptual consciente** para evitar interpretaciones incorrectas en análisis descriptivos tempranos.

### Recortes y sesgos explícitos

* El análisis se restringe inicialmente a registros con `Country = "Argentina"`.
* Se asume conocimiento de que:

  * existen federaciones que ya no están activas,
  * algunas federaciones reportan intentos nulos como valores negativos,
  * otras los reportan como valores `NULL`.

Estas inconsistencias **no se corrigen en esta capa**, pero se documentan para interpretación responsable.

### Alcance temporal

* Se trabaja con **todo el histórico disponible** para Argentina.
* No se aplica recorte temporal implícito en la Capa 01.

---

## 3. Principios de selección de columnas

La selección de columnas para la Capa 01 se rige por los siguientes principios:

1. **Relevancia descriptiva** para entender el ecosistema competitivo.
2. **Claridad semántica** sin necesidad de contexto técnico adicional.
3. **Bajo riesgo de mala interpretación** por parte de público no técnico.
4. **Estabilidad** entre eventos, federaciones y períodos.
5. **Valor comunicable** a deportistas y entrenadores.
6. **Compatibilidad con agregaciones simples**.
7. **Bajo costo computacional**, priorizando queries simples y eficientes en entornos cloud.
8. **Prioridad de comprensión sobre precisión extrema**, buscando un primer panorama general de la situación.

Las columnas que no cumplen estos principios quedan **excluidas o postergadas**, independientemente de su riqueza potencial.

---

## 4. Clasificación de columnas

Las columnas del dataset se clasifican según su **rol conceptual** dentro del análisis.

### 4.1 Identificación y contexto

| Columna    | Estado   | Justificación                                                                                                                        |
| ---------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Name       | Incluida | Utilizada únicamente como **etiqueta** del participante. No se considera identificador único ni se usa para análisis longitudinales. |
| Sex        | Incluida | Permite análisis descriptivos de composición sin ambigüedad semántica.                                                               |
| Country    | Incluida | Aporta contexto geográfico del atleta. Inicialmente se toma el valor original; la normalización se realiza en etapas posteriores.    |
| Federation | Incluida | Describe el marco institucional del evento, relevante para el contexto histórico.                                                    |

Notas:

* `Name` **no se usa como identificador** analítico.
* La normalización de país (ej. `Country → País = Argentina`) se aborda fuera de esta capa.

---

### 4.2 Evento y competencia

| Columna     | Estado   | Justificación                                                           |
| ----------- | -------- | ----------------------------------------------------------------------- |
| Event       | Incluida | Define el tipo de competencia sin requerir interpretación adicional.    |
| Equipment   | Incluida | Describe categoría competitiva de forma estandarizada.                  |
| Sanctioned  | Incluida | Permite diferenciar eventos oficiales de no oficiales.                  |
| MeetCountry | Incluida | Aporta contexto geográfico del evento; se utiliza de forma descriptiva. |
| Date        | Incluida | Habilita análisis temporal básico sin ambigüedad.                       |

Derivaciones permitidas en esta capa:

* Año de competencia.
* Mes de competencia.

Estas derivaciones se consideran **atributos temporales básicos**, no métricas.

---

### 4.3 Categorías y composición

| Columna       | Estado   | Justificación                                            |
| ------------- | -------- | -------------------------------------------------------- |
| WeightClassKg | Incluida | Permite segmentación básica por categoría de peso.       |
| BodyweightKg  | Incluida | Aporta información descriptiva de composición corporal.  |
| Age           | Incluida | Variable descriptiva simple, aun con valores decimales.  |
| AgeClass      | Incluida | Complementa el análisis cuando `Age` no está disponible. |

Criterios explícitos:

* No se prioriza `Age` sobre `AgeClass`.
* Se aceptan edades decimales (ej. 23.5) sin corrección.
* Los valores de `Age` se utilizan para promedios dentro de la categoría definida por `AgeClass`.

---

### 4.4 Resultados agregables

| Columna | Estado   | Justificación                                                    |
| ------- | -------- | ---------------------------------------------------------------- |
| TotalKg | Incluida | Métrica central agregable para análisis descriptivo.             |
| Place   | Incluida | Aporta contexto competitivo sin requerir normalización avanzada. |

Criterios explícitos:

* Valores especiales de `Place` (DQ, NS, etc.) **no se excluyen** en esta capa.
* Registros con `TotalKg` vacío:

  * se conservan,
  * se documenta su existencia,
  * y, cuando sea posible, se intentan reconstruir a partir de datos del meet.

---

## 5. Columnas fuera de alcance en la Capa 01

Quedan explícitamente fuera de esta capa:

* Intentos individuales (Squat1Kg, Bench2Kg, Deadlift3Kg, etc.).
* Métricas normalizadas (Dots, Wilks, Glossbrenner, Goodlift).
* Detalles técnicos de levantamientos.

Estas columnas presentan alto riesgo de:

* mala interpretación,
* comparaciones injustas,
* necesidad de normalización avanzada.

Criterio de exclusión:

* No se eliminan del dataset fuente.
* Se excluyen únicamente del **modelo analítico de la Capa 01**.
* Se consideran candidatas para la **Capa 02**, orientada a análisis comparativos y descriptivos de cargas.

---

## 6. Impacto en capas futuras

La selección realizada:

* habilita análisis descriptivos claros, auditables y reproducibles,
* limita conscientemente el análisis de rendimiento individual fino,
* establece una base semántica sólida para capas posteriores.

Esta restricción es **estratégica**, no técnica, y responde al objetivo fundacional de la Capa 01: ofrecer una visión general, comprensible y responsable del ecosistema competitivo del powerlifting argentino.

---

**Estado del documento:** definitivo para la Capa 01.
