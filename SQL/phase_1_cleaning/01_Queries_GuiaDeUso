# Guía de Uso - Queries Capa 01
## Análisis Descriptivo Base - Powerlifting Argentina

---

## 1. Introducción

Este documento complementa el archivo SQL de queries de la Capa 01 y proporciona:

- Contexto de uso para cada query
- Interpretación esperada de resultados
- Consideraciones de ejecución
- Guía de troubleshooting

---

## 2. Estructura del documento SQL

El archivo de queries está organizado en **10 secciones**, cada una correspondiente a una pregunta analítica específica definida en `03_Query_Strategy_Capa01.md`.

Cada sección incluye:

- Encabezado con número de query y título
- Pregunta que responde
- Valor analítico esperado
- Una o más queries SQL
- Comentarios técnicos cuando son necesarios

---

## 3. Guía por Query

### Q1 — Volumen de Participación

**Propósito:** Establecer la magnitud del dataset y validar el alcance inicial.

**Resultados esperados:**
- `atletas_unicos`: Cantidad de nombres únicos en el dataset
- `participaciones_totales`: Cantidad total de registros
- `participaciones_promedio_por_atleta`: Ratio participaciones/atletas

**Interpretación:**
- Un ratio alto (>10) sugiere atletas con carreras largas o dataset histórico extenso
- Un ratio bajo (<3) puede indicar dataset reciente o alta rotación de atletas

**Uso en visualización:**
- Métrica principal en dashboard de overview
- KPI de contexto para otros análisis

---

### Q2 — Distribución por Sexo

**Propósito:** Describir la composición de género del ecosistema competitivo.

**Resultados esperados:**
- Una fila por cada valor de `Sex` (típicamente M, F)
- Participaciones y porcentaje para cada categoría

**Interpretación:**
- No evalúa desempeño deportivo
- Identifica posibles sesgos en el reporting histórico
- Sirve como filtro base para análisis segmentados

**Consideración importante:**
Si aparecen valores NULL o inesperados, documentar para revisión en capa de montaje.

---

### Q3 — Distribución por Federación

**Propósito:** Mapear la estructura institucional del powerlifting argentino.

**Resultados esperados:**
- Lista de federaciones ordenada por volumen de participaciones
- Porcentaje de representación de cada una
- Cantidad de atletas únicos por federación

**Interpretación:**
- Federaciones con pocos atletas pero muchas participaciones: posible concentración
- Federaciones con muchos atletas únicos: posible alcance territorial amplio
- Ayuda a entender heterogeneidad de criterios de reporting

**Uso analítico:**
- Filtro fundamental para análisis comparativos futuros
- Contexto para evaluar representatividad del dataset

---

### Q4 — Tipos de Eventos y Equipamiento

**Propósito:** Entender las reglas de competición predominantes en el dataset.

**Estructura:**
Esta query tiene dos variantes (Q4a y Q4b):

**Q4a - Distribución por Event:**
- Valores típicos: SBD (completo), B (bench only), D (deadlift only), S (squat only)
- Permite identificar especialización o competencias completas

**Q4b - Distribución por Equipment:**
- Valores típicos: Raw, Wraps, Single-ply, Multi-ply, Unlimited
- Crítico para comparaciones justas de desempeño en capas futuras

**Consideración técnica:**
Estos campos determinan la comparabilidad de resultados. No mezclar categorías sin normalización.

---

### Q5 — Participación a lo Largo del Tiempo

**Propósito:** Describir la evolución temporal del ecosistema sin inferencias causales.

**Resultados esperados:**
- Serie temporal de participaciones por año
- Cantidad de atletas únicos por año
- Federaciones activas por año

**Interpretación posible:**
- Tendencia creciente: expansión del deporte
- Caídas abruptas: posibles gaps en el dataset o eventos históricos (ej. pandemia)
- Picos: años con eventos importantes o mejor reporting

**Advertencia:**
No interprete esto como "crecimiento del deporte" sin validar completitud del dataset.

---

### Q6 — Distribución de Categorías de Peso

**Propósito:** Mapear la composición corporal del sistema sin evaluar rendimiento.

**Estructura:**
Dos variantes para diferentes perspectivas:

**Q6a - Distribución general:**
- Vista consolidada de todas las categorías
- Útil para identificar categorías más pobladas

**Q6b - Segmentación por sexo:**
- Vista detallada por Sex y WeightClassKg
- Calcula porcentaje dentro de cada sexo
- Esencial para análisis de representatividad

**Consideración técnica:**
El `CAST(WeightClassKg AS FLOAT64)` permite ordenamiento numérico correcto de categorías como "52", "56", "60", etc.

---

### Q7 — Clases Etarias

**Propósito:** Describir diversidad etaria y evaluar calidad del dato reportado.

**Estructura:**
Dos variantes complementarias:

**Q7a - Distribución por AgeClass:**
- Categorías típicas: Open, Juniors, Sub-Juniors, Masters 1-4, etc.
- Muestra qué grupos etarios participan más

**Q7b - Completitud del dato de edad:**
- Clasifica registros según disponibilidad de información etaria
- Identifica gaps en el reporting histórico

**Interpretación:**
- Alto porcentaje de "Sin información etaria": dataset histórico o federaciones con reporting limitado
- Predominancia de "Open": posible concentración en categoría general

---

### Q8 — País de Competencia

**Propósito:** Entender el alcance geográfico de las competencias sin comparar desempeño.

**Estructura:**
Dos variantes para análisis granular y agregado:

**Q8a - Distribución por MeetCountry:**
- Lista completa de países donde compitieron atletas argentinos
- Identifica destinos internacionales más frecuentes

**Q8b - Clasificación local vs internacional:**
- Agrupa en tres categorías: Nacional, Internacional, No especificado
- Vista simplificada para análisis de exposición internacional

**Uso analítico:**
- Filtro para análisis de "competencia local" vs "competencia internacional"
- Contexto para interpretar niveles de exigencia competitiva

---

### Q9 — Condición Administrativa del Resultado (Place)

**Propósito:** Evaluar calidad operativa del dataset y condiciones de finalización.

**Estructura:**
Dos variantes para detalle y agregación:

**Q9a - Distribución completa de Place:**
- Muestra todos los valores posibles: 1, 2, 3, ..., DQ, NS, G, DD, NULL
- Permite ver distribución real sin interpretación

**Q9b - Clasificación por tipo de resultado:**
- Agrupa valores en categorías semánticas:
  - Posición válida (números)
  - DQ (descalificado)
  - NS (no show/no presentado)
  - G (guest/invitado)
  - DD (doping disqualified)
  - Sin registro (NULL)

**Interpretación:**
- Alto % de DQ o NS: posible problema en dataset o reglas de federación
- Muchos NULL: gap en reporting histórico
- Predominancia de posiciones válidas: dataset de buena calidad operativa

---

### Q10 — Totales Reportados (TotalKg)

**Propósito:** Evaluar completitud del dato principal de desempeño.

**Estructura:**
Dos variantes para completitud y distribución:

**Q10a - Completitud del dato:**
- Clasifica en: Total reportado, Total no reportado, Total = 0 o inválido
- Métrica crítica de calidad del dataset

**Q10b - Distribución por rangos:**
- Agrupa totales en rangos de 200kg para análisis descriptivo
- NO es un ranking, es una distribución estadística

**Interpretación Q10a:**
- Alto % de "Total no reportado": posible presencia de eventos sin resultados finales
- Correlacionar con Q9 para entender si NULL en TotalKg corresponde a DQ/NS

**Interpretación Q10b:**
- Permite identificar rangos de desempeño más comunes
- Útil para segmentar análisis futuros por "nivel de carga"

**Advertencia crítica:**
Esta query NO debe interpretarse como evaluación de desempeño atlético. Es puramente descriptiva de la distribución de datos.

---

## 4. Consideraciones de Ejecución

### 4.1 Orden de ejecución recomendado

Para validación inicial del dataset:

1. **Q1** — Establecer volumen total
2. **Q10a** — Validar completitud de dato principal
3. **Q9b** — Entender condiciones de finalización
4. **Q2, Q3** — Mapear estructura básica
5. **Q5** — Validar cobertura temporal
6. **Q4, Q6, Q7, Q8** — Profundizar en contexto competitivo

### 4.2 Tiempo de ejecución esperado

Todas las queries están diseñadas para bajo costo computacional:

- Dataset completo Argentina: <5 segundos por query
- Costo estimado por ejecución completa: <USD 0.01

### 4.3 Troubleshooting común

**Error: "Table not found"**
- Verificar que el proyecto ID sea correcto: `burnished-rider-368414`
- Confirmar que el dataset `analytics` exista
- Validar que la tabla se llame exactamente `openpowerlifting`

**Resultados vacíos en todas las queries:**
- Verificar filtro `Country = 'Argentina'`
- Confirmar que existan datos para Argentina en la tabla
- Revisar proceso de montaje de datos

**Valores inesperados en Place o Equipment:**
- Documentar, no corregir en esta capa
- Reportar para revisión en capa de montaje
- Considerar para diccionario de datos

---

## 5. Integración con Looker Studio

### 5.1 Queries recomendadas para dashboard principal

**Página 1 - Overview:**
- Q1 (KPI principal)
- Q2 (gráfico de torta)
- Q5 (gráfico de línea temporal)

**Página 2 - Composición:**
- Q3 (gráfico de barras federaciones)
- Q6a (gráfico de barras categorías)
- Q7a (gráfico de barras clases etarias)

**Página 3 - Contexto Competitivo:**
- Q4a y Q4b (gráficos comparativos)
- Q8b (gráfico de torta local vs internacional)
- Q9b (gráfico de barras condiciones)

### 5.2 Filtros sugeridos

Todos los dashboards deberían permitir filtrar por:
- Año (de Q5)
- Sexo (de Q2)
- Federación (de Q3)

**No implementar en esta capa:**
- Filtros por atleta individual
- Comparaciones de desempeño
- Rankings

---

## 6. Validación de Resultados

### 6.1 Checks de consistencia

Después de ejecutar todas las queries, validar:

```
SUM(participaciones de Q2) = participaciones_totales de Q1 ✓
SUM(participaciones de Q3) = participaciones_totales de Q1 ✓
SUM(participaciones de Q4a) = participaciones_totales de Q1 ✓
```

### 6.2 Alerts de calidad

Documentar si:
- Q10a muestra >30% de NULL en TotalKg
- Q9b muestra >20% de DQ/NS
- Q7b muestra >50% sin información etaria
- Q5 muestra gaps temporales de >2 años

Estos no son errores, pero son importantes para interpretar análisis futuros.

---

## 7. Próximos Pasos

Una vez validadas estas queries:

1. Documentar resultados en reporte de calidad de datos
2. Identificar patrones que requieran investigación
3. Preparar diccionario de valores observados
4. Definir métricas para Capa 02 basadas en estos hallazgos
5. Crear vistas materializadas si el volumen lo justifica

---

## 8. Restricciones y Prohibiciones

**En esta capa NO se permite:**

- Modificar queries para incluir columnas fuera del alcance
- Agregar métricas compuestas (Wilks, Dots, IPF Points)
- Implementar rankings competitivos
- Comparar atletas entre sí
- Aplicar filtros de calidad de dato no documentados

Cualquier necesidad analítica que no pueda resolverse con estas queries debe:
1. Documentarse
2. Evaluarse si pertenece a Capa 02
3. Justificarse según principios del proyecto

---

## 9. Contacto y Soporte

Para dudas sobre:
- **Semántica de datos:** revisar `01_Column_Selection_Criteria.md`
- **Arquitectura:** revisar `02_Data_Mounting_Plan.md`
- **Estrategia analítica:** revisar `03_Query_Strategy_Capa01.md`
- **Testing:** revisar `01_Data_Mounting_Tests.md` (pendiente de compartir)

---

**Documento generado:** Enero 2026  
**Versión:** 1.0 - Capa 01  
**Estado:** Listo para ejecución
