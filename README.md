# Premier League Striker Finishing Analysis (2025/26)

Identifying which Premier League strikers are converting their scoring chances efficiently — and which ones are getting good chances but wasting them — using expected goals (xG) and linear regression.

## Problem

Raw goal totals reward players who simply get more minutes or more shots, not necessarily better finishers. Expected Goals (xG) estimates how many goals a *typical* player should score from the shots they took. Comparing actual goals to xG reveals who's over- or underperforming their chance quality — a much fairer measure of finishing skill.

## Dataset

Gameweek-by-gameweek player data for the 2025/26 Premier League season, from the [Fantasy Premier League historical dataset](https://github.com/vaastav/Fantasy-Premier-League), including `goals_scored` and `expected_goals` per player per gameweek.

## Approach

1. Aggregated gameweek data into full-season totals per player, filtered to forwards (strikers)
2. Filtered to strikers with 900+ minutes played (~10 full matches), to avoid drawing conclusions from tiny samples
3. Normalized to per-90-minutes rates, since raw totals unfairly favor players with more playing time
4. Fit a linear regression predicting goals-per-90 from xG-per-90
5. Ranked all strikers by residual (actual − predicted) to find over/underperformers
6. Visualized the full picture with a labeled scatter plot

## Key finding

Linear regression on **raw season totals** gave R² = 0.89 — but this was partly inflated because both goals and xG naturally scale with playing time. Switching to **per-90 rates** dropped R² to 0.59, and after filtering out small-sample players (under 900 minutes), R² settled at **0.68** — a more honest measure of how much finishing outcomes are explained by chance quality alone, versus individual finishing skill.

**Biggest underperformer (900+ mins):** Yoane Wissa (Newcastle) — scoring 0.17 goals/90 against an expected 0.54 goals/90.

**Notable finding:** Erling Haaland, despite leading the league in raw goals, converts almost exactly at his expected rate — his output comes from getting a huge volume of good chances, not from exceptional finishing skill relative to those chances.

See `plots/` for the full scatter plot with regression line and labeled players.

## Limitations

- Single-season data; a player's finishing "skill" can vary year to year, and this snapshot won't capture that
- Even at 900+ minutes, some residuals are still influenced by small-sample luck — a more rigorous approach would apply shrinkage (regression to the mean weighted by sample size), which is a natural next step
- xG itself is a model, not ground truth — different providers calculate it slightly differently

## What I'd do next

- Apply shrinkage/regularization so small-sample residuals are pulled toward the average rather than trusted at face value
- Extend across multiple seasons to see which players are *consistently* over/underperforming, not just in one season
- Wrap this in an interactive app (e.g. Streamlit) so users can select any player and see their finishing profile live

## Tools

Python, pandas, scikit-learn, matplotlib, numpy — built and run in Google Colab.

## How to run

Open `Striker_Finishing_Analysis.ipynb` in Google Colab or Jupyter. All cells run top to bottom; data loads directly from a public URL, no manual download needed.
