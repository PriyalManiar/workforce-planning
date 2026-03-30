# Workforce Planning: Call Volume & Capacity Forecasting

This repository contains the end-to-end forecasting engine developed for the **UIUC Statistics Datathon 2026**. The project focuses on predicting daily and interval-level call volumes, Average Handle Time (CCT), and Abandonment Rates for four distinct business portfolios (A, B, C, and D) to optimize staffing requirements for August 2025.

##  Team Members

Priyal Maniar - priyalm2@illinois.edu, UIUC, (MSIM’26 Grad Student)

Prisha Singhania - pds4@illinois.edu, UIUC, (MSIM’26 Grad Student)

Kayla Sison 


##  Project Structure

| **File / Folder** | **Function** |
|---|---|
| `data/` | Contains `data.xlsx` (Daily/Interval historical data) and `template_forecast_v00.csv`. |
| `notebooks/` | Directory containing the analytical pipeline. |
| `notebooks/eda.ipynb` | Exploratory Data Analysis: Data quality assessment and trend visualization. |
| `notebooks/forecasting.ipynb` | The core forecasting engine: Implements linear regression and seasonal profiles. |
| `outputs/` | Generated artifacts, including `forecast.csv` with final predictions. |
| `presentations/` | Project deliverables, including the `slide_deck.pptx`. |
| `README.md` | Project overview, methodology, and repository documentation. |
| `requirements.txt` | Python dependencies (Pandas, NumPy, Scipy, etc.). |

##  Project Overview

The challenge involved synthesizing two years of daily historical data with granular 30-minute interval data to create a high-fidelity forecast.

### Key Features:
* **Multi-Portfolio Analysis:** Managed four separate portfolios (A, B, C, D) with unique volume signatures.
* **Intraday Profiling:** Derived 48-slot daily interval shapes to distribute daily forecasts into actionable schedules.
* **Validation Suite:** Automated integrity checks to ensure zero negative volumes and valid abandonment rates (0-1).

##  Methodology

1.  **Exploratory Data Analysis (EDA):** Identified peak volume days (Mondays) and quantified the impact of weekends.
2.  **Daily Forecasting:** Utilized Linear Regression to model growth trends and project daily "anchor" volumes.
3.  **Interval Distribution:** Applied historical interval weights to generate 30-minute granularity predictions.

## How to Run

### Prerequisites
* Python 3.8+
* [Optional] Virtual Environment (`venv` or `conda`)

### Setup
1. **Clone the repository:**
   ```bash
   git clone [https://github.com/PriyalManiar/workforce-planning.git](https://github.com/PriyalManiar/workforce-planning.git)
   cd workforce-planning
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the analysis:**
   * First, run `notebooks/eda.ipynb` to view the data quality assessment and trends.
   * Then, run `notebooks/forecasting.ipynb` to generate the `outputs/forecast_v39.csv` file.

## Tech Stack

* **Analysis:** Python (Pandas, NumPy)
* **Modeling:** SciPy (Linear Regression, Statistical Profiling)
* **Visualization:** Matplotlib, Seaborn

---
*Developed during the UIUC Statistics Datathon 2026 by Team data bytes.*
```
