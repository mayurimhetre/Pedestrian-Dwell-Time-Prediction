# 🚆 Dwell Time Prediction Using Pedestrian Trajectory Data

## 📌 Project Overview

Understanding pedestrian boarding and alighting behavior is crucial for optimizing public transportation systems.  
This project focuses on **analyzing pedestrian trajectory data from a train platform** to **simulate boarding processes and predict train dwell times**.

Using spatiotemporal movement data, the study identifies boarding zones, classifies passenger movements, and models dwell time as a function of passenger flow dynamics. Both **static and dynamic measurement approaches** are evaluated to capture real-world pedestrian behavior more accurately.

---

## 🎯 Project Objectives

The key objectives of this project are:

- Analyze **spatiotemporal pedestrian movement patterns** on a train platform
- Identify **train door locations and boarding zones** using clustering techniques
- Quantify **boarding and alighting durations** during train stops
- Classify passengers as **boarding or alighting** using movement-based rules
- Predict **train dwell time** using regression models
- Compare **static vs dynamic (KDE-based)** passenger measurement methods
- Provide actionable insights for **transport planning and infrastructure design**

---

## 🗂️ Dataset Description

The dataset includes:

### Pedestrian Trajectory Data
- Pedestrian ID  
- Timestamp / frame number (4 FPS)  
- x, y, z coordinates  

### Platform Information
- Platform boundaries
- Platform edges corresponding to train doors

---

## 🔍 Methodology

### 1️⃣ Data Preprocessing
- Filtered trajectories to relevant time windows around train arrivals
- Removed steady and short-lived detections
- Mapped pedestrian positions to platform coordinates

---

### 2️⃣ Train Arrival and Dwell Time Identification
- Detected train arrivals using changes in pedestrian density and movement
- Segmented data into:
  - Pre-arrival
  - Boarding/alighting (dwell period)
  - Post-departure
- Computed dwell time based on active passenger movement duration

---

### 3️⃣ Boarding Zone and Door Detection
- Applied **K-Means clustering** on pedestrian positions near the platform edge
- Identified train door locations
- Assigned **door IDs** to pedestrian movements for door-level analysis

---

### 4️⃣ Passenger Movement Classification

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

### 5️⃣ Dataset Preparation
For each train stop and door, the following features were extracted:

- Train timestamp
- Train ID
- Door ID
- Number of boarding passengers  
- Number of alighting passengers  
- Interaction term (no_of_boarding_passangers * no_of_alighting_passangers)  
- Dwell Time (in seconds)

---

### 6️⃣ Dwell Time Prediction

The dwell time (\(y\)) for a train stop is modeled using a **linear regression** based on the number of boarding and alighting passengers:

Equation 1:

y = intercept + coef_boarding * N_boarding + coef_alighting * N_alighting

Equation 2:

y = intercept + coef_boarding * N_boarding + coef_alighting * N_alighting + coef_interaction * (N_boarding * N_alighting)

Where:
- N_boarding = Number of boarding passengers
- N_alighting = Number of alighting passengers
- coef_boarding, coef_alighting, coef_interaction = Model coefficients
- intercept = Regression intercept


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

## 🚀 Contributions and Impact

This project demonstrates how **trajectory-based pedestrian analytics** can:

- Improve dwell time prediction accuracy
- Enable door-level passenger flow analysis
- Support dynamic passenger management strategies
- Inform platform layout and infrastructure design decisions

The study highlights the importance of **dynamic, time-sensitive modeling** in public transportation systems.

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


