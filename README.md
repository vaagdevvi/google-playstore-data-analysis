Google Play Store App Data Analysis

Exploratory data analysis of 5,400+ Google Play Store apps to understand what separates highly-rated apps from the rest, and how rating, review volume, and category relate to each other.

The Question
Do popular categories (like Family or Games) actually earn better ratings, or just more downloads? And how reliable is the "Rating" field  as a signal — is it evenly distributed, or skewed toward one end?

Dataset
- Source: [Google Play Store Apps dataset](https://www.kaggle.com/datasets/lava18/google-play-store-apps) (Kaggle)
- Size: 5,400 rows × 15 columns (10,841 in the original raw file before initial filtering)
- Key columns: App, Category, Rating, Reviews, Size, Installs, Type, Price, Genres, Last Updated
- Known limitations: ~4% of rows were missing a `Rating` (224 of 5,400); Sentiment column had ~4.6% missing; 105 exact duplicate rows       existed in the raw file

Approach
1. Data cleaning — dropped two unusable columns (`Unnamed: 13` was 100% null, `Android Ver` was redundant with cleaning goals), imputed       missing `Rating` and `Sentiment` with median values, coerced `Current Ver` to numeric, removed 105 duplicate rows
2. Descriptive statistics — mean, median, mode, variance, std, and range for Rating and Reviews, both overall and grouped by Category/App
3. Distribution analysis — skewness and kurtosis on Rating and Reviews to understand shape, not just central tendency
4. Correlation analysis — heatmap across Rating, Reviews, and Current Ver
5. Visualization — category frequency bar chart, rating boxplot, pie chart of top-rated apps' review share

Key Findings

| Metric | Value | What it means |
|---|---|---|
| Rating skewness | -2.05 | Ratings are heavily left-skewed — most apps sit in the 4.0–4.7 range, with a small number of poorly-rated apps pulling the tail down |
| Rating kurtosis | 7.56 | Leptokurtic distribution — ratings cluster tightly around the peak rather than spreading evenly, so small rating differences (e.g. 4.2 vs 4.5) are more meaningful than they'd first appear |
| Rating ↔ Reviews correlation | ~0.08 | Essentially no linear correlation — an app doesn't get better ratings just because it has more reviews, and vice versa |
| Top categories by app count | FAMILY (616), GAME (576), TOOLS (320) | Category popularity is not evenly distributed — 3 categories account for a disproportionate share of the store |

In plain terms: star ratings on the Play Store aren't a reliable measure of an app's actual popularity — a 4.8-rated app with 50 reviews and a 4.2-rated app with 200,000 reviews are not comparable on rating alone. Review volume and rating need to be read together, not separately.

Limitations
- Missing values in Rating/Sentiment were imputed with the median, which is a reasonable default but can understate the true variance in    those columns
- The dataset is a single snapshot (no timestamp range for when it was scraped), so findings reflect a moment in time, not a trend
- Correlation analysis only covers numeric columns — Category and Genre effects on rating were explored via grouping, not formal            statistical testing (e.g. no ANOVA), which would be a natural next step

Tech Stack
Python, Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn (MinMaxScaler)

What I Learned
- How skewness and kurtosis change the interpretation of "average" — a mean rating alone would have hidden how tightly clustered and        left- skewed the data actually is
- That correlation needs to be checked, not assumed — I expected Reviews and Rating to move together and the near-zero correlation was a    genuinely useful finding, not a null result to throw away
- Handling missing data with median imputation vs. simply dropping rows, and when each is the more defensible choice

License
MIT
