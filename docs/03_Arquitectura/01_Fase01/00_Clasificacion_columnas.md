Introducción

El dataset de OpenPowerlifting contiene una gran cantidad de columnas que mezclan:

identidad del levantador

contexto competitivo

intentos individuales

métricas derivadas

metadata del evento

Antes de limpiar, transformar o analizar, es necesario clasificar las columnas por función, no por tipo técnico.

Este documento define cómo se interpreta cada grupo de columnas y qué rol cumple dentro del análisis, especialmente en la Fase 1 del proyecto.

🧩 Principio rector

Una columna no es “útil” o “inútil” por sí sola, sino según la pregunta que queremos responder.

La clasificación no elimina columnas, solo define su rol.

🟦 Grupo 1 — Identidad del levantador
Propósito

Identificar registros únicos de atletas sin asumir unicidad biológica ni deportiva.

Columnas

Name

Sex

Country

State

Consideraciones

Name no es un ID humano real, sino un identificador funcional (incluye #)

No se unifican levantadores entre competencias

El análisis se realiza a nivel registro competitivo, no trayectoria personal

Uso en Fase 1

Conteos

Distribuciones

Segmentación básica

🟦 Grupo 2 — Contexto competitivo
Propósito

Describir cómo y bajo qué reglas ocurrió la performance.

Columnas

Event

Equipment

Division

Tested

Federation

ParentFederation

Sanctioned

Place

Consideraciones

Division es texto libre → no se usa para inferir edad

Place no se interpreta como ranking absoluto

Federation contextualiza reglas y estándares

Uso en Fase 1

Perfil del ecosistema

Composición competitiva

No para ranking fino

🟦 Grupo 3 — Variables corporales y categorías
Propósito

Describir el marco físico y reglamentario del levantador.

Columnas

Age

AgeClass

BirthYearClass

BodyweightKg

WeightClassKg

Consideraciones clave

Age puede ser exacta o aproximada

AgeClass tiene prioridad analítica sobre Division

WeightClassKg puede ser máximo o mínimo (+)

No se imputan pesos ni edades faltantes

Uso en Fase 1

Distribuciones

Segmentaciones simples

Contexto, no inferencia

🟦 Grupo 4 — Intentos individuales
Propósito

Capturar el proceso competitivo, no solo el resultado.

Columnas

Squat1Kg, Squat2Kg, Squat3Kg, Squat4Kg

Bench1Kg, Bench2Kg, Bench3Kg, Bench4Kg

Deadlift1Kg, Deadlift2Kg, Deadlift3Kg, Deadlift4Kg

Consideraciones críticas

Valores negativos = intentos fallidos → se conservan

No todas las federaciones reportan intentos

Cuartos intentos no computan para el total

Valor analítico

Permiten preguntas como:

progresión de carga

tasa de fallos

saltos de peso y riesgo

Uso en Fase 1

Se conservan

No se analizan en profundidad todavía

🟦 Grupo 5 — Resultados consolidados
Propósito

Representar el rendimiento final validado.

Columnas

Best3SquatKg

Best3BenchKg

Best3DeadliftKg

TotalKg

Consideraciones

TotalKg puede ser nulo incluso con datos válidos

Totales antiguos pueden existir sin intentos

No se recalculan totales en Fase 1

Uso en Fase 1

Distribuciones

Percentiles

Perfil general del rendimiento

🟦 Grupo 6 — Métricas derivadas (coeficientes)
Propósito

Permitir comparaciones ajustadas por peso corporal y sexo.

Columnas

Dots

Wilks

Glossbrenner

Goodlift

Consideraciones

No siempre están presentes

Dependen de reglas específicas

No son comparables entre sí

Uso en Fase 1

Documentadas

No priorizadas

Se usarán en capas posteriores

🟦 Grupo 7 — Metadata del evento
Propósito

Contextualizar el momento y lugar de la competencia.

Columnas

Date

MeetName

MeetCountry

MeetState

Consideraciones

Date es la fecha de inicio del meet

No se construyen series temporales aún

MeetName no incluye año ni federación

Uso en Fase 1

Filtro geográfico

Contexto descriptivo

🧠 Columnas CORE para Fase 1

Las siguientes columnas se consideran core, porque permiten construir el perfil del país:

Name

Sex

Event

Equipment

Country

Federation

AgeClass

WeightClassKg

Best3SquatKg

Best3BenchKg

Best3DeadliftKg

TotalKg

El resto se conserva como contexto ampliado.

Cierre

Esta clasificación permite:

limpiar sin perder información valiosa

construir análisis por capas

justificar decisiones técnicas desde el deporte

escalar el proyecto sin rehacer la base

No todas las columnas se usan al mismo tiempo,
pero todas cuentan una parte de la historia.
