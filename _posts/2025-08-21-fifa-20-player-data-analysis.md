---
title: "FIFA 20 Player Data Analysis — EDA & Insights"
description: Exploring 18k FIFA 20 players across ratings, wages, clubs, countries, and roles with clean visualizations.
tags: [EDA, Football, Pandas, Matplotlib]
permalink: /fifa-20-player-data-analysis/
---

**TL;DR:** Compact exploratory data analysis (EDA) on ~18k FIFA 20 players with 110+ features. We inspect distributions, wage/value skews, country/club profiles, and role patterns. Code and notebook are in the repo. <!--more-->

---

## Overview

This project presents an in-depth **EDA** of the FIFA 20 player dataset. With 18,483 players and 110+ fields (attributes, clubs, positions, national teams), it’s a solid playground for slicing player quality, wage/value skews, and team styles.

**Goals**
- Understand how attributes relate to player performance  
- Compare clubs/countries on collective statistics  
- Examine trends in wage, value, potential, and skill  
- Uncover patterns useful for scouting, valuation, and team-building

**Repo:** <https://github.com/prashanth-ds-ml/FIFA_Players_Analysis>

[Download the full PDF](/assets/files/FIFA-Analysis.pdf)

---

## Dataset

- **Source:** Kaggle — “FIFA Complete Player Dataset” (multiple yearly CSVs such as `players_20.csv`)
- **Used file:** `players_20.csv` (≈ 18,483 rows, 110 columns)
- **Notable columns:** `short_name`, `age`, `overall`, `potential`, `wage_eur`, `value_eur`, `preferred_foot`, position skills (passing, dribbling, defending, crossing, long_shots), GK metrics, etc.

> Tip: Keep a small data dictionary of the columns you actually use; it makes your plots self-documenting.

---

## Method (Step-by-step)

1) **Load & inspect**
   - Read `players_20.csv`
   - `df.info()`, missing values, quick percentiles
   - Group columns by families (overall, movement, power, mentality, defending, GK, etc.)

2) **Distributions**
   - Overall rating histogram → most players sit in **65–75**
   - Wage distribution → highly skewed (top 1% dominate)

3) **Relationships**
   - **Wage ↔ Overall**: clear positive trend with outliers
   - **Age ↔ Overall**: rises to ~30, then tapers
   - **Potential ↔ Age**: younger players carry higher potential ceilings
   - **Height/Weight ↔ Overall**: heavier/taller players cluster more in defensive/GK roles

4) **Breakdowns**
   - **Nationalities**: counts, average overall, average age
   - **Clubs**: mean skills to build “style profiles” (passing, dribbling, defending, crossing, long shots, aggression)

5) **Deep dive: Portugal**
   - Filter to Portuguese players (n ≈ 344); rank top-10 by shooting/defending/passing/dribbling

---

## Key findings

### Top earners (wage)
| Player | Wage (EUR) |
|---|---:|
| Lionel Messi | €560,000 |
| Eden Hazard | €470,000 |
| Cristiano Ronaldo | €410,000 |

### General trends
- Most players are rated **65–75**  
- Peak rating window ≈ **28–30 years**  
- Wages/values show a **power-law** skew toward elites  
- Physicality aligns with defensive/GK roles

### Country-level highlights
| Metric | Top Countries |
|---|---|
| Most players | Argentina, Brazil, Spain |
| Highest avg rating | Belgium, Portugal, Spain |
| Oldest avg age | Uruguay, Greece, Russia |

### Club strengths (means)
| Skill | Top Clubs |
|---|---|
| Passing | FC Barcelona, PSG, Man City |
| Dribbling | PSG, Real Madrid, Barcelona |
| Long shots | Man City, Bayern, Juventus |
| Aggression | Atlético Madrid, Roma, Inter |

---

## Selected visuals

Export figures from your notebook to your site repo so they render here. Save them under `assets/fifa/` and reference by path:

```python
# Example: before plt.show()
plt.tight_layout()
plt.savefig("assets/fifa/overall_hist.png", dpi=140, bbox_inches="tight")
```

Then reference in Markdown:

![Overall Rating Distribution](/assets/fifa/overall_hist.png)
*Most players sit in the 65–75 overall band.*

![Wage vs Overall](/assets/fifa/ratings_vs_age.png)
*Positive trend with elite outliers.*

![Top 10 Clubs](/assets/fifa/top_10_clubs.png)

![Club Style Profiles](/assets/fifa/club_styles.png)

> Suggested filenames  
> `overall_hist.png`, `age_vs_overall.png`, `wage_vs_overall.png`, `potential_vs_age.png`,  
> `top_nationalities.png`, `club_passing.png`, `club_dribbling.png`, `club_defending.png`,  
> `club_crossing.png`, `club_longshots.png`, `club_aggression.png`,  
> `portugal_top_shooting.png`, `portugal_top_passing.png`, `portugal_top_dribbling.png`, `portugal_top_defending.png`.

---

## Reproduce locally

```bash
git clone https://github.com/prashanth-ds-ml/FIFA_Players_Analysis
cd FIFA_Players_Analysis
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt  # or: pandas numpy matplotlib seaborn jupyter
jupyter lab
```

Open the notebook/script, generate plots, and copy them into your **portfolio** repo under `assets/fifa/`.

---

## What’s next

- Role clustering (K-Means / UMAP visualization)  
- Value modeling (e.g., log(value) ~ attributes + age + reputation)  
- Cross-season comparisons (FIFA 19 ↔ 20 ↔ 21) for player development trajectories

---

## Links

- **Code & Notebook:** <https://github.com/prashanth-ds-ml/FIFA_Players_Analysis>  
- **Dataset (Kaggle):** FIFA Complete Player Dataset (includes `players_20.csv`)
