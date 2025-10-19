# Citi Bike Data Analysis

This repository contains a Google Colab notebook that performs an exploratory data analysis of the Citi Bike trip data in New York City, assisted by AI.

### Project Overview
The goal of this project is to analyze the Citi Bike trip dataset to identify key trends, user behavior patterns, and station usage, providing insights that could inform operational improvements, enhance user experience, and explore potential revenue opportunities.

### Dataset
The analysis utilizes the publicly available Citi Bike Trips dataset hosted on Google BigQuery: bigquery-public-data.new_york.citibike_trips.

### Analysis Highlights
- Analysis of trip patterns by user type (Subscribers vs. Customers).
- Identification of the busiest start and end stations.
- Examination of trip distribution across different days of the week and hours of the day.
- Visualization of key findings using Python libraries (pandas, matplotlib, seaborn).

### Repository Contents
Assignment1.ipynb: The main Google Colab notebook containing the SQL queries, Python code for visualizations, and interpretations.
README.md: This file, providing an overview of the project.
Reflections.md: File containing individual reflections of contributors.

### How to Reproduce the Analysis
Open the Notebook: Open the Assignment1.ipynb file in Google Colab.
Connect to Google Cloud: Follow the setup instructions in the notebook to authenticate and connect to your Google Cloud project.
Run Cells: Execute the code cells sequentially to perform the data exploration, analysis, and generate visualizations.

### Data Quality Notes
The analysis identified some data quality issues, including:
- Missing user type information for a notable percentage of trips.
- Missing start and end station names for a portion of trips.
- These limitations are acknowledged in the analysis and recommendations.

### Tools and Technologies Used
- Google Colab
- Google BigQuery
- SQL
- Python (pandas, matplotlib, seaborn)
- Gemini (for hypothesis generation and SQL assistance)
- Looker Studio (for dashboarding)
- A public Looker Studio dashboard summarizing some of the key findings can be accessed here: <https://lookerstudio.google.com/reporting/5dba5ba3-df55-40b0-9e84-10c328321a82>

### Team Members
- bulut19
- joy86891
- gabeunix
