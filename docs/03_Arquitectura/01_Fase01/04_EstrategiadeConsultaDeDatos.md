03_Query_Strategy_Capa01.md

Estrategia de Queries — Capa 01 (Análisis Descriptivo Base)

1. Propósito del documento

Este documento define qué queries se implementan en la Capa 01,
qué preguntas responden y qué resultados se espera obtener.

Su objetivo es:

traducir decisiones semánticas en consultas concretas

evitar queries exploratorias sin criterio

garantizar que cada query tenga una razón de existir

Este documento no contiene SQL.
Gobierna el SQL que se escribirá después.

2. Principios de diseño de queries (Capa 01)

Todas las queries de esta capa deben cumplir:

responder una pregunta clara

ser descriptivas, no evaluativas

operar sobre datos ya validados por el montaje

ser reutilizables en dashboards

tener bajo costo de ejecución

ser explicables sin contexto técnico

No se permiten:

lógica condicional compleja

métricas compuestas

comparaciones atleta vs atleta

inferencias de rendimiento

3. Nivel de las queries

Las queries de Capa 01 operan sobre:

tablas analytics_base o vistas equivalentes

datos ya limpiados y casteados

sin joins innecesarios

No se consulta directamente raw.

4. Queries previstas — listado estratégico
Q1 — Volumen de participación

Pregunta que responde
¿Cuántos atletas y participaciones existen en el dataset para Argentina?

Qué devuelve

cantidad de atletas únicos

cantidad de participaciones totales

Valor analítico
Define el tamaño del ecosistema.
Es el punto de entrada para cualquier lectura posterior.

Q2 — Distribución por sexo

Pregunta que responde
¿Cómo se distribuye la participación por sexo?

Qué devuelve

conteo y porcentaje por Sex

Valor analítico
Describe diversidad y composición básica del sistema,
sin evaluar rendimiento.

Q3 — Distribución por federación

Pregunta que responde
¿En qué federaciones compiten los atletas del país?

Qué devuelve

participaciones por Federation

distribución relativa

Valor analítico
Muestra estructura institucional del deporte
y heterogeneidad del origen de los datos.

Q4 — Tipos de eventos y equipamiento

Pregunta que responde
¿Qué tipos de eventos y categorías de equipamiento predominan?

Qué devuelve

distribución por Event

distribución por Equipment

Valor analítico
Aporta contexto competitivo.
Evita interpretar totales sin entender bajo qué reglas se compite.

Q5 — Participación a lo largo del tiempo

Pregunta que responde
¿Cómo evoluciona la participación a lo largo del tiempo?

Qué devuelve

participaciones por año

tendencia general (no predicción)

Valor analítico
Describe crecimiento, estabilidad o contracción del ecosistema.

Q6 — Distribución de categorías de peso

Pregunta que responde
¿Cómo se distribuyen los atletas por categorías de peso?

Qué devuelve

conteo por WeightClassKg

segmentación opcional por sexo

Valor analítico
Permite entender la composición corporal del sistema
sin evaluar rendimiento.

Q7 — Clases etarias

Pregunta que responde
¿Qué clases de edad participan y cómo se distribuyen?

Qué devuelve

distribución por AgeClass

registros con edad conocida vs desconocida

Valor analítico
Describe diversidad etaria y calidad del dato reportado.

Q8 — País de competencia

Pregunta que responde
¿Los atletas compiten principalmente a nivel nacional o internacional?

Qué devuelve

distribución por MeetCountry

proporción local vs internacional

Valor analítico
Agrega contexto competitivo sin comparar resultados.

Q9 — Condición del resultado (Place)

Pregunta que responde
¿Cómo se distribuyen los resultados administrativos de competencia?

Qué devuelve

conteo de Place (numérico, DQ, NS, G, DD)

Valor analítico
Permite entender:

cuántos registros no terminan en resultados válidos

calidad operativa del dataset

Q10 — Totales reportados (TotalKg)

Pregunta que responde
¿Qué proporción de participaciones reporta un total válido?

Qué devuelve

cantidad y porcentaje con TotalKg informado

distribución general de totales (sin ranking)

Valor analítico
Sirve para:

evaluar completitud del dato

preparar terreno para capas posteriores

5. Queries explícitamente fuera de esta capa

No se implementan en Capa 01:

rankings por atleta

comparaciones entre atletas

métricas normalizadas por peso corporal

eficiencia, Wilks, IPF Points

análisis de progresión

Estas queries pertenecen a capas analíticas posteriores.

6. Relación con testing y arquitectura

Cada query:

debe cumplir los tests definidos en 01_Data_Mounting_Tests.md

debe respetar el alcance definido en 01_Column_Selection_Criteria.md

no puede introducir columnas fuera del alcance

Cualquier cambio en una query:

puede romper tests

debe justificarse documentalmente

7. Criterio de cierre de esta etapa

La estrategia de queries se considera completa cuando:

todas las preguntas están explícitas

no hay queries “exploratorias”

el arquitecto puede implementar sin reinterpretar

Looker puede consumir estas salidas sin lógica adicional

Cierre (criterio PM)

Con este documento:

el SQL deja de ser creativo

la arquitectura deja de ser implícita

el proyecto entra en fase de ejecución disciplinada
