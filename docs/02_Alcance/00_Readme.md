## 📐 Alcance del Proyecto
_Introducción_

Este proyecto se estructura en capas de análisis progresivas, con el objetivo de transformar datos crudos del ecosistema deportivo en conocimiento claro, reproducible y transferible.
Cada capa tiene un alcance explícito, decisiones documentadas y criterios técnicos alineados con el uso real de datos en entornos Cloud, priorizando:
- claridad conceptual,
- control de costos,
- reproducibilidad,
- criterio deportivo antes que complejidad técnica.

El proyecto no busca “resultados espectaculares” tempranos, sino construir una base sólida sobre la cual los análisis posteriores tengan sentido.

## 🧱 Enfoque por capas

El análisis se organiza en capas independientes pero encadenadas:

_Capa 00 — Datos Crudos (Raw)_

Propósito:
- preservar el dataset original sin interpretaciones,
- garantizar trazabilidad total.

Características:
- ingesta directa de CSVs completos,
- sin limpieza semántica,
- sin recortes analíticos,
- solo control técnico (tipos, encoding, storage).

_Capa 01 — Análisis Descriptivo Base_

Propósito:
- construir una primera visión confiable del ecosistema,
- habilitar análisis descriptivo sin sesgos interpretativos.

Características:
- limpieza conservadora,
- recortes explícitos (geográficos / contextuales),
- métricas simples y agregadas,
- foco en entender qué hay en los datos,

👉 El detalle completo del alcance, métricas y decisiones de esta capa se documenta en:
docs/01_AnalisisDeCapa/Alcance_Capa_01.md

## Capas posteriores (02+)

Planteadas, pero fuera del alcance actual:
- normalizaciones avanzadas
- análisis longitudinal
- métricas comparativas complejas
- modelos predictivos
- dashboards finales
- IA aplicada

Estas capas se activan solo cuando las anteriores estén cerradas y justificadas.

## 🌎 Alcance geográfico general

Foco inicial:
Argentina

La expansión regional o internacional está prevista para fases posteriores, una vez validada la estructura base.

## 🧠 Criterio rector del proyecto

Si una decisión no puede explicarse desde la práctica deportiva real
o desde el uso responsable de datos, no entra en esta etapa.

## 📦 Entregables esperados por capa

Cada capa debe producir:
- datasets claramente versionados,
- queries documentadas,
- decisiones técnicas y analíticas explícitas,
- costos controlados y reproducibles,
- documentación suficiente para terceros.

🚫 Qué no evalúa este proyecto (en etapas tempranas)
- quién es “el mejor”,
- qué federación es superior,
- qué país tiene un mejor rendimiento deportivo.

qué fórmula es la correcta

Esas preguntas requieren capas posteriores y otro nivel de contexto.
