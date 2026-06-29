# 🍜 Food Delivery Operations & Customer Analytics — EDA

A full-spectrum exploratory data analysis of **15,000 food delivery orders** across 3 city tiers — uncovering the operational, economic, and behavioural dynamics that define modern food delivery platforms.

> 📊 **[View the Interactive Analytics Report →](food-analytics-report.html)**

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Key Findings](#key-findings)
- [Analysis Sections](#analysis-sections)
- [Feature Engineering](#feature-engineering)
- [Top Correlations](#top-correlations)
- [Business Implications](#business-implications)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [Tech Stack](#tech-stack)

---

## Project Overview

This notebook performs a full-spectrum EDA on a food delivery operations dataset, treating every order as a data point in a complex system shaped by time, distance, weather, human behaviour, and economics. The analysis spans **14 sections** — from raw data quality assessment through customer segmentation and correlation analysis — and distils findings into actionable business insights.

| Dimension | Coverage |
|---|---|
| Temporal | Hourly, daily, and monthly order patterns |
| Operational | Distance, delivery time, delay rates, cancellations |
| Economic | Order value, fees, discounts, tips, promo impact |
| Partner | Experience curves, efficiency scores, rating distributions |
| Customer | Loyalty tiers, age segments, premium vs standard behaviour |
| Risk | Delays, cancellations, refund funnel analysis |
| Statistical | Full 18×18 correlation matrix, KDE distributions, density scatter |

---

## Dataset

**Source:** [Food Delivery Operations & Customer Analytics](https://www.kaggle.com/datasets/deepeshkansotia/food-delivery-operations-and-customer-analytics) — Kaggle

| Property | Value |
|---|---|
| Rows | 15,000 orders |
| Columns | 30 features |
| City tiers | 3 (Tier 1, 2, 3) |
| Temporal coverage | 24-hour, multi-month |
| Missing values | 150 nulls across 3 columns (median imputed) |

**Key columns:** `order_id`, `order_hour`, `order_day_of_week`, `order_month`, `customer_age`, `customer_loyalty_score`, `premium_customer_flag`, `delivery_distance_km`, `delivery_time_minutes`, `estimated_delivery_time`, `preparation_time_minutes`, `delivery_efficiency_score`, `traffic_level_score`, `weather_severity_score`, `city_tier`, `restaurant_rating`, `delivery_partner_rating`, `customer_rating`, `order_value`, `delivery_fee`, `discount_amount`, `tip_amount`, `final_amount_paid`, `promo_code_used`, `cancellation_flag`, `delayed_delivery_flag`, `refund_flag`, `delivery_partner_experience_years`, `number_of_items`

---

## Key Findings

### 🚚 Delivery Physics
- Distance–time correlation = **0.86** — the single strongest predictor of delivery duration in the dataset
- Each additional kilometre adds approximately **~2.5 minutes** to a delivery
- Severe weather inflates average delivery time by **12–15 minutes** vs mild conditions — a non-linear discontinuity at the severe threshold
- Delivery efficiency score shows strong negative correlations with distance (r = −0.64) and traffic (r = −0.44)

### 👨‍💼 Partner Performance
- Efficiency scores grow from **46.7 (Year 1) → 71.6 (Year 15)** — a 53% performance uplift over a career at +1.8 pts/year
- This experience curve cannot be replicated by routing optimisation alone; **partner retention is a strategic lever**
- Delivery time remains nearly flat across experience buckets — efficiency captures route quality and reliability, not just speed

### 💰 Economics
- Premium customers average **₹124 vs ₹110** for standard (+13%) but receive identical delivery times and delay rates — an unmet SLA differentiation opportunity
- Promo code usage does **not** negatively correlate with tip rate — promos attract engaged customers, not cheap ones
- Loyalty tiers (Bronze → Diamond) show **zero spend differentiation** at ₹113–114 across all tiers — the programme requires fundamental redesign

### 📊 Operations & Risk
- The operational funnel loses **27% of orders**: 13.4% to cancellations, 9.5% to delays, 4.1% to refunds — only 73.1% are fully clean
- Delay rates jump to **10.4% under severe weather** vs 8.9–9.5% under all other conditions — proactive ETA buffering is warranted
- Cancellation rates are near-identical across all three city tiers (13.2–13.6%) — the driver is platform-level, not geographic

### ⏰ Temporal
- Order volume is relatively uniform across hours with modest peaks; weekdays drive higher volume than weekends
- Monthly order counts and average order values show independent oscillation patterns with no strong co-movement

### ⭐ Ratings
- Customer ratings cluster at **4.0 universally** — across all segments, age groups, loyalty tiers, and city tiers
- This is a measurement artefact, not a satisfaction signal; the rating UX requires redesign to produce usable data

---

## Analysis Sections

| # | Section | Methods Used | Output |
|---|---|---|---|
| 1 | Library Imports & Aesthetics | matplotlib, seaborn, scipy, numpy, pandas · custom colourmap | — |
| 2 | Data Loading | kagglehub · KaggleDatasetAdapter | — |
| 3 | Data Overview | Schema inspection · null audit · .describe() with gradient styling | — |
| 4 | Cleaning & Feature Engineering | Median imputation · 8 derived features | — |
| 5 | Univariate Distributions | Histograms with mean/median overlays for 12 core metrics | `univariate.png` |
| 6 | Temporal Patterns | Hourly bar charts · violin plots · day-of-week · dual-axis monthly trend | `temporal.png` |
| 7 | Delivery Physics | KDE-coloured density scatter · efficiency by city tier · weather boxplots · experience curve · Day×Hour heatmap | `delivery_physics.png` |
| 8 | Order Economics | Stacked bar by city tier · premium scatter · loyalty KDE · promo grouped bars | `economics.png` / `economics2.png` |
| 9 | Ratings Observatory | Hexbin joint plot · overlapping KDE distributions · delay×promo rating heatmap | `ratings.png` |
| 10 | Operational Risk | Cancellation/delay/refund bars · order funnel waterfall · delay distribution · operational event pie | `risk.png` |
| 11 | Partner Experience | Area chart efficiency curves · delivery time curves · experience-bucket boxplots | `partner.png` |
| 12 | Customer Segmentation | Loyalty×spend bars · age-group normalised metrics · city-tier violin · premium KDE · risk-by-loyalty · age heatmap | `segmentation.png` |
| 13 | Correlation Galaxy | Full lower-triangle 18×18 Pearson heatmap · top-10 correlated pairs bar chart | `correlation.png` / `top_corr.png` |
| 14 | Key Takeaways | Narrative synthesis of all analytical findings | — |

---

## Feature Engineering

Eight derived features were created to enable deeper analysis beyond the raw columns:

| Feature | Formula | Purpose |
|---|---|---|
| `delivery_delay_minutes` | delivery_time − estimated_delivery_time | Quantifies under/over-performance vs ETA |
| `revenue_per_km` | final_amount_paid / delivery_distance_km | Unit economics normalised by the dominant cost driver |
| `tip_rate` | tip_amount / order_value | Normalised generosity signal independent of order size |
| `discount_rate` | discount_amount / order_value | Effective markdown depth per order |
| `age_group` | Binned: 18–25, 26–35, 36–45, 46–55, 56–65 | Customer lifecycle segmentation |
| `weather_bucket` | 5-bin cut on weather_severity_score | Reveals the severe-condition discontinuity |
| `loyalty_tier` | Bronze / Silver / Gold / Platinum / Diamond | Human-readable loyalty segmentation |
| `exp_bucket` | 1–3, 4–6, 7–10, 11–15 yrs | Partner career stage grouping |

---

## Top Correlations

| Pair | Pearson r | Direction |
|---|---|---|
| final_amount_paid ↔ order_value | **+0.883** | ↑ Strong positive |
| delivery_time_minutes ↔ delivery_distance_km | **+0.864** | ↑ Strong positive |
| delivery_efficiency_score ↔ delivery_time_minutes | **−0.819** | ↓ Strong negative |
| delivery_efficiency_score ↔ delivery_distance_km | **−0.636** | ↓ Moderate negative |
| delivery_efficiency_score ↔ traffic_level_score | **−0.435** | ↓ Moderate negative |
| delivery_efficiency_score ↔ partner_experience_years | **+0.425** | ↑ Moderate positive |
| delivery_time_minutes ↔ preparation_time_minutes | **+0.394** | ↑ Moderate positive |
| final_amount_paid ↔ discount_amount | **−0.340** | ↓ Moderate negative |
| delivery_efficiency_score ↔ weather_severity_score | **−0.336** | ↓ Moderate negative |

> The majority of off-diagonal values in the full 18×18 matrix are near zero — reflecting a clean, well-structured dataset where causal structure is legible.

---

## Business Implications

| Finding | Recommended Action |
|---|---|
| Distance is the #1 delivery time driver | Prioritise hyperlocal restaurant onboarding in dense zones — each km reduction saves ~2.5 min per order |
| 53% efficiency gap between new and experienced partners | Invest in retention; build mentorship and milestone incentive programmes — the curve compounds over a career |
| Premium customers receive identical SLAs to standard | Introduce SLA-based priority dispatch; quantify willingness-to-pay for guaranteed ETA window |
| ~27% of orders impacted by operational events | Build a pre-dispatch risk scoring model using distance, weather, time-of-day, and partner experience |
| Rating inflation at 4.0 universally | Redesign rating UX with multi-dimensional prompts and friction on 5-star to produce usable signal |
| Loyalty tier spending completely undifferentiated | Redesign with spend-linked thresholds and exclusive Platinum/Diamond perks; A/B test before rollout |
| Severe weather spikes delays non-linearly | Surface proactive weather ETA buffers in the customer app at order placement, not post-delay |

---

## Project Structure

```
food-analytics/
│
├── images/
│   ├── correlation.png
│   ├── delivery_physics.png
│   ├── economics.png
│   ├── economics2.png
│   ├── partner.png
│   ├── ratings.png
│   ├── risk.png
│   ├── segmentation.png
│   ├── temporal.png
│   ├── top_corr.png
│   └── univariate.png
│
├── food-delivery-operations-customer-analytic-eda.ipynb   ← Full analysis pipeline
├── food-analytics-report.html                             ← Interactive executive report
└── README.md
```

---

## How to Run

### Option A — Kaggle (recommended)

1. Open the notebook directly on Kaggle
2. Add the dataset: `deepeshkansotia/food-delivery-operations-and-customer-analytics`
3. Run all cells — no additional setup required

### Option B — Local

```bash
# Clone the repository
git clone https://github.com/Imran3285/food-analytics.git
cd food-analytics

# Install dependencies
pip install pandas numpy matplotlib seaborn scipy kagglehub

# Place Kaggle API credentials
# ~/.kaggle/kaggle.json (Linux/Mac) or %USERPROFILE%\.kaggle\kaggle.json (Windows)

# Launch notebook
jupyter notebook food-delivery-operations-customer-analytic-eda.ipynb
```

> **Note:** The dataset is loaded via `kagglehub` at runtime. A valid Kaggle API token is required for local execution.

---

## Tech Stack

| Layer | Tools |
|---|---|
| Data manipulation | `pandas` · `numpy` |
| Statistical analysis | `scipy.stats` — Pearson r, KDE |
| Visualisation | `matplotlib` · `seaborn` · `LinearSegmentedColormap` |
| Data loading | `kagglehub` · `KaggleDatasetAdapter` |
| Environment | Python 3.x · Jupyter Notebook |

---

## Author

**Muhammad Imran** — Data Analyst / Data Scientist

[LinkedIn](https://www.linkedin.com/in/imran3285) · [Kaggle](https://www.kaggle.com/imran3285) · [GitHub](https://github.com/Imran3285)

*Dataset credit: deepeshkansotia on Kaggle*
Done

You are out of free messages until 1:50 AM
