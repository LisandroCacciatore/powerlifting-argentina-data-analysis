Consultas en BigQuery
Esta carpeta contiene las queries SQL utilizadas para explorar y analizar el dataset de OpenPowerlifting alojado en BigQuery (Google Cloud).

Las consultas forman parte del proceso de entendimiento del dataset y sirven como base para visualizaciones y dashboards de situación actual.

Enfoque
El foco está en:

claridad
legibilidad
replicabilidad
No se prioriza optimización extrema, sino comprensión del proceso y de los datos.

Las queries están pensadas para ser leídas, explicadas y reutilizadas.

Tipos de consultas
Filtros por país (Argentina)
Distribuciones de levantamientos
Comparaciones por categoría y sexo
Métricas simples de rendimiento
Cada query responde a una pregunta concreta, definida antes de consultar los datos.

Relación con el proyecto
Estas consultas alimentan:

análisis exploratorios
gráficos en la carpeta visuals/
futuros dashboards de estado actual
El objetivo no es “sacar números”, sino generar contexto para tomar mejores decisiones.

Filosofía de trabajo
Antes de escribir una query:

Definir la pregunta
Entender la variable
Recién después, consultar los datos
El SQL es una herramienta. El objetivo es el criterio.
