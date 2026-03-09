# ML-TimeSeries: Machine Learning Based SSH Brute-Force Detection

## Overview

This project analyzes **SSH authentication logs** to detect **brute-force attacks** using time-series analysis and machine learning.

The goal is to understand how malicious login behavior differs from normal SSH traffic and build an automated system capable of detecting attacks in real time.

---

## Project Motivation

SSH brute-force attacks attempt to gain unauthorized access by repeatedly trying different login credentials.

These attacks create distinctive patterns in authentication logs:

* high-frequency login attempts
* large numbers of failed logins
* multiple username guesses
* abnormal login timing patterns

By analyzing these patterns statistically and applying machine learning, we can automatically detect malicious activity.

---

## Dataset

The project uses the **SSH Anomaly Dataset** from Kaggle.

Dataset size:

* ~41,825 SSH authentication events

Labels include:

* `normal`
* `brute_force`
* `brute_force_connection_issue`
* `config_anomaly`

Each record contains:

| Column     | Description                 |
| ---------- | --------------------------- |
| timestamp  | time of SSH event           |
| source_ip  | IP address attempting login |
| username   | username used               |
| event_type | SSH authentication event    |
| status     | success or failure          |
| label      | traffic class               |

---

## System Architecture

```
SSH Logs
   ↓
Log Parsing
   ↓
Time-Series Analysis
   ↓
Sliding Window Feature Engineering
   ↓
Machine Learning Models
   ↓
Attack Detection
```

---

## Part 1 — Statistical & Time-Series Analysis

The first part analyzes SSH login behavior using statistical and signal-processing techniques.

Methods used include:

* Descriptive statistics
* Stationarity tests (ADF, KPSS)
* Autocorrelation analysis (ACF, PACF)
* Spectral analysis (FFT, Power Spectral Density)
* Time-frequency analysis (STFT spectrogram)
* Wavelet multi-resolution analysis

### Key Insight

Brute-force traffic appears as **bursty clusters of login attempts**, while normal traffic appears as **low-frequency continuous activity**.

---

## Part 2 — Feature Engineering

Behavioral features are extracted per IP using sliding windows.

Window sizes:

* 30 seconds
* 60 seconds
* 300 seconds

Example features:

| Feature      | Description                   |
| ------------ | ----------------------------- |
| attempt_rate | login attempts per second     |
| fail_ratio   | fraction of failed logins     |
| unique_users | number of usernames attempted |
| iat_mean     | mean inter-arrival time       |
| iat_std      | variability of login timing   |

These features capture patterns typical of brute-force attacks.

---

## Machine Learning Models

Two models were evaluated.

### Isolation Forest

Unsupervised anomaly detection model.

Performance:

* ROC-AUC ≈ 0.998
* F1 Score ≈ 0.997

---

### LightGBM

Supervised gradient boosting classifier.

Performance:

* ROC-AUC ≈ 1.000
* F1 Score ≈ 0.9998

LightGBM achieved near-perfect detection accuracy.

---

## Real-Time Detection System

The project includes an **online SSH brute-force detector** capable of analyzing logs as they arrive.

Example usage:

```python
detector.ingest(log_line)
```

The system outputs:

* attack probability
* alert flag for suspicious activity

---

## Project Structure

```
ML-TimeSeries
│
├── auth.log
├── convert.py
├── log.csv
│
├── statistical_analysis.ipynb
├── ssh_bruteforce_detector.ipynb
│
└── README.md
```

---

## Technologies Used

* Python
* Pandas
* NumPy
* SciPy
* Statsmodels
* PyWavelets
* Scikit-learn
* LightGBM
* Matplotlib
* Seaborn

---

## Applications

This work demonstrates techniques used in:

* Intrusion Detection Systems (IDS)
* Network anomaly detection
* Cybersecurity monitoring
* Security analytics platforms

---

## Authors

Machine Learning class project on SSH security analytics and brute-force detection.
