---
title: FIFA 20 Player Analysis — a simple EDA
description: A quick, honest look at FIFA 20 player data—distributions, relationships, club & country patterns, plus a small Portugal deep dive.
tags: [EDA, Football, Pandas, Matplotlib, Seaborn]
---

## Why I did this
I wanted a small, clean project to practice data exploration and storytelling:
- take a familiar dataset,
- answer basic but useful questions,
- show clear visuals and simple takeaways.

---

## Data
- **Source:** FIFA 20 players (`players_20.csv`)
- **Size:** ~18.5k players, ~110 columns
- **Key columns:** `overall`, `potential`, `age`, `wage_eur`, `value_eur`, `preferred_foot`, plus skill attributes

---

## What I did (at a glance)
- Checked schema, missing values, and column groups
- Looked at **distributions** (overall rating, age, wage)
- Studied **relationships** (wage vs overall, age vs overall, potential vs age)
- Compared **countries** and **clubs** on simple aggregates
- Did a focused **Portugal** slice (top 10 by shooting/passing/dribbling/defending)

**Tools/skills:** pandas, numpy, matplotlib, seaborn, `groupby`, aggregation, sorting, basic plotting

---

## Highlights (findings)
- **Ratings:** Most players sit in the **65–75** overall band  
- **Peak years:** Ratings climb into the **late 20s** and flatten/taper around **30**  
- **Wages:** Very **skewed**—a tiny elite earns disproportionately more  
- **Wage ↔ Overall:** Clear positive trend (with a few outliers)  
- **Club styles:** Mean attributes give quick “profiles” (e.g., passing/dribbling/long shots/aggression)  
- **Portugal:** Cristiano Ronaldo leads shooting; Bruno Fernandes/Bernardo Silva stand out in passing

---

## A few representative visuals
Then reference in Markdown:

![Overall Rating Distribution](/assets/fifa/players_vs_overall_ratings.png)
*Most players sit in the 65–75 overall band.*

![Wage vs Players](/assets/fifa/salary_vs_players.png)
*Positive trend with elite outliers.*

![Top 10 Clubs](/assets/fifa/top_10_clubs.png)

![Number of players vs Nationality](/assets/fifa/Nationality_vs_players.png)

---

## Links

- **Repo:** <https://github.com/prashanth-ds-ml/FIFA_Players_Analysis>  
- **Dataset (Kaggle):** [FIFA Complete Player Dataset (includes `players_20.csv`)](https://www.kaggle.com/datasets/stefanoleone992/fifa-22-complete-player-dataset)
