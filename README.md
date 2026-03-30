# Workforce Planning: Call Volume & Capacity Forecasting

This repository contains the end-to-end forecasting engine developed for the **UIUC Statistics Datathon 2026**. The project focuses on predicting daily and interval-level Call  Volume (CV) , Customer Care Time (CCT), and Abandonment Rates for four distinct business portfolios (A, B, C, and D) to optimize staffing requirements for August 2025 using historical data from 2024 and 2025.

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
| `output/` | Generated artifact - `forecast.csv` with final predictions. |
| `presentation/` | Project deliverables - slide deck and video presentation. |
| `README.md` | Project overview, methodology, and repository documentation. |
| `requirements.txt` | Python dependencies (Pandas, NumPy, Scipy, etc.). |

##  Project Overview

The challenge involved synthesizing two years of daily historical data with granular 30-minute interval data to create a modular and scalable forecasting model.

### Key Features:
* **Multi-Portfolio Analysis:** Managed four separate portfolios (A, B, C, D) with different volumes.
* **Intraday Profiling:** Derived 48-slot daily interval shapes to distribute daily forecasts into 30-minute intervals.
* **Validation Suite:** Automated integrity checks to ensure zero negative volumes and valid abandonment rates (0-1).

## Methodology

The forecasting model was built using a rigorous, a multi-step statistical pipeline designed to handle anomalies, capture trends, and optimize for business risk:

1. **Data Cleaning & "Corrupt Day" Handling:** Holiday schedules caused massive deviations in historical data (e.g., Portfolio C dropping to zero calls on a day that typically sees 20,000+). To fix this, 8 recognized holidays were flagged as "corrupt days." The missing or anomalous data was replaced using a **trimmed mean** calculated from the surrounding 4 weeks for that specific day of the week. The highest and lowest values in that 4-week sample were removed to prevent other anomalies from skewing the imputed replacement value.

2. **Daily Anchoring via YoY Growth Ratios:** Instead of a simple flat average, baseline daily volumes (the **Daily Anchors**) for August 2025 were established using Historical Day-of-Week (DOW) averages adjusted by a calculated Year-over-Year (YoY) growth ratio comparing August 2024 to the known data in 2025. 

3. **Intramonth & Seasonal Trend Analysis:** Evaluated the data for secondary trends using Ordinary Least Squares (OLS) regression to detect intramonth volume slopes, alongside a seasonal ratio analysis comparing Spring 2024/2025 baselines to the August peak.

4. **Intraday Shape Profiling:** Developed high-resolution 30-minute interval profiles using the most recent 3-month block (April–June 2025) to decompose the Daily Anchors into granular predictions.
   * **Call Volume (CV):** Distributed daily predicted totals into 30-minute slots using historical proportional weights.
   * **Average Handle Time (CCT) & Abandonment Rate (ABD):** Because these are rates/averages and cannot be distributed like raw volume, the pipeline grouped historical data by Portfolio, Day of Week, and 30-minute Interval. The historical **median** for each specific slot (e.g., Portfolio A, Monday, 09:00 AM) was calculated and applied directly to the corresponding August forecast slots.

5. **Risk-Optimized Capacity Planning (Newsvendor Model):** To address the datathon's explicit constraint regarding the "negative bias" of understaffing, the pipeline transitioned from pure forecasting to a risk-adjusted staffing model. The optimal buffer capacity was determined using the Newsvendor framework's Critical Ratio ($CR$):
   
   $$CR = \frac{C_u}{C_u + C_o}$$
   
   Where $C_u$ is the high cost of understaffing (abandoned calls, service level penalties) and $C_o$ is the lower cost of overstaffing (idle agent hourly wages). Because $C_u > C_o$, the model mathematically dictates targeting a higher percentile of the forecast distribution. Consequently, the final capacity targets are strategically buffered above the median forecast to ensure optimal Service Level attainment and mitigate financial risk.

## Vizualizations
1. **Call Volume (CV)**
   
   a. Day-of-Week Patterns

<img width="1013" height="261" alt="Screenshot 2026-03-29 at 11 35 25 PM" src="https://github.com/user-attachments/assets/d870f64a-9696-46e7-8885-a11f107a4259" />

   b. Average Monthly Distribution
   
<img width="1007" height="462" alt="Screenshot 2026-03-29 at 11 36 27 PM" src="https://github.com/user-attachments/assets/0a06db3c-2d5d-497d-b254-1dcab89e91f0" />

   c. Intraday Shape Curve
   
<img width="1012" height="332" alt="Screenshot 2026-03-29 at 11 36 12 PM" src="https://github.com/user-attachments/assets/0fe29ed7-cc9d-49ca-9512-3c3c5f154453" />


2. **Customer Care Time (CCT)**
   
   a. Duration Distribution
   
<img width="1009" height="224" alt="Screenshot 2026-03-29 at 11 37 10 PM" src="https://github.com/user-attachments/assets/96368462-149f-40b2-bede-c9a506653586" />

   b. Hourly Distribution
   
<img width="1009" height="343" alt="Screenshot 2026-03-29 at 11 36 57 PM" src="https://github.com/user-attachments/assets/405b363d-fee3-4373-97ff-5aeaa427e613" />


3. **Daily Abandon Rate**
<img width="1010" height="464" alt="Screenshot 2026-03-29 at 11 36 44 PM" src="https://github.com/user-attachments/assets/c6c1862c-6e96-48c3-9857-1fe8a25afd86" />


4. **Aug 2025 Call Volume Forecast Trends**
<img width="1011" height="276" alt="Screenshot 2026-03-29 at 11 35 45 PM" src="https://github.com/user-attachments/assets/184b61b6-ae48-4728-a938-0522c11e9772" />


## Insights
The exploratory data analysis and final modeling revealed several critical operational insights for August 2025 staffing:
* **The "Monday Effect":** Historical aggregation reveals a severe start-of-week surge. Mondays average the highest collective call volume (~40,000 calls), with traffic steadily declining throughout the workweek.
* **Weekend Volume Drops:** Call volumes experience a steep decline of over 50% on weekends compared to peak weekday levels, indicating an opportunity to heavily optimize weekend staffing overhead.
* **Portfolio C Dominance:** Across all four business lines, Portfolio C consistently drives the highest traffic. It requires the heaviest staffing allocation, with a forecasted peak of 26,625 calls/day.
* **August Peak Prediction:** The forecasting engine identifies **Monday, August 4, 2025**, as the highest-stress day for the call center in the predicted month, requiring maximum capacity planning.

  
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
