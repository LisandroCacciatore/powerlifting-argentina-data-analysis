flowchart TD
    A[Fuente de datos externa<br/>CSV / Export] --> B[Validación inicial<br/>estructura y formato]

    B -->|OK| C[Google Cloud Storage<br/>01_Raw]
    B -->|Error| B1[Registro de error<br/>Bitácora / Testing]

    C --> D[Ingesta a BigQuery<br/>Dataset RAW]

    D --> E[Selección de columnas<br/>Criterio de negocio]
    E --> F[Limpieza básica<br/>Phase 1 Cleaning]

    F --> G[Testing de datos<br/>consistencia y calidad]
    G -->|OK| H[Tablas CORE<br/>Phase 2 Core]
    G -->|Error| G1[Corrección / Ajustes<br/>SQL o fuente]

    H --> I[Estrategia de consultas<br/>SQL analítico]
    I --> J[Exploración y análisis<br/>Looker / SQL]

    J --> K[Documentación<br/>Insights y decisiones]
