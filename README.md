# 🎯 Case-2 — Player Analytics, PCA & Behavioral Segmentation

This project presents an end-to-end analytics study based on multi-period game telemetry data that includes:

- transition from event-level to user-level data,  
- distribution-focused feature transformations,  
- correlation analysis,  
- dimensionality reduction via Principal Component Analysis (PCA), and  
- behavior-based player segmentation using K-Means clustering.  

The objective is not only to build a model, but to **conceptualize player behavior and generate actionable segments.**

---

## 📌 Business Problem

This analysis addresses the following key questions:

- What behavioral signals best explain player engagement?  
- Are technical issues (server errors) an independent behavioral dimension?  
- Is recency a reliable indicator of churn risk?  
- How many distinct behavioral player groups exist?  
- What product and marketing actions should be taken for each segment?

> 🖼️ **Image 1 — Use on Slide 1: “Analytics Roadmap”**  
> Place a diagram that illustrates the end-to-end pipeline (Data → Features → PCA → Segments).

---

## 📌 Data Source

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

> 🖼️ **Image 2 — Use on Slide 2: “Raw Data Landscape”**  
> Display a timeline visual showing the multi-period nature of the data.

---

## 📌 Event-Level to User-Level Transformation

Event data was aggregated at the `user_id` level using:

| Variable Type | Aggregation Method |
|----------------|--------------------|
| Behavioral metrics | **sum** |
| Static attributes | **first** |

A key derived metric was created:


This represents player skill, in-game performance, and potential loyalty.

> 🖼️ **Image 3 — Use on Slide 3: “From Events to Users”**  
> Show a flow diagram of the event-to-user transformation.

---

## 📌 Recency & Inter-Visit Gap

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

## 📌 Distribution Analysis (Histograms)

The distributions of all numerical features were examined and found to be highly skewed.  
Therefore, specialized transformations were applied before PCA.

> 🖼️ **Image 4 — Use on Slide 4: “Raw Feature Distributions”**  
> Display the grid of histograms for all numeric variables.

---

## 📌 Feature Transformation Strategy (Pre-PCA)

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

## 📌 Correlation Insights

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

> 🖼️ **Image 5 — Use on Slide 5: “Correlation Heatmap”**

---

## 📌 PCA — Core Behavioral Dimensions

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

## 📌 K-Means Player Segmentation

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

## 📌 Segment Summary

| Segment | Activity | Recency | Efficiency | Technical Issues |
|---------|----------|---------|------------|------------------|
| S1 | Very High | Very Low | High | Medium |
| S3 | High | Low | Low | Medium |
| S2 | Medium | Medium | Medium | Low |
| S0 | Very Low | Very High | Low | Very Low |

> 🖼️ **Image 10 — Use on Slide 10: “Cluster Scatter (PC1 vs PC2)”**  
> Show the PCA scatter plot with segment labels.

---

## 📌 Conclusion

This analysis delivers:

- A robust user-level dataset  
- Interpretable PCA behavioral axes  
- Four actionable player segments  
- Clear product, technical, and marketing recommendations  

---

## ✍️ Author

**[Burak Gocer]**  
**[2026-01-20]**
