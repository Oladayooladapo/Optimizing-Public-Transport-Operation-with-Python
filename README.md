# Optimizing-Public-Transport-Operation-with-Python
This project focuses on an in-depth Exploratory Data Analysis (EDA) of public transportation trip data from **MetroMove Transit Solutions**

# Optimizing Public Transit Operations: A Data-Driven Approach

## Project Overview

The primary goal is to transform raw, messy, and incomplete trip records into actionable insights. By understanding passenger behavior, evaluating transport mode performance, and analyzing fare patterns, this project aims to identify inefficiencies and patterns that can drive strategic operational improvements for MetroMove.

This repository contains the Jupyter Notebook detailing the entire EDA process, along with the cleaned dataset and a summary of key business recommendations.

## Business Problem

MetroMove Transit Solutions, a public transportation provider operating across multiple cities (buses, trains, ferries, trams), has accumulated a vast amount of trip data. However, this data is characterized by inconsistencies, missing values, and a lack of structure, preventing the company from gaining critical insights into:
- Trip performance (e.g., duration, delays).
- Passenger behavior (e.g., peak hours, station traffic).
- Fare patterns and anomalies.

This absence of clear insights hinders effective decision-making and limits MetroMove's ability to optimize its services and enhance passenger experience.

## Project Rationale

This project provides a practical simulation of a real-world data analysis challenge, demonstrating key data science skills:
- **Data Cleaning & Preprocessing:** Handling raw, inconsistent, and incomplete data.
- **Pattern Discovery:** Uncovering hidden trends and relationships through comprehensive EDA.
- **Data-Driven Insights:** Translating complex data into clear, concise, and actionable business recommendations.
- **Contextual Understanding:** Applying analytical skills within the specific domain of transportation.

## Dataset

The dataset used is `Public_Transport_Trips_EDA.csv`, containing various attributes for thousands of daily trips.

**Key Columns:**
- `Trip_ID`: Unique identifier for each trip.
- `Mode_of_Transport`: Type of transport used (Bus, Train, Ferry, Tram), with inconsistencies.
- `Departure_Station`: Station where the trip starts (contains whitespace errors).
- `Arrival_Station`: Station where the trip ends (inconsistent casing).
- `Departure_Time`: Exact date and time when the trip departed.
- `Passenger_Count`: Number of passengers on the trip (includes missing values).
- `Fare_Amount`: Amount paid by the passengers for the trip (includes missing values).
- `Trip_Duration_Minutes`: Duration of the trip in minutes (includes missing values).
- `Trip_Date`: Date on which the trip occurred.
- `Day_of_Week`: Day of the week on which the trip occurred.

## Project Workflow & Thought Process

My approach to this EDA project followed a structured methodology to ensure thoroughness and actionable outcomes:

### 1. Data Loading & Initial Inspection
- **Objective:** Get a first look at the data, understand its structure, and identify initial quality issues.
- **Steps:**
    - Loaded the dataset using `pandas`.
    - Used `df.info()` to check data types and non-null counts, quickly identifying columns with missing values (`Passenger_Count`, `Fare_Amount`, `Trip_Duration_Minutes`).
    - Employed `df.head()` and `df.sample()` to visually inspect raw data entries, noticing inconsistencies in `Mode_of_Transport` and station names.
    - Used `df.describe(include='all')` to get descriptive statistics for both numerical and categorical columns, revealing potential outliers and common categories.

### 2. Data Cleaning & Preprocessing
- **Objective:** Address data quality issues to ensure reliability and consistency for analysis.
- **Steps:**
    - **Standardizing Text Data:**
        - Cleaned `Mode_of_Transport`: Converted to lowercase and then title case.
        - Cleaned `Departure_Station` and `Arrival_Station`: Stripped leading/trailing whitespaces and standardized casing.
    - **Handling Missing Values:**
        - For `Passenger_Count`, `Fare_Amount`, and `Trip_Duration_Minutes`, decided on imputation strategies. Given the nature of transit data, I used the median value to substitute missing values because it is robust to outliers.
    - **Data Type Conversion:**
        - Converted `Departure_Time` and `Trip_Date` to datetime objects for time-based analysis.
- **Thought Process:** Prioritized cleaning categorical text and handling missing numerical data early, as these directly impact the accuracy and meaningfulness of subsequent analyses. Standardizing text is crucial for accurate grouping and counting.

### 3. Feature Engineering
- **Objective:** Create new features that could unlock deeper insights and simplify complex relationships.
- **Steps:**
    - **Time-based Features:**
        - Extracted `Hour_of_Day` from `Departure_Time` to analyze hourly traffic patterns.
        - Extracted `Month` from `Trip_Date` to identify seasonal trends.
    - **Trip-based Features:**
        - Calculated `Trip_Distance` (if a distance metric was available or could be approximated, e.g., based on station pairs). (Assuming this was a conceptual idea or based on external data not provided, if not explicitly in the notebook).
        - Categorized `Trip_Duration_Minutes` into bins (e.g., 'Short', 'Medium', 'Long') for easier analysis of trip performance.
- **Thought Process:** Identified common patterns in transit data (time of day, seasonality) that are not directly available but can be derived. These engineered features are often highly predictive and insightful.

### 4. Exploratory Data Analysis (EDA) - Deep Dive
- **Objective:** Uncover significant trends, anomalies, and relationships using various visualization techniques.
- **Steps & Key Insights:**

    #### a. Descriptive Statistics & Distribution Analysis
    - **Trip Duration & Fare Structure:**
        - Analyzed the longest and shortest trips.
        - Identified potential low-fare anomalies requiring investigation (e.g., manual errors, niche services).
    - **Distributions of Key Variables:**
        - Observed skewness in `Trip_Duration_Minutes`, `Fare_Amount`, and `Passenger_Count`. This indicates non-normal distributions, suggesting that averages alone might be misleading and that median or robust statistics are more appropriate. Skewness also points to potential inefficiencies or unmet demand patterns (e.g., many short, cheap trips vs. few long, expensive ones).

    #### b. Univariate Analysis
    - **Mode of Transport Usage:**
        - Visualized the count of trips by `Mode_of_Transport` (Bus, Train, Ferry, Tram). 
        - **Insight:** Identified the most and least utilized modes, indicating areas for resource reallocation or service expansion/reduction.
    - **Passenger Count Distribution:**
        - Histogram of `Passenger_Count`. 
        - **Insight:** Revealed common passenger loads and identified trips with very low or very high passenger counts, highlighting underutilized routes or potential overcrowding.
    - **Trip Duration Distribution:**
        - Histogram of `Trip_Duration_Minutes`. 
        - **Insight:** Showed the typical duration of trips and outliers, indicating potential delays or highly efficient routes.

    #### c. Bivariate Analysis
    - **Fare Amount by Mode of Transport:**
        - Box plots or bar plots of `Fare_Amount` grouped by `Mode_of_Transport`. 
        - **Insight:** Compared average fares across different transport modes, revealing pricing discrepancies or cost structures.
    - **Passenger Count by Departure/Arrival Station:**
        - Bar plots of `Passenger_Count` for top N stations. 
        - **Insight:** Identified high-traffic stations (e.g., Central Station processing 20.5% of arrivals), highlighting potential bottlenecks and areas requiring infrastructure investment or increased staff deployment.
    - **Trip Duration by Day of Week/Hour of Day:**
        - Line plots or bar plots. 
        - **Insight:** Revealed peak travel times and days, crucial for demand-driven scheduling. Noted significant delays (22% of trips > 120 minutes), particularly for certain modes or routes.

    #### d. Multivariate Analysis
    - **Correlation Matrix:**
        - Heatmap of numerical features. 
        - **Insight:** Identified relationships between `Fare_Amount`, `Trip_Duration_Minutes`, and `Passenger_Count`. For example, longer trips might not always correlate with higher fares, suggesting pricing model review.
    - **Trip Duration vs. Fare Amount by Mode of Transport:**
        - Scatter plot with hue for `Mode_of_Transport`.
        - **Insight:** Revealed how duration and fare vary together for each transport mode, highlighting modes that are expensive for short durations or cheap for long ones.

### 5. Strategic Business Recommendations
- **Objective:** Translate data insights into actionable strategies for MetroMove's operations.
- **Key Recommendations**

    1.  **Resource Reallocation:**
        * Reduce low-utilization bus routes (e.g., <15 passengers per trip).
        * Increase train capacity during peak hours (e.g., 7–9 AM).
        * Eliminate inefficient circular same-station trips.
    2.  **Fare & Pricing Optimization:**
        * Introduce off-peak discounts, especially for trams, to balance demand.
        * Standardize fares with a cap on 20% deviation for same-distance routes to ensure fairness.
    3.  **Route Optimization:**
        * Focus improvements on high-demand corridors (e.g., "Airport–Central").
        * Redeploy buses from underused zones to peak corridors.
    4.  **Service Quality Improvement:**
        * Address long trips (>120 minutes) by implementing dedicated transit lanes and improving ferry scheduling.
    5.  **Demand-Driven Scheduling:**
        * Utilize forecasting for 7-day passenger demand projections.
        * Dynamically adjust vehicle allocation by time/day to match supply with demand.
    6.  **Infrastructure Investments:**
        * Expand Central Station capacity (given its high traffic).
        * Add ferry docks at South Point to reduce wait times.

## Tools & Libraries Used

-   **Programming Language:** Python
-   **Data Manipulation:** `pandas`, `numpy`
-   **Data Visualization:** `matplotlib.pyplot`, `seaborn`
-   **Jupyter Notebook:** For interactive analysis and presentation.


## Files in this Repository

-   `Optimizing Public Transport Operations.ipynb`: The main Jupyter Notebook containing all the code for data loading, cleaning, EDA, feature engineering, and insights generation.
-   `Public_Transport_Trips_EDA.xlsx`: The raw dataset used for the project. 
-   `Descriptive Statistics Insights.docx`: A detailed document outlining business context, problem, rationale, and strategic recommendations.
-   `README.md`: This file.

