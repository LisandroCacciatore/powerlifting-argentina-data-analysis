FASE 1: LIMPIEZA CONTROLADA

Objetivo de la fase

Transformar la data cruda en un dataset coherente, documentado y analizable, sin perder información relevante del dominio deportivo.

La limpieza no es automática: cada decisión tiene justificación explícita.

Qué hay en esta fase

Queries de limpieza y transformación

Documentación de decisiones tomadas

Definición de columnas “core” vs “contexto”

Propuesta de convenciones y estándares

Principios

No borrar información con significado potencial

Preservar intentos fallidos (valores negativos)

Diferenciar “dato faltante” de “dato no reportado”

Priorizar trazabilidad sobre perfección

Ejemplo de decisiones

Intentos negativos se conservan

Totales vacíos no se imputan

Algunas columnas se mantienen solo como contexto histórico

Se explicita qué federaciones reportan qué campos

Resultado esperado

Un dataset limpio pero honesto, listo para análisis exploratorio y comparativo, sin ocultar las limitaciones de la fuente.
