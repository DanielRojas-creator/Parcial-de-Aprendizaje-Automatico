# Descripción del Dataset y Origen

**Proyecto:** Predicción de episodios de hipoglucemia mediante Aprendizaje Automático  
**Dataset:** CGMacros  
**Entorno de trabajo:** Anaconda – Jupyter Notebook – Python  
**Fecha:** 26 de octubre de 2026

## 1. Origen del dataset y lugar del estudio

El dataset CGMacros procede de una investigación científica sobre nutrición personalizada, monitoreo continuo de glucosa y actividad física.

El estudio se desarrolló en el Sansum Diabetes Research Institute (SDRI), ubicado en Santa Bárbara, California, Estados Unidos, donde fueron reclutados los participantes.

En la investigación participaron profesionales vinculados a diferentes instituciones, entre ellas Texas A&M University y Sansum Diabetes Research Institute.

Los participantes fueron reclutados entre 2021 y 2024.

Los resultados se publicaron el 25 de septiembre de 2025 en la revista científica *Scientific Data*, bajo el título:

*CGMacros: a pilot scientific dataset for personalized nutrition and diet monitoring.*

## 2. Participantes del estudio

La investigación incluyó 45 participantes adultos, de entre 18 y 69 años, clasificados según sus valores de hemoglobina glucosilada (HbA1c):

- 15 participantes sin diabetes.
- 16 participantes con prediabetes.
- 14 participantes clasificados con diabetes tipo 2.

Para el presente proyecto se seleccionaron los 14 participantes con diabetes tipo 2, utilizando como criterio un valor de HbA1c superior al 6,4 %.

Esta selección se realizó mediante Python y la biblioteca Pandas, utilizando la información clínica disponible en el archivo `bio.csv`.

## 3. Procedimiento de recopilación de datos

Los participantes fueron estudiados durante aproximadamente 10 días consecutivos, mientras realizaban sus actividades cotidianas.

Para recopilar la información se utilizaron diferentes dispositivos y herramientas:

- **Abbott FreeStyle Libre Pro:** monitoreo continuo de glucosa con mediciones cada 15 minutos.
- **Dexcom G6 Pro:** monitoreo continuo de glucosa con mediciones cada 5 minutos.
- **Fitbit Sense:** registro de actividad física, frecuencia cardíaca y gasto energético.
- **MyFitnessPal:** registro de comidas y composición nutricional.
- **WhatsApp:** fotografías de los alimentos consumidos.
- **Análisis de laboratorio:** determinaciones de HbA1c, glucosa, insulina, colesterol y triglicéridos, entre otras variables.

Los investigadores realizaron un procesamiento de los registros para integrar la información de los distintos dispositivos.

Las mediciones de glucosa fueron interpoladas linealmente para generar registros con intervalos uniformes de un minuto.

Además, las fechas originales fueron desplazadas para preservar la privacidad de los participantes.

## 4. Descripción general del dataset utilizado

El dataset CGMacros contiene registros temporales de glucosa, actividad física, alimentación y características clínicas.

Para nuestro proyecto se seleccionaron 14 archivos CSV, correspondientes a los participantes con diabetes tipo 2.

En conjunto, estos archivos contienen 218.535 registros, distribuidos de la siguiente manera:

| Participante | Registros | Columnas |
|---|---:|---:|
| CGMacros-003 | 14.565 | 15 |
| CGMacros-005 | 14.460 | 15 |
| CGMacros-012 | 14.655 | 14 |
| CGMacros-014 | 17.250 | 14 |
| CGMacros-028 | 18.735 | 13 |
| CGMacros-030 | 15.465 | 14 |
| CGMacros-035 | 14.400 | 14 |
| CGMacros-036 | 17.115 | 15 |
| CGMacros-038 | 14.505 | 14 |
| CGMacros-039 | 17.205 | 14 |
| CGMacros-042 | 14.520 | 14 |
| CGMacros-046 | 14.655 | 14 |
| CGMacros-047 | 15.690 | 14 |
| CGMacros-049 | 15.315 | 15 |
| **Total** | **218.535** | |

La cantidad de columnas varía entre 13 y 15 según el participante.

Cada registro representa una observación temporal de una persona. Por lo tanto, los 218.535 registros corresponden a mediciones repetidas de 14 participantes y no a 218.535 pacientes diferentes.

## 5. Variables y tipos de datos

Los archivos contienen variables relacionadas con el monitoreo de glucosa, la actividad física y la alimentación.

Como ejemplo, se describen las 15 columnas identificadas en el archivo CGMacros-003.

| Variable | Descripción | Tipo de dato |
|---|---|---|
| `Unnamed: 0` | Índice del archivo | Entero |
| `Timestamp` | Fecha y hora | Fecha/hora |
| `Libre GL` | Glucosa registrada por sensor Libre | Decimal |
| `Dexcom GL` | Glucosa registrada por sensor Dexcom | Decimal |
| `HR` | Frecuencia cardíaca | Decimal |
| `Calories (Activity)` | Calorías de actividad física | Decimal |
| `METs` | Equivalentes metabólicos | Entero |
| `Meal Type` | Tipo de comida | Texto |
| `Calories` | Calorías alimentarias | Decimal |
| `Carbs` | Carbohidratos | Decimal |
| `Protein` | Proteínas | Decimal |
| `Fat` | Grasas | Decimal |
| `Fiber` | Fibra alimentaria | Decimal |
| `Amount Consumed` | Cantidad consumida | Decimal |
| `Image path` | Ruta de imagen de comida | Texto |

Las variables presentan diferentes tipos de datos: numéricos, categóricos y temporales.

También existen valores faltantes en algunas columnas, debido a que no todas las variables presentan información en cada registro.

Por ejemplo, en el archivo CGMacros-003, que contiene 14.565 registros, se identificaron:

- **Libre GL:** 14.565 valores disponibles.
- **Dexcom GL:** 14.260 valores disponibles.
- **HR:** 14.478 valores disponibles.
- **Meal Type:** 35 valores disponibles.

Estas diferencias deberán considerarse durante las etapas posteriores de preparación de datos y entrenamiento del modelo.

## 6. Información clínica y demográfica

Además de los archivos individuales, se utiliza el archivo `bio.csv`, que contiene 24 columnas con información clínica y demográfica.

Entre las principales variables se encuentran:

- Edad y sexo.
- Peso, altura e índice de masa corporal (IMC).
- Hemoglobina glucosilada (HbA1c).
- Glucemia en ayunas.
- Insulina.
- Colesterol y triglicéridos.
- Otras variables del perfil lipídico.

La variable HbA1c permitió seleccionar los 14 participantes incluidos en el proyecto.

## 7. Aplicación del dataset al proyecto

El objetivo es desarrollar un modelo de Aprendizaje Automático que permita predecir episodios de hipoglucemia utilizando información previa de glucosa y actividad física.

Para este proyecto se define operativamente un episodio de hipoglucemia como un período durante el cual la glucosa permanece por debajo de 70 mg/dL durante al menos 15 minutos.

Las principales variables de interés son:

- Glucosa registrada mediante monitoreo continuo.
- Actividad física.
- Equivalentes metabólicos (METs).
- Información temporal de las mediciones.

Aunque el dataset contiene frecuencia cardíaca y variables alimentarias, estas no forman parte de las variables principales seleccionadas para el modelo.

El análisis permitirá estudiar la relación entre la actividad física y los episodios de hipoglucemia, para posteriormente desarrollar un modelo de clasificación.

## 8. Procesamiento inicial de los datos

El trabajo se realizó utilizando Anaconda, Jupyter Notebook, Python y la biblioteca Pandas.

El procedimiento inicial consistió en:

1. Cargar el archivo `bio.csv`.
2. Identificar a los participantes con HbA1c superior al 6,4 %.
3. Seleccionar los 14 participantes correspondientes.
4. Localizar y cargar sus archivos CSV individuales.
5. Examinar la cantidad de registros, columnas y tipos de datos.
6. Iniciar el análisis de episodios de hipoglucemia.

Este procedimiento se encuentra documentado en la notebook del proyecto.

## 9. Fuentes científicas

**Dataset original:**

CGMacros, versión 1.0.0. PhysioNet. Publicado el 28 de enero de 2025.

https://physionet.org/content/cgmacros/1.0.0/

DOI: https://doi.org/10.13026/3z8q-x658

**Artículo científico:**

Das, A., Kerr, D., Glantz, N., Bevier, W., Santiago, R., Gutierrez-Osuna, R. y Mortazavi, B. J. (2025).

*CGMacros: a pilot scientific dataset for personalized nutrition and diet monitoring.*

*Scientific Data*, 12, artículo 1557.

https://doi.org/10.1038/s41597-025-05851-7
