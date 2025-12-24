# 🚆 Dwell Time Prediction Using Pedestrian Trajectory Data

## 📌 Project Overview

Understanding pedestrian boarding and alighting behavior is crucial for optimizing public transportation systems.  
This project focuses on **analyzing pedestrian trajectory data from a train platform** to **simulate boarding processes and predict train dwell times**.

Using high-resolution spatiotemporal movement data, the study identifies boarding zones, classifies passenger movements, and models dwell time as a function of passenger flow dynamics. Both **static and dynamic measurement approaches** are evaluated to capture real-world pedestrian behavior more accurately.

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
- x, y coordinates  
- Height (used as a quality indicator)

### Platform Information
- Platform boundaries
- Platform edges corresponding to train doors

### Temporal Metadata
- Time windows covering train arrival, dwell, and departure phases

The high spatial and temporal resolution allows detailed analysis of boarding and alighting dynamics at the door level.

---

## 🔍 Methodology

### 1️⃣ Data Preprocessing
- Filtered trajectories to relevant time windows around train arrivals
- Removed noise and short-lived detections
- Mapped pedestrian positions to platform coordinates
- Reduced dataset size by focusing on active boarding periods

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
- Fixed lines near the platform edge
- Classified passengers based on crossing direction
- Simple but limited in adaptability

#### 🔹 Dynamic Measurement Lines (KDE-based)
- Used **Gaussian Kernel Density Estimation (KDE)** to detect high-density movement regions
- Measurement lines adapted dynamically to pedestrian behavior
- More effective in capturing real-world boarding and alighting patterns

---

### 5️⃣ Feature Engineering
For each train stop and door, the following features were extracted:

- Number of boarding passengers  
- Number of alighting passengers  
- Interaction term (boarding × alighting)  
- Platform edge / door ID  
- Time window indicators (low vs high crowd periods)

---

### 6️⃣ Dwell Time Prediction
- Built **linear regression models** to predict dwell time
- Evaluated models across:
  - Different platform edges
  - Different time windows
- Performance metrics:
  - Root Mean Squared Error (RMSE)
  - Mean Absolute Percentage Error (MAPE)

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


