# Predicción de hipoglucemia asociada al ejercicio en personas con diabetes

## Descripción del proyecto

Proyecto de Aprendizaje Automático que analiza la relación entre la actividad física y la aparición de episodios de hipoglucemia en personas con diabetes tipo 2.

Se utiliza el dataset científico CGMacros, que contiene registros de glucosa, actividad física y otras variables fisiológicas.

## Datos utilizados

Para este análisis se seleccionaron 14 participantes con diabetes tipo 2.

El conjunto analizado contiene 218.535 observaciones, correspondientes a mediciones repetidas de esas 14 personas. Por lo tanto, las observaciones no representan pacientes independientes.

Los datos utilizados se encuentran en la carpeta `data/` del repositorio.

## Estructura del repositorio

```text
Parcial-de-Aprendizaje-Automatico/
├── data/
│   ├── CGMacros_14_CSV_GitHub/
│   │   └── 14 archivos CSV de participantes
│   ├── bio.csv
│   └── README.md
│__ docs/
│   ├── Descripcion_Dataset_CGMacros.md
│   └── Diccionario_de_datos_CGMacros.md
├── models/
├── notebooks/
│   └── hipoglucemia_ejercicio.ipynb
├── references/
├── reports/
├── .gitignore
└── README.md
```

## Diccionario de datos

[Consultar Diccionario de Datos CGMacros](docs/Diccionario_de_datos_CGMacros.md)


## Instrucciones para ejecutar la notebook

1. Ingresar al repositorio de GitHub.
2. Seleccionar **Code → Download ZIP**.
3. Descomprimir el repositorio completo, conservando la estructura de carpetas.
4. Abrir la notebook `hipoglucemia_ejercicio.ipynb`, ubicada en `notebooks/`, utilizando Jupyter Notebook o JupyterLab.
5. Verificar que estén instaladas las bibliotecas necesarias, entre ellas pandas, numpy, matplotlib y scikit-learn.
6. Reiniciar el kernel y ejecutar todas las celdas en orden mediante **Run All**.

La notebook utiliza rutas relativas a la estructura del repositorio, sin depender de una ubicación absoluta de la computadora del autor.

## Metodología

Se analiza la relación entre los valores de glucosa, la actividad física y la aparición de episodios de hipoglucemia.

Para reducir el riesgo de fuga de información, los conjuntos de entrenamiento y prueba se separan por participante.

El balanceo de clases se aplica exclusivamente al conjunto de entrenamiento. El escalado de variables se ajusta con los datos de entrenamiento.

Las asociaciones identificadas por el análisis no permiten establecer relaciones causales.

## Reproducibilidad

El repositorio incluye los archivos de datos y la notebook necesarios para reproducir el procedimiento de análisis.

La ejecución completa debe comprobarse reiniciando el kernel y ejecutando todas las celdas en orden, verificando que no se produzcan errores.

## Descripción y origen del dataset

Para este proyecto se utilizó **CGMacros**, un conjunto de datos de investigación que contiene registros de monitoreo continuo de glucosa y otras variables fisiológicas y de actividad física.

El dataset permite estudiar el comportamiento de la glucosa en relación con diferentes factores, incluida la actividad física.

Los archivos utilizados fueron:

- **bio.csv:** contiene información de los participantes, incluida la hemoglobina glucosilada (HbA1c).
- **CGMacros-XXX.csv:** contiene registros individuales de cada participante, con fecha y hora de medición, glucosa y otras variables disponibles.

### Variables utilizadas en el proyecto

| Variable | Descripción |
|---|---|
| `subject` | Identificador del participante |
| `A1c PDL (Lab)` | Hemoglobina glucosilada (HbA1c) |
| `Timestamp` | Fecha y hora del registro |
| `Libre GL` | Glucosa registrada en mg/dL |
| `METs` | Intensidad de actividad física expresada en equivalentes metabólicos |

### Selección y utilización de los datos

Se seleccionaron inicialmente **14 participantes con HbA1c superior al 6,4 %**.

Se analizaron 218.535 registros originales. Cuatro participantes no disponían de datos de actividad física expresados en MET, por lo que el análisis continuó con 10 participantes.

Para construir el modelo predictivo se utilizaron dos variables:

- **Glucosa media:** promedio de glucosa durante los 60 minutos previos.
- **MET medio:** promedio de intensidad de actividad física durante esos mismos 60 minutos.

A partir de los registros se construyeron ventanas de 120 minutos:

- **Primera hora:** obtención de las variables predictoras.
- **Segunda hora:** identificación de posibles episodios de hipoglucemia.

Se consideró un episodio válido cuando la glucosa permaneció por debajo de 70 mg/dL durante al menos 15 registros consecutivos de un minuto.

Después del control de calidad y la aplicación de los criterios de exclusión, se obtuvieron **1.213 ventanas válidas**, utilizadas para desarrollar y evaluar el modelo de regresión logística.

El objetivo fue investigar si la glucosa y la actividad física registradas durante una hora permiten anticipar episodios de hipoglucemia durante la hora siguiente.

## Análisis exploratorio de datos (EDA)

Se realizó un análisis descriptivo de las variables glucosa media y actividad física (MET), comparando las ventanas con y sin hipoglucemia.

Se calcularon la media y la mediana de ambas variables y se elaboraron dos diagramas de caja (*boxplots*) en un mismo gráfico.
<img width="834" height="344" alt="image" src="https://github.com/user-attachments/assets/5e718454-3a3b-46e6-895d-c3d857e94479" />


**Resultados:**

- Glucosa previa sin hipoglucemia: media de 133,41 mg/dL.
- Glucosa previa con hipoglucemia: media de 95,51 mg/dL.
- Actividad física sin hipoglucemia: MET medio de 1,77.
- Actividad física con hipoglucemia: MET medio de 1,59.

La glucosa previa presentó diferencias descriptivas más marcadas entre ambos grupos. La actividad física mostró diferencias menores y una importante superposición de valores.

Estas asociaciones no permiten establecer relaciones causales.

## Preprocesamiento de los datos

Se seleccionaron 14 participantes del dataset CGMacros con HbA1c superior al 6,4 %. De ellos, 10 disponían de datos de glucosa y actividad física.

Se realizaron las siguientes operaciones:

- Control de fechas, duplicados y valores faltantes.
- Ordenamiento cronológico de los registros.
- Construcción de ventanas consecutivas de 120 minutos, sin superposición.
- Cálculo de la glucosa media y actividad física media durante los primeros 60 minutos.
- Identificación de episodios de hipoglucemia durante los 60 minutos siguientes.
- Exclusión de ventanas incompletas, con interrupciones o que no cumplían los criterios establecidos.

Se consideró episodio válido una glucosa inferior a 70 mg/dL durante al menos 15 registros consecutivos de un minuto.

Se obtuvieron **1.213 ventanas válidas: 12 positivas y 1.201 negativas**.

## Entrenamiento del modelo

Se utilizó un modelo de regresión logística para clasificar las ventanas según la aparición de hipoglucemia.

La separación de datos se realizó por participante:

- Entrenamiento: 7 participantes, 867 ventanas originales.
- Prueba: 3 participantes, 346 ventanas.

Debido al desbalance de clases, se seleccionaron 6 ventanas positivas y 6 negativas para entrenar el modelo.

Se aplicó `StandardScaler` para escalar las variables, ajustándolo exclusivamente con los datos de entrenamiento.

La regresión logística se configuró con `max_iter=1000` y `random_state=42`.

El conjunto de prueba permaneció sin balancear.

## Evaluación del modelo

Se evaluó el modelo con las 346 ventanas de los tres participantes reservados para prueba.

| Métrica | Resultado |
|---|---|
| Exactitud de entrenamiento | 75 % |
| Exactitud de prueba | 69,9 % |
| Exactitud balanceada | 68,3 % |
| Sensibilidad para hipoglucemia | 66,7 % |
| Precisión para hipoglucemia | 3,8 % |
| F1-score para hipoglucemia | 0,07 |
| ROC-AUC | 0,731 |

**Matriz de confusión:**

- Verdaderos positivos: 4.
- Falsos negativos: 2.
- Verdaderos negativos: 238.
- Falsos positivos: 102.

El modelo identificó 4 de las 6 ventanas positivas del conjunto de prueba, pero produjo 102 falsos positivos.

## Conclusiones

La glucosa previa mostró una asociación descriptiva con la aparición posterior de hipoglucemia. La actividad física presentó diferencias menores entre los grupos analizados.

Aunque el modelo identificó parte de los episodios, la elevada cantidad de falsos positivos limita su utilidad predictiva.

El reducido número de episodios positivos también limita la interpretación de los resultados.

Se trata de un estudio académico y exploratorio que requiere una validación más amplia por participante y una evaluación independiente antes de considerar aplicaciones clínicas.
