# People Analytics — IBM
¿Qué factores explican que un empleado abandone la empresa?

---

## ¿Cuál era el problema de negocio?

IBM gestionaba una plantilla de miles de empleados con un problema que los datos de RRHH no terminaban de explicar: por qué ciertos perfiles abandonaban la empresa y otros no.

En este caso práctico, asumí el rol de analista de RRHH utilizando el dataset público IBM HR Analytics para responder una pregunta concreta: **¿Qué factores explican que un empleado abandone la empresa?**

Antes de diseñar cualquier estrategia, hay que saber quién se va y por qué.

---

## ¿Qué datos usé y de dónde salieron?

Dataset público de Kaggle — [IBM HR Analytics Employee Attrition & Performance](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset).

**1.470 empleados · 35 variables originales**

De las 35 variables originales se descartaron tres por ser constantes (`EmployeeCount`, `StandardHours`, `Over18`) y el resto por baja relevancia para la pregunta de negocio.

Variables seleccionadas:

| Dimensión | Variables |
|---|---|
| Objetivo | `Attrition` |
| Posición en la empresa | `Department` `JobRole` |
| Condiciones del puesto | `OverTime` `BusinessTravel` |
| Trayectoria | `YearsAtCompany` |
| Calculada | `Tenure Bracket` |

---

## ¿Qué herramienta usé y por qué?

Usé Power BI.

- Es la herramienta de visualización y análisis más extendida en el entorno empresarial, especialmente en contextos de RRHH y reporting.
- Permite construir dashboards interactivos —a diferencia de Excel o Python, que ofrecen gráficos estáticos— con filtros que se actualizan en tiempo real.
- El foco del proyecto es traducir los datos en insights visuales — Power BI es la herramienta idónea para ese objetivo.

---

## Proceso analítico

**Paso 1 — Carga y selección de variables**
- Carga del archivo Excel original sin alterar los datos base. Siempre a través de *Transformar datos*.
- De 35 variables originales se seleccionan 7: tres se descartan por ser constantes y no aportar información analítica (`EmployeeCount`, `StandardHours`, `Over18`), el resto por baja relevancia para la pregunta de negocio. Menos variables bien elegidas generan más claridad que muchas variables mal justificadas.

**Paso 2 — Variable calculada: `Tenure Bracket`**
- Los años de antigüedad por sí solos no tienen significado operativo en RRHH. Se crea `Tenure Bracket` segmentando `YearsAtCompany` en cuatro tramos que representan momentos distintos del ciclo de vida del empleado: Early (0-2 años), Growing (3-5), Established (6-10) y Veteran (11+).
- `YearsAtCompany` se mantiene en el modelo — `Tenure Bracket` depende de ella.

**Paso 3 — Medidas DAX**
- A diferencia de columnas calculadas, las medidas DAX se recalculan dinámicamente según el contexto: qué filtros están activos, qué slicer ha seleccionado el usuario, qué segmento se está visualizando.
- Tres medidas: Total Empleados (`COUNTROWS`), Empleados Rotados (`CALCULATE` filtrando Attrition = "Yes") y Tasa de Rotación (`DIVIDE` de las dos anteriores, formateado como porcentaje).

**Paso 4 — Dashboard**
- La parte superior concentra los tres KPIs globales (total de empleados, empleados que abandonaron y tasa de rotación) para que cualquier persona que abra el dashboard entienda la magnitud del problema antes de ver cualquier gráfico.
- El centro muestra cinco gráficos de barras, uno por dimensión: Department, JobRole, OverTime, Tenure Bracket y BusinessTravel. Cada uno muestra la tasa de rotación por categoría.
- Los tres slicers interactivos (Department, OverTime, JobRole) permiten cruzar dimensiones en tiempo real: al activar un filtro, todos los gráficos se actualizan simultáneamente.

---

## Resultados

La tasa de rotación global es del 16,12% (237 de 1.470 empleados) — ligeramente por encima del benchmark del sector tecnológico (~15%). El dato agregado no es una cifra de alarma, pero esconde una concentración muy clara: el problema no está repartido uniformemente, está localizado en perfiles y condiciones específicas.

### 1. Sales Representative — el rol más vulnerable

**~40% de tasa de rotación** — casi 1 de cada 2 Sales Representatives abandona la empresa.

El rol con mayor fuga de talento del dataset. Un comercial que se va no solo implica el coste de reemplazo — implica pérdida de cartera de clientes y relaciones construidas durante meses. Es el predictor con mayor impacto potencial sobre el negocio.

### 2. Horas extra — el predictor más claro

**~30% de rotación con OverTime vs ~10% sin OverTime** — los empleados con horas extra tienen tres veces más probabilidad de abandonar.

Es el indicador de desgaste más directo del dataset. Las horas extra sostenidas en el tiempo no son una causa aislada — son la señal de un problema estructural de carga de trabajo o de dimensionamiento de plantilla.

### 3. Early Tenure (0–2 años) — el periodo crítico

**~30% de rotación en los primeros dos años** — la franja de menor antigüedad concentra la mayor fuga.

Cuando alguien se va en ese periodo, el problema rara vez está en el salario o el plan de carrera. Suele estar en la gestión de expectativas, el onboarding o la falta de acompañamiento inicial — factores que se pueden intervenir antes de que se conviertan en baja.

### 4. Viajes frecuentes — desgaste por desplazamiento

**~30% con Travel_Frequently vs ~10% con Non-Travel** — viajar frecuentemente triplica la tasa de rotación.

Es el predictor menos visible pero uno de los más potentes. Los desplazamientos frecuentes no suelen aparecer como motivo explícito de baja — pero los datos lo confirman como uno de los factores con mayor correlación con el abandono.

---

## Acciones recomendadas

**Sales Representative — revisión urgente**
Analizar si la banda salarial está por debajo del mercado, si la carga de trabajo es sostenible y si las expectativas del rol están bien gestionadas desde la selección. Métrica de seguimiento: tasa de rotación trimestral por rol.

**Horas extra — gestión de la carga**
Compensar económicamente las horas extra o revisar el dimensionamiento de plantilla. Las horas extra crónicas son un síntoma, no una solución. Métrica de seguimiento: % de empleados con OverTime activo por departamento.

**Primeros 2 años — programa de acompañamiento**
Implementar mentoring estructurado y revisiones de satisfacción periódicas durante los primeros 24 meses. El objetivo es detectar señales de riesgo antes de que se conviertan en baja. Métrica de seguimiento: tasa de rotación en Early Tenure trimestre a trimestre.

**Viajes frecuentes — revisión de política**
Analizar qué desplazamientos son realmente necesarios y compensar adecuadamente los que no se pueden eliminar. Métrica de seguimiento: tasa de rotación por categoría de BusinessTravel antes y después de aplicar cambios.

---

## Limitaciones

El dataset no incluye variable de fecha. Si los registros corresponden a periodos distintos, comparar la tasa global con un benchmark anual concreto (~15% sector tecnológico) sería metodológicamente incorrecto — se estarían mezclando realidades temporales diferentes. Se asume que todos los registros pertenecen a un mismo periodo, pero no se puede verificar.

---

## Archivos del repositorio

```
.xlsx    → Excel de los datos
.pbix    → dashboard Power BI
README.md → documento del proceso
```
