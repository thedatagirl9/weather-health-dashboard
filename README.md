# 🌤️ Weather & Health Dashboard – Influenza & Heart Attack

This is a data analytics case study where I explored how **weather patterns influence health issues**, specifically **Influenza and Heart Attacks**. The goal was to build an end-to-end project that includes data cleaning, SQL-based analysis, visualizations, and final delivery in the form of a public dashboard.

---

## 📌 Project Overview

As weather and climate continue to change globally, there is growing interest in how these environmental shifts impact public health. In this project, I analyzed a weather-disease dataset to understand the impact of **temperature** and **humidity** on the prevalence of **Influenza** and **Heart Attacks**.

### 🔧 Project Components

- **Dataset Source**: [Kaggle - Weather-Related Disease Prediction](https://www.kaggle.com/datasets/ayushmishra032/weather-related-disease-prediction-dataset)
- **Tools Used**: BigQuery (SQL), Tableau Public, Google Sheets
- **Dashboard**: [View on Tableau Public](https://public.tableau.com/views/HowWeatherAffectsHealthInfluenzaHeartAttack/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

I performed SQL analysis in **BigQuery**, exported the results via **Google Sheets**, and built an interactive dashboard using **Tableau Public (Web)**. The dashboard includes:
- A **heat map** to visualize disease cases across temperature and humidity categories  
- A **bar chart** comparing case counts across temperature bands  
- A **treemap** summarizing total cases  
- A **custom text panel** using a dummy sheet to explain the project clearly

---

## 🛠️ Tools Used

- **SQL (BigQuery Sandbox)**
- **Google Sheets**
- **Tableau Public (Web)**
- **Kaggle public dataset**

---

## 📈 Process

### 1. Dataset Preparation
- Imported the dataset into **Google BigQuery**  
- Dataset included weather fields (`Temperature (C)`, `Humidity`) and symptoms with a `prognosis` column

### 2. Business Question
- **How do temperature and humidity conditions influence the number of Influenza and Heart Attack cases?**

### 3. SQL-Based Data Processing
- Created categorized weather variables:
  - Temperature: Low (≤15°C), Moderate (15–25°C), High (>25°C)
  - Humidity: Low (≤40%), Moderate (41–70%), High (>70%)
- Filtered for only two conditions: `'Influenza'` and `'Heart Attack'`
- Aggregated counts per category using the following SQL query:

```sql
SELECT
  prognosis,
  CASE
    WHEN `Temperature (C)` <= 15 THEN 'Low'
    WHEN `Temperature (C)` > 15 AND `Temperature (C)` <= 25 THEN 'Moderate'
    ELSE 'High'
  END AS temperature_category,
  CASE
    WHEN Humidity <= 40 THEN 'Low'
    WHEN Humidity > 40 AND Humidity <= 70 THEN 'Moderate'
    ELSE 'High'
  END AS humidity_category,
  COUNT(*) AS case_count
FROM `expanded-metric-464414-a1.Weather_related_Disease_Prediction.weather_data`
WHERE prognosis IN ('Influenza', 'Heart Attack')
GROUP BY prognosis, temperature_category, humidity_category
ORDER BY prognosis, temperature_category, humidity_category;
