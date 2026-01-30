# Arquitectura del proyecto

La arquitectura del proyecto está pensada para ser simple,
realista y escalable.

## Arquitectura actual

CSV grandes  
→ Google Cloud Storage (raw data)  
→ BigQuery (tablas raw y análisis)
→ LookerStudio (Visualización y presentación de datos)

## Principios

- Separar ingestión de análisis
- Proteger la exploración con límites
- Priorizar claridad sobre complejidad

La arquitectura puede evolucionar,
pero la base está diseñada para no rehacerse desde cero.
