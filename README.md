# A/B Testing Analysis: Statistical Significance in Python and Tableau

Analysis of four e-commerce A/B tests. A Python function compares the control and treatment groups for four checkout and sign-up conversion metrics with a two-proportion Z-test, and an interactive Tableau dashboard shows which changes are statistically significant.

- **Notebook:** [`ab_test_analysis.ipynb`](ab_test_analysis.ipynb)
- **Dashboard:** [Open in Tableau Public](https://public.tableau.com/views/ab_17793668147860/ABtest?:language=en-US&:display_count=n&:origin=viz_share_link)
- **Results file:** [`ab_tests_results.csv`](ab_tests_results.csv)

## Project context

The project was completed as an assignment of the Mate Academy Data Analytics course. In the course, statistical significance was checked with online calculators; here the calculation is automated in Python, and the results are visualized in Tableau.

## What was analyzed

- 4 A/B tests, each with a control group (group 1) and a treatment group (group 2), with about 45K, 51K, 70K and 105K sessions per group.
- 4 conversion metrics, each calculated per session: `add_payment_info`, `add_shipping_info`, `begin_checkout` and `new account`.
- 16 comparisons in total (4 tests × 4 metrics).

## Key results

| Test | Result | Details (relative change in conversion, p-value) |
|---|---|---|
| 1 | Positive | `add_payment_info` +12.5% (p = 0.0001), `add_shipping_info` +6.6% (p = 0.009), `begin_checkout` +6.7% (p = 0.003). `new account` −3.4%, not significant. |
| 2 | No significant changes | All four metrics changed by +1.2% to +3.6%, all p ≥ 0.21. |
| 3 | Negative | `begin_checkout` −3.4% (p = 0.012). The other metrics are not significant. |
| 4 | Negative | `begin_checkout` −2.4% (p = 0.046) and `new account` −3.4% (p = 0.018). The other two metrics are also down by about 3.5%, but not significantly. |

**Recommendations:** Test 1 is the best candidate for rollout (after checking the effect on completed orders), Test 2 is inconclusive, and Tests 3 and 4 should not be rolled out.

**Caveat:** 16 comparisons were made at a significance level of 0.05 without adjustment. With a Bonferroni correction (threshold 0.05 / 16 ≈ 0.003), only two results remain significant: `add_payment_info` and `begin_checkout` in Test 1. The full discussion and other limitations are in the notebook's Conclusions section.

## Method

1. Extract and aggregate sessions and events per test and test group from BigQuery with SQL.
2. For every test and metric, calculate the conversion rate (events / sessions) in each group and the relative change.
3. Run a two-sided two-proportion Z-test (`proportions_ztest` from `statsmodels`); a difference is significant if p < 0.05.
4. Export the results to CSV and visualize them in Tableau Public, with a filter by test number.

The calculation is a function that loops over tests and over a list of metrics, so more metrics can be added without changing the logic.

## Data and reproducibility

The source data is the course's e-commerce dataset in BigQuery, which is private to Mate Academy participants, so the notebook cannot be re-run outside the course. The notebook also uses Google Colab authentication and Drive export. The results of the calculation are provided in `ab_tests_results.csv`, and the notebook contains the code and the saved outputs.

To explore the notebook locally:

```bash
pip install -r requirements.txt
jupyter notebook ab_test_analysis.ipynb
```

## Repository contents

| File | Description |
|---|---|
| `ab_test_analysis.ipynb` | Notebook with the SQL query, the significance calculation, the dashboard screenshots and the conclusions |
| `ab_tests_results.csv` | Results per test and metric: counts, conversion rates, relative change, Z-statistic, p-value and significance flag |
| `requirements.txt` | Python packages used in the notebook |

## Tools

Python (pandas, NumPy, statsmodels), BigQuery SQL, Google Colab, Tableau Public.

## Author

Oleksandra Korostil, Data Analyst. [LinkedIn](https://www.linkedin.com/in/oleksandra-korostil/)
