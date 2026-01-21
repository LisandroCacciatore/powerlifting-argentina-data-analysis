# Mi Flujo de Datos OpenPowerlifting

Aquí puedes ver el diagrama de arquitectura y consultas:

```mermaid
flowchart TD
    %% --- Estilos ---
    classDef rawStage fill:#f9f,stroke:#333,stroke-width:1px;
    classDef analytics fill:#bbf,stroke:#333,stroke-width:1px;
    classDef query_general fill:#aaffaa,stroke:#333,stroke-width:1px;
    classDef query_temporal fill:#ffd699,stroke:#333,stroke-width:1px;
    classDef query_compos fill:#ffbbbb,stroke:#333,stroke-width:1px;
    classDef query_calidad fill:#d0d0d0,stroke:#333,stroke-width:1px;
    classDef dashboard fill:#fff2cc,stroke:#333,stroke-width:2px,stroke-dasharray: 5 5;

    %% --- Fuentes y Montaje ---
    A[CSV Dataset OpenPowerlifting]:::rawStage --> B[Cloud Storage]:::rawStage
    B --> C[BigQuery - Raw]:::rawStage
    C --> D[BigQuery - Stage]:::rawStage
    D --> E[BigQuery - Analytics Base]:::analytics

    %% --- Queries ---
    E --> Q1[Q1 - Volumen de participación]:::query_general
    E --> Q2[Q2 - Distribución por sexo]:::query_general
    E --> Q3[Q3 - Distribución por federación]:::query_general
    E --> Q4[Q4 - Tipos de eventos y equipamiento]:::query_general
    E --> Q5[Q5 - Participación a lo largo del tiempo]:::query_temporal
    E --> Q6[Q6 - Distribución de categorías de peso]:::query_compos
    E --> Q7[Q7 - Clases etarias]:::query_compos
    E --> Q8[Q8 - País de competencia]:::query_general
    E --> Q9[Q9 - Condición administrativa del resultado]:::query_calidad
    E --> Q10[Q10 - Totales reportados (TotalKg)]:::query_calidad

    %% --- Dashboards ---
    Q1 --> F[Dashboard - Resumen general]:::dashboard
    Q2 --> F
    Q3 --> F
    Q4 --> F
    Q5 --> G[Dashboard - Evolución temporal]:::dashboard
    Q6 --> H[Dashboard - Composición corporal]:::dashboard
    Q7 --> H
    Q8 --> F
    Q9 --> I[Dashboard - Calidad de datos]:::dashboard
    Q10 --> I
