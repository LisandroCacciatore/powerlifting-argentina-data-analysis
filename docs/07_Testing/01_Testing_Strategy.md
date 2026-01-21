Estrategia de Testing – Proyecto de Análisis de Datos Deportivos
Propósito

Este documento define la estrategia de testing del proyecto powerlifting-argentina-data-analysis.

El objetivo del testing no es validar dashboards, sino proteger el criterio analítico, la coherencia del dominio deportivo y las decisiones técnicas desde las primeras etapas del proyecto.

El enfoque es Shift Left: detectar errores conceptuales, de datos o de interpretación antes de que se conviertan en métricas, visualizaciones o conclusiones.

Principios del enfoque
1. Testing temprano, simple y explícito

Se testea desde la ingesta y limpieza

Se priorizan reglas claras antes que automatización

Los tests se expresan como queries SQL de validación

2. El dominio manda

Las reglas de testing surgen del dominio del powerlifting, no de convenciones genéricas.

Ejemplos:

Un Total no existe si falta un lift

Un intento negativo no es un levantamiento exitoso

Un DQ no puede tener puntos

El evento define qué lifts son válidos

3. Testing como contrato entre capas

Cada fase y cada capa:

declara qué promete

define qué datos usa

especifica qué errores no acepta

Los tests funcionan como contrato de análisis, no como control policial.

Qué se testea en este proyecto
1️⃣ Testing de estructura (Schema / Contract)

Valida que los datos cumplan con la forma esperada.

Incluye:

existencia de columnas obligatorias

tipos de datos coherentes

valores esperados en columnas categóricas

ausencia de valores imposibles

Ejemplos:

Date con formato válido

Country no nulo

Sex ∈ {M, F, Mx}

Event dentro del set definido

2️⃣ Testing de reglas de negocio

Valida que las transformaciones respeten el dominio deportivo.

Ejemplos:

TotalKg solo existe si hay 3 lifts válidos

intentos negativos no cuentan como exitosos

Place = DQ ⇒ puntos vacíos

eventos parciales no mezclan lifts inexistentes

Este tipo de test protege el sentido del análisis, no solo la técnica.

3️⃣ Testing de coherencia interna

Valida relaciones lógicas entre columnas.

Ejemplos:

Best3SquatKg ≥ mejores intentos válidos

TotalKg = suma de Best3*

lifts inexistentes según evento deben ser NULL

puntos solo existen si el levantador es válido

Detecta errores silenciosos y supuestos rotos.

4️⃣ Testing de intención analítica

Valida que una query responda realmente a la pregunta declarada.

Ejemplos:

¿Estamos contando levantadores únicos o participaciones?

¿Estamos mezclando categorías sin aclararlo?

¿Incluimos NS o DQ sin querer?

Este test no siempre es automático:
se documenta y se revisa conscientemente.

Cómo se implementan los tests
Tests como SQL

Los tests se escriben como queries SQL

No devuelven métricas finales

Devuelven:

registros inválidos

conteos de errores

flags de violación de reglas

Si una query de test devuelve filas, hay algo para revisar.

Ubicación de los tests
SQL/
 ├── phase_1_cleaning/
 │    ├── cleaning.sql
 │    ├── tests_structure.sql
 │    ├── tests_rules.sql
 │
 ├── phase_2_core/
 │    ├── core_metrics.sql
 │    ├── tests_consistency.sql

Relación entre Docs y SQL

Docs definen el criterio

SQL implementa

SQL valida

Docs registran decisiones y límites

Cada fase tiene su documento de tests asociado.

Qué NO se hace (por ahora)

No frameworks de testing de datos

No automatización CI/CD

No pipelines complejos

No cobertura exhaustiva artificial

Este proyecto prioriza:

claridad

aprendizaje progresivo

criterio transferible

La automatización vendrá cuando el lenguaje del dominio esté consolidado.

Evolución futura

Esta estrategia permite escalar hacia:

testing automatizado

validaciones continuas

integración con herramientas como dbt o Vertex AI

Sin rehacer lo ya construido.

Cierre

El testing en este proyecto no es un paso final.

Es una herramienta para:

pensar mejor

decidir con datos

comunicar con honestidad

Primero criterio.
Después métricas.
Luego producto.
