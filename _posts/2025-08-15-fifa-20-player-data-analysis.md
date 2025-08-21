---
title: FIFA 20 Player Data Analysis — EDA & Insights
description: Exploring 18k FIFA 20 players across ratings, wages, clubs, countries, and roles with clean visualizations.
tags: [EDA, Football, Pandas, Matplotlib, Seaborn]
---

**TL;DR:** A compact exploratory data analysis of the FIFA 20 player dataset (~18k players, 110+ features). We look at ratings, wages, clubs, nationalities, and role patterns. Code and notebook are in the repo. <!--more-->

---

## Overview

This project presents an in-depth **EDA** of the FIFA 20 player dataset. With 18,483 players and 110+ fields (attributes, clubs, positions, national teams), it’s a nice playground for slicing player quality, wage/value skews, and team styles.

**Goals**
- Understand how attributes relate to player performance
- Compare clubs/countries on collective stats
- Examine trends in wage, value, potential, and skill
- Uncover patterns relevant to scouting, valuation, and team-building

**Repo:** <https://github.com/prashanth-ds-ml/FIFA_Players_Analysis>

---

## Dataset

- Source: Kaggle — “FIFA Complete Player Dataset” (includes multiple yearly CSVs such as `players_20.csv`)
- File used: `players_20.csv` (≈ 18,483 rows, 110 columns)
- Notable columns: `short_name`, `age`, `overall`, `potential`, `wage_eur`, `value_eur`, `preferred_foot`, positional skill columns, etc.

> Tip: Keep a lightweight data dictionary of the columns you actually use; it makes your plots self-documenting.

---

## What I did (Step-by-step)

1) **Load & inspect**
   - Read `players_20.csv`
   - Peek into `df.info()`, missing values, and column groups (overall, movement, power, mentality, defending, GK, etc.)

2) **Distributions**
   - Overall rating histogram → most players fall between **65–75**
   - Wage distribution → extremely skewed (top 1% dominate)

3) **Relationships**
   - **Wage ↔ Overall**: clear positive trend with outliers
   - **Age ↔ Overall**: rises to ~30, then tapers
   - **Potential ↔ Age**: younger players peak in potential
   - **Height/Weight ↔ Overall**: heavier/taller cluster more in defensive/GK roles

4) **Breakdowns**
   - **Top nationalities by count**; average overall & average age per nationality
   - **Club “style profiles”** via mean attributes (passing, dribbling, defending, crossing, long shots, aggression)

5) **Deep dive: Portugal**
   - Filter to Portuguese players (n≈344) and rank top-10 in shooting/defending/passing/dribbling

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
- Wages/values are **power-law** skewed toward elites
- Physicality lines up with defensive/GK roles

### Country / Club highlights
- **Most players**: Argentina, Brazil, Spain  
- **Highest avg rating**: Belgium, Portugal, Spain  
- **Oldest avg age**: Uruguay, Greece, Russia

**Club strengths (means)**
- **Passing**: FC Barcelona, PSG, Man City  
- **Dribbling**: PSG, Real Madrid, Barcelona  
- **Long shots**: Man City, Bayern, Juventus  
- **Aggression**: Atlético Madrid, Roma, Inter

---

## Selected visuals

> Export your figures to your site so the post renders them:

```python
# In your notebook, before plt.show():
plt.tight_layout()
plt.savefig("assets/dribbling_vs_passing.png", dpi=140, bbox_inches="tight")
