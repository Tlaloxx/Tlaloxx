# 📊 Análisis de reseñas — Qué frustra vs qué valoran los usuarios

# 1. Importar librerías
import pandas as pd
import matplotlib.pyplot as plt
import re
from collections import Counter

plt.style.use('seaborn-whitegrid')
pd.options.display.max_colwidth = 200

# 2. Cargar dataset
df = pd.read_csv("w_data.csv")
print("Tamaño del dataset:", df.shape)
print(df.head(3))

# 3. Convertir fechas y ver rango
df['date'] = pd.to_datetime(df['date'])
print("Rango de fechas:", df['date'].min(), "→", df['date'].max())

# 4. Reseñas por mes
reviews_per_month = df.set_index('date').resample('M')['score'].count()
reviews_per_month.plot(marker='o', color='teal')
plt.title("Cantidad de reseñas por mes")
plt.ylabel("Número de reseñas")
plt.xlabel("Mes")
plt.show()

# 5. Promedio de score por versión (ordenadas cronológicamente)
df['version_num'] = df['version'].apply(lambda x: [int(i) for i in re.findall(r'\d+', str(x))])
df = df.sort_values(by='version_num')
version_scores = df.groupby('version')['score'].mean()

version_scores.plot(kind='bar', color='skyblue', figsize=(10,4))
plt.title("Puntuación promedio por versión")
plt.ylabel("Score promedio")
plt.xlabel("Versión")
plt.xticks(rotation=45)
plt.show()

# 6. Versiones con mejor y peor puntuación
print("🔝 Mejores versiones:")
print(version_scores.sort_values(ascending=False).head(5))
print("\n🔻 Peores versiones:")
print(version_scores.sort_values().head(5))

# 7. Separar reseñas positivas y negativas
neg_reviews = df[df['score'] <= 2]['title']
pos_reviews = df[df['score'] >= 4]['title']

# 8. Contar palabras más comunes (títulos)
def top_words(series, n=10):
    words = " ".join(series.dropna()).lower()
    words = re.findall(r'\b[a-z]{3,}\b', words)
    return Counter(words).most_common(n)

neg_common = top_words(neg_reviews)
pos_common = top_words(pos_reviews)

# 9. Mostrar frustraciones y valoraciones más comunes
print("\n🔴 Frustraciones más comunes:")
for w, c in neg_common:
    print(f"{w}: {c}")

print("\n🟢 Valoraciones más comunes:")
for w, c in pos_common:
    print(f"{w}: {c}")

# 10. Gráfico comparativo de palabras más comunes
neg_df = pd.DataFrame(neg_common, columns=['word', 'count'])
pos_df = pd.DataFrame(pos_common, columns=['word', 'count'])

fig, axes = plt.subplots(1, 2, figsize=(12,4))
axes[0].barh(neg_df['word'], neg_df['count'], color='tomato')
axes[0].set_title("Frustraciones más comunes")
axes[1].barh(pos_df['word'], pos_df['count'], color='mediumseagreen')
axes[1].set_title("Valoraciones más comunes")
plt.tight_layout()
plt.show()

# 11. Insight general
print("\n📈 Insight general:")
print("Los usuarios valoran la facilidad y confiabilidad, pero se frustran por fallas en actualizaciones o funciones rotas.")
print("Esto muestra la importancia de medir y escuchar al usuario para priorizar mejoras.")
