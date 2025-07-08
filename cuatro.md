# 📷 Sentiment Visualization – Digital Camera Reviews

**By Tlaloc Álvarez**  
💼 *Role:* Data Analyst  
🧰 *Tools:* Python · pandas · TextBlob · Matplotlib · Seaborn  

---

## 🧠 Overview

This project explores how to extract business value from customer feedback using **Natural Language Processing**.  
We analyzed real product reviews from a **digital camera** to understand user sentiment and visualize patterns.

> **Why it matters:** Open-text reviews hide insights. With the right analysis, we can detect dissatisfaction trends, improve product strategy, and support data-driven decision-making.

---

## 🎯 Objectives

- Convert unstructured feedback into quantifiable sentiment scores  
- Detect emotional trends and spikes in negative sentiment  
- Visualize user perception with intuitive heatmaps and line charts  
- Demonstrate value for product and marketing teams  

---

## 🖼 Product Context

To better understand the analysis, here’s the kind of product we're working with:

![Digital Camera](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/Digital_Camera_20060313.jpg/800px-Digital_Camera_20060313.jpg)  
<sub>📸 Example image for context – Wikimedia Commons</sub>

---

## 📄 Sample Data Preview

We start by loading the dataset and inspecting the comments:

```python
import pandas as pd

df = pd.read_csv("digital_camera_reviews.csv")
df = df.dropna(subset=['comment'])  # Remove empty comments
df[['comment']].head()
```

---

## 🧪 Sentiment Analysis

We used `TextBlob` to assign sentiment polarity scores to each review, ranging from **-1** (negative) to **+1** (positive).

```python
from textblob import TextBlob

df['sentiment_score'] = df['comment'].apply(lambda x: TextBlob(str(x)).sentiment.polarity)
```

---

## 🔥 Visualization

We visualized the first 50 reviews using both a heatmap and a trendline.

```python
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
from matplotlib.colors import LinearSegmentedColormap

# Select 50 sentiment scores
data = df['sentiment_score'].values[:50]
rows, cols = 5, 10
if len(data) < rows * cols:
    data = np.append(data, [np.nan] * (rows * cols - len(data)))
heat_matrix = np.reshape(data, (rows, cols))

# Create a custom colormap
blue_cmap = LinearSegmentedColormap.from_list(
    "gray_to_blue", ["#f5f5f5", "#90caf9", "#42a5f5", "#1565c0"], N=256
)

# Plot heatmap and line chart
fig, axs = plt.subplots(2, 1, figsize=(12, 6), gridspec_kw={"height_ratios": [2, 1]}, constrained_layout=True)

sns.heatmap(
    heat_matrix, ax=axs[0], cmap=blue_cmap, vmin=-1, vmax=1,
    linewidths=0.5, linecolor='white', xticklabels=False, yticklabels=False
)
axs[0].set_title("💬 Sentiment Heatmap – 50 Digital Camera Reviews", fontsize=13, weight='bold')

axs[1].plot(range(1, len(data)+1), data, marker='o', linestyle='-', color='#1976d2', linewidth=2)
axs[1].fill_between(range(1, len(data)+1), data, color="#bbdefb", alpha=0.5)
axs[1].axhline(0, color='gray', linestyle='--', linewidth=1)
axs[1].set_title("📈 Sentiment Trend Over Time", fontsize=12)
axs[1].set_xlabel("Review Order")
axs[1].set_ylabel("Polarity Score")
axs[1].set_ylim(-1.1, 1.1)
axs[1].grid(True, linestyle='--', alpha=0.3)

plt.savefig("sentiment_heatmap.png", dpi=300, bbox_inches='tight')
plt.show()
```

---

## 💡 Business Impact

- 🛠 Helps QA teams **detect defects or product pain points**
- 📢 Enables marketing to **adjust messaging based on real customer emotion**
- 📦 Informs product design by **monitoring changes in satisfaction over time**
- 📉 Early warnings on **negative spikes** before they escalate

---

## 🗂 File Structure

```
sentiment-visualization/
├── digital_camera_reviews.csv
├── sentiment_camera_analysis.ipynb
├── sentiment_heatmap.png
└── README.md
```

---

## 🚀 Future Improvements

- Group reviews by product features (e.g., battery, image, price)  
- Use **topic modeling** (e.g., LDA) for deeper insights  
- Build an **interactive dashboard** using Streamlit or Dash  
- Detect urgency or frustration using **subjectivity analysis**

---

## 🤝 Final Thoughts

Text analysis is not just technical—it's deeply **human**.  
It gives voice to the customer, and with the right lens, it can change how we build, sell, and serve.

> “If you listen closely, data tells stories.”
