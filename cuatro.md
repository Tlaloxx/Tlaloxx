# 📷 Sentiment Visualization – Digital Camera Reviews

**By Tlaloc Álvarez**  
💼 *Role:* Data Analyst  
🧰 *Tools:* Python · pandas · TextBlob · Matplotlib · Seaborn

---

## 🧠 Overview

This project shows how to extract value from open-text reviews using **sentiment analysis**.  
We analyzed real customer feedback from a **digital camera** to understand user emotion and trends.

> **Why it matters:** Customer reviews hold insights. We turned emotion into data to guide business strategy.

---

## 🎯 Objectives

- Score customer sentiment from raw text  
- Spot patterns and spikes in negative feedback  
- Visualize emotion through simple, effective charts  
- Generate insights for product and marketing teams  

---

## 🖼 Product Context

![Digital Camera](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/Digital_Camera_20060313.jpg/800px-Digital_Camera_20060313.jpg)  
<sub>Example device – Wikimedia Commons</sub>

---

## 📄 Sample Data Preview

```python
df = pd.read_csv("digital_camera_reviews.csv")
df = df.dropna(subset=['comment'])
df[['comment']].head()
```

---

## 🧪 Sentiment Scoring

```python
from textblob import TextBlob
df['sentiment_score'] = df['comment'].apply(lambda x: TextBlob(str(x)).sentiment.polarity)
```

---

## 🔥 Visualization

```python
# First 50 scores → heatmap and line chart
# (custom blue colormap for clarity and tone)

sns.heatmap(...); plt.plot(...); plt.show()
```

---

## 💡 Business Insights

- Detect common user pain points from negative trends  
- Guide product fixes and feature improvements  
- Support marketing tone with real feedback  
- Spot early warning signs before complaints escalate  

---

## 🚀 What’s Next?

- Group by product features (battery, price, etc.)  
- Add topic modeling for deeper analysis  
- Build dashboard for real-time review tracking  
- Detect urgency and emotion levels  

---

## 🔁 Back to Home

[← Return to my GitHub Portfolio](https://github.com/Tlaloxx)
