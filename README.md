# Indian-E-commerce-analytics
End-to-end analytics project on a retail dataset: Power BI for dashboards, Python for customer segmentation, and Excel as the hand-off layer between the two.

Dataset: 250,000 orders · 40,000 customers · 2,000 products · Jun 2024 – Jun 2026

Tech Stack
Tool	Used for
Power BI (DAX, Power Query)	4-page dashboard on a star-schema model with 22 measures (Net Sales, YoY Growth, Return Rate, Avg Order Value, etc.)
Python (Pandas, NumPy, Matplotlib, scikit-learn)	Data cleaning, EDA, RFM feature engineering, K-Means clustering, cohort retention, CLV estimate
Excel	Export of enriched customer segments, consumed back by the Power BI model
Workflow
Raw tables (sales, customers, products, calendar)
        │
        ├──► Power BI: data model + DAX measures + 4 dashboard pages
        │
        └──► Python notebook:
                 clean & merge → EDA → RFM → K-Means (k=4) → cohort retention → CLV
                                                   │
                                                   ▼
                                     customer_segments_export.xlsx
                                                   │
                                                   ▼
                                  Back into Power BI as a slicer-ready column
Power BI Dashboard Pages
Sales & Operations Summary: headline KPIs, monthly trend, category and brand performance
Customer Insights: age group, customer tier, state distribution, top customers
Orders & Fulfillment Trends: order volume vs net sales, cancelled vs returned value
Category & Brand Performance: category table with YoY growth and return rate, top brands
Python Analysis Highlights
RFM segmentation: K-Means (k=4, chosen via elbow method) splits customers into Champions, Loyal Customers, At Risk, Lapsed / Low-Value
Cohort retention: retention settles around ~20% after month 1 and stays flat, rather than declining steadily
Revenue leakage: cancellations and returns together remove roughly 10% of gross order value
CLV: simplified historical-rate projection (a heuristic, not a BG/NBD model; see limitations)
Project Structure
Retail_Customer_Analytics_Project/
├── Retail_Customer_Analytics.ipynb     # full analysis notebook (executed)
├── PrateekshaProject.pbix              # Power BI dashboard
├── customer_segments_export.xlsx       # Python output → Power BI
├── data/                               # customers, products, sales, calendar CSVs
├── charts/                             # exported PNGs from the notebook
└── README.md
How to Run
bash
pip install pandas numpy matplotlib scikit-learn openpyxl jupyter
jupyter notebook Retail_Customer_Analytics.ipynb

Run all cells top to bottom. The notebook reads from data/ and writes the Excel export and charts to the project root. Open the .pbix in Power BI Desktop to view the dashboards.

Limitations
CLV is a simple annualized spend projection with a 90-day tenure floor and a 99th-percentile cap on outliers. A production version would use a probabilistic model (e.g. BG/NBD + Gamma-Gamma via the lifetimes library).
Total Profit and Profit Margin % measures are placeholders; the source data has no cost field.
The dataset appears to be synthetic, so patterns like flat cohort retention may not reflect real customer behavior.
Possible Next Steps
Churn classifier on RFM + demographic features
Streamlit app for live customer segment lookup
Scheduled refresh of the Excel export so Power BI always shows current segments
