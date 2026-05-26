# The Waiting Game: Survival Analysis of MLB Home Run Droughts

Applying Cox Proportional Hazards modeling to quantify the statistical anomaly of Fernando Tatis Jr.'s 2026 home run drought using Statcast plate appearance data.

---

## Overview

Fernando Tatis Jr. entered the 2026 MLB season as one of the most feared power hitters in baseball, but he reached 180 plate appearances without a single home run. His exit velocity remained elite. His hard-hit rate ranked among the league leaders. So why no home runs?

This project applies **survival analysis**, a statistical framework borrowed from medical research, to model home run drought duration at the plate appearance level. Rather than asking *will he hit a home run*, the model asks a more analytically interesting question: *given a hitter's Statcast profile, how long should we expect a drought to last before the event occurs?*

Tatis Jr. serves as the motivating case study, but the methodology is built on his full career of 3,122 plate appearances across six seasons, making the findings interpretable against his own historical baseline.

---

## Key Findings

- **0.85%**  probability of reaching 180 plate appearances homer-less based on his career drought distribution (Kaplan-Meier, unadjusted)
- **3.96%**  probability at 180 PAs after adjusting for his actual 2026 Statcast profile (Cox model)
- **7.0σ**  the current drought sits 7 standard deviations above his career mean drought length
- **Barrel rate** is the dominant predictor of drought duration (hazard ratio = 47.17, p < 0.0005)
- His 2026 **launch angle collapsed 82.6%** from his career average (13.54° → 2.35°), with barrel rate down 55.4% and pull rate at a career low, while exit velocity remained essentially unchanged (-3.2%)
- The model predicts a median drought of **26 PAs** under his 2026 profile vs **12 PAs** under his career baseline

> His mechanics explain much of the drought. But not all of it.

---

## Methods

| Component | Detail |
|---|---|
| Data source | Statcast via `pybaseball` |
| Seasons | 2019–2026 (regular season) |
| Unit of analysis | Plate appearance |
| Survival event | Home run |
| Censoring | Right-censored for active/season-ending droughts |
| EDA | Kaplan-Meier survival curves, covariate distributions by season |
| Model | Cox Proportional Hazards (`lifelines`) |
| Regularization | Ridge penalty (penalizer = 0.1) |
| Assumption test | Schoenfeld residuals — all covariates passed (all p > 0.05) |
| Model performance | Concordance index: 0.773 |

**Covariates:** mean exit velocity, mean launch angle, barrel rate, pull rate, mean plate X, mean plate Z

---

## Repository Structure

```
mlb-tatis-hr-drought-analysis/
│
├── MLB_TatisHRDrought_SurvivalAnalysis.ipynb   # Full annotated analysis notebook
├── Tatis_HRDrought_Analysis.pdf                # PDF version of the notebook
├── FinalWhitePaper_Tatis-HR-Drought_SurvivalAnalysis.pdf  # White paper
├── Tatis_HRDrought_Presentation.pdf            # Presentation slides
└── Audience_Q&A.pdf                            # Anticipated audience Q&A
```

---

## How to Run

**Requirements:**
```
python >= 3.9
pybaseball
lifelines
pandas
numpy
matplotlib
seaborn
```

**Install dependencies:**
```bash
pip install pybaseball lifelines pandas numpy matplotlib seaborn
```

**Run the notebook:**
```bash
jupyter notebook MLB_TatisHRDrought_SurvivalAnalysis.ipynb
```

> Note: The first run will pull Statcast data via `pybaseball`. Caching is enabled in the notebook to speed up subsequent runs.

---
  
*Part of my [Data Science Portfolio](https://github.com/nananmorgan/data-science-portfolio)*
  
---
