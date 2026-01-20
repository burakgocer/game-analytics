# Mobile Game User Behavior Analysis & Segmentation

This project presents a comprehensive case study on analyzing mobile game event data to understand user engagement, optimize performance metrics, and perform player segmentation using machine learning.

## 🚀 Project Purpose
The analysis aims to transform raw activity logs into actionable business intelligence. By calculating key performance indicators (KPIs) and applying clustering algorithms, we identify player personas and provide strategic recommendations for retention and monetization.

## 🛠 Methodology

### 1. Data Integration & Preparation
- **Unified Pipeline:** Consolidated 17 separate CSV files into a robust event-level dataset.
- **Data Cleaning:** Standardized timestamps, handled missing values, and ensured data integrity for long-term behavioral tracking.

### 2. Feature Engineering (User-Level Aggregation)
Raw events were transformed into a comprehensive user profile dataset including:
- **Engagement:** Total sessions, play duration, and session frequency.
- **Performance (Win Rate):** A custom metric derived from victories and total matches to assess skill levels.
- **Retention (Recency):** Calculated as `days_since_last_active` to monitor potential churn.
- **Loyalty (Inter-Visit Gap):** Measured the average time between sessions starting from the user's install date.

### 3. User Segmentation (K-Means Clustering)
Players were categorized into 4 distinct segments based on their interaction patterns:
- **Segment 1 (Core Players):** The most loyal and highly engaged group with the highest win rates.
- **Segment 2 (Regulars):** Consistent players with steady engagement and growth potential.
- **Segment 3 (High-Volume):** Active players with high match counts but varying efficiency.
- **Segment 0 (At-Risk):** Casual players with minimal activity and high churn risk (averaging **12.7 days** of inactivity).

> **Task 1 - a Conclusion:** Based on the analysis of engagement metrics and test variants, the **Winner Variant is [A/B - Burayı doldurun]**.

## 📊 Visualizations & Findings

### 1. User Metric Distributions
Understanding the spread of player activity across the platform.

![User Distributions](./screenshots/distributions.png)
*Recommended: Add histograms or boxplots of session counts, win rates, or total duration.*

### 2. Feature Correlations
Analyzing the relationship between player performance and engagement time.

![Correlation Analysis](./screenshots/correlation.png)
*Recommended: Add the Seaborn heatmap showing correlations between aggregated features.*

### 3. Segment Profiles
A clear comparison of the four player segments across key performance indicators.

![Segment Comparison](./screenshots/segments.png)
*Recommended: Add bar charts comparing the average values (Win Rate, Recency, etc.) for each cluster.*

## 📝 Key Assumptions
- User attributes like `country` and `platform` are assumed to be static based on the initial installation record.
- Inactivity of more than 10 days is treated as a high-risk churn indicator based on the observed distribution.

## 💻 Setup & Requirements
- **Language:** Python
- **Libraries:** Pandas, NumPy, Scikit-Learn, Matplotlib, Seaborn.
- **Structure:** Modular code design for reproducibility and clean analysis.

---
**Version:** v1.0  
**Status:** Completed Case Study
