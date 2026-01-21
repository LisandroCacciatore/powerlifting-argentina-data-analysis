Test Plan — 
Capa 01 · Análisis Descriptivo Base
1. Objetivo del test

Validar que el conjunto completo de queries Capa 01:

Describe correctamente el volumen, composición y completitud del dataset para Country = 'Argentina'

Mantiene consistencia interna entre queries relacionadas

Expone explícitamente NULLs, valores atípicos y estados administrativos

No introduce suposiciones implícitas ni sesgos semánticos

Este test es bloqueante para:

Uso en Looker Studio

Definición de métricas en Capa 02

Comunicación de resultados a usuarios no técnicos

2. Dataset de prueba propuesto (mínimo y controlado)
2.1 Columnas necesarias (subset crítico)
Columna	Uso principal
Name	Atletas únicos, ratios
Country	Scope del dataset
Sex	Q2
Federation	Q3, Q5
Event	Q4a
Equipment	Q4b
Date	Q5
WeightClassKg	Q6
Age	Q7b
AgeClass	Q7a / Q7b
MeetCountry	Q8
Place	Q9
TotalKg	Q10
2.2 Filas mínimas (dataset sintético)
Name	Country	Sex	Federation	Event	Equipment	Date	WeightClassKg	Age	AgeClass	MeetCountry	Place	TotalKg
A1	Argentina	M	F1	SBD	Raw	2019-06-01	83	28	Open	Argentina	1	600
A1	Argentina	M	F1	SBD	Raw	2020-06-01	83	29	Open	Argentina	2	610
A2	Argentina	F	F1	B	Raw	2020-07-01	63	NULL	Open	Argentina	DQ	NULL
A3	Argentina	M	F2	SBD	Wraps	2021-05-01	93	NULL	NULL	Brazil	NS	0
A4	Argentina	NULL	NULL	D	Single-ply	NULL	NULL	NULL	NULL	NULL	NULL	NULL
A5	Argentina	F	F2	S	Raw	2023-08-01	52	17	Juniors	Argentina	G	350

Notas de diseño

Atleta con múltiples participaciones (A1)

Valores NULL sistemáticos (A4)

Estados administrativos variados (DQ, NS, G, NULL)

TotalKg válido, NULL y 0

MeetCountry nacional, internacional y NULL

Gaps temporales (no hay 2022)

3. Escenarios de test
3.1 Escenario nominal

Dataset con participaciones válidas y consistentes

Esperable que todas las queries devuelvan filas

Porcentajes suman ~100% (± rounding)

3.2 Casos límite

Sex IS NULL → Q2 debe devolver fila NULL

Federation IS NULL → Q3 debe devolver fila NULL

WeightClassKg IS NULL → Q6a/Q6b deben incluirlo

Date IS NULL → excluido solo en Q5

TotalKg = 0 → clasificado como “inválido” en Q10a

3.3 Casos de datos faltantes

Age IS NULL + AgeClass IS NOT NULL → “Solo AgeClass disponible”

Ambos NULL → “Sin información etaria”

MeetCountry IS NULL → “No especificado” en Q8b

Place IS NULL → “Sin registro” en Q9b

3.4 Casos inválidos pero posibles

Place = 'G' con TotalKg > 0

NS con TotalKg = 0

Equipment no estándar (ej. typo futuro)

WeightClassKg no casteable (riesgo latente)

4. Resultados esperados (checks clave)
4.1 Consistencia global
Check	Resultado esperado
SUM Q2 participaciones	= Q1 participaciones_totales
SUM Q3 participaciones	= Q1 participaciones_totales
SUM Q4a participaciones	= Q1 participaciones_totales
Q5 suma participaciones	≤ Q1 (por Date NULL)
4.2 Resultados cualitativos esperados

Q2 incluye fila Sex = NULL

Q3 incluye fila Federation = NULL

Q7b muestra las 3 categorías

Q9b clasifica correctamente todos los Place

Q10a separa NULL, 0 y >0 correctamente

Q10b incluye “Sin dato” y “0 kg”

4.3 Resultados que NO deberían aparecer

Filas duplicadas por atleta

Porcentajes >100%

Filas perdidas por JOIN implícito (no hay joins, OK)

Exclusión silenciosa de NULLs (excepto Date en Q5)

5. Clasificación del resultado
Condición	Clasificación
Todos los checks de suma OK	PASS
NULLs altos según alerts documentados	WARN
Inconsistencias entre Q1 y Q2/Q3/Q4	FAIL
Q9/Q10 no clasifican correctamente	FAIL
6. Riesgos detectados
6.1 Riesgos semánticos

Name ≠ atleta único real (homónimos)

Place numérico no implica validez deportiva

TotalKg > 0 no implica competencia válida

6.2 Riesgos de interpretación

Usuarios pueden leer Q5 como “crecimiento del deporte”

Q10b puede interpretarse como nivel de rendimiento

Q3 puede interpretarse como ranking de federaciones

(Mitigado parcialmente por la Guía de Uso, bien documentada)

6.3 Riesgos de calidad de datos

WeightClassKg no numérico rompe CAST

Sex fuera de {M, F}

Federaciones con naming inconsistente

Totales reportados en eventos parciales (B, S, D)

7. Conclusión de QA

Estado general del set de queries: ✅ APTO PARA USO EN CAPA 01
