# Rimsha Shakya — MBI Portfolio (806 / 807)

Dashboards, analytics projects and presentations from my Master of Business Informatics coursework at Yoobee, plus my internship project at ACM Removals.

Every dashboard is a single self-contained HTML file. Download it and open it in any browser, no install needed. If GitHub Pages is switched on for this repo, `index.html` gives a clickable home page for all of them.

## Projects

### NZ Short-Term Rentals BI (MBI807B)
Business intelligence on Inside Airbnb New Zealand data (51,096 listings, 3.47M reviews, 18.7M calendar days): ETL in Python, a SQLite star schema, and Power BI / web dashboards showing price and demand by ward.

| File | What it is |
| --- | --- |
| [`nz-airbnb-bi/dashboard.html`](nz-airbnb-bi/dashboard.html) | NZ Airbnb BI dashboard |
| [`nz-airbnb-bi/presentation/`](nz-airbnb-bi/presentation/index.html) | 5-slide assessment presentation, with speaker notes |
| [`nz-airbnb-bi/smart-pricing-insights.html`](nz-airbnb-bi/smart-pricing-insights.html) | Smart Pricing Insights |
| [`nz-airbnb-bi/mini-dashboard-example.html`](nz-airbnb-bi/mini-dashboard-example.html) | Small teaching example: HTML, CSS and JavaScript dashboard basics |

### Rioel Aotearoa — Nationwide Dashboard
Web prototype for exploring NZ short-term rental data nationwide.

| File | What it is |
| --- | --- |
| [`rioel-aotearoa/nationwide-dashboard.html`](rioel-aotearoa/nationwide-dashboard.html) | Nationwide dashboard |
| [`rioel-aotearoa/nationwide-dashboard-v2.html`](rioel-aotearoa/nationwide-dashboard-v2.html) | Second version |

### SmartCare Health Console (smart watch data)
Health console built on smart watch data for 50 patients (sleep, stress, steps, heart rate): risk tiers, readiness scores, care plans, what-if simulator, and role-based access.

| File | What it is |
| --- | --- |
| [`smartcare/health-console.html`](smartcare/health-console.html) | Interactive health console |
| [`smartcare/powerbi-build-guide.md`](smartcare/powerbi-build-guide.md) | Step-by-step guide to rebuilding it as a 7-page Power BI report |

### ShopSmart — NextGen Retail Analytics
Case study on sales forecasting (gradient boosting, ARIMA/Prophet) and customer clustering (K-means) for a retailer.

| File | What it is |
| --- | --- |
| [`shopsmart/nextgen-retail-case-study.md`](shopsmart/nextgen-retail-case-study.md) | Case study write-up |

### Project Phoenix — KiwiSoft Board Pitch
10-slide board pitch for a 3-year hybrid human-AI strategy: workforce plan, financial projection, positioning, risks and roadmap.

| File | What it is |
| --- | --- |
| [`project-phoenix/`](project-phoenix/index.html) | Slide deck with speaker notes |

### Rioel Vehicle Tracker (internship, ACM Removals)
Web dashboard that turns vehicle GPS exports into payroll-ready start and finish times. Full project, data and reports: [Rioel-Vehicle-Tracker repo](https://github.com/rimshashakya11-dotcom/Rioel-Vehicle-Tracker).

| File | What it is |
| --- | --- |
| [`rioel-vehicle-tracker/index.html`](rioel-vehicle-tracker/index.html) | The tracker dashboard |
| [`rioel-vehicle-tracker/internship-concept-proposal.md`](rioel-vehicle-tracker/internship-concept-proposal.md) | Internship concept proposal |
| [`rioel-vehicle-tracker/letter-to-director.md`](rioel-vehicle-tracker/letter-to-director.md) | Letter to the director |

## Run it locally (localhost)

### Option 1 – Docker (recommended)
Requires [Docker Desktop](https://www.docker.com/products/docker-desktop/).

```bash
git clone https://github.com/rimshashakya11-dotcom/806.git
cd 806
docker compose up -d --build
```

Then open:
- Portfolio home: <http://localhost:8080>
- SmartCare Health Console: <http://localhost:8080/smartcare/health-console.html>  
  Sign-ins: `admin / SmartCare@2026`, `clinician / Clinic@2026`, `analyst / Analyst@2026`

Stop it with `docker compose down`.

### Option 2 – No Docker
Any static web server works, for example with Python installed:

```bash
cd 806
python -m http.server 8080
```

Then open <http://localhost:8080/smartcare/health-console.html>.

## Tools used
Python (pandas), SQLite, Power BI and DAX, HTML / CSS / JavaScript, D3.
