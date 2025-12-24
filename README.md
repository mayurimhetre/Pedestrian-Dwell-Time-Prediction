# 🚆 Dwell Time Prediction Using Pedestrian Trajectory Data

## 📌 Project Overview

Understanding pedestrian boarding and alighting behavior is crucial for optimizing public transportation systems.  
This project focuses on **analyzing pedestrian trajectory data from a train platform** to **simulate boarding processes and predict train dwell times**.

Using spatiotemporal movement data, the study identifies boarding zones, classifies passenger movements, and models dwell time as a function of passenger flow dynamics. Both **static and dynamic measurement approaches** are evaluated to capture real-world pedestrian behavior more accurately.

---

# Train Dwell Time Modeling

## Overview
Brief description of the project and its purpose.

## Dwell Time Regression Model
Explanation of how dwell time is modeled.

### Model 1: Additive Linear Model
Description of the baseline linear regression model.

### Model 2: Linear Model with Interaction Term
Description of the extended model with interaction effects.

## Data Description
Information about the input data and variables.

## Results
Summary of model performance and findings.

## Usage
Instructions on how to run the code.

## License
Project license information.


[Model 1](#model-1-additive-linear-model)


## 🎯 Project Objectives

The key objectives of this project are:

- Analyze **spatiotemporal pedestrian movement patterns** on a train platform
- Identify **train door locations and boarding zones** using clustering techniques
- Quantify **boarding and alighting durations** during train stops
- Classify passengers as **boarding or alighting** using movement-based rules
- Predict **train dwell time** using regression models
- Compare **static vs dynamic (KDE-based)** passenger measurement methods

---

## 🗂️ Input Raw Dataset Description

The dataset includes:

### Pedestrian Trajectory Data
- Pedestrian ID  
- Timestamp / frame number (4 FPS)  
- x, y, z coordinates  

### Platform Information
Defines spatial layout of platform and obstacles
Columns include:
 - x-coordinate
 - y-coordinate
 - Description (e.g., Bahnsteig, Treppe)

---
## Exploratory Data Analysis

### 1. Pedestrian Trajectory Data

Input raw data contains informationf for specific Date & Time: November 2, 2021, from 1:00 a.m. to 11:00 p.m
Dwell periods are distributed unevenly over time.

### 2. Platform Layout

The platform layout features a measurement line with plotted obstacles.
Measurement line coordinates are adjusted using a 2-meter offset.
Upper measurement line: 12.72 − 2 = 10.72
Lower measurement line: −3.77 + 2 = −1.77

![](https://github.com/mayurimhetre/Pedestrian-Dwell-Time-Prediction/blob/main/images/platform_top_view.png)

### 3. Passenger Trajectory Patterns: Boarding vs. Alighting
Analyzes pedestrian trajectories within a 60-second window around the peak second (57748).
Identifies individuals crossing the upper (y = 10.72) or lower (y = -1.77) platform boundaries.
Determines crossing direction to classify each person as boarding or alighting.

![](https://github.com/mayurimhetre/Pedestrian-Dwell-Time-Prediction/blob/main/images/alighting_passanger.png)

Trajectory of an alighting passenger: movement from the train side to the platform side.

![](https://github.com/mayurimhetre/Pedestrian-Dwell-Time-Prediction/blob/main/images/boarding_passanger.png)

Trajectory of a boarding passenger: movement from the platform side to the train side.

### 4. Identification of Train Door Positions:
Peak time identified as 5:00 PM based on highest observed passenger density.
Applies K-Means clustering to X-coordinates of pedestrian crossings at upper and lower platform edges.
Uses the elbow method and silhouette score to determine the optimal number of clusters (door positions).
Visualize cluster centers to infer train door locations.
Identified 9 doors on the upper edge and  10 doors on the lower edge of the platform

![](https://github.com/mayurimhetre/Pedestrian-Dwell-Time-Prediction/blob/main/images/door_positions.png)

### 5. Passanger count during peak period (8 am to 9 am)
It was observed that more passengers were alighting than boarding during the peak period

![](https://github.com/mayurimhetre/Pedestrian-Dwell-Time-Prediction/blob/main/images/passanger_count.png)

## 🔍 Methodology

### 1️⃣ Data Preprocessing
- Filtered trajectories to relevant time windows around train arrivals
- Removed steady and short-lived detections
- Mapped pedestrian positions to platform coordinates

---

### 2️⃣ Passenger Movement Classification

Two approaches were implemented:

#### 🔹 Static Measurement Lines
- Fixed lines near the platform edge (approx. 1.5 meters from the platform edge)
- Classified passengers based on crossing direction
- Simple but limited in adaptability

#### 🔹 Dynamic Measurement Lines (KDE-based)
- Used **Gaussian Kernel Density Estimation (KDE)** to detect high-density movement regions
- Measurement lines adapted dynamically to pedestrian behavior
- More effective in capturing real-world boarding and alighting patterns
---

### 3️⃣ Boarding Zone and Door Detection
- Applied **K-Means clustering** on pedestrian positions near the platform edge
- Identified train door locations
- Assigned **door IDs** to pedestrian movements for door-level analysis

---

### 4️⃣ Final Dataset Preparation for Predictive Modelling
For each train stop and door, the following features were extracted:

- Train timestamp
- Train ID
- Door ID
- Number of boarding passengers  
- Number of alighting passengers  
- Interaction term (no_of_boarding_passangers * no_of_alighting_passangers)  
- Dwell Time (in seconds)

---

### 5️⃣ Dwell Time Prediction

The dwell time (`y`) of a train at a station is estimated using a linear regression model based on the number of boarding and alighting passengers.

### Model 1: Additive Linear Model

This model assumes that boarding and alighting passengers independently contribute to the dwell time.

Formula:

    y = intercept
        + coef_boarding  * N_boarding
        + coef_alighting * N_alighting

Where:
- `y` — dwell time at the station
- `N_boarding` — number of boarding passengers
- `N_alighting` — number of alighting passengers
- `intercept` — baseline dwell time
- `coef_boarding` — effect of boarding passengers
- `coef_alighting` — effect of alighting passengers

### Model 2: Linear Model with Interaction Term

To account for the combined impact of simultaneous boarding and alighting, an interaction term is included.

Formula:

    y = intercept
        + coef_boarding    * N_boarding
        + coef_alighting   * N_alighting
        + coef_interaction * (N_boarding * N_alighting)

Where:
- `coef_interaction` captures the additional dwell time caused by concurrent boarding and alighting activity

The interaction term captures how **simultaneous boarding and alighting affects dwell time**.

**Performance metrics** used to evaluate the model include:  
- **Root Mean Squared Error (RMSE)** – measures the average prediction error in the same units as dwell time  
- **Mean Absolute Percentage Error (MAPE)** – measures prediction accuracy as a percentage

---

## 📊 Key Findings

- Boarding passengers have a **stronger influence on dwell time** than alighting passengers
- The interaction between boarding and alighting is **negative**, indicating reduced efficiency when both occur simultaneously
- **Dynamic KDE-based measurement lines outperform static lines**
- Prediction accuracy improves during **high-crowd periods**
- Dwell time behavior varies across **platform edges and doors**

---

## 🔮 Future Work

- Extend the framework for **real-time dwell time prediction**
- Integrate **train schedules, delays, and headways**
- Include **environmental and contextual factors** (e.g., weather)
- Explore **non-linear and machine learning models**
- Apply the approach to **multiple stations and platforms**

---

## 🛠️ Technologies Used

- Python  
- NumPy, Pandas  
- Scikit-learn  
- Matplotlib / Seaborn  
- Spatial analytics and KDE methods  

---


