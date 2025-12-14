**Real-Time Wildfire Risk Prediction System**

An end-to-end cloud data pipeline and machine learning system designed to predict wildfire severity and potential acreage damage in real-time. This project integrates 24 years of historical wildfire data with live weather streams to provide actionable intelligence for emergency response planning and resource allocation.

**Key Technologies:** Google Cloud Platform (Cloud Scheduler, Cloud Functions, Pub/Sub, Dataflow, BigQuery), Python (functions-framework, google-cloud-pubsub), SQL, Open-Meteo API, Looker Studio.

**Highlights:**
    •    Three-Layer Architecture: Implemented a Lambda architecture combining a batch layer for historical model training and a speed layer for real-time data ingestion.
    •    Real-Time Data Pipeline: Constructed a fully automated streaming pipeline using Cloud Scheduler and Dataflow to ingest live weather data (temperature, humidity, wind) every 15 minutes.
    •    Advanced Predictive Modeling: Developed and deployed BigQuery ML models (Boosted Tree Regressor) to forecast potential fire size based on current meteorological conditions.
    •    Live Operational Dashboard: Created an interactive Looker Studio dashboard featuring real-time risk gauges, trend tracking, and geospatial visualization to support immediate decision-making.
