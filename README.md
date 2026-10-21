# 🛋️ E-commerce Sales Analytics | BigQuery SQL + Python + Tableau

**[▶ Open the Tableau dashboard](https://public.tableau.com/app/profile/taras.kubitskyi/viz/E-commerceSalesAnalyticsPortfolioProject/Overview)** · **[Notebook](Portfolio_Project_1___E_commerce_Sales_Analytics_v3.ipynb)**

## 🔍 Project Overview

An end-to-end analysis of an online furniture store (IKEA-like catalogue) for November 2020 – January 2021: data extraction from a relational database in Google BigQuery, cleaning, exploratory analysis, statistical testing and a Tableau dashboard.

## 🎯 Business Questions

- Which markets, product categories, devices and traffic channels drive sales?
- How do sales change over time, and is there seasonality?
- How do registered and unregistered users differ?
- Which of the observed differences are statistically reliable, and which are noise?

## 🗄 Data

A relational BigQuery dataset with sessions, session parameters (geo, device, traffic source), orders, products, accounts and email activity.

Two SQL queries build two analytical tables:
- **`df_traffic`** – one row per session: geography, device, browser, traffic channel, account status (`LEFT JOIN` sessions → session params → accounts).
- **`df_sales`** – one row per order: product, category, price, geography, device, channel, registration flag (`INNER JOIN` orders → products, `LEFT JOIN` to sessions and accounts).

## ⚙️ What I Did

1. **SQL in BigQuery:** joined 5 tables into two analysis-ready datasets with different grains (sessions vs. orders).
2. **Data cleaning:** duplicates, column standardisation, missing browser language, date types.
3. **Data validation:** found that the orders table ends on 27 Jan 2021 while sessions continue to 31 Jan, and aligned the periods before calculating conversion and trends.
4. **EDA:** geography, categories, devices, browsers, traffic channels, registered users, daily / weekly / monthly dynamics, pivot tables.
5. **Statistics:** Pearson correlations, Shapiro-Wilk normality checks, Mann-Whitney U, Kruskal-Wallis, two-proportion z-test and a χ² test for differences in conversion between countries.
6. **Tableau dashboard** for interactive exploration.

## 📈 Key Insights

- **Strong dependence on one market.** The Americas generate ~55% of sales ($17.7M); the US alone ($13.9M) sells more than Asia and Europe combined ($13.5M).
- **Seating and sleeping furniture drive the business.** Sofas & armchairs, Chairs and Beds are the top 3 categories in every major country.
- **December peak.** Comparing sales per day (months have different numbers of days in the data): December ~$401K/day, +21% vs. November and +12% vs. January, with the peak on 7–20 December. January stays above November (~$357K vs. ~$330K per day).
- **Weekdays sell more.** An average weekend day brings ~30% less revenue than a weekday; Tuesday and Wednesday are the strongest days.
- **Traffic is a good leading indicator.** Daily sessions and daily sales correlate at r = 0.93.
- **Conversion is ~10% and fairly similar across large markets.** Among 38 countries with 1,000+ sessions, differences are small but real (χ², p = 0.027). Pakistan stands out at 12.4%, which is a hypothesis to validate rather than a conclusion.
- **Organic Search is the largest channel** (35.8% of sales), followed by Paid Search (26.6%) and Direct (23.4%). Channel ROI cannot be assessed without marketing spend data.
- **Desktop brings 59% of sales**, mobile 39%; Chrome and Safari together account for 91.8%.

## 🔁 What I Changed After Reviewing My First Version

Re-checking the analysis changed several conclusions:

- **Days with traffic but zero sales were a data gap, not real behaviour.** The last 4 days (28–31 Jan) had 13,469 sessions and no orders because the orders table ends earlier. After aligning the periods, conversion went from 9.6% to 10.0% and the sessions–sales correlation from 0.79 to 0.93.
- **Seasonality was compared on unequal months.** January had only 27 days of order data. Comparing sales per day turned "December +27%, November ≈ January" into "December +21% vs. November, +12% vs. January, and January above November".
- **Small countries topped the conversion ranking by chance.** Trinidad & Tobago, Jamaica and Malta (15–18%) had very few sessions. The ranking now includes only countries with 1,000+ sessions.
- **"Customers buy furniture in bundles" was not supported.** Every order in the data contains exactly one item; categories correlate because they all follow overall daily demand.
- **Borderline results are labelled as hypotheses.** Differences in average order value (weekday vs. weekend, registered vs. unregistered) have p ≈ 0.04 among dozens of tests.

## ⚠️ Limitations

- Only 3 months of data, including the holiday season.
- One item per order; no quantity, discount, cost or repeat-purchase data.
- No marketing spend data, so channel efficiency cannot be measured.

## 🛠 Stack

`SQL` · `Google BigQuery` · `Python` · `pandas` · `NumPy` · `SciPy` · `statsmodels` · `matplotlib` · `seaborn` · `Tableau`
