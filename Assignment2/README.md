# Flight Diversion Prediction with BigQuery ML

### Project Overview
This project leverages BigQuery ML to predict U.S. flight diversions, aiming to help airlines and airports proactively manage disruptions, minimize costs, and optimize resource allocation through machine learning.

### Models Developed
This notebook develops and evaluates multiple BigQuery ML models:
- Model A (Baseline): Initial logistic regression.
- Engineered Model (B): Incorporates route, day_of_week, dep_delay_bucket.
- Model C (Subgroup-Specialized): Trained on hub-specific data (ATL, ORD, JFK).
- Model D (Cost-Optimized): Builds on Model B with an analytically derived decision threshold.
- Engineered Model (with TRANSFORM): Similar to B, using BQML's TRANSFORM clause.
- Additional Baseline Model (Extra Credit): Another simple logistic regression.
Each model is evaluated with ML.EVALUATE (AUC, precision, recall, F1) and detailed confusion matrices.

### Key Findings & Insights
- **Optimal Threshold:** Model D identified an optimal decision threshold of 0.1429 to minimize operational costs (FP= 1000,𝐹𝑁= 6000), prioritizing recall due to high FN costs.
- **Feature Engineering:** Model B improved recall (0.634) over Model A (0.590), showing the value of features like dep_delay and route.
- **Subgroup Specialization:** Model C (hub-specific) significantly boosted recall (0.879) for hub flights compared to global Model B (0.739) on the same data, highlighting tailored modeling benefits.
- **Low Acceptable FNR:** Model D achieved an acceptable False Negative Rate of approximately 0.85%, reflecting a strong emphasis on not missing actual diversions.

### Operational Recommendations
- **Prioritize Specialized & Cost-Optimized Models:** Implement Model C-like approaches with Model D's cost-optimized thresholds for critical operational segments.
- **Establish Comprehensive Monitoring:** Continuously monitor model performance across various data slices (routes, carriers, seasons) to ensure reliability and detect degradation.

### Reproducibility
- **Parameters:** Models use MODEL_TYPE='LOGISTIC_REG', INPUT_LABEL_COLS=['diverted'], and AUTO_CLASS_WEIGHTS=TRUE.
- **Schema Mapping:** Raw table (boxwood-veld-471119-r6.flights_data.flights_raw) is mapped to canonical fields (e.g., FlightDate to flight_date).
- **BigQuery Location:** Project boxwood-veld-471119-r6, dataset unit2_flights.
- **Query Costs:** Business costs for FP/FN were used; BQ query costs were not explicitly logged.
- **Data Quality:** Canonical flight selection includes all diverted flights; BQML implicitly handles NULLs (e.g., for DepDelay, Distance).

### Governance Notes
- **Assumptions & Limitations:** Models rely on post-departure data (DepDelay), face challenges with rare events, and may not generalize globally (localized models). Static feature engineering and logistic regression's inability to capture complex interactions are noted.
- **Slices for Ongoing Monitoring:** Focus on new routes/carriers, specific airports (hubs), seasonal/day-of-week changes, extreme weather, low probability segments, and changes in dep_delay distributions to maintain model performance and fairness.

### Repository Contents
assignment2.ipynb: The main Colab notebook containing all model development, evaluation, and analysis.
README.md: This file, providing an overview of the project.
Executive_Summary.md: Team Ops Brief discussion decision rule, operating threshold, evidence of model performance, deployment policy, and a monitoring plan specifying metrics, thresholds, cadence, and ownership.

### Tools and Technologies Used
- Google Colab
- Google BigQuery
- SQL
- Python (pandas, matplotlib, seaborn)
- Gemini (for hypothesis generation and SQL assistance)
