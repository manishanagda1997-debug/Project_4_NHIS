# Flipkart Smartphone Price Analysis — EDA Project

## Project Overview

This project analyzes smartphone data from Flipkart to understand how different features influence price.

The focus is not only on numbers, but on patterns, how brands, features, and technology shape pricing across budget, mid, and premium segments.

> Which smartphone features influence price the most?

---

## Project Structure

```
 flipkart-smartphone-eda
 ┣ Flipkart_Smartphones_EDA.ipynb
 ┣ Processed_Flipdata.xlsx
 ┣ Flipkart_Smartphones_EDA.pptx
 ┗ README.md
 
```

## Dataset

| Detail | Value |
|---|---|
| Source | Flipkart smartphone listings |
| Total rows | 541 |
| Total columns | 11 |
| Price range | ₹920 — ₹80,999 |

**Original columns:**
`Unnamed:0`, `Model`, `Colour`, `Memory`, `RAM`, `Battery`, `Rear Camera`, `Front Camera`, `AI Lens`, `Mobile Height`, `Processor`, `Price`

---

## What This Project Does

### 1. Data Cleaning
- Fix column name typos (`Prize` - `Price`, `Battery_` - `Battery`, `Processor_` - `Processor`)
- Remove exact duplicate rows
- Handle zero values and missing camera entries

### 2. Feature Engineering
New features are built from the raw columns to make the analysis more useful:

| Feature |
|---|---|
| `camera_score` | Combines front and rear MP after normalization. Divided by camera count, so rear-only and front-only phones are not unfairly penalized |
| `camera_tier` | Groups phones into Low / Mid / High camera quality |
| `brand` | Extracted from model name. Sub-brands like Redmi, POCO, and Mi merged under Xiaomi |
| `proc_family` | Groups 123 unique processor names into families: Snapdragon, Dimensity, Helio, Exynos, Tensor, Bionic, Unisoc |
| `is_5g` | Binary flag — 1 if model name contains '5G' |
| `price_tier` | Budget (< ₹10k), Mid (₹10k–₹20k), Premium (> ₹20k) |
### Feature Trends by Price Segment

<p align="center">
  <img src="Feature averages by price tier.png" width="600">
</p>

<p align="center">
  RAM, storage, and camera increase clearly from budget to premium devices.
</p>

### 3. Outlier Detection
- IQR method applied to all numeric columns
- Outliers are classified as: real premium devices, data errors, or IQR limitation cases
- Only `Mobile Height` is treated — feature phones (< 10cm) and one data entry error (41.94cm) are removed
- All other outliers are retained with a proper justification
### Data Cleaning Impact

<p align="center">
  <img src="Outliers treatment - Mobile height.png" width="600">
</p>

<p align="center">
  Cleaning removes unrealistic values and keeps only valid smartphone data.
</p>

### 4. Univariate Analysis
Individual distribution of every feature, such as price, RAM, memory, battery, camera score, brand, processor family, 5G split, AI Lens, and price tier.

### 5. Bivariate Analysis
How each feature relates to price:
- RAM vs Price
- Storage vs Price
- Brand vs Price
- Processor Family vs Price
- 5G vs Price
- Camera Score vs Price
- Battery vs Price
- Mobile Height vs Price

### Brand Distribution

<p align="center">
  <img src="Top 10 smartphones brand.png" width="600">
</p>

<p align="center">
  A few brands dominate the dataset, especially Xiaomi and Realme.
</p>

### 6. Multivariate Analysis
- Correlation heatmap of all numeric features
- RAM × Processor Family × Price interaction
- Feature averages across price tiers
- 5G adoption by brand

### Feature Relationships

<p align="center">
  <img src="Feature correlation matrix.png" width="600">
</p>

<p align="center">
  Strong relationships exist between RAM, storage, and price, while battery and size show a weak impact.
</p>

### 7. Business Insights & Recommendations
Key findings and actionable advice for pricing strategy and marketing decisions.

---

## Key Insights

- Processor family shows strong influence on price. High-end processors like Bionic and Tensor appear in higher price ranges, while Helio and Unisoc appear in lower segments.  
- Brand plays a major role in pricing. Devices with similar specifications still show large price differences across brands.  
- RAM shows a clear and consistent relationship with price. Higher RAM devices generally fall in higher price ranges.  
- Camera score shows a moderate relationship with price. Better camera quality often comes with a higher price, but it is not the only factor.  
- Battery size does not show a strong impact on price. Most devices offer similar battery capacity across segments.  
- 5G devices show a higher average price. This is also linked to better overall specifications and positioning in mid and premium segments.  
- Mobile height does not affect price after cleaning incorrect entries.  

---

## Business Understanding

Price is not driven by a single feature. It reflects a combination of:
- Performance (processor + RAM)  
- Storage  
- Camera quality  
- Brand positioning  
- Technology like 5G  
Different brands follow different strategies based on their target segment.

---

## Recommendations

- Align processor choice with the target price segment  
- Combine camera, performance, and storage instead of focusing on one feature  
- Use 5G in mid and premium segments where customers expect it  
- Avoid over-investing in a single feature without overall balance  

---

## Tools & Libraries

| Tool | Purpose |
|---|---|
| `pandas` | Data loading, cleaning, feature engineering |
| `numpy` | Numerical operations |
| `matplotlib` | Plotting |
| `seaborn` | Statistical visualisation |
| `scipy` | Statistical checks |
| `re` | Regex for processor and camera text cleaning |

---

## How to Run

**1. Clone the repository**
```bash
git clone https://github.com/your-username/flipkart-smartphone-eda.git
cd flipkart-smartphone-eda
```

**2. Install dependencies**
```bash
pip install pandas numpy matplotlib seaborn scipy openpyxl
```

**3. Place the dataset**
Make sure `Processed_Flipdata.xlsx` is in the same folder as the notebook.

**4. Open the notebook**
```bash
Jupyter Notebook Flipkart_Smartphones_EDA.ipynb
```

**5. Run all cells**
```
Kernel → Restart & Run All
```

> Always use **Restart & Run All** — running cells out of order will give incorrect outputs because earlier cells build features that later cells depend on.

---

## Feature Engineering Decisions

### Camera Score
```
camera_score = (front_norm + rear_norm) / camera_count × 10
```
- Both cameras are normalized to 0–1 before combining to fix scale difference (rear goes up to 200MP, front only 60MP)
- Divided by `camera_count` (1 or 2), not always 2, so rear-only phones are judged on their rear camera alone
- AI Lens is excluded, because it has a negative correlation with price (r = -0.16) and appears mainly on budget devices
- Equal weights are used, as correlation-based weights would be overconfident given the dataset size and bias

### Why No Processor Score
Processor is a categorical feature. Price variation within each family is high; even two Snapdragon phones can differ by ₹30,000. A single score would introduce false assumptions. Grouping into families is more honest and more useful.

### Mobile Height Treatment
- 17 rows removed, not just as outliers, but also as out-of-scope entries:
- 15 feature phones (height < 10cm, no cameras, price < ₹4,000) — outside the scope of smartphone analysis
- 2 rows with height 41.94cm — confirmed data entry error (Motorola G62 5G actual height = 16.52cm)


---

## Author
Manisha Nagda  
GitHub: https://github.com/manishanagda1997-debug
