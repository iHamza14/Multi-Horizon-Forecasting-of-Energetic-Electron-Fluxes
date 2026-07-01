# PRAGYA AI
### Predictive Radiation Analysis for Geostationary Applications

> Physics-Informed Multi-Horizon Forecasting of Energetic Electron Fluxes for ISRO Geostationary Satellites

---

## Overview

PRAGYA AI is a physics-informed deep learning framework designed to forecast energetic (>2 MeV) electron fluxes at geostationary orbit.

The objective is to provide reliable forecasts

- 30–45 minutes ahead
- 6 hours ahead
- 12 hours ahead

using historical GOES electron flux measurements together with upstream solar wind observations from the WIND spacecraft.

Unlike conventional forecasting systems, PRAGYA AI combines heliophysics knowledge with deep learning to improve robustness during geomagnetic storms while maintaining operational feasibility.

---

# Problem Statement

Energetic electrons trapped within Earth's outer radiation belt can damage satellites through

- Surface charging
- Deep dielectric charging
- Solar panel degradation
- Electronic component failures

Current forecasting techniques either

- rely heavily on computationally expensive physics simulations, or
- purely data-driven machine learning models that struggle during extreme space weather events.

This project aims to bridge both approaches by integrating physical understanding directly into the AI pipeline.

---

# Project Goals

- Read and process GOES and WIND satellite CDF datasets
- Build a robust preprocessing pipeline
- Engineer physically meaningful features
- Train a hybrid deep learning architecture
- Generate forecasts for multiple prediction horizons
- Quantify uncertainty
- Produce operational risk alerts
- Validate predictions against ISRO GRASP observations

---

# Dataset

## GOES

- >2 MeV Electron Flux
- 11 years
- 5-minute cadence
- CDF format

---

## WIND

Solar wind parameters

- Speed
- Density
- IMF Components
- Magnetic Field Strength

11 years of observations

---

## ISRO GRASP

Used only for independent validation.

---

# Overall Pipeline

```
GOES CDF
          \
           \
            ---> Data Ingestion
           /
WIND CDF /
         

↓

Physics-Based Time Alignment

↓

Quality Control

↓

Gap Handling

↓

Feature Engineering

↓

Feature Selection

↓

Sliding Window Construction

↓

Hybrid Forecast Model

↓

Multi-Horizon Prediction

↓

Physics Constraints

↓

Uncertainty Estimation

↓

Operational Alerts

↓

Visualization Dashboard
```

---

# System Architecture

The project consists of three major modules.

```
Data Layer
      ↓
Preprocessing Layer
      ↓
Forecasting Engine
      ↓
Post Processing
      ↓
Visualization
```

---

# Phase 1 — Data Preprocessing

The preprocessing pipeline converts raw satellite observations into machine learning ready tensors.

## Steps

### Reading CDF Files

- Parse GOES observations
- Parse WIND observations
- Convert timestamps
- Merge observations

---

### Physics-Based Time Alignment

Solar wind measured at the L1 point reaches Earth after approximately 30–80 minutes.

Instead of directly joining timestamps, solar wind observations are shifted using propagation delay.

This prevents look-ahead bias during training.

---

### Data Cleaning

- Remove corrupted measurements
- Remove spikes
- Handle missing observations
- Cross-calibrate multiple satellites

---

### Gap Filling

Short gaps

- Linear interpolation

Medium gaps

- Physics-based exponential decay

Large gaps

- Removed from training

---

### Feature Engineering

Raw measurements are transformed into physically meaningful variables.

Examples include

- Dynamic Pressure
- Southward IMF
- Solar Wind Electric Field
- Newell Coupling Function
- Magnetic Local Time

---

### Feature Selection

Features are ranked using

- Mutual Information
- Correlation Analysis
- Physical relevance

Only the most informative variables are retained.

---

### Sequence Generation

Historical observations are converted into sliding windows.

Example

```
Past 24 Hours
↓

Window

↓

Target

45 min

6 hour

12 hour
```

---

# Phase 2 — Forecasting Engine

The forecasting engine combines recurrent neural networks with transformers.

```
Input Tensor

↓

Feature Projection

↓

Transformer Encoder

↓

BiLSTM Encoder

↓

Temporal Attention

↓

Shared Representation

↓

Prediction Heads

├── 45 min

├── 6 hr

└── 12 hr
```

---

## Why Hybrid?

Transformer

- Long-range dependencies
- Global context

BiLSTM

- Local temporal dynamics
- Storm evolution

Together they learn both

- rapid disturbances
- slow radiation belt evolution

---

# Multi-Horizon Forecasting

The network predicts

```
T + 45 min

T + 6 hr

T + 12 hr
```

simultaneously from one shared representation.

This ensures consistent forecasts across different prediction horizons.

---

# Phase 3 — Post Processing

Raw neural network outputs are converted into operational forecasts.

This stage includes

- Adaptive bias correction
- Physical constraints
- Confidence estimation
- Risk classification

---

## Adaptive Learning

Recent prediction errors are continuously monitored.

Rolling statistics are used to compensate for

- sensor drift
- seasonal variation
- solar cycle changes

without retraining the neural network.

---

## Physics Constraints

Predictions are constrained to remain physically realistic.

Examples

- Non-negative flux
- Maximum allowable flux
- Maximum decay rate

---

## Confidence Estimation

Prediction uncertainty is estimated using rolling historical error statistics.

Each forecast is accompanied by

- confidence interval
- uncertainty score

---

## Risk Classification

Forecasts are mapped into operational alerts.

| Level | Description |
|---------|------------|
| 🟢 Green | Normal |
| 🟡 Yellow | Elevated |
| 🔴 Red | High Radiation |

---

# Visualization

The web dashboard provides

- Historical observations
- Live predictions
- Multi-horizon forecasts
- Confidence intervals
- Solar wind monitoring
- Risk alerts

---

# Repository Structure

```
PRAGYA-AI/

│

├── data/

│ ├── raw/

│ ├── processed/

│ └── validation/

│

├── notebooks/

│

├── preprocessing/

│

├── feature_engineering/

│

├── models/

│ ├── bilstm/

│ ├── transformer/

│ └── hybrid/

│

├── training/

│

├── evaluation/

│

├── inference/

│

├── dashboard/

│

├── utils/

│

├── configs/

│

├── tests/

│

└── README.md
```

---

# Technology Stack

## AI / ML

- Python
- PyTorch
- NumPy
- Pandas
- Scikit-learn

---

## Data Processing

- cdflib
- xarray
- SciPy

---

## Visualization

- Plotly
- Matplotlib

---

## Backend

- FastAPI

---

## Frontend

- React
- TypeScript
- Tailwind CSS

---

## Deployment

- Docker
- ONNX Runtime

---

# Development Roadmap

## Phase 1

- Dataset collection
- CDF parser
- Data preprocessing

---

## Phase 2

- Feature engineering
- Dataset generation
- Exploratory analysis

---

## Phase 3

- Baseline models
- LSTM
- Transformer
- XGBoost

---

## Phase 4

- Hybrid BiLSTM Transformer

---

## Phase 5

- Hyperparameter optimization
- Validation
- Benchmarking

---

## Phase 6

- Dashboard
- APIs
- Deployment

---

# Expected Deliverables

- Physics-informed forecasting pipeline
- Multi-horizon prediction model
- Operational web dashboard
- Risk alert system
- Independent GRASP validation
- Open-source implementation

---

# Team ORCA

Developed as part of the Bharatiya Antariksh Hackathon (ISRO)
