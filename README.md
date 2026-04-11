# Patient-Wait-Time-and-Appointment-Efficiency-Analytics-Dashboard

## 📌 Project Overview

This project presents a **Power BI dashboard** designed to analyze and optimize patient flow within a healthcare system. The focus is on identifying inefficiencies in **patient wait times** to improve service delivery and operational performance.

By leveraging healthcare data, the dashboard provides actionable insights into patient wait lists by analyzing wait times across different departments, enabling the identification of delays, operational bottlenecks, and opportunities to improve access to care and overall service efficiency.

---

## 🎯 Objectives
The primary goal of this dashboard is to transform raw clinical data into actionable operational intelligence. It is engineered to address five core pillars:

* **Optimize Resource Allocation**: Provide a granular view of patient distribution across case types (Inpatient, Outpatient, Day Case) to ensure beds, staff, and equipment are deployed where demand is most critical.

*  **Enhance Workforce Scheduling**: Align staffing levels with patient arrival patterns by identifying peak periods across various demographic age groups.

*  **Improve Patient Flow & Throughput**: Detect and mitigate bottlenecks in the patient journey by monitoring wait-time KPIs and service efficiency metrics.

*  **Monitor Clinical Specialty Demand**: Identify medical departments under extreme pressure to guide strategic investment and balanced resource growth

---

## 📊 Key Features

* Interactive **wait time analysis** by department, age group, and time of day
* Time-band analysis for peak congestion periods
* KPI cards for quick performance monitoring

---

## 🛠️ Tools & Technologies

* **Power BI** (Data visualization & dashboarding)
* **DAX** (Calculated measures & KPIs)
* **Excel / CSV** (Data source)
---

## 📂 Dataset

### 🏥 Inpatient Dataset (2018–2021)

This dataset contains information related to inpatient waitlists across different specialties and demographic groups, covering the period **2018 to 2021**.

#### **Columns**
- **Archive_Date**: The date when the data snapshot was recorded (2018–2021)  
- **Specialty_HIPE**: Unique identifier for the medical specialty  
- **Specialty_Name**: Name of the medical specialty  
- **Case_Type**: Type of case (e.g., elective, emergency)  
- **Adult_Child**: Indicates whether the patient is an adult or child  
- **Age_Profile**: Age grouping of patients  
- **Time_Bands**: Categorization of waiting time durations  
- **Total**: Number of patients within each category  

---

### 🏥 Outpatient Dataset (2018–2021)

This dataset captures outpatient waitlist data, focusing on patient distribution across specialties and waiting times, covering the period **2018 to 2021**.

#### **Columns**
- **Archive_Date**: The date when the data snapshot was recorded (2018–2021)  
- **Specialty_HIPE**: Unique identifier for the medical specialty  
- **Speciality**: Name of the medical specialty  
- **Adult_Child**: Indicates whether the patient is an adult or child  
- **Age_Profile**: Age grouping of patients  
- **Time_Bands**: Categorization of waiting time durations  
- **Total**: Number of patients within each category  

---

### 🔗 Specialty Mapping Dataset

This dataset provides a mapping between individual specialties and their broader specialty groups.

#### **Columns**
- **Speciality**: Name of the medical specialty  
- **Speciality_Group**: Higher-level grouping of related specialties  

---

## 🧹 Data Preprocessing and Transformation

The following steps were performed to prepare the inpatient and outpatient datasets for integration and analysis:

- **Data Standardization**  
  Standardized column structures across both inpatient and outpatient datasets to enable seamless merging into a unified dataset (*All_Data*).

- **Column Renaming**  
  Renamed columns to ensure consistency in naming conventions (e.g., aligning *Specialty_Name* and *Speciality*).

- **Handling Missing Values**  
  Removed null or irrelevant columns to improve data quality and reduce noise in analysis.

- **Data Cleaning (Time Bands)**  
  Standardized inconsistent values by combining:
  - `18 Months+`
  - `18+ Months`  
  into a single unified category for accurate reporting.

---

## 🧩 Data Modelling

The data model was designed to integrate inpatient and outpatient datasets into a unified structure for analysis and reporting.

---

### 🔗 Dataset Integration

- The **Inpatient** and **Outpatient** datasets were combined to form a single consolidated dataset called **`All_Data`**.
- This unified dataset enables consistent analysis across both patient types.

<p align="center">
  <img src="https://github.com/user-attachments/assets/e98ef374-d034-492d-b3b8-5aca95ac465c" alt="All Data Model" width="600"/>
</p>

---

### 🔗 Relationship with Mapping Table

- The **`All_Data`** table was joined with the **Mapping Speciality** table.
- The join was performed using the **`Speciality_Name`** column.
- This relationship allows specialties to be grouped into higher-level categories for better aggregation and reporting.

<p align="center">
  <img src="https://github.com/user-attachments/assets/82055776-5334-407a-beab-d945cf041b0b" alt="Mapping Relationship" width="600"/>
</p>
 
---

## 📌 All Measures

```DAX
-- Average Wait List
Average Wait List = 
AVERAGE(All_Data[Total])

-- Median Wait List
Median Wait List = 
MEDIAN(All_Data[Total])

-- Dynamic Average / Median
Avg/Median Wait List =
SWITCH(
    VALUES('Calculation Method'[Calc Method]),
    "Average", [Average Wait List],
    "Median", [Median Wait List]
)

-- Latest Month Wait List
Latest Month Wait List =
CALCULATE(
    SUM(All_Data[Total]),
    All_Data[Archive_Date] = MAX(All_Data[Archive_Date])
)

-- Previous Year Latest Month Wait List
PY Latest Month Wait List =
VAR CurrentMaxDate = MAX(All_Data[Archive_Date])
RETURN
CALCULATE(
    SUM(All_Data[Total]),
    MONTH(All_Data[Archive_Date]) = MONTH(CurrentMaxDate),
    YEAR(All_Data[Archive_Date]) = YEAR(CurrentMaxDate) - 1
) + 0

-- No Data Indicator
NoDataLeft =
IF(
    ISBLANK(
        CALCULATE(
            SUM(All_Data[Total]),
            All_Data[Case_Type] <> "Out Patient"
        )
    ),
    "No Data for selected criteria",
    " "
)

-- Dynamic Title
Dynamic Title =
SWITCH(
    VALUES('Calculation Method'[Calc Method]),
    "Average", "Key Indicators - Patient Wait List (Average)",
    "Median", "Key Indicators - Patient Wait List (Median)"
)
```
---

## 🚀 Live Dashboard
👉 [View Interactive Dashboard](https://app.powerbi.com/view?r=eyJrIjoiMDM4ZGRjZjEtYzJkMi00YmY4LTliNTktMTZjNDNiN2Y0ZjNjIiwidCI6IjBmYmVhNWZhLWNiN2UtNDllYS1hYzgyLTJmYTBmZTllZjY5YyJ9)

---

## 📸 Dashboard Preview
<img width="869" height="488" alt="image" src="https://github.com/user-attachments/assets/fb07272c-ebc6-4f1a-9874-fec40837c0e0" />
<img width="867" height="441" alt="image" src="https://github.com/user-attachments/assets/fa065225-9c43-4e4a-9758-2c41df4fa420" />

---

## 📊 Results & Insights

- The **total wait list** for the latest archived month was **709,000**, representing an increase of **640,000 compared to the previous year**.

- **Patient distribution**:
  - Outpatient: **74%**
  - Day Case (Inpatient): **15%**
  - Inpatient: **10%**

- **Wait Time Analysis**:
  - The **0–3 months** category recorded the highest number of cases.
  - The **40–60 age group** was the most dominant among patients.

- **Top 5 Specialties (Median Wait List)**:
  - Accident & Emergency: **70**
  - Dermatology: **35**
  - Clinical (Medical): **29**
  - Cardiology: **28**
  - Pain Relief: **27**

- **Top 5 Specialties (Average Wait List)**:
  - Paediatric Dermatology: **168**
  - Paediatric ENT: **148**
  - Paediatric Orthopaedic: **115**
  - Accident & Emergency: **111**
  - Paediatric Cardiology: **102**

- **Monthly Trend**:
  - Outpatient volumes peaked at **~5.4K in 2019** but declined to **~3.3K by 2021**.
  - Day Cases showed steady growth, exceeding **300 cases in 2020**.
  - Inpatient volumes remained **low and stable**, rarely exceeding **27 cases**.
  - Overall trend suggests a shift toward **shorter-stay treatments**.

---

## 💡 Recommendations

- **Optimize Outpatient Services**  
  Address the decline in outpatient volumes by improving scheduling efficiency and reducing bottlenecks.

- **Expand Day Case Capacity**  
  Invest in day-case infrastructure, as demand is increasing and aligns with shorter treatment cycles.

- **Target High-Wait Specialties**  
  Prioritize resource allocation for specialties with high median and average wait lists (e.g., A&E, Dermatology).

- **Focus on Early-stage Wait Times (0–3 Months)**  
  Implement early intervention strategies to manage the largest patient group effectively.

- **Age-Specific Planning**  
  Tailor services and resource allocation to the **40–60 age group**, which represents the majority of patients.

---
## 🤝 Contribution

Contributions, feedback, and suggestions are welcome. Feel free to fork the repository and submit a pull request.

---

## 📧 Contact

For questions or collaboration opportunities, reach out via GitHub or LinkedIn.
