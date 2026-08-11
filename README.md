# Customer Churn & Retention Analysis

A telecom company has a 26.5% customer churn rate. That number alone doesn't tell leadership why customers are leaving, or which ones matter most to save. This project digs into that question using the IBM Telco Customer Churn dataset: 7,043 customers, their account details, the services they subscribe to, and whether they churned.

## The approach

Rather than jumping straight to a model, I worked through this the way an analyst would. Calculate the headline number, break it into segments, find where risk factors compound, quantify the revenue at stake, and only then check whether a predictive model adds anything beyond what the segmentation already shows.

## What I found

Contract type is the single strongest signal. Month-to-month customers churn at 42.7%, versus 11.3% for one-year contracts and just 2.8% for two-year contracts. Churn is also heavily front-loaded: customers in their first 6 months churn at 52.9%, dropping to 9.5% by years 4-6.

Digging deeper, three risk factors compound. Customers who are on a month-to-month contract, have fiber optic internet, and have no tech support churn at 57.5%, more than double the baseline. This isn't a small group either, it's 1,796 customers, about a quarter of the customer base.

Churned customers account for a disproportionate 30.5% of monthly revenue, and the top-spending quarter of customers churns at 32.8%, above the overall average. The highest-value customers are not the safest ones.

## The model

I trained Logistic Regression, Random Forest, and XGBoost on the same features, evaluated against a naive "no one churns" baseline. Because churn is imbalanced (26.5%/73.5%), I weighted recall and ROC-AUC more heavily than raw accuracy. A model that just predicts "no churn" for everyone gets about 73% accuracy while catching zero at-risk customers.

Logistic Regression and Random Forest tied on ROC-AUC at 0.842. I'd lean toward Logistic Regression since it's simpler and just as interpretable for a business audience, with essentially the same ranking power. XGBoost catches more true churners (75% recall) at the cost of more false alarms, which is worth considering if the business would rather over-flag than under-flag risk.

The model's top features (contract type, tenure, internet service, payment method) line up with the segmentation. That's what makes it trustworthy to act on instead of a black box.

## Recommendation

Prioritize retention spend on the compounding-risk segment first: month-to-month, fiber optic, no tech support. The offer should address what's actually controllable (support, contract length), not the service itself. Second, target the first-6-month window where churn is highest in absolute terms. And weight retention spend toward high-value accounts, since they're not automatically the safest ones.

This is an exploratory analysis on public data, not a live experiment, so there's no measured before/after. But it turns "customers are leaving" into a specific, evidence-based shortlist a retention team could act on.

## Files

Customer_Churn_Analysis_Presentation.pptx is the full case study deck covering problem, methodology, findings, and recommendations. customer_churn_analysis.ipynb is the full analysis notebook, including data cleaning, segmentation, root-cause analysis, revenue impact, and model comparison. Churn_data.csv is the source data (IBM Telco Customer Churn dataset).

## Tech stack

Python, pandas, scikit-learn, XGBoost, matplotlib/seaborn, Jupyter.

## Running it

Install requirements with pip install pandas numpy scikit-learn xgboost matplotlib seaborn, then open customer_churn_analysis.ipynb in Jupyter.
