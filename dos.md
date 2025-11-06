# 📊 WhatsApp Business Reviews — Qué valoran y qué frustra a los usuarios

**Fuente:** https://www.kaggle.com/datasets/kanchana1990/whatsapp-business-reviews-app-store  
**Dataset:** `w_data.csv` (1,459 reseñas)  
**Objetivo:** identificar rápidamente qué frustra y qué valoran los usuarios usando análisis simple y visual.

---

INSTRUCCIONES: copia todo este texto (es Markdown). En tu repo GitHub pega este archivo como `mini_analysis.md` o como contenido de una celda Markdown en un notebook. Cada bloque de código está marcado como ` ```python ` para que sea fácilmente copiable.

---

## 1) Librerías y configuración
_Explicación:_ importamos pandas y matplotlib; configuramos estilo para gráficos y ancho de columna para visualizar texto largo.

```python
import pandas as pd
import matplotlib.pyplot as plt
import re
from collections import Counter

```

---

## 2) Cargar datos y vista rápida
_Explicación:_ cargamos el CSV y mostramos tamaño y primeras filas para confirmar la estructura.

```python
df = pd.read_csv("w_data.csv")
print("Tamaño del dataset:", df.shape)
df.head(2)
```

---

## 3) Fechas y rango temporal
_Explicación:_ convertimos `date` a datetime para poder agrupar por mes y detectar picos.

```python
df['date'] = pd.to_datetime(df['date'], errors='coerce')
print("Rango de fechas:", df['date'].min(), "→", df['date'].max())
```

---

## 4) Reseñas por mes (serie temporal)
_Explicación:_ vemos la actividad por mes; picos suelen indicar lanzamientos o regresiones.

```python
reviews_per_month = df.set_index('date').resample('ME')['score'].count().fillna(0)
plt.figure(figsize=(9,3))
plt.plot(reviews_per_month.index, reviews_per_month.values, marker='o')
plt.title("Reseñas por mes")
plt.ylabel("Número de reseñas")
plt.xlabel("Mes")
plt.tight_layout()
plt.show()
```

---

## 5) Distribución de puntuaciones
_Explicación:_ histograma simple para ver si predominan reseñas positivas o negativas.

```python
plt.figure(figsize=(6,3))
counts = df['score'].value_counts().sort_index()
plt.bar(counts.index.astype(int), counts.values, color=['tomato','tomato','gold','seagreen','seagreen'])
plt.title("Distribución de calificaciones")
plt.xlabel("Score")
plt.ylabel("Cantidad")
plt.tight_layout()
plt.show()
```

---

## 6) Promedio de score por versión
_Explicación:_ detectar versiones con baja satisfacción (posibles regresiones).

```python
# ordenar versiones por sus números (si contienen números)
def parse_version(v):
    nums = re.findall(r'\d+', str(v))
    return tuple(int(x) for x in nums) if nums else (0,)

df['version_sort'] = df['version'].apply(parse_version)
version_scores = df.groupby('version')['score'].mean().sort_values()
# gráfico
plt.figure(figsize=(10,3.5))
version_scores.plot(kind='bar', color='skyblue')
plt.title("Puntuación promedio por versión")
plt.ylabel("Score promedio")
plt.xlabel("Versión")
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
```

---

## 7) Versiones: top 3 mejores y peores
_Explicación:_ imprimimos las 3 versiones con peor y mejor score promedio para priorizar investigación.

```python
print("Peores versiones (avg score):")
print(version_scores.head(3))
print("\nMejores versiones (avg score):")
print(version_scores.tail(3))
```

---

## 8) Separar reseñas positivas y negativas
_Explicación:_ definimos negativas (score ≤2) y positivas (score ≥4) para comparar lenguaje.

```python
neg = df[df['score'] <= 2]['text'].fillna('').astype(str)
pos = df[df['score'] >= 4]['text'].fillna('').astype(str)
print("Negativas:", len(neg), "| Positivas:", len(pos))
```

---

## 9) Palabras más frecuentes (simple, sin NLP pesado)
_Explicación:_ tokenizamos por regex y contamos palabras (removiendo stopwords básicos). Esto resalta temas como "sound", "crash", "update" vs "easy", "useful".

```python
def top_words(texts, n=12):
    text = " ".join(texts).lower()
    text = re.sub(r'http\S+','', text)
    words = re.findall(r'\b[a-z]{3,}\b', text)
    stop = {'the','and','for','with','that','this','from','your','have','been','just','very','app','its','not','but','you','are','was','will'}
    words = [w for w in words if w not in stop]
    return Counter(words).most_common(n)

neg_words = top_words(neg)
pos_words = top_words(pos)
print("Top palabras - negativas:", neg_words[:10])
print("Top palabras - positivas:", pos_words[:10])
```

---

## 10) Gráficos comparativos: frustraciones vs valoraciones
_Explicación:_ mostramos dos barras horizontales para que el público vea visualmente las palabras clave que indican frustración y las que indican valoración.

```python
import pandas as pd

neg_df = pd.DataFrame(neg_words, columns=['word','count']).head(10)
pos_df = pd.DataFrame(pos_words, columns=['word','count']).head(10)

fig, axes = plt.subplots(1,2, figsize=(12,4))
axes[0].barh(neg_df['word'], neg_df['count'], color='tomato')
axes[0].set_title("Frustraciones más comunes")
axes[0].invert_yaxis()
axes[1].barh(pos_df['word'], pos_df['count'], color='mediumseagreen')
axes[1].set_title("Valoraciones más comunes")
axes[1].invert_yaxis()
plt.tight_layout()
plt.show()
```

---

```
from wordcloud import WordCloud

neg_text = " ".join([w for w, _ in neg_words])
pos_text = " ".join([w for w, _ in pos_words])

neg_wc = WordCloud(width=600, height=400, background_color='white', colormap='Reds').generate(neg_text)
pos_wc = WordCloud(width=600, height=400, background_color='white', colormap='Greens').generate(pos_text)

plt.figure(figsize=(12,5))
plt.subplot(1,2,1)
plt.imshow(neg_wc, interpolation='bilinear')
plt.axis('off')
plt.title("🔴 Frustraciones más comunes")

plt.subplot(1,2,2)
plt.imshow(pos_wc, interpolation='bilinear')
plt.axis('off')
plt.title("🟢 Valoraciones más comunes")
plt.show()
```

---


