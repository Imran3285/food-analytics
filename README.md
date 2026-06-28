# 🍜 Food Delivery Operations & Customer Analytics — EDA

> **A comprehensive exploratory data analysis of 15,000 food delivery orders across 3 city tiers, uncovering the operational, economic, and behavioural dynamics that define modern food delivery platforms.**

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Key Findings](#key-findings)
- [Analysis Sections](#analysis-sections)
- [Feature Engineering](#feature-engineering)
- [Tech Stack](#tech-stack)
- [Notebook Structure](#notebook-structure)
- [How to Run](#how-to-run)
- [Business Implications](#business-implications)

---

## Project Overview

This notebook performs a full-spectrum EDA on a food delivery operations dataset, treating every order as a data point in a complex system shaped by time, distance, weather, human behaviour, and economics. The analysis spans 14 sections — from raw data quality assessment through customer segmentation and correlation analysis — and distils findings into actionable business insights.

**Analytical scope:**

| Dimension | Coverage |
|---|---|
| Temporal | Hourly, daily, and monthly order patterns |
| Operational | Distance, delivery time, delay rates, cancellations |
| Economic | Order value, fees, discounts, tips, promo impact |
| Partner | Experience curves, efficiency scores, rating distributions |
| Customer | Loyalty tiers, age segments, premium vs. standard behaviour |
| Risk | Delays, cancellations, refund funnel analysis |
| Statistical | Full correlation matrix, KDE distributions, density scatter |

---

## Dataset

**Source:** [Food Delivery Operations & Customer Analytics](https://www.kaggle.com/datasets/deepeshkansotia/food-delivery-operations-and-customer-analytics) — Kaggle

| Property | Value |
|---|---|
| Rows | 15,000 orders |
| Columns | 30 features |
| City tiers | 3 (Tier 1, 2, 3) |
| Temporal coverage | 24-hour, multi-month |
| Missing values | 150 nulls across 3 columns (imputed) |

### Key columns

```
order_id, order_hour, order_day_of_week, order_month
customer_age, customer_loyalty_score, premium_customer_flag
delivery_distance_km, delivery_time_minutes, estimated_delivery_time
preparation_time_minutes, delivery_efficiency_score
traffic_level_score, weather_severity_score, city_tier
restaurant_rating, delivery_partner_rating, customer_rating
order_value, delivery_fee, discount_amount, tip_amount, final_amount_paid
promo_code_used, cancellation_flag, delayed_delivery_flag, refund_flag
delivery_partner_experience_years, number_of_items
```

---

## Key Findings

### 🚚 Delivery Physics
- **Distance ↔ delivery time correlation = 0.86** — distance is the single strongest predictor of delivery duration
- Each additional kilometre adds approximately **~4.8 minutes** to a delivery
- Severe weather inflates average delivery time by **12–15 minutes** vs mild conditions

### 👨‍💼 Partner Performance
- Partner efficiency scores grow from **46.7 (Year 1) → 71.6 (Year 15)** — a **53% performance uplift** over a career
- This experience curve cannot be replaced by routing optimisation alone; partner retention is a strategic lever

### 💰 Economics
- Premium customers place higher-value orders but **do not receive meaningfully faster service** — an unmet SLA differentiation opportunity
- Promo code usage reduces final payment but does **not negatively correlate with tip rate** — promos attract engaged customers
- Loyalty tiers show **minimal spend differentiation**, suggesting the current programme requires redesign

### 📊 Operations & Risk
- The operational funnel loses approximately **23% of orders** to cancellations (~13.4%), delays, and refunds combined
- Cancellation rates vary across city tiers — Tier 1 cities exhibit distinct patterns from Tier 2/3
- Customer ratings cluster tightly around **4.0 across all segments**, suggesting systemic rating inflation rather than genuine satisfaction signal

### ⏰ Temporal
- Peak order hours align with **lunch (12–13h) and dinner (19–21h)** windows
- Weekends drive elevated volume; delivery time distributions widen on high-volume days

---

## Analysis Sections

| # | Section | Methods Used |
|---|---|---|
| 1 | Library Imports & Global Aesthetics | `matplotlib`, `seaborn`, `scipy`, `numpy`, `pandas` |
| 2 | Data Loading | `kagglehub`, `KaggleDatasetAdapter` |
| 3 | Data Overview | Schema inspection, null audit, `.describe()` with gradient styling |
| 4 | Data Cleaning & Quality | Median imputation, 6-column feature engineering |
| 5 | Univariate Distributions | Histograms with mean/median overlays for 12 core metrics |
| 6 | Temporal Patterns | Hourly bar charts, violin plots, day-of-week bars, dual-axis monthly trend |
| 7 | Delivery Physics | Density scatter (KDE-coloured), efficiency by city tier, weather boxplots, experience↔efficiency trend, Hour×Day heatmap |
| 8 | Order Economics | Stacked bar by city tier, premium scatter, loyalty tier KDE, promo/premium grouped bars |
| 9 | Ratings Observatory | Hexbin joint plot, overlapping KDE distributions, delay×promo rating heatmap |
| 10 | Operational Risk | Cancellation/delay/refund bars, order funnel waterfall, delay distribution, operational event pie |
| 11 | Partner Experience | Area chart efficiency curves, delivery time decline curves, experience-bucket boxplots |
| 12 | Customer Segmentation | Loyalty×spend bars, age-group normalised radar substitute, city-tier violin, premium KDE, risk-by-loyalty, age heatmap |
| 13 | Correlation Galaxy | Full lower-triangle Pearson heatmap (18×18), top-10 correlated pairs bar chart |
| 14 | Key Takeaways | Narrative synthesis of all analytical findings |

---

## Feature Engineering

Six derived features were created to enable deeper analysis:

| Feature | Formula | Purpose |
|---|---|---|
| `delivery_delay_minutes` | `delivery_time - estimated_delivery_time` | Quantifies under/over-performance vs. ETA |
| `revenue_per_km` | `final_amount_paid / delivery_distance_km` | Unit economics per distance unit |
| `tip_rate` | `tip_amount / order_value` | Normalised customer generosity signal |
| `discount_rate` | `discount_amount / order_value` | Effective discount depth |
| `age_group` | Binned: 18–25, 26–35, 36–45, 46–55, 56–65 | Customer lifecycle segmentation |
| `weather_bucket` | 5-bin cut on `weather_severity_score` | Categorical weather severity for group analysis |
| `loyalty_tier` | Bronze / Silver / Gold / Platinum / Diamond | Human-readable loyalty segmentation |
| `exp_bucket` | 1–3, 4–6, 7–10, 11–15 yrs | Partner career stage grouping |

---

## Tech Stack

```python
# Core
pandas          # Data manipulation
numpy           # Numerical operations
scipy.stats     # Statistical tests, KDE, Pearson r

# Visualisation
matplotlib      # Base plotting, GridSpec, FuncFormatter
seaborn         # Heatmaps, violin plots, statistical visuals
matplotlib.colors.LinearSegmentedColormap  # Custom diverging colourmap

# Data Source
kagglehub       # Dataset loading via KaggleDatasetAdapter
```

**Python version:** 3.x  
**Figures exported:** `univariate.png`, `temporal.png`, `delivery_physics.png`, `economics.png`, `economics2.png`, `risk.png`, `partner.png`, `segmentation.png`, `correlation.png`, `top_corr.png`

---

## Notebook Structure

```
food-delivery-operations-customer-analytic-eda.ipynb
│
├── § 1   Library Imports
├── § 2   Data Loading (Kaggle)
├── § 3   Data Overview
├── § 4   Data Cleaning & Feature Engineering
├── § 5   Univariate Distributions         → univariate.png
├── § 6   Temporal Patterns                → temporal.png
├── § 7   Delivery Physics                 → delivery_physics.png
├── § 8   Order Economics                  → economics.png / economics2.png
├── § 9   Ratings Observatory
├── § 10  Operational Risk Dashboard       → risk.png
├── § 11  Partner Experience & Performance → partner.png
├── § 12  Customer Segmentation            → segmentation.png
├── § 13  Correlation Galaxy               → correlation.png / top_corr.png
└── § 14  Key Takeaways
```

---

## Project Structure

```
food-analytics/
│
├── images/
│   ├── correlation.png
│   ├── delivery_physics.png
│   ├── economics.png
│   ├── risk.png
│   ├── segmentation.png
│   ├── partner.png
│   ├── temporal.png
│   ├── univariate.png
│   ├── ratings.png
│   ├── top_corr.png
│   └── economics2.png
│
├── food-delivery-operations-customer-analytic-eda.ipynb
├── README.md
├── requirements.txt
└── LICENSE
```

---

## How to Run

### Option A — Kaggle (recommended)

1. Open the notebook directly on Kaggle
2. Add the dataset: `deepeshkansotia/food-delivery-operations-and-customer-analytics`
3. Run all cells — no additional setup required

### Option B — Local

```bash
# Clone or download the notebook
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# Install dependencies
pip install pandas numpy matplotlib seaborn scipy kagglehub

# Set up Kaggle API credentials
# Place kaggle.json in ~/.kaggle/ (Linux/Mac) or %USERPROFILE%\.kaggle\ (Windows)

# Launch notebook
jupyter notebook food-delivery-operations-customer-analytic-eda.ipynb
```

> **Note:** The dataset is loaded via `kagglehub` at runtime. A valid Kaggle API token (`~/.kaggle/kaggle.json`) is required for local execution.

---

## Business Implications

| Finding | Recommended Action |
|---|---|
| Distance is the #1 delivery time driver | Prioritise hyperlocal restaurant onboarding in dense delivery zones |
| 53% efficiency gap between new and experienced partners | Invest in partner retention; build structured mentorship or incentive programmes for high-experience partners |
| Premium customers don't receive faster deliveries | Introduce SLA-based priority queuing for premium tier |
| ~23% of orders lost to cancellations, delays, refunds | Build a real-time operational risk scoring model to flag at-risk orders pre-dispatch |
| Rating inflation around 4.0 | Redesign rating UX to include multi-dimensional feedback (food quality, packaging, punctuality) |
| Loyalty tier spending is undifferentiated | Redesign loyalty programme with spend-linked rewards and exclusive perks for Platinum/Diamond |
| Severe weather spikes delays by 12–15 min | Surface weather ETA adjustments in customer-facing app proactively |

---

## Author

**Muhammad Imran**  
Data Analyst / Data Scientist  
[LinkedIn](https://www.linkedin.com/in/imran00286/) · [Kaggle](https://www.kaggle.com/imran3285) · [GitHub](https://github.com/Imran3285)

---

*Dataset credit: [deepeshkansotia](https://www.kaggle.com/datasets/deepeshkansotia/food-delivery-operations-and-customer-analytics) on Kaggle*
