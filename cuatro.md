
# Sentiment Heatmap – 50 User Comments (Digital Camera Amazon Reviews)

![Digital Camera Heatmap](https://github.com/Tlaloxx/Tlaloxx/blob/main/cuatro.png)

This analysis demonstrates how we can extract business value from customer feedback.  
It can be easily applied to any product or service that collects open-ended reviews.

---

## Step 1: Import libraries

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

## Step 2: Load dataset and preview first values

```python
df = pd.read_csv("digital_camera_reviews.csv")
df = df.dropna(subset=['comment'])  # Remove empty comments
df[['comment']].head()
```

---

## Step 3: Sentiment analysis using TextBlob

```python
df['sentiment_score'] = df['comment'].apply(lambda x: TextBlob(str(x)).sentiment.polarity)
```

---

## Step 4: Select 50 values and reshape for visualization

```python
data = df['sentiment_score'].values[:50]
rows, cols = 5, 10

if len(data) < rows * cols:
    data = np.append(data, [np.nan] * (rows * cols - len(data)))

heat_matrix = np.reshape(data, (rows, cols))
```

---

## Step 5: Create custom blue colormap

```python
blue_cmap = LinearSegmentedColormap.from_list(
    "gray_to_blue",
    ["#f5f5f5", "#90caf9", "#42a5f5", "#1565c0"],
    N=256
)
```

---

## Step 6: Visualize the results

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

## Conclusion: Why is this analysis useful?

This type of sentiment analysis allows companies to:
- Detect problems early and act proactively on negative feedback.
- Improve products and services based on real customer emotions.
- Support marketing and UX decisions with clear and visual data.
- Turn free-text feedback into strategic, actionable insights.

As a data analyst, delivering this kind of value turns emotion into business decisions.  
It enables me to directly support product improvement and customer satisfaction.

---

[← Back to portfolio](https://github.com/Tlaloxx)

