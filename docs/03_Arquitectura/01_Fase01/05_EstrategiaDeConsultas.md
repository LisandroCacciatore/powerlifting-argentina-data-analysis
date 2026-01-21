## Estrategia de Queries — Capa 01 (Análisis Descriptivo Base)

---

## 1. Propósito del documento

Este documento define **qué queries se implementan en la Capa 01**,  
**qué preguntas analíticas responden** y **qué resultados se espera obtener**.

Su objetivo es:

- traducir decisiones semánticas en consultas analíticas concretas  
- evitar queries exploratorias sin criterio previo  
- garantizar que cada query tenga una razón de existir  
- asegurar coherencia entre alcance, arquitectura y ejecución  

Este documento **no contiene SQL**.  
Gobierna el SQL que será escrito posteriormente.

---

## 2. Principios de diseño de queries (Capa 01)

Todas las queries de esta capa deben cumplir los siguientes principios:

- responder una **pregunta explícita y formulable**
- ser **descriptivas**, no evaluativas
- operar sobre datos **ya validados por el proceso de montaje**
- ser reutilizables en visualizaciones
- tener **bajo costo de ejecución**
- ser explicables a público no técnico

No se permiten en esta capa:

- lógica condicional compleja
- métricas compuestas o normalizadas
- comparaciones atleta vs atleta
- inferencias de rendimiento
- rankings competitivos

---

## 3. Nivel de operación de las queries

Las queries de la Capa 01 operan exclusivamente sobre:

- tablas analíticas (`analytics_base`) o vistas equivalentes
- datos ya limpiados, tipificados y filtrados según el alcance
- estructuras estables y documentadas

No se consulta directamente sobre tablas `raw`.

Los joins se limitan a los estrictamente necesarios para exposición descriptiva.

---

## 4. Queries previstas — listado estratégico

### Q1 — Volumen de participación

**Pregunta que responde**  
¿Cuántos atletas y participaciones existen en el dataset para Argentina?

**Qué devuelve**
- cantidad de atletas únicos  
- cantidad total de participaciones  

**Valor analítico**  
Define el tamaño del ecosistema.  
Es el punto de entrada para cualquier análisis posterior.

---

### Q2 — Distribución por sexo

**Pregunta que responde**  
¿Cómo se distribuye la participación por sexo?

**Qué devuelve**
- conteo por categoría de `Sex`  
- distribución porcentual  

**Valor analítico**  
Describe composición y diversidad básica del sistema,  
sin evaluar resultados deportivos.

---

### Q3 — Distribución por federación

**Pregunta que responde**  
¿En qué federaciones compiten los atletas del país?

**Qué devuelve**
- participaciones por `Federation`  
- distribución relativa  

**Valor analítico**  
Describe la estructura institucional del deporte  
y contextualiza la heterogeneidad del origen de los datos.

---

### Q4 — Tipos de eventos y equipamiento

**Pregunta que responde**  
¿Qué tipos de eventos y categorías de equipamiento predominan?

**Qué devuelve**
- distribución por `Event`  
- distribución por `Equipment`  

**Valor analítico**  
Aporta contexto competitivo.  
Evita interpretar resultados sin comprender bajo qué reglas se compite.

---

### Q5 — Participación a lo largo del tiempo

**Pregunta que responde**  
¿Cómo evoluciona la participación a lo largo del tiempo?

**Qué devuelve**
- participaciones por año  
- tendencia general (no predictiva)  

**Valor analítico**  
Describe crecimiento, estabilidad o contracción del ecosistema  
sin inferencias causales.

---

### Q6 — Distribución de categorías de peso

**Pregunta que responde**  
¿Cómo se distribuyen los atletas por categorías de peso?

**Qué devuelve**
- conteo por `WeightClassKg`  
- segmentación opcional por sexo  

**Valor analítico**  
Permite entender la composición corporal del sistema  
sin evaluar desempeño.

---

### Q7 — Clases etarias

**Pregunta que responde**  
¿Qué clases de edad participan y cómo se distribuyen?

**Qué devuelve**
- distribución por `AgeClass`  
- proporción de registros con edad conocida vs desconocida  

**Valor analítico**  
Describe diversidad etaria y calidad del dato reportado.

---

### Q8 — País de competencia

**Pregunta que responde**  
¿Los atletas compiten principalmente a nivel nacional o internacional?

**Qué devuelve**
- distribución por `MeetCountry`  
- proporción de competencias locales vs internacionales  

**Valor analítico**  
Agrega contexto competitivo  
sin comparar resultados deportivos.

---

### Q9 — Condición administrativa del resultado (Place)

**Pregunta que responde**  
¿Cómo se distribuyen los estados administrativos de competencia?

**Qué devuelve**
- conteo de valores de `Place`  
  (numérico, DQ, NS, G, DD)

**Valor analítico**  
Permite entender:
- cuántos registros no finalizan en resultados válidos  
- la calidad operativa del dataset  

---

### Q10 — Totales reportados (`TotalKg`)

**Pregunta que responde**  
¿Qué proporción de participaciones reporta un total válido?

**Qué devuelve**
- cantidad y porcentaje con `TotalKg` informado  
- distribución general de totales (sin ranking)  

**Valor analítico**  
Sirve para:
- evaluar completitud del dato  
- preparar terreno para capas analíticas posteriores  

---

## 5. Queries explícitamente fuera de esta capa

No se implementan en la Capa 01:

- rankings por atleta
- comparaciones directas entre atletas
- métricas normalizadas por peso corporal
- Wilks, Dots, IPF Points u otros scores
- análisis de progresión o mejora

Estas queries pertenecen a capas analíticas posteriores.

---

## 6. Relación con testing y arquitectura

Cada query definida en este documento:

- debe cumplir los tests establecidos en `01_Data_Mounting_Tests.md`
- debe respetar el alcance definido en `01_Column_Selection_Criteria.md`
- no puede introducir columnas fuera del alcance aprobado

Cualquier modificación en una query:

- puede invalidar tests existentes  
- debe justificarse documentalmente  

---

## 7. Criterio de cierre de esta etapa

La estrategia de queries se considera completa cuando:

- todas las preguntas están explícitamente definidas
- no existen queries “exploratorias”
- el arquitecto puede implementar sin reinterpretar
- Looker Studio puede consumir los resultados sin lógica adicional

---

## Cierre (criterio PM)

Con este documento:

- el SQL deja de ser creativo  
- la arquitectura deja de ser implícita  
- el proyecto entra en una fase de ejecución disciplinada  

La Capa 01 queda formalmente lista para implementación.
