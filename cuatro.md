
# 📷 Sentiment Visualization – Digital Camera Reviews

![Sentiment Heatmap](https://github.com/Tlaloxx/Tlaloxx/blob/main/cuatro.png)

**By Tlaloc Álvarez**  
🧰 Python · pandas · TextBlob · Matplotlib · Seaborn  

---

## Overview

This project analyzes customer reviews of a digital camera to uncover sentiment trends and business insights.

---

## Goals

- Transform raw text into sentiment scores  
- Visualize trends to support product strategy  
- Highlight areas for product and marketing improvements  

---

## Sample Data

```python
df = pd.read_csv("digital_camera_reviews.csv")
df = df.dropna(subset=['comment'])
df[['comment']].head()
```

---

## Sentiment Analysis

```python
from textblob import TextBlob
df['sentiment_score'] = df['comment'].apply(lambda x: TextBlob(str(x)).sentiment.polarity)
```

---

## Visualization

```python
# Heatmap and line plot of sentiment
# Blue gradient shows polarity intensity from -1 to +1
```

---

## Business Impact

- Detect dissatisfaction patterns for early resolution  
- Support marketing messages with real user feedback  
- Monitor sentiment changes over time  

---

[🔙 Back to Portfolio](https://github.com/Tlaloxx)

