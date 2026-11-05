# F1-Race-Intelligence-System 

# F1 Race Intelligence System

A data-driven engineering project exploring Formula 1 race performance, tyre degradation, prediction and strategy using real-world race data.

The long-term goal is to develop an intelligent race analysis and strategy system that combines **data engineering, mathematics, statistics, machine learning and optimisation**.

---

## Project Vision

Formula 1 generates large amounts of race and telemetry data. This project investigates how that data can be transformed into useful engineering intelligence.

The system will progressively answer questions such as:

- How does lap performance change throughout a race?
- How does tyre degradation affect lap time?
- Which drivers are fastest and most consistent?
- How do sector performances differ between drivers?
- Can future lap times be predicted?
- How can tyre strategy and pit-stop timing be optimised?
- Can machine learning support race-strategy decisions?

The project is being developed progressively through multiple versions rather than attempting to build the complete system at once.

---

# Current Version: V1 — Race Data Intelligence

V1 focuses on establishing a reliable foundation for analysing real Formula 1 race data.

### V1 Objectives

- Acquire real F1 race data through an API.
- Understand the structure and quality of the data.
- Clean and validate race records.
- Analyse driver lap-time performance.
- Investigate lap-time consistency.
- Analyse sector performance.
- Investigate tyre stints and degradation.
- Produce meaningful engineering findings from the race data.

---

## Data Source

Current data is retrieved from the **OpenF1 API**.

The project currently uses the:

**2024 Abu Dhabi Grand Prix**

Circuit:

**Yas Marina Circuit**

OpenF1 session key:

```text
9662
```

OpenF1 provides structured Formula 1 data through REST API endpoints.

---

# V1 Progress

| Component | Status |
|---|---|
| Project structure | Complete |
| API connection | Complete |
| 2024 race-session retrieval | Complete |
| Abu Dhabi session identification | Complete |
| Lap-data retrieval | Complete |
| Data inspection | Complete |
| Missing-value investigation | Complete |
| Duplicate investigation | Complete |
| Initial data cleaning | Complete |
| Driver information retrieval | Complete |
| Driver-performance analysis | Complete |
| Average lap-time visualisation | Complete |
| Driver consistency analysis | In progress |
| Sector analysis | Planned |
| Tyre-stint analysis | Planned |
| Tyre degradation modelling | Planned |
| V1 conclusions | Planned |

---

# Data Acquisition

The project initially explored FastF1 but moved to OpenF1 for this version because the required historical timing data was not accessible without the appropriate F1 TV access.

OpenF1 is accessed using Python's `requests` library.

Example:

```python
url = "https://api.openf1.org/v1/sessions"

params = {
    "year": 2024,
    "session_name": "Race"
}

response = requests.get(
    url,
    params=params,
    timeout=30
)

response.raise_for_status()
```

The API response is converted into Pandas DataFrames for analysis.

---

# Dataset

The 2024 Abu Dhabi Race session initially returned:

```text
1,037 lap records
```

The dataset contains 16 fields, including:

- `driver_number`
- `lap_number`
- `date_start`
- `duration_sector_1`
- `duration_sector_2`
- `duration_sector_3`
- `i1_speed`
- `i2_speed`
- `lap_duration`
- `is_pit_out_lap`
- `st_speed`
- sector segment information

---

# Data Quality

Missing values were investigated before analysis.

Some fields contain incomplete measurements, which is expected in real-world race data.

Duplicate detection initially produced:

```text
TypeError: unhashable type: 'list'
```

This occurred because some sector-segment columns contain Python lists.

Instead of checking complete rows, lap identity was checked using:

```text
session_key
driver_number
lap_number
```

The result was:

```text
Duplicate laps: 0
```

This confirmed that each driver/lap combination appears only once in the retrieved dataset.

---

# Data Cleaning

A separate working DataFrame was created:

```python
race_data = laps.copy()
```

For the initial lap-performance analysis:

1. Records with missing `lap_duration` were removed.
2. Pit-out laps were excluded.

This produced:

```text
Original records: 1,037
Cleaned records: 1,005
```

The original API data remains preserved in `laps`.

---

# Initial Performance Analysis

Driver lap performance was aggregated using:

- Mean lap time
- Minimum lap time
- Maximum lap time
- Standard deviation
- Number of recorded laps

The current analysis shows an important distinction between **speed** and **consistency**.

For example, in the current dataset:

```text
Driver #4 — Norris
Average lap time: 89.232 seconds
```

This was the lowest average lap time among the drivers in the cleaned dataset.

However, the driver with the lowest standard deviation was different.

Therefore:

> A driver can have a faster average lap time without having the most consistent lap times.

This distinction will be investigated further using lap distributions, sector performance and race conditions.

---

# Current Analytical Pipeline

The current V1 pipeline is:

```text
OpenF1 API
      ↓
Session Selection
      ↓
Lap Data Retrieval
      ↓
Data Inspection
      ↓
Data Quality Checks
      ↓
Cleaning
      ↓
Driver Data Integration
      ↓
Statistical Analysis
      ↓
Visualisation
      ↓
Engineering Findings
```

---

# Planned V1 Analysis

### 1. Driver Consistency

Investigate variation in lap times using standard deviation and lap-time distributions.

### 2. Sector Performance

Compare drivers across:

- Sector 1
- Sector 2
- Sector 3

This will help determine where performance differences occur around the circuit.

### 3. Lap-Time Distribution

Investigate how lap times change throughout the race and identify unusual periods.

### 4. Tyre Data

Retrieve tyre/stint information and connect tyre compound and stint length with lap performance.

### 5. Tyre Degradation

Investigate whether lap time changes systematically as a tyre stint progresses.

A simplified model may initially take the form:

```text
Lap Time = Baseline Pace + Degradation × Tyre Age + Other Effects
```

The actual model will be determined after examining the data.

### 6. V1 Engineering Conclusions

The final stage of V1 will document:

- What the data shows
- Which patterns are statistically meaningful
- Limitations of the analysis
- What should be investigated in V2

---

# Future Versions

## V2 — Predictive Race Intelligence

The next version will move from describing race performance to predicting it.

Potential work:

- Feature engineering
- Regression
- Lap-time prediction
- Tyre degradation prediction
- Model evaluation
- Prediction uncertainty

---

## V3 — Race Strategy Optimisation

The system will progress toward decision-making.

Potential work:

- Pit-stop simulation
- Tyre strategy comparison
- Stint modelling
- Race scenario simulation
- Mathematical optimisation
- Strategy recommendations

---

## V4 — Intelligent Race Engineering System

The long-term system will combine the predictive and optimisation components.

Potential features:

```text
Race Data
    ↓
Feature Engineering
    ↓
Performance Models
    ↓
Tyre Degradation Model
    ↓
Race Simulation
    ↓
Strategy Optimisation
    ↓
Engineering Recommendation
```

The final objective is not simply to visualise historical races, but to build a system capable of using data and models to support race-engineering decisions.

---

# Technologies

### Programming

- Python

### Data

- Pandas
- NumPy
- REST APIs
- JSON

### Visualisation

- Matplotlib

### Mathematics / Statistics

- Descriptive statistics
- Mean
- Variance
- Standard deviation
- Regression
- Least squares
- Linear algebra

### Future

- Machine learning
- Optimisation
- Simulation
- Predictive modelling

---

# Project Structure

```text
F1-Race-Intelligence-System/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── notebooks/
│   └── 01_initial_data_exploration.ipynb
│
├── src/
│   ├── data_loader.py
│   ├── data_cleaning.py
│   └── performance_analysis.py
│
├── visualizations/
│
├── README.md
│
└── requirements.txt
```

---

# Engineering Philosophy

This project is being developed as an engineering and research-oriented system rather than a simple dashboard.

Each stage should answer a meaningful question using:

1. Real data
2. Appropriate mathematical/statistical methods
3. Reproducible code
4. Visual evidence
5. Critical interpretation
6. Awareness of limitations

The project will progressively move from:

**Data → Analysis → Prediction → Simulation → Optimisation**

---

# Status

**Current version: V1 — Race Data Intelligence**

**Current stage: Driver performance and race-data analysis**

The system is still under active development.