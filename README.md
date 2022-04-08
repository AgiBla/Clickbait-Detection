# Clickbait-Detection

Este proyecto utiliza el dataset [Webis-Clickbait-17](https://webis.de/data/webis-clickbait-17.html) para entrenar y evaluar un modelo BERT para la tarea de detección de clickbait en Tweets publicados por medios de comunicación.

## Resultados
Utilizamos la versión *base* de BERT para esta tarea, así como un modelo ya preentrenado en detección de noticias clickbait. Haremos *fine-tuning* en ambos modelos utilizando la partición de entrenamiento del dataset, y analizaremos si el utilizar un modelo ya preentrenado en otro dataset puede ser útil para mejorar los resultados.

Evaluamos nuestros modelos utilizando el evaluador oficial y comparamos nuestros resultados con los mejores resultados publicados en la competición (*goldfish*) como representante del estado del arte. Tabla completa de la competición [aquí](https://webis.de/events/clickbait-challenge/shared-task.html). Nuestros modelos son los siguientes:

* `BERT`: El modelo [BERT-Base-cased](https://huggingface.co/bert-base-cased) original fine-tuneado en el dataset de Webis.
* `BERT-CB-Org`: El modelo [BERT-Base-cased-clickbait](https://huggingface.co/elozano/bert-base-cased-clickbait-news) original sin fine-tunear en nuestro dataset.
* `BERT-CB`: El modelo [BERT-Base-cased-clickbait](https://huggingface.co/elozano/bert-base-cased-clickbait-news) fine-tuneado en el dataset de Webis.

En la siguiente tabla se comparan nuestros resultados en la partición *test* del dataset.

```
Dataset Stats (TEST)
Size: 18979
Clickbait: 4515      % 23.79
No-Clickbait: 14464  % 76.21
```

| Model | Accuracy	| Precision	| Recall | **F1 Score** |
| -------- | -------- | -------- | -------- | ----------- |
| *goldfish* | 0.876	| 0.739	| 0.742 | 0.741 |
| BERT | 0.859 | 0.702 | 0.71 | 0.706 |
| BERT-CB-Org | 0.773 | 0.623 | 0.113 | 0.191 |
| BERT-CB | 0.864 | 0.786 | 0.587 | 0.672 |

Puesto que únicamente hay dos clases, las métricas como la precisión no son muy fiables (clasificar todos los casos como *No-clickbait* daría una precisión del 76%). Por ese motivo utilizamos la métrica F1 para comparar resultados. Podemos ver cómo simplemente *fine-tuneando* un modelo BERT-Base podemos obtener unos resultados competitivos con aquellos publicados en la competición del 2017. Utilizar un modelo re-entrenado en otro dataset puede ser útil, pero en este caso no obtiene buenos resultados. *Fine-tunear* ese modelo mejora los resultados, pero se obtienen mejores resultados utilizando el modelo original.

Se podrían analizar las versiones BERT-Large para comprobar cómo afecta a los resultados el utilizar un modelo más grade; así como un modelo *uncased* que suele ser la norma en estos casos. También se podrían analizar otros modelos basados en la misma arquitectura de BERT en caso de disponer de más tiempo y recursos. Utilizar la información del título de las noticias en conjunto con el texto del Tweet podría ser interesante, puesto que ambos textos están fuertemente relacionados.

Puesto que el dataset incluye mayoritariamente casos clasificados como no-clickbait en el entrenamiento, los resultados obtenidos en los casos del *test* etiquetados como clickbait son notablemente peores. Esto indica que el modelo tiende a clasificar los casos como no-clickbait con mayor facilidad. Esto se podría solucionar añadiendo algún tipo de corrección al modelo, o igualando el número de casos para cada clase en el entrenamiento. 

El tamaño del dataset (19.000 ejemplos) es un poco pequeño para poder entrenar un modelo de este estilo. Una opción sería aumentar la partición de entrenamiento utilizando ejemplos de la partición test (que también incluye otros 19.000 ejemplos). El modelo resultante no sería valido a la hora de compararlo con otros modelos evaluados en este dataset, pero sería útil en caso de querer utilizarlo en casos de la vida real o en otros datasets.
