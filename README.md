# 🎮 Mobile Legend Sentiment Crisis Analysis

![Python](https://img.shields.io/badge/Python-3.10-blue) ![Streamlit](https://img.shields.io/badge/Streamlit-deployed-green) ![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

> Mobile Legend's 2026 update cycle triggered the most severe negative sentiment spike in the app's 10-year history. This project analyzes 200K+ Play Store reviews to find out **what broke, when, and why two problems hit at once**.

---

## Dataset

10.3M+ reviews (2016–2026) → stratified sample of **200K** (100K positive / 70K negative / 30K neutral). 100% Indonesian text, heavy gaming slang. Labels derived from `score` (1–2 = negatif, 3 = netral, 4–5 = positif). Source: [Kaggle](https://www.kaggle.com/datasets/dewanakretarta/mobile-legend-playstore-dataset).

## Method

`TF-IDF (1-2 grams) + Logistic Regression` for sentiment classification (vs. XGBoost as comparison) → `rule-based aspect extraction` across 5 business-defined aspects → `version-level timeline analysis`. Builds on a prior LDA + Naive Bayes project, swapped for a more business-actionable and more accurate approach.

**Data quality fix:** 709 reviews (0.43%) had extreme rating-text contradictions (e.g. 5★ review reading *"Game tolol jaringan lag tolol banget lu"*) — likely rating-farming from in-game prompts. These were filtered out before final training.

---

## Key Findings

**1. 2026 is a real crisis, not noise.** Negative sentiment was stable at 32–41% from 2016–2022. Two spikes followed: 2023 (57.3%, recovered) and 2026 (**71.5%, never recovered** — highest on record).

**2. Model confirms the cause.** Logistic Regression (F1: **0.873**, up from 0.868 pre-filtering) — strongest negative-driving words: `rusak`, `kecewa`, `tim`, `matchmaking`, `adil`, `seimbang`. Bugs and unfairness drive complaints, not price or content.

**3. Two aspects, near-equal severity:**

| Aspect | % Negative | Mentions |
|---|---|---|
| Matchmaking & Fairness | 84.6% | 14,207 |
| Bug & Performa Teknis | 78.3% | 29,743 |
| Konten & Originalitas | 54.1% | 8,183 |
| Apresiasi Positif | 15.9% | 65,221 |

Bug has the highest *volume*; Matchmaking has nearly the same *severity*. They moved together, not separately.

**4. v2.1.x = compound crisis, not one bug.** Comparing v1.9 (baseline) → v2.1 (crisis):

| Aspect | v1.9 | v2.1 | Delta |
|---|---|---|---|
| Bug & Performa Teknis | 22.9% | 31.2% | **+8.4 pts** |
| Matchmaking & Fairness | 19.1% | 26.3% | **+7.2 pts** |
| Apresiasi Positif | 44.7% | 27.6% | **−17.1 pts** |

Bugs and matchmaking complaints rose *together*; positive sentiment nearly halved. Two systems degraded at once.

**5. Word clouds add texture:** `dark` (≈ "dark system", a perceived hidden matchmaking algorithm) appears in both negative and positive Matchmaking reviews — a recognized community term, not an isolated gripe. In Konten & Originalitas, positive reviews lean on `gratis`/`berlian` (free rewards), negative reviews lean on `plagiat` — satisfaction tracks free rewards, dissatisfaction tracks originality, not price.

---

## Model Performance

| Model | F1 (original) | F1 (filtered) |
|---|---|---|
| Logistic Regression | 0.868 | **0.873** |
| XGBoost | 0.854 | 0.857 |

Both improved after filtering; LogReg won both times — linear models suit sparse TF-IDF better than tree-based models here.

---

## Business Recommendations

1. Fix matchmaking *transparency*, not just server stability — users rate unfairness as harshly as instability.
2. Treat v2.1.x as a **compound regression** — fixing bugs alone won't recover sentiment if matchmaking balance isn't addressed too.
3. Track **Apresiasi Positif rate** as a leading indicator — its 17-point collapse moved faster than the negative rate itself.
4. Re-run this v1.9→v2.1-style comparison on a beta cohort *before* future major releases.

---

## Dashboard

```
```
4 tabs: Overview · Model Performance · Aspect Analysis · Patch Timeline.

---

## Limitations

- Rule-based aspect extraction misses sarcasm and out-of-dictionary phrasing.
- Some individual versions (e.g. `2.1.40.11461`, n=126) have low sample sizes — trust the major-version aggregates more.
- Correlational, not causal — this shows *what* moved with sentiment, not proof a specific patch note caused it.
- Unexplored: `boong` ("lying") appears in *positive*-labeled reviews under Frustrasi Umum — possibly promo/event-related distrust. Flagged for future work.

---

## Tech Stack

`Python` `pandas` `scikit-learn` `XGBoost` `Sastrawi` `Streamlit` `matplotlib` `seaborn` `kagglehub`


