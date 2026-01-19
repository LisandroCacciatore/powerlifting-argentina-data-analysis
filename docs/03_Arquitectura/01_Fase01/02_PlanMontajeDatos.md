# 02_Data_Mounting_Plan — Capa 01

## 1. Propósito técnico

Este documento describe el **proceso técnico** mediante el cual los datos del dataset **OpenPowerlifting** son montados en **Google Cloud**, respetando estrictamente las decisiones conceptuales definidas en `01_CriterioSeleccionDeColumnas.md`.

Su función es **implementar**, no decidir.

El objetivo principal es transformar un dataset crudo en una base analítica **reproducible, económica y explicable**, lista para análisis descriptivo y visualización.

> Este proceso técnico puede rehacerse, optimizarse o reimplementarse sin afectar la **semántica del dato** definida para la Capa 01.

### Qué se entiende por semántica en este contexto

La semántica del dato refiere al **significado analítico acordado** de cada columna, métrica y relación:

* qué representa cada campo
* cómo puede interpretarse
* para qué tipo de análisis es válido

Este documento **no modifica** esos significados; solo los materializa técnicamente.

### Versionado del documento

Este documento se versiona por **fase y capa** cuando cambian decisiones técnicas (infraestructura, particionado, optimización), sin que ello implique cambios semánticos en los análisis existentes.

---

## 2. Alcance técnico del montaje

Esta sección define explícitamente **qué hace y qué no hace** este documento, para evitar ambigüedades y solapamientos con otros artefactos del proyecto.

### Dentro del alcance

Este documento cubre:

* carga del dataset OpenPowerlifting en Google Cloud Storage
* organización del storage (buckets, carpetas lógicas)
* ingesta en BigQuery
* definición de datasets técnicos (`raw`, `stage`, `analytics`)
* criterios básicos de particionado y tipado
* preparación de tablas para consumo analítico y visual

Todas las decisiones técnicas aquí descritas están subordinadas a:

* el alcance de la Capa 01
* los criterios de selección de columnas
* los principios de bajo costo y simplicidad

### Fuera del alcance

Este documento **no define**:

* métricas analíticas
* queries específicas de análisis
* lógica de negocio
* joins complejos
* visualizaciones
* testing (cubierto en documento separado)
* optimizaciones avanzadas
* automatización compleja de pipelines

Cualquier cambio semántico, analítico o interpretativo debe resolverse en documentos conceptuales previos.

---

## 3. Arquitectura general

### Flujo lógico

```
CSV
→ Cloud Storage
→ BigQuery (raw → stage → analytics)
→ Looker Studio
```

### Principios de diseño

* simplicidad deliberada
* bajo costo operativo
* baja barrera de entrada técnica
* reproducibilidad total
* foco en análisis, no en infraestructura

Esta arquitectura está pensada para ser **replicable por instituciones deportivas**, organizaciones pequeñas o equipos con recursos limitados.

No se incorporan servicios adicionales (Dataflow, Composer, etc.) en esta fase para evitar complejidad innecesaria.

---

## 4. Componentes de la arquitectura

### 4.1 Cloud Storage

* contiene el dataset original en formato CSV
* actúa como zona de aterrizaje (landing zone)
* no se aplican transformaciones semánticas

Estructura lógica sugerida:

```
data/
  └── 01_raw/
      └── openpowerlifting.csv
```

### 4.2 BigQuery

Se definen tres datasets técnicos:

* **raw**: reflejo fiel del CSV original
* **stage**: limpieza básica y casteos
* **analytics**: tablas listas para análisis descriptivo

No se introducen métricas nuevas ni lógica interpretativa en esta etapa.

### 4.3 Looker Studio

* consume tablas del dataset `analytics`
* se utiliza únicamente para visualización descriptiva
* no contiene lógica de negocio crítica

---

## 5. Consideraciones de costo

El diseño apunta a un costo mensual objetivo de **USD 10–20**, mediante:

* uso de BigQuery con queries simples
* datasets acotados
* ausencia de procesos en streaming
* consultas reproducibles y auditables

---

## 6. Escalabilidad futura

Esta arquitectura deja explícitamente preparado el camino para:

* capas analíticas más avanzadas
* incorporación de nuevos datasets
* automatización progresiva
* integración futura con **Vertex AI**

Vertex AI **no forma parte del alcance operativo de la Capa 01**, pero se reconoce como extensión natural del proyecto en fases posteriores.

---

## 7. Relación con documentos complementarios

Este documento se apoya y se complementa con:

* `01_CriterioSeleccionDeColumnas.md` — gobierno semántico
* documentos de arquitectura por capa
* documento de testing del montaje
* queries SQL documentadas

Cada uno cumple un rol específico y no intercambiable.
