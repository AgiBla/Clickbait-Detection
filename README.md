# Clickbait-Detection

Este proyecto pretende utilizar el dataset [Webis-Clickbait-17](https://webis.de/data/webis-clickbait-17.html) para entrenar y evaluar un modelo BERT para detección de Tweets clickbait.

## Resultados
Utilizamos la versión *base* de BERT para esta tarea ([link](https://huggingface.co/bert-base-cased)), así como un modelo preentrenado para detección de noticias clickbait ([link](https://huggingface.co/elozano/bert-base-cased-clickbait-news)). Haremos *fine-tuning* utilizando la partición de entrenamiento del dataset, y analizaremos si utilizando un modelo ya preentrenado en otro dataset se pueden mejorar los resultados.

Evaluamos nuestros modelos utilizando el evaluador oficial y comparamos nuestros resultados con los mejores publicados en la competición (*goldfish*). Tabla completa [aquí](https://webis.de/events/clickbait-challenge/shared-task.html).

* `BERT`: BERT-Base-cased fine-tuneado en el dataset de Webis.
* `BERT-CB-Org`: BERT-Base-cased-clickbait original sin fine-tunear en nuestro dataset.
* `BERT-CB`: BERT-Base-cased-clickbait fine-tuneado en el dataset de Webis.

En la siguiente tabla se comparan nuestros resultados en la partición *test* del dataset.

```
Dataset Stats
Size: 18979
Clickbait: 4515      % 23.79
No-Clickbait: 14464  % 76.21
```

| Modelo | Accuracy	| Precision	| Recall | **F1 Score** |
| -------- | -------- | -------- | -------- | ----------- |
| *goldfish* | 0.876	| 0.739	| 0.742 | 0.741 |
| BERT | 0,859 | 0,702 | 0,71 | 0,706 |
| BERT-CB-Org | 0,773 | 0,623 | 0,113 | 0,191 |
| BERT-CB | 0,864 | 0,786 | 0,587 | 0,672 |

Puesto que únicamente hay dos clases, las métricas como la precisión no son muy fiables (clasificar todos los casos como *No-clickbait* daría una precisión del 76%). Por ese motivo utilizamos la métrica F1 para comparar resultados. Podemos ver cómo simplemente *fine-tuneando* un modelo BERT-Base podemos obtener unos resultados competitivos con aquellos publicados en la competición del 2017. Utilizar un modelo re-entrenado en otro dataset puede ser útil, pero en este caso no obtiene buenos resultados. *Fine-tunear* ese modelo mejora los resultados, pero se obtienen mejores resultados utilizando el modelo original.

Se podrían analizar las versiones BERT-Large para comprobar cómo afecta a los resultados el utilizar un modelo más grade; así como un modelo *uncased* que suele ser la norma en estos casos. También se podrían analizar otros modelos basados en la misma arquitectura de BERT en caso de disponer de más tiempo y recursos.
