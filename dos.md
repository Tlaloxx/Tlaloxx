# 📊 WhatsApp Business Reviews 2023 — Análisis de Frustraciones y Valoraciones de los Usuarios

**Autor:** [Tu nombre aquí]  
**Fuente del dataset:** [Kaggle — WhatsApp Business Reviews App Store](https://www.kaggle.com/datasets/kanchana1990/whatsapp-business-reviews-app-store)  
**Objetivo:** Analizar de forma visual y concisa qué aspectos **frustran** y cuáles **valoran** los usuarios de la app *WhatsApp Business*, usando datos de reseñas del App Store.

---

## 🧭 Introducción

Este análisis explora más de mil reseñas reales de *WhatsApp Business* publicadas en el App Store durante **2023–2024**.  
El propósito es identificar patrones de satisfacción e insatisfacción a través de **puntuaciones, versiones y palabras frecuentes** en las reseñas.

Preguntas guía:
- ¿Qué versiones generaron más frustración?
- ¿Qué palabras aparecen con mayor frecuencia en reseñas negativas y positivas?
- ¿Cómo ha evolucionado el sentimiento de los usuarios a lo largo del tiempo?

El análisis combina **gráficos claros** con una **narrativa simple**, ideal para proyectos educativos o demostraciones de análisis exploratorio (EDA).

---

## 🚀 Código completo del análisis

```python
# 📊 Análisis de reseñas — Qué frustra vs qué valoran los usuarios
# Dataset: WhatsApp Business Reviews App Store (Kaggle)
# Autor: [Tu nombre aquí]

# 1️⃣ Importar librerías
import pandas as pd
import matplotlib.pyplot as plt
import re
from collections import Counter

plt.style.use('seaborn-whitegrid')
pd.options.display.max_colwidth = 200

# 2️⃣ Cargar dataset
df = pd.read_csv("w_data.csv")
print("Tamaño del dataset:", df.shape)
print(df.head(3))

# 3️⃣ Convertir fechas y revisar rango temporal
df['date'] = pd.to_datetime(df['date'])
print("Rango de fechas:", df['date'].min(), "→", df['date'].max())

# 4️⃣ Reseñas por mes
reviews_per_month = df.set_index('date').resample('M')['score'].count()
reviews_per_month.plot(marker='o', color='teal')
plt.title("📅 Cantidad de reseñas por mes")
plt.ylabel("Número de reseñas")
plt.xlabel("Mes")
plt.show()

# 5️⃣ Promedio de score por versión
# Se extraen los números de versión para ordenarlas correctamente
df['version_num'] = df['version'].apply(lambda x: [int(i) for i in re.findall(r'\d+', str(x))])
df = df.sort_values(by='version_num')
version_scores = df.groupby('version')['score'].mean()

version_scores.plot(kind='bar', color='skyblue', figsize=(10,4))
plt.title("⭐ Puntuación promedio por versión")
plt.ylabel("Score promedio")
plt.xlabel("Versión")
plt.xticks(rotation=45)
plt.show()

# 6️⃣ Versiones con mejor y peor puntuación
print("🔝 Mejores versiones:")
print(version_scores.sort_values(ascending=False).head(5))
print("\n🔻 Peores versiones:")
print(version_scores.sort_values().head(5))

# 7️⃣ Separar reseñas positivas y negativas
neg_reviews = df[df['score'] <= 2]['title']
pos_reviews = df[df['score'] >= 4]['title']

# 8️⃣ Función para obtener las palabras más comunes
def top_words(series, n=10):
    words = " ".join(series.dropna()).lower()
    words = re.findall(r'\b[a-z]{3,}\b', words)
    return Counter(words).most_common(n)

neg_common = top_words(neg_reviews)
pos_common = top_words(pos_reviews)

# 9️⃣ Mostrar frustraciones y valoraciones más comunes
print("\n🔴 Frustraciones más comunes:")
for w, c in neg_common:
    print(f"{w}: {c}")

print("\n🟢 Valoraciones más comunes:")
for w, c in pos_common:
    print(f"{w}: {c}")

# 🔟 Gráfico comparativo de palabras más comunes
neg_df = pd.DataFrame(neg_common, columns=['word', 'count'])
pos_df = pd.DataFrame(pos_common, columns=['word', 'count'])

fig, axes = plt.subplots(1, 2, figsize=(12,4))
axes[0].barh(neg_df['word'], neg_df['count'], color='tomato')
axes[0].set_title("🔴 Frustraciones más comunes")
axes[1].barh(pos_df['word'], pos_df['count'], color='mediumseagreen')
axes[1].set_title("🟢 Valoraciones más comunes")
plt.tight_layout()
plt.show()
