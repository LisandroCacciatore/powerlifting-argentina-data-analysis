# Análisis de datos aplicado al deporte  
_De la práctica en el gimnasio a la toma de decisiones con datos_

Este proyecto nace de una historia real.

Empezó en un salón de pilates, con cuatro amigos, dos kettlebells y muchas ganas de entrenar mejor.
No había métricas ni dashboards: había observación, prueba y error, y una pregunta constante:
¿cómo sé que esto está funcionando?

Años después, tras cerrar mi propio gimnasio, esa misma pregunta reaparece desde otro lugar.
Este repositorio existe para mostrar cómo la experiencia práctica,
el pensamiento analítico y el aprendizaje en Cloud pueden convivir en un mismo proyecto.

---

## 🎯 Objetivo del proyecto

Mostrar, de forma simple y replicable, cómo el análisis de datos puede:

- aportar contexto al rendimiento deportivo  
- ayudar a comparar, interpretar y comunicar resultados  
- transformar experiencia práctica en conocimiento transferible  

El foco no está en modelos complejos, sino en criterio, proceso y storytelling con datos.

---

## ☁️ Enfoque actual (Google Cloud)

El proyecto evolucionó hacia un enfoque Cloud para trabajar con volúmenes reales de datos
y aplicar buenas prácticas desde el inicio.

Arquitectura base actual:
CSV grandes → Google Cloud Storage → BigQuery (raw)

Se prioriza:
- control de costos  
- exploración segura  
- separación entre datos crudos y análisis  

---

## 📊 Dataset

Se utiliza el dataset público de OpenPowerlifting,
con un recorte específico de competidores de Argentina.

---

## 📁 Estructura del repositorio

- `slides/`  
  Material visual para charla introductoria (10–15 minutos).

- `pdf/`  
  Material descargable (4–6 páginas) que explica el enfoque paso a paso.

- `sql/`  
  Queries utilizadas para exploración y análisis en BigQuery.

- `visuals/`  
  Gráficos generados a partir del análisis.

- `docs/`  
  Documentación del proceso, decisiones técnicas y evolución del proyecto.

---

## 🚀 Estado del proyecto

MVP educativo y técnico en evolución.

El repositorio funciona como:
- material de formación  
- portfolio profesional  
- registro vivo del proceso de aprendizaje  

El énfasis está en cómo se piensa un problema,
no solo en el resultado final.

