## Alcance — Capa 01
_Análisis Descriptivo Base_

## _Objetivo de la capa_

Definir de forma explícita, verificable y reproducible qué datos, métricas y transformaciones forman parte de la Capa 01, estableciendo una base confiable para análisis posteriores.
Esta capa busca responder qué es el ecosistema, no explicar por qué ocurre ni predecir comportamientos.

## 🎯 Objetivo específico

Transformar el dataset crudo de OpenPowerlifting en una base limpia y estandarizada de competidores vinculados a Argentina, apta para:
- análisis descriptivo,
- exploración segura,
- comunicación clara de contexto.

## 📦 Dataset fuente

_OpenPowerlifting — Full Dataset_
_Formato: CSV único (bulk)_
_Sin joins externos_
_Cargado en BigQuery como tabla raw_
_Sin modificaciones semánticas sobre el contenido original_

## 🗺️ Alcance geográfico
Incluido
Levantadores que cumplan al menos una de las siguientes condiciones:
Country = 'Argentina'
MeetCountry = 'Argentina'

Esta doble condición permite capturar:
atletas argentinos compitiendo fuera del país
competencia local con participación internacional

Excluido
- análisis comparativo entre países
- rankings internacionales
- contexto regional ampliado (Sudamérica)

## 🧱 Campos incluidos

Solo los necesarios para análisis descriptivo básico:
_Identificación y contexto_

- Name
- Sex
- Country
- Federation
- Evento y competencia
- Event
- Equipment
- Sanctioned
- MeetCountry
- Date
- Categorías
- WeightClassKg
- BodyweightKg
- Age
- AgeClass
- Resultados agregables
- TotalKg
- Place

## 📐 Métricas permitidas

Exclusivamente métricas descriptivas y agregadas:
- conteo de atletas,
- conteo de participaciones,
- promedios simples,
- distribuciones y frecuencias,
- tendencias temporales generales,
- rankings agregados por volumen o promedio.

## 🚫 Métricas excluidas

Quedan fuera de esta capa:
- métricas de eficiencia,
- scores normalizados avanzados,
- comparaciones atleta vs atleta,
- indicadores predictivos,
- inferencias causales.

## 🧹 Filosofía de limpieza de datos

La limpieza en Capa 01 es conservadora.

Principios:
- no borrar datos con potencial valor analítico
- no imputar valores sin justificación deportiva
- distinguir entre “dato sucio” y “dato incómodo”

Ejemplos:
- intentos negativos → se conservan
- totales nulos → no se rellenan
- edades aproximadas → se mantienen
- inconsistencias entre federaciones → se documentan
El objetivo es hacer explícitos los problemas, no ocultarlos.

## 🔄 Transformaciones permitidas

- casteo de tipos
- normalización básica de textos
- limpieza simple de nulos
- derivación temporal (año, mes)

No se permite:

- imputación compleja
- enriquecimiento externo
- lógica de negocio subjetiva

## 📁 Entregables de la Capa 01
_En BigQuery_
- raw_openpowerlifting_arg,
- clean_openpowerlifting_arg,
- primeras vistas descriptivas

_En GitHub_

documentación de alcance
- criterios de selección de columnas,
- decisiones de limpieza,
- queries organizadas por fase.

## ✅ Criterio de cierre

La Capa 01 se considera completa cuando:
- las métricas son reproducibles
- el costo de ejecución es controlado

u tercero puede entender el análisis sin contexto previo

todas las decisiones están documentadas
