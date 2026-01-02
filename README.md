# Delivery Market Analysis

A business‑oriented exploratory analysis of a Takeaway database to uncover actionable insights for:
- **Restaurant partners** (pricing, positioning, quality/ops opportunities)
- **Consumers** (discovery of “best value”, hidden gems, and coverage gaps)
- **Takeaway** (where to win market share tomorrow)

---

## What this project answers

### Key business questions
1. **What is the price distribution of menu items?**
2. **What is the distribution of restaurants per location?** (delivery coverage by postal code)
3. **Which are the top 10 pizza restaurants by rating?** (raw + Bayesian‑adjusted ranking)
4. **Map locations offering kapsalons (or a favorite dish) and their average price.**

### Open‑ended questions
5. **Which restaurants have the best price‑to‑rating ratio?**
6. **Where are the delivery “dead zones” (minimal restaurant coverage)?**
7. **How does the availability of vegetarian and vegan dishes vary by area?**
8. **Identify the World Hummus Order (WHO): top hummus restaurants**  
   - by **Bayesian rating**
   - by **variety** (# hummus items)
   - by **specialist filter** (≥ 10 hummus items)
   - by **combined score** (quality + variety)

### Strategy questions (market-share focused)
9. **OQ1 — Where can Takeaway win market share tomorrow?**  
   Targets “underserved” areas (low restaurant coverage) with “high value” signals (higher basket proxy).
10. **OQ2 — Pricing mismatch opportunities**  
   Finds restaurants that are **overpriced vs their local market** and **underperform on ratings**.

---

## Core methodology (high-level)

### Pricing & basket proxies
- **Menu item price distribution:** prices converted to EUR and cleaned (caps + null handling).
- **Restaurant price proxy:** **median menu item price** (robust to outliers).

### Ratings (stability)
- **Bayesian-adjusted rating** is used for rankings and “local market” comparisons to avoid small-sample bias:
\[
\text{bayes} = \frac{v}{v+m}R + \frac{m}{v+m}C
\]
Where:
- **R** = restaurant raw rating  
- **v** = review count  
- **C** = global mean rating within the filtered segment  
- **m** = median review count within the segment

### “Local market” baseline
- Local market is defined as **city** (`restaurants.city`).
- We compute a **city median price** and **city mean Bayesian rating**, then compare each restaurant to its city baseline.

---

## Outputs

### Charts
- Price distribution by bands (business-friendly)
- Delivery coverage by postal code buckets
- Pizza ranking: raw vs Bayesian
- Pricing mismatch scatter plot (premium vs rating gap)

### Interactive maps (HTML)
Maps are saved under:
- `notebook/report/maps/`

Typical outputs:
- `pricing_mismatch_map.html`
- `pricing_mismatch_map_3segments.html`
- dish maps (e.g., kapsalon / hummus) depending on notebook cells

---

## Repository layout (suggested / used in notebook)

```
delivery-market-analysis/
├─ notebook/
│  ├─ analysis.ipynb
│  └─ report/
│     ├─ maps/
├─ data/
│  └─ takeaway.db              # SQLite DB (path may vary)
└─ README.md
```

---

## How to run

### 1) Install dependencies
```bash
pip install -U pandas numpy matplotlib folium
```

(Optional) If you manage environments:
```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -U pandas numpy matplotlib folium
```

### 2) Point the notebook to the DB
In your notebook, create a connection:
```python
import sqlite3
con = sqlite3.connect("data/takeaway.db")  # adjust path if needed
```

### 3) Run the notebook
Execute cells top-to-bottom. Generated HTML maps will be written to `notebook/report/maps/`.

---

## Key assumptions (used across analyses)

- **Prices:** converted to EUR via a project helper (e.g., `price_eur()`), then filtered to a reasonable cap (e.g., ≤ €60).
- **Restaurant price proxy:** median item price (robust).
- **Ranking stability:** Bayesian rating used whenever “Top” lists can be skewed by low review counts.
- **Minimum data thresholds:** typical defaults include `MIN_ITEMS >= 10`, `MIN_REVIEWS >= 10`, and city baselines require a minimum number of restaurants (e.g., `MIN_RESTS_PER_CITY >= 8`).
- **Dish detection (pizza/hummus/kapsalon/veg/vegan):** keyword-based matching on `menuItems.name` + `menuItems.description` with normalization (case/accents/punctuation).

---


## 📄 License

MIT License - see LICENSE file for details.

---

## 👤 Author

**Amine Sam**
- GitHub: [@AmineSam](https://github.com/AmineSam)

---
