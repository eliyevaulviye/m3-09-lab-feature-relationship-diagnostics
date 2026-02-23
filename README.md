![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Lab | Feature Relationship Diagnostics

## Overview

Correlation is one of the most commonly used — and most commonly misused — tools in data science. A single correlation coefficient can summarize the linear relationship between two variables in one number, but that number hides a surprising amount of nuance. Outliers can inflate or deflate correlations, non-linear relationships can produce misleading values, and aggregate correlations can reverse direction when you look at subgroups.

In this lab you will work with a realistic dataset to compute both Pearson and Spearman correlations, visualize relationships with scatterplots and heatmaps, and stress-test your findings against outliers and subgroup effects. By the end, you will have hands-on experience diagnosing when a correlation tells the truth and when it tells a convincing lie.

The final deliverable is a short feature relationship report that summarizes which pairs of features are genuinely associated, which associations are fragile, and where Simpson's paradox lurks in the data.

## Learning Goals

By the end of this lab, you should be able to:

- Compute Pearson and Spearman correlation coefficients and explain when each is appropriate
- Build an annotated correlation heatmap to scan many feature pairs at once
- Identify how outliers distort correlation strength and direction
- Detect Simpson's paradox by comparing aggregate and subgroup correlations
- Assess whether a bivariate relationship is linear, monotonic, or neither
- Compile findings into a concise feature relationship report

## Setup and Context

You will work inside a single Jupyter notebook (`m3-09-feature-relationship-diagnostics.ipynb`) and write all code, visualizations, and narrative answers there. Use markdown cells to explain your reasoning — a correlation coefficient without interpretation is just a number.

The dataset you will use should contain a mix of continuous and categorical features so that you can explore both overall and subgroup-level correlations. Good options include the **Penguins** dataset (`seaborn.load_dataset("penguins")`), the **Tips** dataset, or any tabular dataset with at least five numeric columns and one categorical grouping column. Your instructor may specify a particular dataset; if not, Penguins is recommended because it provides clear subgroup structure (species) that produces Simpson's paradox in several feature pairs.

## Requirements

### Tools and Libraries

- Python 3.9+
- pandas, numpy
- scipy (`scipy.stats.pearsonr`, `scipy.stats.spearmanr`)
- matplotlib, seaborn
- Jupyter Notebook or JupyterLab

### Installation

If any packages are missing, install them in your environment:

```bash
pip install pandas numpy scipy matplotlib seaborn
```

### Repository Setup

1. Fork this repository to your GitHub account.
2. Clone your fork to your local machine:

```bash
git clone https://github.com/<your-username>/m3-09-lab-feature-relationship-diagnostics.git
cd m3-09-lab-feature-relationship-diagnostics
```

3. Open the notebook `m3-09-feature-relationship-diagnostics.ipynb` and work through the tasks below.

## Getting Started

1. Import all required libraries at the top of your notebook.
2. Load your chosen dataset and run basic checks (`.info()`, `.describe()`, missing-value counts).
3. Identify which columns are numeric and which are categorical — you will need both.
4. Drop or impute missing values as appropriate, documenting your choice.

## Tasks

### Task 1: Compute Pearson and Spearman Correlations

1. Select all numeric feature pairs in the dataset.
2. Compute the **Pearson** correlation matrix using `pandas` or `scipy`.
3. Compute the **Spearman** correlation matrix for the same pairs.
4. Create a summary table that shows, for each pair, both the Pearson *r* and the Spearman *ρ*, along with their p-values.
5. In a markdown cell, answer:
   - Which pairs show the strongest linear relationships?
   - Are there any pairs where Pearson and Spearman disagree noticeably? What might cause that?

### Task 2: Build a Correlation Heatmap

1. Using `seaborn.heatmap`, create an annotated heatmap of the **Pearson** correlation matrix.
   - Use a diverging colormap (e.g., `coolwarm` or `RdBu_r`) centered at zero.
   - Annotate each cell with the correlation value rounded to two decimal places.
   - Mask the upper triangle so the matrix is easier to read.
2. Create a second heatmap for the **Spearman** matrix using the same layout.
3. In a markdown cell, compare the two heatmaps:
   - Where do they agree?
   - Where do they differ, and what does that tell you about the shape of those relationships?

### Task 3: Scatterplot Analysis of Key Pairs

1. Pick the **three** feature pairs with the highest absolute Pearson correlation.
2. For each pair, produce a scatterplot with:
   - A regression line (use `seaborn.regplot` or `seaborn.lmplot`).
   - Points colored by a categorical grouping variable (e.g., species, sex, or day).
3. In a markdown cell, discuss:
   - Does the regression line capture the actual pattern well?
   - Do subgroups show different slopes or intercepts?

### Task 4: Investigate Outlier Effects on Correlation

1. Choose one feature pair that has a moderately strong Pearson correlation (|r| between 0.4 and 0.7).
2. Compute and record the Pearson *r* for the full data.
3. Identify potential outliers using either:
   - A z-score threshold (e.g., |z| > 3), or
   - The IQR method (values beyond 1.5 × IQR from Q1/Q3).
4. Remove the identified outliers and recompute Pearson *r*.
5. Alternatively, add 2–3 synthetic extreme points and recompute *r*.
6. Create a before-and-after scatterplot (side by side) showing how the regression line shifts.
7. In a markdown cell, summarize:
   - How much did *r* change?
   - What does this tell you about the robustness of Pearson correlation to outliers?
   - When might Spearman be a safer choice?

### Task 5: Demonstrate Simpson's Paradox

1. Compute the overall Pearson correlation for a feature pair across the entire dataset (e.g., `bill_length_mm` vs `bill_depth_mm` in the Penguins dataset).
2. Now compute the same Pearson correlation **within each subgroup** (e.g., within each species).
3. Create a scatterplot that overlays all subgroups with different colors and their individual regression lines, plus the overall regression line in a contrasting style (e.g., dashed black).
4. In a markdown cell, answer:
   - Does the direction of the correlation change when you condition on the grouping variable?
   - Why does the aggregate trend differ from the subgroup trends?
   - How could ignoring Simpson's paradox lead to incorrect conclusions in a real project?

### Task 6: Feature Relationship Report

1. Create a summary markdown cell (or multiple cells) at the end of your notebook titled **Feature Relationship Report**.
2. The report should include:
   - A table listing each feature pair, its Pearson *r*, Spearman *ρ*, whether the relationship is linear/monotonic/neither, and any caveats (outlier sensitivity, Simpson's paradox).
   - A brief paragraph ranking the top 3 most reliable feature associations and explaining why you trust them.
   - One paragraph discussing which correlations are misleading and what a downstream model builder should watch out for.
3. Keep the report concise — aim for about 300–500 words plus the table.

## Submission

### What to Submit

- Your completed Jupyter notebook `m3-09-feature-relationship-diagnostics.ipynb` containing all code, visualizations, and markdown explanations.

### Definition of Done

- [ ] Pearson and Spearman correlation matrices are computed and compared
- [ ] At least two annotated heatmaps are produced (Pearson and Spearman)
- [ ] Scatterplots with regression lines and subgroup coloring are included
- [ ] Outlier sensitivity analysis shows before-and-after correlation values
- [ ] Simpson's paradox is demonstrated with aggregate vs. subgroup correlations
- [ ] A feature relationship report summarizes findings with a comparison table
- [ ] All markdown cells contain clear, concise interpretations

### How to Submit

1. Save your notebook and make sure all cells have been run (Kernel → Restart & Run All).
2. Stage and commit your changes:

```bash
git add .
git commit -m "Completed feature relationship diagnostics lab"
```

3. Push to your fork:

```bash
git push origin main
```

4. Create a Pull Request from your fork to the original repository.
5. Submit the Pull Request link on the student portal.
