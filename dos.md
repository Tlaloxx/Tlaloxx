# 📊 WhatsApp Business Reviews — Qué valoran y qué frustra a los usuarios

**Dataset:** [Kaggle — WhatsApp Business Reviews App Store (2023)](https://www.kaggle.com/datasets/kanchana1990/whatsapp-business-reviews-app-store)  
**Tamaño:** 1,459 reseñas  
**Columnas:** id, date, userName, userUrl, version, score, title, text, url, country, appId  

---

## 🧭 Objetivo del análisis

Este análisis busca descubrir **qué valoran** y **qué frustra** a los usuarios de *WhatsApp Business* analizando reseñas del App Store.  
A través de texto y puntuaciones, se identificarán patrones simples con gráficos claros y observaciones prácticas.  

---

## 1️⃣ Importar librerías necesarias

Se importan las librerías base para análisis de datos y visualización.
````python
import pandas as pd
import matplotlib.pyplot as plt
import re
from collections import Counter

plt.style.use('seaborn-whitegrid')
pd.options.display.max_colwidth = 200

````python
df = pd.read_csv("whatsapp_reviews.csv")
print("Tamaño del dataset:", df.shape)
df.head(2)
