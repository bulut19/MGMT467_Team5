## Executive Summary

### Project Objective  
This project presents an exploratory data analysis of Citi Bike usage in New York City using the publicly available dataset hosted in Google BigQuery (`bigquery-public-data.new_york.citibike_trips`). Our aim was to uncover usage patterns across user types, temporal trends by weekday and hour, and high-demand stations to suggest operational improvements, user engagement strategies, and system optimization.

### Methodology  
1. **Dataset Familiarization:** Initial query exploration to understand dataset fields, volume, and potential data quality limitations.
2. **AI-Assisted Hypothesis Generation:** Business questions refined through iterative prompting using Gemini.
3. **SQL Exploration:** BigQuery was used to extract insights related to ride frequency, user segmentation, trip duration, top stations, and temporal ride distribution.
4. **Python Visualization (Colab):** The dataset was analyzed and visualized with pandas, matplotlib, seaborn, and Plotly to explore trends and validate findings.
5. **Dashboard Development:** Key trends were synthesized into an interactive Looker Studio dashboard featuring KPIs, bar charts, and time-based distribution visuals.
6. **Insight Interpretation:** Analytical observations were discussed, and operational and strategic implications were derived.

### Data Overview  
The dashboard revealed the following overall metrics:
- **Total Trips Analyzed:** 58,937,715  
- **Average Trip Duration:** 16.04 minutes  
- **Subscriber Trip Proportion:** 79.61%  
- **Unique Bikes Used:** 17,682  

### Key Findings  

#### 1. Distinct Usage Patterns by User Type  
- Subscriber usage significantly outweighs customer usage, accounting for over three-quarters of all trips.
- Subscribers typically have shorter trip durations, suggesting commute-related or utilitarian usage.
- Customers, although fewer in number, tend to engage in longer rides, suggesting leisure-oriented or non-commute purposes.

#### 2. Station Demand Concentration  
- Stations such as **Pershing Square North** and **E 17 St & Broadway** consistently appear among the busiest for both starting and ending trips.
- The overlap between high-traffic start and end stations suggests established commuting corridors and frequent round-trip travel.

#### 3. Temporal Demand Alignment with Commuting Behavior  
- Weekday rides show clear peaks around **8:00 AM** and **5:00–6:00 PM**, aligning with typical work commute hours.
- Weekend patterns are less pronounced and more evenly distributed throughout the day, supporting the hypothesis of more recreational usage outside traditional commute cycles.

#### 4. Data Quality Observations  
- Missing user type and station name values were noted in a subset of the data.
- These gaps may limit the accuracy of behavioral segmentation and location-based performance insights.

### Recommendations  

#### 1. Improve Bike Redistribution During Peak Hours  
- Use weekday temporal patterns to proactively position bikes at high-demand stations such as Pershing Square North before morning peaks.
- Implement predictive rebalancing strategies to redistribute bikes more efficiently ahead of evening return demand.

#### 2. Enhance Engagement Strategies with User-Specific Programs  
- Subscriber usage suggests the importance of service reliability during commute hours; optimizing bike availability for morning and evening riders should be prioritized.
- Longer trip durations among customers present opportunities for targeted leisure passes, tourist promotions, or partnerships with nearby recreational or cultural attractions.

#### 3. Strengthen Infrastructure at Critical Stations  
- High-demand stations may require increased dock capacity, maintenance responsiveness, and station-level monitoring to accommodate sustained commuter volumes.
- Consider station-level traffic forecasts to guide decisions on expansion and resource allocation.

#### 4. Address Data Quality Issues for Improved Analysis  
- Standardize data validation processes to reduce occurrences of missing user type or station metadata.
- Enhanced data completeness would improve future segmentation accuracy and allow more robust performance forecasting.

### Conclusion  
This analysis confirms that Citi Bike plays a crucial role in weekday urban commuting while also supporting recreational use among casual riders. The concentration of traffic at s
pecific stations and during commute hours highlights opportunities for targeted operational optimizations. The findings support investments in predictive rebalancing, station capacity planning, differentiated engagement strategies, and improved data integrity. Together, these strategies can improve system efficiency, user satisfaction, and long-term growth potential.
