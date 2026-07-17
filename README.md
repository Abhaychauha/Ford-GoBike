# 🚲 Ford GoBike (Bay Wheels) 2018 Trip Data — Exploratory Data Analysis

An end-to-end EDA of ~1.86 million Ford GoBike bike-share trips across the San Francisco Bay Area
covering all 12 months of 2018. The project merges 12 monthly datasets, cleans and engineers
features, and explores what drives trip duration and ridership patterns — with a focus on **user
type (Subscriber vs. Customer)** and **seasonality**.

![Trip Duration Distribution](assets/trip_duration_distribution.png)

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Problem Statement](#-problem-statement)
- [Dataset](#-dataset)
- [Repository Structure](#-repository-structure)
- [Getting Started](#-getting-started)
- [Analysis Approach](#-analysis-approach)
- [Key Findings](#-key-findings)
- [Business Recommendations](#-business-recommendations)
- [Sample Visualizations](#-sample-visualizations)
- [Tools & Libraries](#-tools--libraries)
- [Author](#-author)
- [License](#-license)

---

## 📖 Project Overview

Ford GoBike (now Bay Wheels) is a regional public bike-share system serving San Francisco, East
Bay, and San Jose. Each trip record captures when and where a trip started/ended, how long it
lasted, which bike was used, and rider information (subscriber vs. casual customer, birth year,
gender).

This project combines the 12 monthly 2018 CSV files into a single dataset, performs data cleaning
and feature engineering, and carries out a structured **Univariate → Bivariate → Multivariate
(UBM)** exploration with 21 charts, each paired with an insight and a business-impact takeaway.

## ❓ Problem Statement

Ford GoBike wants to understand how people use its bike-share system in order to improve
operations, pricing, and marketing. Since trip duration is closely tied to bike availability,
rebalancing costs, and revenue, the analysis answers:

1. **How long does the average trip take?**
2. **Is trip duration affected by weather/season (month)?**
3. **Does the above depend on whether a user is a Subscriber or a Customer?**

**Business objective:** identify the key factors driving trip duration and ridership patterns so
Ford GoBike can understand its revenue drivers and design targeted strategies — such as converting
frequent Customers into Subscribers and optimizing bike availability around commute peaks — to
grow ridership and revenue.

## 🗂 Dataset

- **Source:** Ford GoBike monthly trip-history data (`201801` – `201812`)
- **Size:** 12 CSV files, ~1.86 million trips combined
- **Granularity:** one row per trip

| Feature | Description |
|---|---|
| `duration_sec` | Trip duration in seconds |
| `start_time` / `end_time` | Trip start and end date & time |
| `start_station_id/name/lat/lng` | Where the trip began |
| `end_station_id/name/lat/lng` | Where the trip ended |
| `bike_id` | Unique ID of the bike used |
| `user_type` | `Subscriber` (paying member) or `Customer` (casual/one-off rider) |
| `member_birth_year` | Rider's birth year (used to derive age) |
| `member_gender` | Rider's self-reported gender |
| `bike_share_for_all_trip` | Whether the trip was part of the discounted access program |

> **Note:** Due to file size, the raw CSVs may be tracked with [Git LFS](https://git-lfs.github.com/)
> or excluded from the repo (see `.gitignore`). If you fork this repo, download the source data
> from the [Bay Wheels / Ford GoBike open data page](https://www.lyft.com/bikes/bay-wheels/system-data)
> and place the 12 monthly CSVs in the repo root (or update `DATA_PATH` in the notebook).

## 📁 Repository Structure

```
├── FordGoBike_2018_EDA.ipynb        # Main analysis notebook (merge, wrangle, EDA, insights)
├── 201801-fordgobike-tripdata.csv   # Monthly raw data files (Jan–Dec 2018)
├── ...                              # (201802 ... 201812)
├── assets/                          # Exported chart images used in this README
├── requirements.txt                 # Python dependencies
└── README.md                        # Project documentation (this file)
```

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/fordgobike-2018-eda.git
cd fordgobike-2018-eda
```

### 2. Set up the environment
```bash
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Add the data
Place all 12 monthly CSV files (`201801-fordgobike-tripdata.csv` … `201812-fordgobike-tripdata.csv`)
in the repository root, alongside the notebook. (Update the `DATA_PATH` variable near the top of
the notebook if you store them elsewhere.)

### 4. Run the notebook
```bash
jupyter notebook FordGoBike_2018_EDA.ipynb
```
Run all cells top to bottom (`Kernel → Restart & Run All`). The notebook merges the 12 files,
cleans the data, and generates all 21 charts with commentary.

## 🔍 Analysis Approach

1. **Merge** — concatenate all 12 monthly CSVs into a single DataFrame (~1,863,721 rows).
2. **Wrangle** — parse datetimes, convert duration to minutes, derive rider age, engineer
   `month`, `day_of_week`, `hour`, `season`, and `is_weekend` features.
3. **Clean** — drop rows missing station/demographic info and unrealistic outliers (age > 90,
   duration > 12 hrs), retaining **93.3%** of trips (**1,739,029** rows) for the main analysis.
4. **Explore (UBM)**:
   - **Univariate** — distribution of duration, age, user type, gender, and ride volume by
     month/day/hour.
   - **Bivariate** — duration vs. user type, season, weekday/weekend, gender; ridership by hour
     split by user type; top stations.
   - **Multivariate** — duration by season *and* user type; duration by day-type *and* user
     type; an hour × day-of-week ride-volume heatmap; a correlation heatmap; a pair plot.
5. **Synthesize** — translate each chart's insight into a business-impact takeaway and a final
   set of recommendations.

## 💡 Key Findings

- **Average trip length:** ~9 minutes (median) / ~14 minutes (mean) — the distribution is
  strongly right-skewed.
- **Season/weather has only a minor effect on duration** — mean duration varies by roughly
  **1 minute** across Winter, Spring, Summer, and Fall.
- **User type has a large effect on duration** — Customers ride far longer than Subscribers
  (median ~14.6 vs. ~8.6 minutes; mean ~23.2 vs. ~10.8 minutes), and this gap holds across every
  season and every day type.
- **Ridership mix:** Subscribers account for ~88.6% of trips vs. ~11.4% for Customers.
- **Commute pattern:** Subscribers show sharp ridership peaks at **8 AM** and **5–6 PM** on
  weekdays; Customers ride more evenly across the day and disproportionately more on weekends.
- **Demographics:** riders skew male (~73.5%) and working-age (median age ~32); age and gender
  have little effect on trip duration compared with user type.
- **Busiest stations** cluster around SF transit hubs — Caltrain, the Ferry Building, and the
  Embarcadero corridor.

## 📈 Business Recommendations

1. Build pricing/marketing strategy around **user type**, not season — the user-type effect on
   duration is ~12x larger than the seasonal effect.
2. Target **Customers for subscription conversion**, especially around their longer weekend
   trips.
3. Prioritize **bike rebalancing** around the two sharp weekday commute peaks (8 AM & 5–6 PM).
4. Offer **off-season promotions for Customers**, who appear more weather-sensitive than
   Subscribers.
5. Invest capacity at **top transit-hub stations** driving the highest trip volume.
6. Explore marketing/safety initiatives to grow the **female ridership segment**, a large
   under-tapped audience.

## 🖼 Sample Visualizations

| | |
|---|---|
| ![User Type Distribution](assets/user_type_distribution.png) | ![Trips by Hour](assets/trips_by_hour.png) |
| ![Duration by User Type](assets/duration_by_user_type.png) | ![Duration by Season and User Type](assets/duration_by_season_and_user_type.png) |

![Ride Volume Heatmap](assets/ride_heatmap_hour_day.png)

*(See the notebook for the full set of 21 charts and accompanying insights.)*

## 🛠 Tools & Libraries

- **Python 3**
- `pandas`, `numpy` — data wrangling
- `matplotlib`, `seaborn` — visualization
- `jupyter` — notebook environment

## 👤 Author

Feel free to connect or open an issue with questions/suggestions.

## 📄 License

This project is released under the [MIT License](LICENSE). The underlying trip data is provided
by Ford GoBike / Bay Wheels under their own open-data terms — see the
[Bay Wheels system data page](https://www.lyft.com/bikes/bay-wheels/system-data) for details.
