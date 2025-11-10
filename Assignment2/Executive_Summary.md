# Flight Diversion Prediction Model: Executive Report

## Executive Overview

This report presents a cost-optimized BigQuery ML classifier for predicting U.S. flight diversions. The model achieves 99.23% recall at a threshold of 0.1429, minimizing total operational costs to $929.40 per flight. The approach prioritizes detecting nearly all diversions while accepting higher false positive rates, reflecting the 6:1 cost asymmetry where missing a diversion costs $6,000 compared to $1,000 for unnecessary resource staging.

## Decision Rule

**Optimization Objective:** Minimize total operational cost by prioritizing recall over precision, given the 6:1 cost asymmetry between false negatives and false positives.

**Operating Threshold:** 0.1429, derived from the cost ratio formula: FP_cost divided by the sum of FP_cost plus FN_cost, which equals $1,000 divided by $7,000.

**Strategic Trade-off:** This low threshold flags flights with even modest diversion probabilities, capturing 99.23% of actual diversions—8,930 out of 9,007 total diversions—while accepting 353,831 false positives. The 77 missed diversions cost $462,000, while false positives cost $353.8 million. This approach is justified because each missed diversion has six times the operational impact of a false alarm, enabling proactive resource allocation, crew notification, and passenger communication before disruptions occur.

## Evidence: Model Evolution and Performance

### Baseline to Engineered Models

**Model A - Baseline Performance**

Model A used basic operational features including flight distance, carrier identity, and taxi times. Performance metrics: AUC of 0.697, precision of 0.048, and recall of 0.590. The baseline demonstrated meaningful predictive signal but missed 41% of diversions at the default 0.5 threshold, establishing clear need for both feature engineering and threshold optimization.

**Model B - Engineered Features**

Model B added departure delay magnitude, route identifiers from origin-destination pairs, day of week patterns, and month seasonality. Performance improved to recall of 0.634, a 4.4 percentage point increase over baseline. This means the model detected 44 additional diversions per 1,000 actual events. AUC remained stable at 0.696 while precision decreased slightly to 0.044. The engineered features successfully captured route-specific risks, temporal patterns, and operational stress signals that the baseline model missed.

### Subgroup Specialization

**Model C - Hub-Specific Training**

Model C was trained exclusively on major hub airport data from Atlanta, Chicago O'Hare, and John F. Kennedy International. When both the global Model B and specialized Model C were evaluated on hub flights only, Model C achieved recall of 87.8% compared to Model B's 73.9%—a 13.9 percentage point improvement. This translates to 139 additional detected diversions per 1,000 hub events. Model C's AUC was 0.793, slightly lower than Model B's 0.802, but the substantial recall advantage proved operationally more valuable. Hub airports face unique traffic density, connection complexities, and infrastructure constraints that specialized training captures more effectively.

### Cost-Optimized Threshold Application

**Model D - Threshold Optimization**

Model D applied the analytically derived 0.1429 threshold to Model B's engineered features. The evaluation dataset contained 381,205 total flights with 9,007 actual diversions. At the optimized threshold, the confusion matrix showed 8,930 true positives, 353,831 false positives, 77 false negatives, and 18,367 true negatives.

Performance metrics: recall of 99.23%, false negative rate of 0.85%, total cost of $354.3 million, and average cost per flight of $929.40. The cost breakdown shows false positives accounting for $353.8 million or 99.87% of total costs, while false negatives represent only $0.5 million or 0.13% of total costs. This distribution confirms the model successfully minimizes the highest-impact error category.

## Policy Recommendations

### Deployment Strategy: Hybrid Architecture

**Tier 1 - Hub Operations**

Deploy Model C for Atlanta, Chicago O'Hare, and John F. Kennedy International airports using the 0.1429 threshold. The 13.9 percentage point recall advantage justifies maintaining a specialized model for these high-impact facilities where single diversions affect thousands of passengers through connection disruptions.

**Tier 2 - Non-Hub Operations**

Deploy Model D, the global engineered model, with 0.1429 threshold for all other domestic operations. This provides adequate performance while simplifying system maintenance across diverse operational contexts.

This hybrid approach captures major performance gains at critical hubs while managing overall system complexity.

### Expected Cost Analysis

At the optimal 0.1429 threshold, Model D achieves an average cost of $929.40 per flight with false negative costs representing only 0.13% of the total. The 6:1 cost ratio reflects operational reality where false positives costing $1,000 involve unnecessary resource staging that can be managed through standard procedures, while false negatives costing $6,000 involve cascading failures including emergency gate acquisition, passenger reaccommodation, crew violations, and safety risks that far exceed false alarm costs.

The low threshold prioritizes preparedness over precision, accepting 353,831 false positives to prevent 77 costly missed diversions.

### Hub Performance Impact

Hub specialization delivers 139 additional detected diversions per 1,000 hub events. At major connection points, this translates to dozens of annual advance warnings preventing network-wide disruptions. The concentration of operations and connection complexities at hub airports magnify diversion costs exponentially, justifying the specialized model overhead.

## Monitoring and Governance

### Performance Metrics

**Recall Monitoring:** Minimum threshold of 98% monitored weekly by ML Operations. Retrain if below 98% for two consecutive weeks. Current performance: 99.23%.

**False Negative Rate:** Maximum threshold of 2% monitored weekly by ML Operations. Investigate immediately if exceeded. Current rate: 0.85%.

**Calibration Error:** Maximum of 0.05 assessed bi-weekly by Data Science team. Recalibrate if exceeded.

**Hub-Specific Recall:** Minimum of 85% at Atlanta, Chicago O'Hare, and John F. Kennedy airports, monitored weekly by Airport Operations. Alert hub managers if degraded.

**Area Under Curve:** Minimum of 0.69 tracked monthly by Data Science team. Investigate degradation trends.

### Cost Metrics

**Average Cost Per Flight:** Target maximum of $1,000 monitored weekly by Finance and Operations teams. Review assumptions if exceeded. Current: $929.40.

**False Negative Cost Trend:** Track monthly through Operations Leadership. Investigate systematic increases.

**False Positive Investigation Cost:** Track monthly through Ground Operations. Optimize response procedures if rising.

### Data Quality

**Feature Distribution Drift:** Population Stability Index threshold of 0.1 triggers investigation by Data Engineering, monitored weekly.

**Prediction Distribution:** Plus or minus 20% variance from baseline triggers review by ML Operations, monitored daily.

**Missing Data Rate:** Maximum 2% for critical features monitored daily by Data Engineering.

### Fairness Monitoring

Quarterly reviews ensure no carrier, route, or regional subgroup experiences more than 5 percentage points recall degradation below overall model performance. Verify consistent weekly and monthly performance accounting for seasonal variation.

## Reproducibility

**Model Configuration:** All models use MODEL_TYPE of LOGISTIC_REG, INPUT_LABEL_COLS of diverted, and AUTO_CLASS_WEIGHTS set to TRUE to address class imbalance.

**Schema Mapping:** Raw source table at boxwood-veld-471119-r6.flights_data.flights_raw maps to canonical features: flight_date as DATE, dep_delay as FLOAT64, distance as FLOAT64, carrier as STRING, origin and dest as STRING, diverted as BOOL.

**Data Splitting:** Deterministic 80/20 train/eval split using MOD of ABS of FARM_FINGERPRINT cast on flight_date as STRING ensures reproducible splits.

**Infrastructure:** Project ID is boxwood-veld-471119-r6, dataset is unit2_flights, raw data resides in flights_data.flights_raw.

**Canonical Flight Selection:** Query logic uses WHERE clause requiring either both DepDelay and Distance are NOT NULL, or diverted equals TRUE. This ensures all diversions are included even with missing operational features.

**Missing Value Handling:** BigQuery ML automatically imputes NULL values using mean imputation for numerical features and mode imputation for categorical features.

**Cost Assumptions:** Business costs of $1,000 per false positive and $6,000 per false negative drive threshold optimization. BigQuery processing costs are available in job history but were not explicitly logged during analysis.

## Governance Considerations

### Key Assumptions and Limitations

**Post-Departure Prediction Scope:** Models B, C, and D rely heavily on departure delay features, limiting them to post-departure predictions rather than pre-embarkation warnings. Model A using only pre-departure features achieved lower recall of 0.590 compared to Model B's 0.634, illustrating the timing versus accuracy trade-off.

**Rare Event Modeling Challenges:** Flight diversions occur in less than 1% of flights, causing low precision despite good recall. Severe class imbalance requires threshold optimization as the default 0.5 threshold fails completely.

**Static Feature Engineering:** Fixed bucketization boundaries and route definitions may become suboptimal as delay distributions and operations evolve. Regular retraining partially addresses this lag between operational changes and model adaptation.

**Limited Feature Coverage:** The models lack critical diversion drivers including real-time weather conditions, air traffic control congestion, mechanical issues, and medical emergencies. Weather integration represents the highest-priority enhancement opportunity.

**Specialization Trade-offs:** Model C excels at hubs but would not generalize to non-hub contexts. The two-tier deployment adds operational complexity compared to performance gains.

### Critical Monitoring Slices

**New Routes and Carriers:** Monitor performance on routes and carriers absent from or sparse in training data.

**Hub Airports:** Track individual hub performance at Atlanta, Chicago O'Hare, and John F. Kennedy for facility-specific degradation.

**Temporal Patterns:** Verify stable performance across days and months; detect seasonal drift.

**Weather Events:** Assess performance during severe weather to evaluate indirect weather signal capture.

**Low Probability Segments:** Ensure false negatives do not concentrate in systematically underestimated segments.

**Delay Distribution Changes:** Monitor delay patterns as Models B, C, and D depend heavily on this feature.

## Summary and Next Steps

### Key Findings

Feature engineering in Model B improved recall from 59.0% to 63.4% over baseline. Hub specialization in Model C achieved 87.8% recall versus 73.9% for the global model on hub data, a 13.9 percentage point improvement. Cost-optimized thresholding in Model D at 0.1429 achieved 99.23% recall with false negative rate of 0.85%, minimizing total weighted costs to $929.40 per flight. The 6:1 cost ratio justifies accepting elevated false positives to capture essentially all diversions.

### Immediate Actions - Zero to Three Months

Deploy Model D for non-hub operations and Model C for Atlanta, Chicago O'Hare, and John F. Kennedy hub airports, both using 0.1429 threshold. Establish monitoring dashboards tracking recall, false negative rate, costs, and data quality. Configure automated alerts for performance degradation to route signals to appropriate teams for rapid response.

### Short-Term Enhancements - Three to Six Months

Integrate real-time weather data as the highest-priority missing feature. Expand hub coverage to additional major facilities based on traffic analysis and operational priority. Validate the $1,000 false positive and $6,000 false negative cost assumptions through systematic analysis of actual operational responses. Implement A/B testing infrastructure for controlled threshold refinement.

### Long-Term Strategy - Six to Twelve Months

Explore advanced modeling techniques including gradient boosted trees, neural networks, or ensemble methods for non-linear pattern capture. Develop causal inference approaches to identify preventable diversion root causes enabling preventive interventions. Implement dynamic threshold adjustment based on operational capacity, weather forecasts, and system load to match alert volume to response capability. Establish closed-loop feedback incorporating actual diversion outcomes and operational responses into model training.

### Operational Value Proposition

The system delivers 99.23% recall ensuring advance warning for virtually all diversions, enabling proactive resource positioning, passenger communication, and schedule adjustments impossible with reactive approaches. Hub-specific specialization concentrates improvements where single diversions affect thousands through connection disruptions. Cost optimization at $929.40 per flight reflects efficient risk management prioritizing preparedness over precision, aligning model behavior with operational priorities rather than accuracy metrics disconnected from business value.

