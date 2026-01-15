Testing – Phase 01: Ingesta y Limpieza Controlada
Objetivo de la Fase 01

La Fase 01 tiene como objetivo transformar el CSV crudo de OpenPowerlifting en una base confiable, sin perder información relevante del dominio deportivo.

No se busca:

optimizar performance

crear métricas finales

simplificar el dataset en exceso

Sí se busca:

consistencia estructural

reglas de negocio mínimas

datos listos para análisis posterior

Alcance del Testing en esta fase

En Phase 01 se testea:

estructura del dataset

valores obligatorios

reglas básicas del dominio

coherencia mínima entre columnas

No se testea todavía:

rankings

comparaciones entre levantadores

agregaciones complejas

visualizaciones

Principio clave de esta fase

Nada entra a la capa de análisis si viola reglas básicas del dominio.

Los datos inválidos:

no se eliminan automáticamente

se identifican

se documentan

se excluyen explícitamente en capas posteriores

1️⃣ Tests de estructura (Schema)
Columnas obligatorias

Se valida la existencia y no-null de:

Name

Sex

Event

Equipment

Place

Federation

Date

MeetCountry

MeetName

📌 Motivo
Sin estas columnas no es posible interpretar el resultado deportivo.

Valores categóricos válidos

Se valida que:

Sex ∈ { M, F, Mx }

Event ∈ { SBD, BD, SD, SB, S, B, D }

Equipment dentro del set documentado

Place ∈ valores definidos (numérico o códigos válidos)

📌 Motivo
Evita errores silenciosos y categorías inventadas.

2️⃣ Tests de reglas de negocio (dominio)
TotalKg

Reglas:

TotalKg solo puede existir si hay 3 lifts válidos

si falta un lift o todos fallan → TotalKg debe ser NULL

📌 Motivo
Un total incompleto no representa rendimiento competitivo real.

Intentos negativos

Reglas:

valores negativos indican intentos fallidos

no se convierten a NULL

no cuentan como levantamientos exitosos

📌 Motivo
Preservar información para análisis de fallos y progresiones.

Relación Event ↔ Lifts

Se valida que:

eventos parciales no tengan lifts inexistentes

Event = B → Squat y Deadlift deben ser NULL

Event = D → Squat y Bench deben ser NULL

Event = S → Bench y Deadlift deben ser NULL

📌 Motivo
Evita contaminar análisis por evento.

Place y estado del levantador

Reglas:

Place = DQ, DD, NS ⇒ no deben tener puntos calculados

Guest (G) no entra en rankings competitivos

📌 Motivo
Separar rendimiento deportivo de registros administrativos.

3️⃣ Tests de coherencia interna
Best3 vs Intentos

Reglas:

Best3*Kg debe ser ≥ mejor intento exitoso

no puede ser menor que un intento válido previo

puede ser NULL si no hay intentos reportados

📌 Motivo
Evita inconsistencias históricas y errores de carga.

TotalKg = suma de Best3

Regla:

cuando existen los 3 Best3 → TotalKg = suma

📌 Motivo
Validación aritmética básica del resultado.

4️⃣ Tests de país (recorte Argentina)

Reglas:

MeetCountry = Argentina

no se infiere país por State

levantadores extranjeros compitiendo en Argentina sí se incluyen

📌 Motivo
El análisis es por país del evento, no nacionalidad del atleta.

5️⃣ Tests de intención analítica (manuales)

Estas validaciones no son automáticas y se revisan conscientemente:

¿Se están contando personas únicas o participaciones?

¿Se mezclan categorías etarias sin aclaración?

¿Se incluyen resultados no competitivos sin advertencia?

📌 Motivo
Evitar conclusiones incorrectas por ambigüedad.

Criterios de salida de Phase 01

La fase se considera válida cuando:

los tests de estructura no devuelven errores críticos

las violaciones de reglas están identificadas

los supuestos están documentados

el dataset está listo para agregaciones controladas

Qué se documenta como riesgo conocido

federaciones con carga incompleta de intentos

datos históricos con reglas distintas

inconsistencias heredadas del dataset original

Estos riesgos no bloquean la fase, pero se registran.

Resultado esperado

Una base de datos:

fiel al dominio del powerlifting

estructuralmente consistente

honesta sobre sus limitaciones

lista para análisis comparativos

Cierre

Phase 01 no busca perfección.

Busca confianza mínima.

Si no confiamos en los datos crudos,
no hay dashboard que lo arregle.
