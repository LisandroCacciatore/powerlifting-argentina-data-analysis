Test Set — Capa 01 · Análisis Descriptivo Base

Dominio: Powerlifting Argentina
Plataforma: BigQuery
Dataset: burnished-rider-368414.analytics.openpowerlifting
Scope: Queries Q1–Q10
Nivel: Dataset / Query Layer
Tipo: Data Quality + Semantic Validation

Test Set ID: TS-C01-DESCRIPTIVE-BASE
Objetivo del Test Set

Validar que las queries de la Capa 01:

Representan fielmente el dataset montado

Son internamente consistentes

Preservan valores NULL y estados administrativos

No introducen supuestos implícitos

Feature 1 — Volumen y Consistencia Global
Feature: Volumen y consistencia global del dataset
  Como Data Tester
  Quiero validar el volumen total y la consistencia entre queries
  Para asegurar que todas describen el mismo universo de datos

Test Case TC-C01-Q1-01 — Volumen básico del dataset

Objetivo:
Validar que Q1 establece correctamente el universo base de análisis.

Scenario: Calcular volumen total de atletas y participaciones
  Given el dataset openpowerlifting contiene registros con Country = 'Argentina'
  When ejecuto la query Q1
  Then el resultado debe contener exactamente una fila
  And atletas_unicos debe ser mayor que 0
  And participaciones_totales debe ser mayor que 0
  And participaciones_promedio_por_atleta debe ser >= 1


Resultado esperado:

1 fila

Métricas no nulas

Ratio consistente

Clasificación: PASS / FAIL

Test Case TC-C01-CONSISTENCY-01 — Consistencia de participaciones entre queries

Objetivo:
Validar que todas las queries describen el mismo total de participaciones.

Scenario: Consistencia de conteos globales
  Given ejecuto Q1 y obtengo participaciones_totales
  When ejecuto Q2, Q3 y Q4a
  Then la suma de participaciones en cada query debe ser igual a participaciones_totales de Q1


Notas:

Excepción documentada: Q5 (excluye Date NULL)

Clasificación:

PASS si todas coinciden

FAIL si alguna difiere

Feature 2 — Distribuciones categóricas y NULLs
Feature: Distribuciones categóricas
  Como Data Tester
  Quiero validar que las distribuciones preservan valores reales y NULL
  Para evitar pérdida silenciosa de información

Test Case TC-C01-Q2-01 — Distribución por sexo

Objetivo:
Validar que Q2 no filtra ni normaliza valores inesperados.

Scenario: Distribución por sexo con valores NULL
  Given existen registros con Sex = 'M', 'F' y NULL
  When ejecuto la query Q2
  Then el resultado debe incluir una fila por cada valor distinto de Sex
  And la suma de participaciones debe ser igual a Q1


Resultado esperado:

Fila Sex = NULL visible

Porcentajes suman ~100%

Clasificación: PASS / WARN / FAIL

Test Case TC-C01-Q3-01 — Distribución por federación

Objetivo:
Validar preservación de federaciones no especificadas.

Scenario: Federaciones con valores NULL
  Given existen registros con Federation NULL
  When ejecuto la query Q3
  Then el resultado debe incluir Federation = NULL
  And atletas_unicos debe ser consistente con Name

Feature 3 — Temporalidad y cobertura histórica
Feature: Participación a lo largo del tiempo
  Como Data Tester
  Quiero validar cobertura temporal sin inferencias implícitas

Test Case TC-C01-Q5-01 — Serie temporal válida

Objetivo:
Validar correcta agregación anual y exclusión explícita de Date NULL.

Scenario: Evolución temporal de participaciones
  Given existen registros con Date válida y Date NULL
  When ejecuto la query Q5
  Then solo deben incluirse registros con Date no NULL
  And los años deben estar ordenados ascendentemente


Resultado esperado:

Años consecutivos o gaps visibles

Sin años NULL

Clasificación: PASS / WARN

Feature 4 — Clasificaciones semánticas (Edad, País, Place)
Feature: Clasificaciones semánticas
  Como Data Tester
  Quiero validar que las reglas CASE reflejan fielmente el dato original

Test Case TC-C01-Q7-01 — Completitud del dato etario
Scenario: Clasificación de estado etario
  Given existen registros con Age, solo AgeClass y ambos NULL
  When ejecuto la query Q7b
  Then deben aparecer exactamente tres categorías de estado_edad
  And la suma de participaciones debe ser igual a Q1

Test Case TC-C01-Q8-01 — Clasificación nacional vs internacional
Scenario: Ámbito de competencia
  Given existen registros con MeetCountry = 'Argentina', otro país y NULL
  When ejecuto la query Q8b
  Then deben aparecer las categorías Nacional, Internacional y No especificado

Test Case TC-C01-Q9-01 — Clasificación administrativa del resultado
Scenario: Estados administrativos de Place
  Given existen registros con Place numérico, DQ, NS, G, DD y NULL
  When ejecuto la query Q9b
  Then cada registro debe clasificarse en una sola categoría
  And no deben perderse participaciones

Feature 5 — Completitud y validez de TotalKg
Feature: Totales reportados
  Como Data Tester
  Quiero validar completitud del dato principal de carga

Test Case TC-C01-Q10-01 — Completitud del TotalKg
Scenario: Clasificación del estado de TotalKg
  Given existen registros con TotalKg > 0, TotalKg = 0 y TotalKg NULL
  When ejecuto la query Q10a
  Then deben aparecer las tres categorías de estado_total
  And la suma de participaciones debe ser igual a Q1

Test Case TC-C01-Q10-02 — Distribución por rangos
Scenario: Distribución por rangos de TotalKg
  Given existen valores de TotalKg en distintos rangos
  When ejecuto la query Q10b
  Then cada participación debe caer en un solo rango
  And debe existir el rango 'Sin dato' si hay NULL

Matriz de severidad
Resultado	Acción
PASS	Apto para Looker / Capa 02
WARN	Documentar en reporte de calidad
FAIL	Bloquea uso analítico
