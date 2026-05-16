# 🌪️ Data Storm 7.0 - Storming Round | Team Cylodea

## 📌 Project Overview
This repository contains the official submission for **Team Cylodea** for the Data Storm 7.0 Storming Round. The objective of this project is to design a robust analytical framework to estimate the **Latent Maximum Monthly Volume Potential (in liters)** for a network of 20,000 traditional retail outlets in Sri Lanka for January 2026.

## 🏗️ Architecture: The Lakehouse Pipeline
Our codebase strictly adheres to industry-standard data Lakehouse practices, structured into three distinct layers:

1. **Bronze Layer (Raw Ingestion):** Stores original datasets exactly as provided, untouched.
2. **Silver Layer (Cleaned & Sanitized):** Contains reusable data quality checks. Anomalies (e.g., negative volumes, duplicates, nulls) are quarantined into a dedicated `rejected` records store.
3. **Gold Layer (Enriched & Model-Ready):** Integrates base latent potential calculations and engineered external features (POI spatial data).

## 🚀 Key Methodologies

* **Data Forensics:** Handled legacy system artifacts by isolating and removing over 4,700 records with negative volumes and bill values.
* **Causal Base Logic:** Addressed the right-censored data challenge by utilizing the **95th Percentile** of historical sales to estimate the true, uncapped base potential, eliminating extreme outliers while capturing peak operational capacity.
* **Robust POI Pipeline (Spatial Mapping):** Extracted nationwide school and bus stop data via the Overpass API. Instead of inefficient iterative API calls, we utilized the **Scikit-Learn `BallTree` algorithm** with Haversine distance to map POIs within a 1km radius for all 20,000 outlets simultaneously.
* **Final Potential Calculation:** `Base Potential * (1 + POI Boost) * Seasonality Multiplier`

## ⚙️ How to Reproduce the Pipeline

This pipeline was developed using Google Colab. To run the code seamlessly, follow these exact steps:

### 1. Folder Setup in Google Drive
Ensure your Google Drive has the following directory structure:
```text
My Drive/
└── DataStorm_Project/
    └── data/
        ├── bronze/   (Place original .csv files here)
        ├── silver/   (Auto-generated during run)
        ├── gold/     (Auto-generated during run)
        └── rejected/ (Auto-generated during run)
