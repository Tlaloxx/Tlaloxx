
# 💬 Sentiment Heatmap – 50 User Reviews (Digital Camera) [Go back to Portfolio](https://github.com/Tlaloxx)

**By Tlaloc Álvarez**  
📍 Mexico City | 💼 Role: Data Analyst  
🧰 Tools: Python · pandas · TextBlob · Matplotlib · Seaborn  

---

## 🧠 Overview

This project demonstrates how to extract **customer sentiment** from written feedback and transform it into meaningful visualizations.  
Although we analyze reviews for a **digital camera**, this approach is applicable to **any product or service** that receives open-text comments.

> **Why it matters:** Text reviews hide valuable insights. Analyzing tone and sentiment helps teams detect pain points, guide product improvements, and support data-driven decisions.

---

## 🖼️ Product Context

To visualize what kind of reviews we are analyzing, here’s a sample product:

![Digital Camera](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/Digital_Camera_20060313.jpg/800px-Digital_Camera_20060313.jpg)  
<sub>📸 Example image – Wikimedia Commons</sub>

---

## 🔍 Step 1: Load and Prepare the Data

We load the dataset, which includes customer comments, and remove empty ones.

```python
import pandas as pd

df = pd.read_csv("digital_camera_reviews.csv")
df = df.dropna(subset=['comment'])  # Remove empty comments
df[['comment']].head()
```

---

## 🧠 Step 2: Analyze Sentiment with TextBlob

Using `TextBlob`, we calculate a sentiment score for each comment.  
- Score ranges from **-1** (negative) to **+1** (positive)

```python
from textblob import TextBlob

df['sentiment_score'] = df['comment'].apply(lambda x: TextBlob(str(x)).sentiment.polarity)
```

---

## 🔢 Step 3: Prepare Data for Heatmap

We select the first 50 sentiment scores and reshape them for a 5x10 heatmap grid.

```python
import numpy as np

data = df['sentiment_score'].values[:50]
rows, cols = 5, 10

# Pad with NaNs if there are fewer than 50 values
if len(data) < rows * cols:
    data = np.append(data, [np.nan] * (rows * cols - len(data)))

heat_matrix = np.reshape(data, (rows, cols))
```

---

## 🎨 Step 4: Create a Custom Blue Colormap

We define a soft blue gradient for easier interpretation of tone.

```python
from matplotlib.colors import LinearSegmentedColormap

blue_cmap = LinearSegmentedColormap.from_list(
    "gray_to_blue",
    ["#f5f5f5", "#90caf9", "#42a5f5", "#1565c0"],
    N=256
)
```

---

## 📊 Step 5: Visualize with Heatmap and Trendline

We plot a heatmap (grid of reviews) and a line chart (sentiment over time).

```python
import seaborn as sns
import matplotlib.pyplot as plt

fig, axs = plt.subplots(2, 1, figsize=(12, 6), gridspec_kw={"height_ratios": [2, 1]}, constrained_layout=True)

# Heatmap
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

# Line Chart
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

## 💡 Business Value & Impact

This kind of analysis is simple but powerful:

- 🎯 Helps product teams **identify features that users love or dislike**
- 📢 Supports marketing with **real emotional tone for campaign alignment**
- ⚠️ Flags potential **quality issues early** from negative feedback trends
- 💬 Enhances **customer understanding** from raw feedback data

> 📌 This method can be applied to feedback from **apps, services, courses, events**, and more.

---

## 🗂 File Structure

```
sentiment-heatmap/
├── digital_camera_reviews.csv
├── sentiment_analysis.ipynb
├── sentiment_heatmap.png
└── README.md
```

---

## ⏭️ Ideas for Future Work

- Group reviews by **topics or product features**  
- Apply **topic modeling** (e.g., LDA or BERTopic)  
- Build an **interactive dashboard** with Streamlit  
- Include **subjectivity** for emotional depth  

---

## 🔁 Back to Portfolio

[← View All Projects](https://github.com/Tlaloxx)


