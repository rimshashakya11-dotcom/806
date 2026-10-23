# NextGen Retail Analytics – ShopSmart Case Study

Oct 18, 2026 · @Rimsha Shakya

## Introduction and data overview

ShopSmart's sample data shows $1,350 of store revenue on 1 October 2025, with product P003 close to a stockout and P001 the strongest seller across both channels. This report answers the six case-study questions using the three NextGen Retail datasets as worked evidence.

| Dataset | Rows | Fields | What it tells us |
| --- | --- | --- | --- |
| Sales (store) | 4 | Store, date, product, units sold, revenue, stock left | S002 earned $800 vs S001 $550; 70 units sold |
| Website | 4 | Date, product, page views, add-to-cart, purchases | 1,400 views led to 80 online purchases (5.7% conversion) |
| Customers | 4 | Age, gender, segment, visits in 30 days, average spend | Young adults visit most (3 and 5 visits); the senior customer visits once |

Three findings from the preprocessed data shape the answers below:

- **P003 is at risk of selling out.** S002 has 30 units left and sold 25 in a day, so about 1.2 days of cover.
- **P003 converts best online.** 8.8% of its page views became purchases, against 2.5% for P004.
- **P004 sells only online.** It has no store sales and the weakest funnel, so it is a candidate for review or promotion.

Limitation: each file holds a single day and four records. That is enough to demonstrate the methods, but a real model needs at least two years of daily history per store and product to capture trends and seasonality.

## 1. AI & ML applications

### Which ML algorithms would you use for predicting sales, and why?

I would use gradient-boosted trees (XGBoost or LightGBM) as the main forecasting model, with linear regression as a baseline and a time-series model to check it. Sales prediction is a regression problem: the target (units sold or revenue) is a continuous number.

| Algorithm | Role | Why it suits ShopSmart |
| --- | --- | --- |
| Multiple linear regression | Baseline | Simple and explainable. Shows how price, promotions and web traffic each move sales. |
| Random Forest | Robust challenger | Handles non-linear effects and mixed data. Ranks feature importance, and copes with outliers. |
| Gradient boosting (XGBoost / LightGBM) | Main model | Usually the most accurate on tabular retail data. Learns store × product × day patterns from features like day of week, holidays and stock level. |
| ARIMA / Prophet | Time-series check | Captures trend and seasonality (weekends, Christmas, Black Friday) per product. |
| LSTM neural network | Later, at scale | Learns long sequences across hundreds of products once years of data exist. Less transparent. |

Key input features from the datasets: store, product, date features, unit price (Revenue ÷ Units), current stock, and online interest (page views, add-to-cart). Online interest is a useful leading indicator. P003 had the highest cart rate (15%) and also the tightest store stock.

Models are compared on a time-based train/test split (train on earlier months, test on later ones) using MAE and RMSE. A random split would leak future information.

For demand-related yes/no questions, a classification model fits better. For example, logistic regression or a Random Forest classifier can predict "will this product sell out within 3 days?" P003 at S002 (1.2 days of cover) would be flagged.

### How can clustering improve customer targeting?

Clustering groups customers by behaviour rather than by assumption, so marketing spend goes to the groups most likely to respond. K-means is the standard choice. Inputs are scaled first so that spend ($100–$200) does not swamp visits (1–5). The elbow method or silhouette score picks the number of clusters. RFM features (recency, frequency, monetary value) make the clusters more meaningful.

Running K-means (k = 2) on age, visits and average spend separates the four sample customers like this:

| Cluster | Customers | Profile | Targeting action |
| --- | --- | --- | --- |
| Engaged regulars | C001, C002, C003 | 2–5 visits a month, $150–$200 average spend, $400–$900 monthly value | Loyalty rewards, early access, cross-sell recommendations |
| Low-engagement | C004 | 1 visit, $100 spend, $100 monthly value | Win-back offers, in-store service, reminders |

C003 alone is worth $900 a month (5 visits × $180), more than C002, C004 and half of C001 combined. Clustering with more data would separate such high-value frequent shoppers from occasional big spenders such as C002. The benefits are:

- **Personalised campaigns.** Each cluster gets its own message, channel and offer instead of one blanket promotion.
- **Better recommendations.** The online recommendation engine can start new users from their cluster's favourite products.
- **Smarter stock and pricing.** Stores serving mostly high-frequency clusters can stock deeper ranges.
- **Measurable ROI.** Campaign response is tracked per cluster, so budget moves to what works.
