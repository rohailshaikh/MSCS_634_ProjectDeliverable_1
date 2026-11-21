# Project Deliverable 1: Data Collection, Cleaning, and Exploration

**Student:** Rohail Shaikh  
**Course:** Advanced Big Data and Data Mining (MSCS-634-M50)

## Dataset and purpose
The UCI Bike Sharing dataset records hourly Capital Bikeshare rentals in Washington, D.C.,
from January 1, 2011, through December 31, 2012. The included `hour.csv` has **17,379 rows
and 17 columns**, exceeding the project requirement of 500 records and 8–10 attributes.
Each row represents a recorded hour across the system, not a person or a station.

The project examines when demand is higher and how it relates to calendar and weather conditions.
Rental counts support regression; a training-defined demand class can support classification.
Weather and time features support clustering, while hour-level categorical conditions support
association mining. This deliverable covers data cleaning and EDA only.

Source: https://archive.ics.uci.edu/dataset/275/bike+sharing+dataset  
DOI: https://doi.org/10.24432/C5W894  
License: [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/).
Original CSV unchanged; cleaned CSV uses clearer names, actual years, and a timestamp.
The UCI page lists 17,389 instances; the actual hourly file contains 17,379.

## Files

| File | Purpose |
|---|---|
| `ProjectDeliverable_1_Rohail_Shaikh.ipynb` | Commented code, saved outputs, attribute dictionary, cleaning decisions, labeled plots, and interpretations. |
| `hour.csv` | Original UCI hourly dataset; keep unchanged. |
| `bike_sharing_cleaned.csv` | Verified export for later deliverables: 17,379 rows and 17 columns. |
| `requirements.txt` | Packages needed to run the notebook. |
| `README.md` | Dataset, cleaning, findings, challenges, and run instructions. |

All five files belong in the same folder. Figures are embedded in the executed notebook.

## Cleaning and exploration
1. Loaded the source using Pandas and inspected structure, ranges, categories, and summary statistics.
2. Checked missing markers and duplicate rows: **0 missing cells and 0 exact duplicates**.
   No imputation or duplicate removal was needed for this source file.
3. Parsed dates and numbers; validated category codes, normalized ranges, calendar fields,
   working-day status, nonnegative integer counts, unique date-hour keys, and rental totals.
   All validation checks passed.
4. Renamed abbreviated columns, converted year codes to actual years, removed `instant`, added
   `timestamp`, sorted chronologically, and restored categorical types in memory.
5. Identified **165 unrecorded hours**. Kept them absent rather than filling unknown demand with zeros.
6. Used IQR fences to inspect outliers. Retained **505 high-rental observations** above 642.5
   because checks did not establish errors and deleting them would discard busy-hour demand.
7. Reviewed **22 zero-humidity readings**, all on March 10, 2011. Retained them with an explicit
   sensitivity comparison; zero is not documented as a missing marker. Other valid extremes
   and calm wind readings were also retained.
8. Plotted rental and weather distributions, rental outliers, working-day hourly profiles,
   monthly patterns by year, weather categories, a correlation heatmap, and a temperature scatterplot.

## Key insights and modeling implications
- Hourly rentals are right-skewed: **mean 189.5, median 142**, maximum 977. Retain peak demand
  and examine later model errors at high counts.
- Working-day demand peaks at **8 a.m. (477.0)** and **5 p.m. (525.3)**. Non-working days have
  a midday peak at **1 p.m. (372.7)**. Consider cyclic hour features and working-day interactions.
- Mean recorded-hour demand rises from **143.8 in 2011 to 234.7 in 2012 (63.2%)**. Use
  chronological splits and calendar-based validation windows, accounting for unrecorded hours.
- Temperature and rentals have **r = 0.405**; humidity and rentals have **r = −0.323**.
  These are associations, not causal effects.
- Temperature and apparent temperature have **r = 0.988**. Compare dropping a redundant feature
  with Ridge regularization during Deliverable 2.
- Heavy precipitation has only **3 recorded hours**. Avoid broad claims from that category.
- **Casual plus registered rentals equals total rentals.** Exclude the two components from
  total-rental regression, and exclude all three counts from predictors of a derived demand class.

## Challenges and decisions
- The archived description conflicts with the current UCI page on season labels and temperature
  conversions. Use current UCI season labels and retain normalized temperatures.
- Missing hours differ from missing cells. Do not manufacture zero-demand records.
- IQR flags do not establish data errors. Keep valid high demand, document suspect humidity,
  and compare model sensitivity later.
- Calendar codes are categorical; `weekday` uses Sunday = 0. The notebook verifies this against dates.
- This is historical data from one city. It does not establish current demand, individual behavior,
  station shortages, neighborhood access, or causal weather effects.

## How to run with Anaconda
1. Extract the project folder and keep all five files together.
2. Open **Anaconda Prompt** and change to that folder using `cd /d "YOUR_PROJECT_FOLDER"`.
3. Run `python -m pip install -r requirements.txt`.
4. Run `jupyter notebook ProjectDeliverable_1_Rohail_Shaikh.ipynb`.
5. In Jupyter, choose **Kernel → Restart Kernel and Run All Cells** (or the equivalent restart-and-run-all command),
   wait for the final validation message, and save the notebook.

No internet connection is needed to read the included dataset after packages are installed.
Loading the cleaned CSV later requires `parse_dates=['date', 'timestamp']` and restoring categorical types.
Fit scalers, encoders, imputers, and any learned demand threshold inside training data in later deliverables.

## References
Fanaee-T, H. (2013). *Bike sharing* [Data set]. UCI Machine Learning Repository. https://doi.org/10.24432/C5W894

Fanaee-T, H., & Gama, J. (2014). Event labeling combining ensemble detectors and background knowledge.
*Progress in Artificial Intelligence, 2*, 113–127. https://doi.org/10.1007/s13748-013-0040-3

Wu, D. (2024). *Data mining with Python: Theory, application, and case studies*. CRC Press.

Raw `hour.csv` SHA-256: `e03de4ee4ef4dc376ac6e04bf829673c6269e8eba5c60fa121640fa2f829504f`
