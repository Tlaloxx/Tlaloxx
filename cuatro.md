
# Sentiment Heatmap – 50 User Comments (Digital Camera Amazon Reviews)

![Digital Camera Heatmap](https://github.com/Tlaloxx/Tlaloxx/blob/main/cuatro.png)

Este análisis muestra cómo podemos extraer valor de negocio a partir de comentarios de clientes.  
Se puede aplicar fácilmente a cualquier producto o servicio que recopile opiniones abiertas.

---

## Paso 1: Importar librerías

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from textblob import TextBlob
from matplotlib.colors import LinearSegmentedColormap

%matplotlib inline
```

---

## Paso 2: Cargar el dataset y mostrar los primeros valores

```python
df = pd.read_csv("digital_camera_reviews.csv")
df = df.dropna(subset=['comment'])  # Eliminar comentarios vacíos
df[['comment']].head()
```

---

## Paso 3: Análisis de sentimiento con TextBlob

```python
df['sentiment_score'] = df['comment'].apply(lambda x: TextBlob(str(x)).sentiment.polarity)
```

---

## Paso 4: Seleccionar 50 valores y dar forma para la visualización

```python
data = df['sentiment_score'].values[:50]
rows, cols = 5, 10

if len(data) < rows * cols:
    data = np.append(data, [np.nan] * (rows * cols - len(data)))

heat_matrix = np.reshape(data, (rows, cols))
```

---

## Paso 5: Crear el mapa de color

```python
blue_cmap = LinearSegmentedColormap.from_list(
    "gray_to_blue",
    ["#f5f5f5", "#90caf9", "#42a5f5", "#1565c0"],
    N=256
)
```

---

## Paso 6: Visualizar los resultados

```python
fig, axs = plt.subplots(2, 1, figsize=(12, 6), gridspec_kw={"height_ratios": [2, 1]}, constrained_layout=True)

sns.heatmap(
    heat_matrix,
    ax=axs[0],
    cmap=blue_cmap,
    vmin=-1,
    vmax=1,
    linewidths=0.5,
    linecolor='white',
    xticklabels=False,
    yticklabels=False
)
axs[0].set_title("Sentiment Heatmap – 50 Digital Camera Reviews", fontsize=13, weight='bold')

axs[1].plot(range(1, len(data)+1), data, marker='o', linestyle='-', color='#1976d2', linewidth=2)
axs[1].fill_between(range(1, len(data)+1), data, color="#bbdefb", alpha=0.5)
axs[1].axhline(0, color='gray', linestyle='--', linewidth=1)
axs[1].set_title("Sentiment Trend Over Time", fontsize=12)
axs[1].set_xlabel("Review Order")
axs[1].set_ylabel("Polarity Score")
axs[1].set_ylim(-1.1, 1.1)
axs[1].grid(True, linestyle='--', alpha=0.3)

plt.savefig("sentiment_heatmap.png", dpi=300, bbox_inches='tight')
plt.show()
```

---

## Conclusión: ¿Por qué es útil este análisis?

Este tipo de análisis de sentimiento permite a las empresas:
- Detectar problemas antes de que escalen, actuando proactivamente sobre comentarios negativos.
- Mejorar productos y servicios basados en emociones reales de los clientes.
- Apoyar decisiones en marketing y experiencia del usuario con datos claros y visuales.
- Convertir opiniones en texto en información estratégica accionable.

Como analista de datos, aportar este tipo de valor convierte las emociones en decisiones reales.  
Esto me permite contribuir directamente a la mejora continua de productos y la satisfacción del cliente.

---

[← Volver al portafolio](https://github.com/Tlaloxx)
