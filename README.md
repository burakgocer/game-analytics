# Case-1 — A/B Test Simulation & Strategic Scenario Comparison

This project analyzes an A/B test on a game difficulty flow using a simulation-based approach.  
The goal is to understand how two variants (A and B) perform over time in terms of:

- **Retention dynamics**,  
- **Daily Active Users (DAU)**, and  
- **Total monetization under different strategic scenarios.**

Rather than relying only on observed retention points, the analysis fits a parametric retention model and projects outcomes up to Day 30 under multiple what-if settings.

---

## Business Questions

The analysis answers the following questions:

1. Which variant has more **Daily Active Users on Day 15?**  
2. Which variant earns more **total revenue by Day 15?**  
3. Does the winner change when looking at **Day 30 revenue?**  
4. What happens if we run a **10-day temporary sale (Day 15–25)?**  
5. What if a **new user source** is introduced on Day 20 with different retention dynamics?  
6. If only one improvement can be made, which should be prioritized:  
   - a temporary sale, or  
   - a permanent new user source?

---

## Modeling Approach

### 1) Retention Modeling

Observed retention points were available at D1, D3, D7, and D14 for both variants.

To estimate daily retention between these points, an exponential decay model was fitted:
R(d) = R1 · exp(−k · (d − 1))

This allowed us to:

- interpolate retention for every day from 1 to 30,  
- compute cohort survival accurately, and  
- derive realistic DAU curves over time.

---

### 2) DAU Calculation

For each day: 
DAU(d) = daily_installs × sum of retention of all active cohorts


This means Day-15 DAU reflects the **combined survivors of 15 daily cohorts**, not just the D15 retention rate.

---

### 3) Revenue Simulation

Daily revenue consisted of two components:

**Ad Revenue**
DAU × ads_per_DAU × eCPM / 1000

**IAP Revenue (assumed)**
DAU × purchase_ratio × avg_purchase_value


Total revenue was accumulated day by day up to Day 30.

---

## Key Findings

### Day 15 Results

- **DAU winner:** Variant **B**  
  - Lower initial retention, but **slower decay** → more active users from older cohorts.

- **Revenue winner by Day 15:** Variant **A**  
  - Higher early retention + higher ad impressions per user created a short-term advantage.

---

### Day 30 Results

- By Day 30, **Variant B overtakes Variant A in total revenue.**  
- The reason is its **stronger long-term retention (D14: 9% vs 6%)** combined with better monetization efficiency.

Lesson:  
> Short-term wins do not guarantee long-term value.

---

## 10-Day Sale Scenario (Day 15–25)

A temporary 1% uplift in purchase ratio was simulated.

**Outcome:**
- The sale increases total revenue for both variants.
- However, **Variant B remains the winner by Day 30.**

This suggests that retention advantages dominate short-term purchase boosts.

---

## New User Source (Day 20 Onwards)

From Day 20 onward:

- 12,000 users/day from the original source  
- 8,000 users/day from a new source with different retention curves

Under this mixed-source setting:

- **Variant B again generates more total revenue by Day 30.**
- Better long-term retention compounds even more with fresh users.

---

## Strategy Prioritization (Part f)

We compared two strategic options:

1. **Run a 10-day sale (temporary uplift)**  
2. **Add a new permanent user source**

### Decision rule:
Compare total company revenue (A + B) under each strategy versus baseline.

**Winner: New User Source Mix**

Why?

- The sale creates a **one-time spike**,  
- The new source creates **sustained incremental value over time**.

---

## Visualization (Single Key Chart)

![Strategy comparison](screenshots/strategy-comparison.png)

This bar chart compares **30-day total revenue** under three scenarios:

- Baseline (no change)  
- 10-Day Sale  
- New User Source Mix  

It visually summarizes the strategic trade-off in a single, clear figure.

---

## Conclusion

This case demonstrates that:

- Retention modeling is critical for A/B evaluation,  
- Early wins can be misleading,  
- Long-term retention dominates short-term monetization tweaks, and  
- Structural user growth (new source) is often more valuable than temporary promotions.

---

## Author

**Burak Gocer**  
**2026-01-20**





# Case-2 — Player Analytics, PCA & Behavioral Segmentation

This project presents an end-to-end analytics study based on multi-period game telemetry data that includes:

- transition from event-level to user-level data,  
- distribution-focused feature transformations,  
- correlation analysis,  
- dimensionality reduction via Principal Component Analysis (PCA), and  
- behavior-based player segmentation using K-Means clustering.  

The objective is not only to build a model, but to **conceptualize player behavior and generate actionable segments.**

---

## Business Problem

This analysis addresses the following key questions:

- What behavioral signals best explain player engagement?  
- Are technical issues (server errors) an independent behavioral dimension?  
- Is recency a reliable indicator of churn risk?  
- How many distinct behavioral player groups exist?  
- What product and marketing actions should be taken for each segment?

---

## Data Source

The raw dataset consists of daily event records per user, including:

**Gameplay Behavior**
- `total_session_count`  
- `total_session_duration`  
- `match_start_count`  
- `match_end_count`  
- `victory_count`  
- `defeat_count`  

**Technical Signals**
- `server_connection_error`  

**Monetization**
- `iap_revenue`  
- `ad_revenue`  

**Time & Profile Attributes**
- `event_date`  
- `install_date`  
- `country`  
- `platform`  

A total of **17 separate CSV files** were merged into a single unified event table.

---

## Event-Level to User-Level Transformation

Event data was aggregated at the `user_id` level using:

| Variable Type | Aggregation Method |
|----------------|--------------------|
| Behavioral metrics | **sum** |
| Static attributes | **first** |

A key derived metric was created:


This represents player skill, in-game performance, and potential loyalty.

---

## Recency & Inter-Visit Gap

### Recency  

- Higher value → higher churn risk  
- Lower value → stronger engagement  

### Average Inter-Visit Gap  
Calculated as:
- time from install to first event  
- plus the average gap between consecutive events  

Interpretation:
- High gap → irregular player  
- Low gap → frequently returning player  

---

## Distribution Analysis (Histograms)

The distributions of all numerical features were examined and found to be highly skewed.  
Therefore, specialized transformations were applied before PCA.

![Feature distributions](screenshots/hist.png)

---

## Feature Transformation Strategy (Pre-PCA)

### 1) Custom Binning (Zero-Inflated Variables)
Applied to:
- `iap_revenue`  
- `ad_revenue`  
- `server_connection_error`  

Bins:  
0 = none, 1 = low, 2 = high.

### 2) Yeo-Johnson (Skewed Count Variables)
Applied to:
- `match_end_count`  
- `victory_count`  
- `defeat_count`  
- `avg_days_between_visits_incl_start`

### 3) Log1p (Engagement Variables)
Applied to:
- `total_logins_sessions`  
- `total_session_duration`

### 4) Original Scale  
Kept unchanged:
- `win_rate`  
- `days_since_last_active`

Finally, all features were standardized using **StandardScaler**.

---

## Correlation Insights

Key findings from the correlation heatmap:

1. **Engagement drives everything**  
   More playtime → more sessions and matches.

2. **Match metrics are redundant**  
   Match start, match end, and victories are highly correlated.

3. **Ad revenue follows activity**  
   More active players generate more ad revenue.

4. **IAP reflects distinct behavior**  
   Paying users form a small but separate segment.

5. **Recency is a strong churn signal**  
   Lower engagement → higher days since last active.

![Correlation heatmap](screenshots/corr_matrix.png)

---

## PCA — Core Behavioral Dimensions

Four principal components were selected:

### **PC1 — Gameplay Engagement**
Driven by sessions, matches, and victories.  
Higher PC1 = highly active player.

### **PC2 — Recency / Churn Risk**
Dominated by `days_since_last_active`.  
Higher PC2 = higher churn risk.

### **PC3 — Technical Friction**
Almost entirely driven by `server_connection_error`.  
Represents an independent technical dimension.

### **PC4 — Playstyle Trade-off**
Contrasts win efficiency vs play volume.  
Positive = efficient winner  
Negative = high-volume but less efficient player.

## PCA Results

### PCA — Component 1 (Gameplay Engagement)
![PCA1 Loadings](screenshots/PCA1.png)

### PCA — Component 2 (Recency / Churn Risk)
![PCA2 Loadings](screenshots/PCA2.png)

### PCA — Component 3 (Technical Friction)
![PCA3 Loadings](screenshots/PCA3.png)

### PCA — Component 4 (Efficiency vs Volume)
![PCA4 Loadings](screenshots/PCA4.png)

---

## K-Means Player Segmentation

Four segments were identified using PCA scores.

### **Segment 1 — Super Heavy / Power Players (Core Users)**
- ~142 average sessions  
- Highest victory count  
- Recency ≈ 0.17 days  
- Very high PC1, positive PC4  

**Actions**
- VIP program  
- Exclusive rewards  
- Prioritize server stability for this group  

---

### **Segment 3 — Heavy but Less Efficient Players**
- ~94 sessions  
- High activity but lower win efficiency  
- High PC1, negative PC4  

**Actions**
- Improve matchmaking  
- Provide tutorials and skill training  
- In-game guidance systems  

---

### **Segment 2 — Mid-Core / Regular Players**
- ~42 sessions  
- Recency ≈ 1.9 days  
- Few technical issues  

**Actions**
- Light promotions  
- Push notifications  
- Retention campaigns  

---

### **Segment 0 — Casual / At-Risk Players**
- ~3 sessions  
- Recency ≈ 12.7 days  
- Very low activity  

**Actions**
- Re-engagement campaigns  
- “Welcome back” bonuses  
- Simplified onboarding  

---

## Segment Summary

| Segment | Activity | Recency | Efficiency | Technical Issues |
|---------|----------|---------|------------|------------------|
| S1 | Very High | Very Low | High | Medium |
| S3 | High | Low | Low | Medium |
| S2 | Medium | Medium | Medium | Low |
| S0 | Very Low | Very High | Low | Very Low |

---

## Conclusion

This analysis delivers:

- A robust user-level dataset  
- Interpretable PCA behavioral axes  
- Four actionable player segments  
- Clear product, technical, and marketing recommendations  

---

## Author

**Burak Gocer**  
**2026-01-20**
