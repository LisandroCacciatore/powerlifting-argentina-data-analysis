flowchart TD
    A[Fuente de datos<br/>CSV / Export] --> B[Test de estructura<br/>schema, tipos, headers]

    B -->|Falla| B1[Rechazo de dataset<br/>Log + Bitácora]
    B -->|OK| C[Test de volumen<br/>filas esperadas]

    C -->|Falla| C1[Alerta de inconsistencias<br/>fuente o corte]
    C -->|OK| D[Carga controlada<br/>GCS 01_Raw]

    D --> E[Test de integridad<br/>nulls, duplicados]
    E -->|Falla| E1[Corrección<br/>reglas o limpieza]
    E -->|OK| F[Ingesta a BigQuery<br/>RAW]

    F --> G[Test de columnas clave<br/>criterio de negocio]
    G -->|Falla| G1[Revisión de criterio<br/>documentación]
    G -->|OK| H[Limpieza básica<br/>Phase 1 Cleaning]

    H --> I[Test de consistencia lógica<br/>rangos, reglas]
    I -->|Falla| I1[Ajustes SQL / reglas]
    I -->|OK| J[Tablas CORE<br/>Phase 2 Core]

    J --> K[Test de consultas<br/>performance y coherencia]
    K -->|Falla| K1[Optimización SQL]
    K -->|OK| L[Análisis y exploración<br/>uso analítico]

    L --> M[Documentación final<br/>decisiones e insights]
