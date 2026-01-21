    %% Fuentes
    A[Dataset OpenPowerlifting CSV] --> B[Cloud Storage]
    B --> C[BigQuery - Raw]
    C --> D[BigQuery - Stage]
    D --> E[BigQuery - Analytics Base]

    %% Queries Capa 01
    E --> Q1[Q1 - Volumen de participación]
    E --> Q2[Q2 - Distribución por sexo]
    E --> Q3[Q3 - Distribución por federación]
    E --> Q4[Q4 - Tipos de eventos y equipamiento]
    E --> Q5[Q5 - Participación a lo largo del tiempo]
    E --> Q6[Q6 - Distribución de categorías de peso]
    E --> Q7[Q7 - Clases etarias]
    E --> Q8[Q8 - País de competencia]
    E --> Q9[Q9 - Condición administrativa del resultado]
    E --> Q10[Q10 - Totales reportados (TotalKg)]

    %% Dashboards / Visualización
    Q1 --> F[Dashboard - Resumen general]
    Q2 --> F
    Q3 --> F
    Q4 --> F
    Q5 --> G[Dashboard - Evolución temporal]
    Q6 --> H[Dashboard - Composición corporal]
    Q7 --> I[Dashboard - Distribución etaria]
    Q8 --> J[Dashboard - Competencias internacionales]
    Q9 --> K[Dashboard - Calidad de datos]
    Q10 --> K
