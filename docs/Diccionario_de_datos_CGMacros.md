# Diccionario de datos — Proyecto de hipoglucemia

| Variable | Descripción | Tipo de dato |
| --- | --- | --- |
| Participante | Identificador de cada persona del estudio. | Texto |
| Timestamp | Fecha y hora de cada medición. | Fecha y hora |
| Libre GL | Glucosa registrada por el sensor, expresada en mg/dL. | Numérico |
| METs | Intensidad de actividad física registrada por Fitbit. | Numérico |
| A1c PDL (Lab) | Hemoglobina glucosilada utilizada para seleccionar participantes. | Numérico |
| Glucosa_media | Promedio de glucosa durante los 60 minutos de observación. | Numérico |
| MET_medio | Promedio de actividad física durante los 60 minutos de observación. | Numérico |
| Hipoglucemia | Variable objetivo: 1 si comienza un episodio válido durante los siguientes 60 minutos; 0 si no se registra hipoglucemia. | Binario (0/1) |

## 1. Archivo bio.csv

45 participantes registrados en el archivo original.

| Variable original | Descripción | Unidad o tipo |
| --- | --- | --- |
| subject | Identificador del participante | Entero |
| Age | Edad | Años |
| Gender | Sexo registrado | Categórica |
| BMI | Índice de masa corporal | kg/m² |
| Body weight | Peso corporal | Probablemente libras |
| Height | Estatura | Probablemente pulgadas |
| Self-identify | Identificación étnica o racial declarada | Categórica |
| A1c PDL (Lab) | Hemoglobina glucosilada | % |
| Fasting GLU - PDL (Lab) | Glucosa en ayunas | mg/dL |
| Insulin | Insulina en laboratorio | Unidad por confirmar |
| Triglycerides | Triglicéridos | mg/dL |
| Cholesterol | Colesterol total | mg/dL |
| HDL | Colesterol HDL | mg/dL |
| Non HDL | Colesterol no HDL | mg/dL |
| LDL (Cal) | Colesterol LDL calculado | mg/dL |
| VLDL (Cal) | Colesterol VLDL calculado | mg/dL |
| Cho/HDL Ratio | Relación colesterol total/HDL | Sin unidad |
| Collection time PDL (Lab) | Hora de extracción de laboratorio | Hora |
| #1 Contour Fingerstick GLU | Primera glucemia capilar | mg/dL |
| Time (t) | Hora de la primera glucemia capilar | Hora |
| #2 Contour Fingerstick GLU | Segunda glucemia capilar | mg/dL |
| Time (t).1 | Hora de la segunda glucemia capilar | Hora |
| #3 Contour Fingerstick GLU | Tercera glucemia capilar | mg/dL |
| Time (t).2 | Hora de la tercera glucemia capilar | Hora |

## 2. Archivo CGMacros-003(1).csv

14.565 registros del participante 003.

| Variable original | Descripción | Unidad o tipo |
| --- | --- | --- |
| Unnamed: 0 | Índice de fila exportado | Entero |
| Timestamp | Fecha y hora de la medición | Fecha/hora |
| Libre GL | Glucosa del sensor FreeStyle Libre | mg/dL |
| Dexcom GL | Glucosa del sensor Dexcom | mg/dL |
| HR | Frecuencia cardíaca | Latidos/min |
| Calories (Activity) | Gasto energético asociado a la actividad | Probablemente kcal |
| METs | Intensidad de actividad física | Valor codificado, dividido por 10 en el análisis |
| Meal Type | Tipo de comida | Categórica |
| Calories | Energía de los alimentos consumidos | kcal |
| Carbs | Carbohidratos | Probablemente gramos |
| Protein | Proteínas | Probablemente gramos |
| Fat | Grasas | Probablemente gramos |
| Fiber | Fibra alimentaria | Probablemente gramos |
| Amount Consumed | Cantidad consumida | Unidad por confirmar |
| Image path | Ruta de la fotografía del alimento | Texto |
| Hipoglucemia | Variable objetivo: 1 si comienza un episodio válido en la hora siguiente; 0 si no se registra hipoglucemia |  |

## Notas metodológicas y alcance

- **Procedencia:** este diccionario transcribe las variables documentadas en `bio.csv` y `CGMacros-003(1).csv` y las variables derivadas descritas para el proyecto de hipoglucemia.
- **Unidad de análisis:** los registros de sensores son mediciones repetidas de una misma persona; las ventanas del modelo tampoco equivalen a pacientes independientes.
- **Ventanas y objetivo:** se utilizan 60 mediciones consecutivas (aproximadamente 60 minutos) para calcular `Glucosa_media` y `MET_medio`; las siguientes 60 mediciones se usan para determinar la etiqueta. Se considera episodio válido al menos 15 mediciones consecutivas inferiores a 70 mg/dL. La clase 0 exige ausencia de mediciones inferiores a 70 mg/dL en la hora futura; los casos ambiguos se excluyen.
- **Tratamiento de MET:** el código divide los valores originales de `METs` por 10 antes de utilizarlos. Esta transformación debe conservarse documentada para interpretar `MET_medio`.
- **Datos faltantes:** una variable presente en el archivo puede contener valores ausentes. En el proyecto se descartan ventanas con faltantes de glucosa o MET y discontinuidades temporales; no se interpolan las mediciones para crear predictores.
- **Unidades pendientes:** donde se indica «probablemente» o «por confirmar», la unidad no debe darse por verificada hasta consultar la documentación original de CGMacros.
- **Alcance de la comprobación:** las columnas de `CGMacros-003(1).csv` no demuestran por sí solas que todos los archivos individuales tengan idéntico esquema. Para un diccionario exhaustivo de todos los archivos, se debe comparar sus encabezados.
- **Uso de las variables:** en el modelo final se utilizan `Glucosa_media` y `MET_medio` como predictoras y `Hipoglucemia` como variable objetivo; las demás variables originales se documentan, pero no se incorporan como predictores.
