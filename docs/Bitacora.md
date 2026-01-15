# Bitácora del proyecto

Este documento registra el proceso de construcción del proyecto,
incluyendo decisiones técnicas, cambios de enfoque y aprendizajes.

No busca justificar el resultado,
sino documentar el camino.

---

## Inicio del proyecto – enfoque local
El proyecto comenzó como un ejercicio de análisis exploratorio
utilizando herramientas locales (Python, Pandas),
con foco en entender el dataset y formular buenas preguntas.

Este enfoque permitió:
- comprender la estructura de los datos
- detectar limitaciones de escala
- definir qué preguntas eran relevantes

---

## Cambio de enfoque – adopción de Google Cloud
Se decide migrar el proyecto a Google Cloud
para trabajar con un entorno más realista y escalable.

Motivos:
- volúmenes de datos mayores
- reproducibilidad
- aprendizaje práctico de Cloud

---

## Configuración inicial en Cloud
Decisiones tomadas:

- Presupuesto mensual bajo con alertas.
- Límite de bytes por query en BigQuery.
- Uso de Cloud Storage como capa de staging.
- Separación clara entre datos raw y análisis.

Arquitectura inicial:
CSV → Cloud Storage → BigQuery (raw)

Estado:
Base preparada para exploración controlada de datos.
