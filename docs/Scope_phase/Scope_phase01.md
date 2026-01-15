Scope

Ingesta, limpieza controlada y base analítica (Argentina)

Introducción

La Fase 1 de este proyecto establece la base técnica y conceptual sobre la cual se construirá todo el análisis posterior.

El objetivo no es producir insights avanzados ni comparaciones complejas, sino crear un dataset confiable, entendible y reproducible, alineado con un uso realista de Google Cloud y con foco en control de costos.

Esta fase prioriza criterio antes que complejidad.

🎯 Objetivo de la Fase 1

Transformar el dataset crudo de OpenPowerlifting en una base limpia y estandarizada de competidores de Argentina, lista para análisis descriptivo del ecosistema nacional.

Al finalizar esta fase, el proyecto debe poder responder con claridad:

quiénes componen el ecosistema

cómo se distribuye la participación

qué datos son confiables y cuáles requieren contexto

📦 Dataset de partida

Fuente: OpenPowerlifting (bulk CSV)

Versión: openpowerlifting-latest

Formato: CSV único, sin joins

Volumen: dataset completo, con recorte posterior

🗺️ Alcance geográfico
Incluido

Levantadores con:

Country = 'Argentina' y/o

Meets realizados en Argentina (MeetCountry = 'Argentina')

Esta doble condición permite capturar tanto el perfil de levantadores argentinos como el contexto competitivo local.

Excluido

Otros países (por ahora)

Comparaciones regionales o internacionales

La expansión a Sudamérica forma parte de una fase posterior.

🧱 Alcance técnico
Incluido en Fase 1

Ingesta de CSV grandes vía Google Cloud Storage

Tabla raw sin modificaciones

Creación de una tabla clean con:

tipos de datos consistentes

nombres de columnas normalizados

recorte geográfico explícito

Documentación de decisiones de limpieza

Queries descriptivas de Capa 1

Excluido en Fase 1

Modelos de datos complejos

Particionado avanzado

Materializaciones optimizadas

Automatizaciones

Dashboards finales

IA o modelos predictivos

🧹 Filosofía de limpieza de datos

La limpieza en Fase 1 es conservadora.

Principios

No borrar información que pueda tener valor analítico

No imputar valores sin justificación deportiva

Diferenciar entre “dato sucio” y “dato incómodo”

Ejemplos explícitos

Valores negativos en intentos → se conservan

Totales nulos → no se rellenan

Edades aproximadas → se mantienen

Inconsistencias entre federaciones → se documentan

El objetivo es hacer explícito el problema, no ocultarlo.

📊 Tipo de análisis habilitado

La Fase 1 habilita únicamente análisis de:

perfil del ecosistema

composición por sexo, evento y equipamiento

distribución general del rendimiento (sin ranking fino)

No se habilitan aún:

comparaciones entre países

análisis longitudinales

inferencias causales

📁 Entregables de la Fase 1

Al cerrar esta fase, el proyecto debe contar con:

En BigQuery

raw_openpowerlifting_arg

clean_openpowerlifting_arg

primeras vistas o tablas analíticas de perfil

En GitHub

docs/phase_1_scope.md

docs/column_classification.md

docs/data_standard_argentina.md

queries organizadas por fase

🚫 Qué no se evalúa en esta fase

Quién es el mejor levantador

Qué país rinde más

Qué fórmula es “mejor”

Qué federación es superior

Todas esas preguntas requieren capas posteriores.

🧠 Criterio rector

Si una decisión no puede explicarse desde el entrenamiento y la competencia real, no entra en esta fase.
