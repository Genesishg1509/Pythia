# Pythia — Cafe Madrid Analytics App

A Streamlit web app that analyzes a coffee shop's business and predicts future sales. It was built as part of a Master's thesis (TFM) project, and this repository is a cleaned, portfolio version of that work.

## The problem, the technique, and why

**Problem:** a classic coffee shop in Madrid needs to know how much it will sell in the next few days — both total revenue and units of its top-selling product categories — so it can plan stock and avoid running out of ingredients or over-buying perishables. Demand also depends on things the shop doesn't control: weather, nearby foot traffic (like visitor numbers at CaixaForum, a museum right next door), and public holidays. Beyond sales, the shop also wanted to understand its customers better: what they praise and complain about, and how it compares to nearby competitor cafes.

**Technique:**
- **Sales forecasting:** a separate [Prophet](https://facebook.github.io/prophet/) time series model per target (total revenue, classic coffee, pastries, breakfast items), using weather, the public holiday calendar, and estimated nearby foot traffic as extra input signals, to forecast the next 14 days.
- **Own reviews:** aspect-based sentiment analysis, breaking feedback down by topic (coffee, service, price, atmosphere, etc.) instead of a single sentiment score, plus a comparison of 4 classic ML models (Logistic Regression, Random Forest, XGBoost, SVM) for sentiment classification — see `notebooks/`.
- **Competitor reviews:** only the *positive* reviews of nearby competitor cafes were analyzed, to see which products customers mention most and what those cafes are valued for.

**Why:**
- Prophet over a generic regression, because cafe sales follow strong weekly and seasonal patterns, and Prophet makes it easy to add weather/holidays/foot-traffic as extra signals while keeping the forecast explainable to a non-technical business owner — not just a number, but *why* the number is what it is.
- Aspect-based analysis over a single sentiment score, because "customers are unhappy" isn't actionable on its own — "customers are unhappy with the service, not the coffee" tells the owner exactly what to fix.
- Competitor reviews were filtered to the positive ones on purpose: the goal wasn't to criticize other cafes, it was to benchmark what's already working in the market — which products and qualities customers value most nearby — as a source of ideas.

## Group project

This project was built by a team of 6 students for the **Master in Data Science, Big Data & Business Analytics 2024–2025** at Universidad Complutense de Madrid (UCM):

- Ilan Alexander Arvelo Yagua
- Anabel Jose Baéz Rodríguez
- Genesis Karollay Hernández Gallegos
- Luca Iacomino
- Marcio Yassuhiro Iha
- Fatima Tawfik Vázquez

This specific repository is maintained by **Genesis Karollay Hernández Gallegos** as a personal portfolio piece, with a smaller, cleaned-up copy of the original team project. It keeps the real analysis and models, but removes all private business data (real invoices, customer data, passwords) so it can be shown and shared publicly.

This project won **2nd Prize in the ntic master's Scholarship Competition (Becas)** for the Data Science, Big Data & Business Analytics program, awarded by ntic master and Universidad Complutense de Madrid.

![The team after winning 2nd prize](images/team_2nd_prize_ntic_master.jpg)

## What the app does

The app has 3 pages:

1. **Homepage** — introduction to the project and the team.
2. **Cafe Predictions** — forecasts sales for the next 14 days using [Prophet](https://facebook.github.io/prophet/) time series models. It predicts:
   - Total revenue (€)
   - Classic coffee units sold
   - Pastries & sweets units sold
   - Breakfast/toast units sold

   Each forecast can be shown under 3 scenarios (baseline, low, and high), built from average weather data and estimated visitor numbers from a nearby museum (CaixaForum).

3. **Reviews** — analyzes customer reviews to find what people like and dislike, grouped by topic (coffee, service, price, atmosphere, etc.) and by sentiment. It also includes a competitor analysis view, comparing nearby cafes on a map and showing which products and qualities stand out most in their *positive* reviews.

Both data pages work out of the box with a small demo dataset, so anyone can try the app without uploading any file.

## Tech stack

- **Streamlit** — web app framework
- **Prophet** — time series forecasting
- **Pandas / NumPy** — data processing
- **Plotly** — interactive charts
- **scikit-learn / XGBoost** — sentiment classification models (see `notebooks/`)
- **spaCy / TextBlob** — text processing for the reviews analysis

## Project structure

```
Homepage.py                  Main app entry point
pages/                        The 2 other Streamlit pages (Predictions, Reviews)
data_pipeline_etl/            Code that turns raw invoice text into model-ready data
models/                       Trained Prophet models (.joblib files)
notebooks/                    Jupyter notebooks with the original model training and analysis
data/demo/                    Small, safe demo datasets (no real business data)
images/                       Logos, charts, and the competitor map
```

## Notebooks

The `notebooks/` folder has the real data science work behind the app:

- 4 notebooks that train the Prophet forecasting models (one per prediction)
- 2 notebooks that build the customer review analysis: sentiment classification (comparing Logistic Regression, Random Forest, XGBoost and SVM) and aspect-based sentiment analysis (ABSA)

These notebooks were built and run in Google Colab, so some cells (like `!wget` or `google.colab.userdata`) will only run there. They are included here to document the methodology.

## Running it locally

```bash
pip install -r requirements.txt
streamlit run Homepage.py
```

The app will open with demo data already loaded, so no extra setup is needed to try it out.

## A note on data privacy

This repository only contains data that is safe to share publicly: precomputed demo datasets, aggregated review insights, and trained model files. No real invoices, customer information, or credentials are included anywhere in this repo.
