# NBA Draft, Players & Performance Analysis (1990–2020)

A statistical analysis of NBA draft picks and their first-year performance, exploring how draft position, physical attributes, geography, and college background relate to on-court outcomes.

## Overview

This project uses a dataset of ~12,800 player-season records spanning 1990–2020 to answer questions like:

- Do #1 overall picks actually outperform their draft class?
- Are first-round picks measurably taller — and do they perform better — than second-round picks?
- Does a player's country or continent predict their draft position or performance?
- Which colleges produce the most first-round talent?
- How strongly does draft number correlate with rookie stats?

The analysis pipeline starts with data exploration and cleaning in **Python** (Jupyter Notebook), then moves to statistical testing in **R**.

## Project Structure

```
├── data/
│   ├── all_seasons.csv              # Raw dataset (~12,800 player-season records)
│   └── NBA_cleaned_data.csv         # Cleaned dataset (~2,300 unique players)
├── NBA - Exploration and Cleaning.ipynb  # Python: EDA & data cleaning
├── NBA_Analysis.Rmd                 # R: Statistical analysis notebook
├── NBA_Analysis.nb.html             # Rendered R notebook output
├── .gitignore
└── README.md
```

## Dataset

The raw dataset (`all_seasons.csv`) contains one row per player per season with 21 features including:

| Feature | Description |
|---------|-------------|
| `player_name` | Player's full name |
| `draft_year`, `draft_round`, `draft_number` | Draft selection details |
| `player_height`, `player_weight` | Physical measurements |
| `college`, `country` | Background info |
| `pts`, `reb`, `ast`, `gp` | Counting stats |
| `net_rating`, `ts_pct`, `usg_pct` | Advanced metrics |

## Analysis

### Phase 1 — Data Cleaning (Python)

The Jupyter notebook handles:

- Exploratory analysis of player distributions across draft years, heights, and rounds
- Imputation of missing height values with the median
- Removal of draft years with unusually low player counts
- Deduplication by player name (keeping the first occurrence), reducing the dataset from ~12,800 rows to ~2,300 unique players

### Phase 2 — Statistical Testing (R)

#### Z-Scores for #1 Overall Picks

Calculated z-scores for each #1 pick's rookie scoring average relative to their own draft class. Key finding: **z-score rankings differ substantially from raw scoring rankings**. For example, Shaquille O'Neal had the highest raw points (26.2) but ranked only #17 in z-score because his 1992 class was exceptionally deep. Ben Simmons ranked #2 in z-score despite scoring under 20 PPG, because his 2016 class had few high scorers. The z-scores may reflect draft class competitiveness as much as individual talent.

#### T-Tests

- **Height by draft round** (p = 0.012): First-round picks are significantly taller than second-round picks, suggesting height plays a role in draft selection.
- **Net rating by draft round** (p = 0.001): First-round picks have significantly higher net ratings, confirming that early picks generally deliver on their promise.

#### ANOVA + Post Hoc (Tukey HSD)

- **Height across continents** (significant): Players from Asia, Africa, and Europe tend to be significantly taller than players from the Americas. This likely reflects selection bias — international players need to stand out more to get drafted.
- **Net rating across continents** (not significant): Among drafted players, continent of origin does not significantly predict first-year performance.

#### Chi-Square Tests

- **Draft round × country** (p = 0.99): No association — a player's country does not predict which round they are drafted in.
- **Draft round × college** (p < 0.001): Strong association — certain programs (Kentucky, Duke, North Carolina) are overrepresented in the first round, reflecting both program quality and recruiting advantages.

#### Correlation Analysis (Spearman)

Moderate negative correlations between draft number and key stats (points, rebounds, games played, net rating, assists), confirming that earlier picks generally produce stronger rookie seasons.

## Tech Stack

- **Python 3** — pandas, matplotlib (data cleaning & EDA)
- **R** — dplyr, agricolae, countrycode (statistical analysis)

## How to Run

1. Clone the repo
2. **Cleaning notebook**: Open `NBA - Exploration and Cleaning.ipynb` in Jupyter and run all cells. This reads `data/all_seasons.csv` and outputs `data/NBA_cleaned_data.csv`.
3. **Analysis notebook**: Open `NBA_Analysis.Rmd` in RStudio. Update the `setwd()` path to your local `data/` directory, then knit or run all chunks.

## Key Takeaways

1. Draft position matters — earlier picks score more, contribute more, and have higher net ratings in their rookie year.
2. Height gives a measurable edge in the draft, and international players who get drafted tend to be taller (likely a selection effect).
3. Where you went to school matters for draft round, but where you're from (country/continent) does not.
4. Individual z-scores are shaped as much by draft class depth as by the player's own ability — a useful reminder that context matters when evaluating talent.
