![Restaurant Recommendation Dashboard](./images/Restaurant_Dashboard_Cover.png)

# 🍽️ Restaurant & Consumer Rating Dashboard | Consumer Behavior & Restaurant Performance Analysis | Power BI

_Analyzing consumer behavior and restaurant rating performance to identify target customer segments and the operational factors driving customer experience | Power BI_

**+ Business question:** Which restaurants, which customer segments, and which operational factors (parking, smoking area, price tier...) are truly driving positive customer experience — and where are the bottlenecks causing a high share of negative ratings?

**+ Domain:** F&B / Restaurant & Consumer Analytics (Restaurant — food, service ratings, and consumer behavior)

---

## 📑 Table of Contents
1. [📌 Background & Overview](#-background--overview)
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)
3. [🧠 Design Thinking Process](#-design-thinking-process)
4. [⚒️ Main Process](#️-main-process)
5. [📊 Key Insights & Visualizations](#-key-insights--visualizations)
6. [🛠️ Skills & Tools Applied](#️-skills--tools-applied)
7. [🔎 Final Conclusion & Recommendations](#-final-conclusion--recommendations)

---

## 📌 Background & Overview

### 📖 What is this project about?

This project analyzes **Restaurant & Consumer Rating data** using **Power BI**, building an interactive dashboard with **3 main pages**. Objectives:

✔️ Track the overall health of the restaurant system — total restaurants, total ratings, positive rating rate.
✔️ Compare rating performance across cities, price tiers, and service types to identify strengths/weaknesses.
✔️ Analyze operational factors (parking, smoking area, franchise) that impact the positive rating rate.
✔️ Profile customers by age, occupation, marital status, and drinking habits.
✔️ Identify the most preferred cuisines/foods to guide menu direction.
✔️ Provide interactive filters (Cuisine, Restaurant, Group Age, Drink Level) to explore the data from multiple angles.

The dashboard is split into **3 pages** — Overview, Restaurants, Customers — letting users move from the big picture down to detailed customer behavior.

### 👤 Who is this project for?

✔️ Data analysts & business analysts
✔️ Restaurant owners / F&B operations managers
✔️ Marketing & CRM teams
✔️ Investors / parties evaluating restaurant chain expansion potential

---

## 📂 Dataset Description & Data Structure

### 📌 Data Source
- **Source:** Restaurant & Consumer Data (a popular public dataset, commonly sourced from the UCI Machine Learning Repository / Kaggle, collected across cities in Mexico)
- **Coverage:** 130 restaurants, 138 consumers, 1,161 ratings
- **Format:** `.csv`, modeled as a star schema in Power BI

### 📊 Data Structure & Relationships

#### 1️⃣ Table Schema

**Table 1: `fact_ratings`** (Fact table — one row per rating)

| Column Name | Data Type | Description |
|---|---|---|
| Consumer_ID | TEXT | Unique identifier of the reviewing consumer |
| Restaurant_ID | TEXT | Unique identifier of the rated restaurant |
| Food_Rating | INT | Food rating score (0 = Negative, 1 = Neutral, 2 = Positive) |
| Service_Rating | INT | Service rating score |
| Overall_Rating | INT | Overall rating score |

**Table 2: `dim_restaurants`** (Dimension — restaurant information)

| Column Name | Data Type | Description |
|---|---|---|
| Restaurant_ID | TEXT | Unique restaurant identifier |
| Name | TEXT | Restaurant name |
| City | TEXT | City |
| Area | TEXT | Area |
| Country | TEXT | Country |
| Latitude / Longitude | DECIMAL | Location coordinates |
| Alcohol_Service | TEXT | Whether alcohol is served |
| Parking | TEXT | Parking type (Yes/None/Public/Valet) |
| Franchise | TEXT | Whether it's a franchise chain |
| Price | TEXT | Price tier (Low/Medium/High) |
| Smoking_Allowed | TEXT | Whether smoking is allowed |

**Table 3: `dim_consumers`** (Dimension — consumer information)

| Column Name | Data Type | Description |
|---|---|---|
| Consumer_ID | TEXT | Unique consumer identifier |
| Age | INT | Consumer age |
| Age Group | TEXT | Age group (18-25, 26-35, 60+...) |
| Budget | TEXT | Spending budget level |
| Children | TEXT | Whether they have children |
| Marital_Status | TEXT | Marital status (Single/Married) |
| Drink_Level | TEXT | Drinking level (Abstemious/Casual Drinker/Social Drinker) |
| City / Country | TEXT | Place of residence |

**Table 4: `dim_restaurant_cuisines`** (Dimension — restaurant cuisine type)

| Column Name | Data Type | Description |
|---|---|---|
| Restaurant_ID | TEXT | Restaurant identifier (links to `dim_restaurants`) |
| Cuisine | TEXT | Cuisine type (Mexican, American, Fast Food, Cafeteria, Pizzeria...) |

**Table 5: `dim_consumer_preferences`** (Dimension — consumer food preferences)

| Column Name | Data Type | Description |
|---|---|---|
| Consumer_ID | TEXT | Consumer identifier (links to `dim_consumers`) |
| Preferred_Cuisine | TEXT | Cuisine type preferred by the consumer |

#### 2️⃣ Data Relationships

The model follows an **extended star schema**, with `fact_ratings` as the central table, connected to the dimension tables:

- `dim_restaurants` (1 → \*) via `Restaurant_ID` → analysis by city, price tier, parking...
- `dim_consumers` (1 → \*) via `Consumer_ID` → analysis by age, drinking habits, marital status...
- `dim_restaurant_cuisines` (1 → \*) via `Restaurant_ID` → analysis by the cuisine type each restaurant serves
- `dim_consumer_preferences` (1 → \*) via `Consumer_ID` → analysis by the consumer's preferred cuisine

![Data Model](./images/Model_View.png)

---

## 🧠 Design Thinking Process

Before building the dashboard, a **stakeholder requirement analysis** was conducted using the Design Thinking framework to avoid designing based on assumptions.

**1️⃣ Empathize**

![Step 1 - Empathize](./images/design_thinking_01_empathize.svg)

Identified the primary stakeholder (**Restaurant Owner / Operations Manager**), someone who needs to quickly grasp customer sentiment but doesn't have time to read raw data row by row.

**2️⃣ Define**

![Step 2 - Define](./images/design_thinking_02_define.svg)

Problem statement: *"The restaurant owner needs a visual, easy-to-understand dashboard to quickly identify which customer segments, operational factors, and cuisine types are driving positive experiences — enabling confident decisions on service improvement and menu direction."*

**3️⃣ Ideate**

![Step 3 - Ideate](./images/design_thinking_03_ideate.svg)

Mapped out the decision points that needed support: identifying well-rated restaurants, pinpointing operational factors that affect ratings, profiling customers by age/occupation, and identifying preferred cuisines.

**4️⃣ Prototype & Review**

![Step 4 - Prototype and Review](./images/design_thinking_04_prototype.svg)

Structured the report into **3 focused pages** (Overview → Restaurants → Customers) so the dashboard can be understood within 30 seconds, matching how it's actually used in quick operational review meetings.

---

## ⚒️ Main Process

1️⃣ **Data Cleaning & Preprocessing**
- Standardized labels for Parking, Price, Smoking Allowed, Drink Level
- Checked and removed duplicate Consumer_ID / Restaurant_ID, handled blank values

2️⃣ **Exploratory Data Analysis (EDA)**
- Examined the distribution of Food Rating, Service Rating, and Overall Rating by city and age group
- Identified which factors (parking, smoking, price tier) correlate with the positive rating rate

3️⃣ **Power BI Visualization**
- Built relationships (extended star schema) in the Power BI Data Model
- Created DAX measures: Positive Rating %, Positive Food/Service Rating %, Food vs Service Gap, rating by segment
- Designed a 3-page report with cross-filtering slicers (Cuisine, Restaurant, Group Age, Drink Level)

---

## 📊 Key Insights & Visualizations

### 1️⃣ Overview

![Overview](./images/Overview.jpg)

📌 **Insight 1 — Customer satisfaction remains low, primarily driven by poor service quality:**
- **Observation:** Overall Positive Rating reaches only around **42–46%**, meaning more than half of the restaurants in the survey receive negative ratings from customers. Looking closer, **Food Rating (44%)** is higher than **Service Rating (36%)**, yet both sit below 50% — with Service Rating being the weakest metric. This shows food quality isn't the biggest issue; **the service experience is the strongest driver** of customer satisfaction.
- **Recommendation:** Prioritize improving service quality through staff training, reducing wait times, and standardizing service procedures.

📌 **Insight 2 — Restaurant price segment may influence customer satisfaction:**
- **Observation:** Looking at the city level, **Cuernavaca** has the highest positive rating rate and also has a higher share of restaurants in the **High** price tier than other areas. Conversely, **Ciudad Victoria** has the lowest Overall Rating and is mostly made up of **Medium** and **Low** price-tier restaurants. This suggests price tier could be a contributing factor to customer experience, though this needs further validation by comparing Food Rating and Service Rating across price tiers.
- **Recommendation:** Dig deeper into each price segment to determine whether price tier or service quality is the primary driver behind lower satisfaction.

### 2️⃣ Restaurants

![Restaurant](./images/Restaurant.jpg)

📌 **Insight 3 — Good food alone does not guarantee customer traffic:**
- **Observation:** **Mexican** is the most preferred cuisine in the system (104 selections), well ahead of **Bar (78)** and **Fast Food (75)**. However, the **Morelos** area has the highest Food Rating yet noticeably lower customer traffic than other areas — showing that good food quality alone isn't enough to attract customers; factors like marketing, location, or brand recognition also affect footfall. In addition, restaurants with **Valet** parking achieve the highest positive rating rate (57.1%), and the **Bar Only** group in the smoking category leads in positive ratings (61%).
- **Recommendation:** Increase promotional activity for restaurants in Morelos to leverage their food-quality advantage, while also encouraging investment in valet parking services — a factor clearly correlated with customer satisfaction.

📌 **Insight 4 — Customer traffic is concentrated in a few locations:**
- **Observation:** **San Luis Potosí** has significantly higher customer traffic than the other cities in the system. This shows revenue and footfall are concentrated in a handful of areas, creating a risk of over-reliance on a single market if demand there declines.
- **Recommendation:** Expand marketing efforts and restaurant development in lower-traffic areas to balance the market.

### 3️⃣ Customers

![Customers](./images/Customers.jpg)

📌 **Insight 5 — Survey results mainly reflect young customers' opinions:**
- **Observation:** About **106/138** survey participants fall into the **18–25** age group, indicating most responses come from students or early-career employees (mostly **Employed** or **Student**, and mostly single — 121 single customers versus only 10 married). However, the **26–35** age group shows a noticeably lower Overall Rating and, in particular, Service Rating — suggesting more mature customers tend to have higher expectations for service quality and experience. **Mexican** food remains the top favorite, chosen by **74%** of customers, and the **Casual Drinker** segment dominates across most traffic levels.
- **Recommendation:** Design programs and services tailored to each customer segment, focus on improving the service experience for the higher-spending 26–35 age group, and consider drink promotions targeted at the Casual Drinker segment.

---

## 🛠️ Skills & Tools Applied

| Category | Skills / Tools |
|---|---|
| **Data Modeling** | Designed an extended star schema (1 fact table + 4 dimension tables), configured relationships (1-to-many) |
| **Data Preparation** | Power Query (cleaning, standardizing Parking/Price/Smoking/Drink Level labels, handling missing data) |
| **DAX (Power BI)** | Measures for Positive Rating %, Positive Food/Service Rating %, Food vs Service Gap, Above/Below Average segmentation |
| **Data Visualization** | Designed a 3-page report, chart selection (KPI cards, bar charts, donut, matrix with conditional formatting), cross-filtering slicers |
| **Business Analysis** | Gathered stakeholder requirements using the Design Thinking framework, translated business questions into measurable KPIs |
| **Analytical Thinking** | Root-cause analysis (linking operational factors to rating outcomes), behavior-based customer segmentation |
| **Data Storytelling** | Structured findings using an Observation → Recommendation format for each dashboard page |

---

## 🔎 Final Conclusion & Recommendations

1. **Service is the primary cause of low satisfaction**

   Insight: Overall Positive Rating reaches only about 42–46%; Service Rating (36%) is noticeably lower than Food Rating (44%), showing the service experience — not food quality — is the biggest bottleneck.

   Recommendation: Prioritize improving service quality through staff training, reducing wait times, and standardizing service procedures.

2. **Price tier may affect customer experience**

   Insight: Cuernavaca (more High-tier restaurants) has the highest positive rating rate, while Ciudad Victoria (mostly Medium/Low) has the lowest Overall Rating.

   Recommendation: Dig deeper into each price segment to determine whether price tier or service quality is the main driver.

3. **Good food quality alone isn't enough to attract customers**

   Insight: Morelos has the highest Food Rating but noticeably lower customer traffic than other areas.

   Recommendation: Increase promotion for restaurants in Morelos to capitalize on their existing food-quality advantage.

4. **Customer traffic is concentrated in a few areas**

   Insight: San Luis Potosí has significantly higher traffic than the other cities, creating market-dependency risk.

   Recommendation: Expand marketing and restaurant development in lower-traffic areas to balance the market.

5. **Survey results mainly reflect younger customers' opinions**

   Insight: 106/138 survey respondents are in the 18–25 age group, while the 26–35 group has noticeably lower Service Rating, indicating higher expectations for the service experience.

   Recommendation: Design age-specific programs and services, with particular focus on improving the service experience for the 26–35 age group.

---

> **Note:** This README follows the same structure as the SuperStore Dashboard sample report. Replace the image paths (`./images/...`) with your actual image links (e.g. raw GitHub links) once you've uploaded the dashboard images to your repository.
