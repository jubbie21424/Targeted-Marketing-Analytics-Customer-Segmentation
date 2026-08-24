# Marketing Mix Modeling & Causal Inference: Price Effects on TV Sales

An R analysis addressing a core marketing analytics question — what is the *causal* effect of price on sales? — using a weekly Amazon TV sales panel, moving from a naive OLS regression through endogeneity diagnosis, an instrumental variables (2SLS) correction, an A/B/N test design, and a regression discontinuity proposal for star ratings.

---

## Project Highlights

- **Exposes the price–sales endogeneity trap.** A naive OLS regression of `sales` on `final_price` and `marketing_expense` returns a *positive* price coefficient (0.618, p < 0.001) — the classic spurious result caused by Amazon raising prices in high-demand weeks, not evidence that higher prices drive more sales.
- **Diagnoses all three sources of endogeneity with concrete Amazon-context examples**: omitted variable bias (unobserved demand drivers like product quality, ratings, competitor promotions), reverse causality/simultaneity (Amazon's algorithmic discounting responds to expected demand), and measurement error (recorded prices not capturing temporary promotions).
- **Corrects the bias with a two-stage least squares (IV) design.** Using a proposed cost-shifter instrument (component/shipping costs — exogenous, relevant, and excluded from directly affecting sales), the two-stage regression flips the sign entirely: the causal effect of price on sales is **–0.734** (p < 0.001) — a one-unit price increase *reduces* weekly sales by ~0.73 units, the opposite conclusion from the naive OLS estimate.
- **Shows why "bigger screen = more sales" doesn't hold.** Controlling for price, marketing spend, brand, and other features, the 50–59 inch screen bracket has the largest positive effect on sales (~144 units vs. the sub-40" baseline) — larger than the 60"+ bracket (~82 units), indicating demand peaks at a mid-size range rather than scaling with screen size.
- **Designs a full-scale A/B/N test for a virtual try-on feature**, including individual-level randomisation (chosen over device/household/region level to minimise spillovers), a 10%-of-users experimental sample split evenly across two treatments, and a power calculation (via the `pwr` package) showing ~6,300 users are needed per treatment arm for 80% power to detect a £5 spending lift.
- **Proposes a Regression Discontinuity Design (RDD)** to identify the causal effect of Amazon's *displayed* star rating (e.g., 4.5 vs. 4 stars) on sales, exploiting the fact that sellers cannot precisely manipulate which side of a rounding cutoff (e.g., 4.25) their true average rating falls on.

## Repository Structure

```
.
├── Targeted_Marketing_Analytics___Customer_Segmentation.qmd   # Full analysis (Quarto/R Markdown)
├── requirements.txt                                           # R packages required
└── README.md
```

**Note:** `data_full.csv` (the underlying weekly sales panel) is course-provided and is **not included** in this repository. To render the document, place your own copy of `data_full.csv` in the same directory as the `.qmd` file.

## Setup

1. Install [R](https://cran.r-project.org/) and [RStudio](https://posit.co/download/rstudio-desktop/) (or [Quarto CLI](https://quarto.org/docs/get-started/) if rendering outside RStudio).
2. Install the required packages — see `requirements.txt`:
   ```r
   install.packages("pacman")
   pacman::p_load(dplyr, ggplot2, fixest, modelsummary, pwr)
   ```

## Running the Analysis

Open `Targeted_Marketing_Analytics___Customer_Segmentation.qmd` in RStudio and click **Render**, or from the command line:

```bash
quarto render "Targeted_Marketing_Analytics___Customer_Segmentation.qmd"
```

This produces a formatted Word (`.docx`) document with all code, regression tables, and written interpretation.

## Method Summary

| Section | Method | Key Result |
|---|---|---|
| Descriptive analytics | Scatter plot, `dplyr` aggregation | Positive but non-causal price–sales correlation; Samsung has the highest average weekly dollar sales |
| Marketing mix model | OLS (`fixest::feols`) | Naive price coefficient: **+0.618** (biased upward by endogeneity) |
| Instrumental variables | Two-stage least squares (cost-shifter instrument) | Corrected causal price coefficient: **–0.734** |
| Screen size | OLS with categorical screen-size controls | Non-monotonic effect; 50–59" segment performs best |
| A/B/N test design | Power analysis (`pwr`), individual-level randomisation | ~6,300 users needed per treatment arm |
| Star ratings | Regression Discontinuity Design (proposed) | Framework to isolate the causal effect of the 4 → 4.5 star display threshold |
# Targeted-Marketing-Analytics-Customer-Segmentation
