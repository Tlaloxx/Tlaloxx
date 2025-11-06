# 🎯 Análisis de Reseñas de Usuarios - Whatsapp Business Review (2023)
**Objetivo:** descubrir qué frustra vs qué valoran los usuarios del app.  
Dataset: 1,459 reseñas (score, texto, versión, país, fecha).


import pandas as pd, matplotlib.pyplot as plt, re
from collections import Counter

df = pd.read_csv("w_data.csv")
df['text'] = df['text'].fillna('').astype(str)
df['score'] = pd.to_numeric(df['score'], errors='coerce')


