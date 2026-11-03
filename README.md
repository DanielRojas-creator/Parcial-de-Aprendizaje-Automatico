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
