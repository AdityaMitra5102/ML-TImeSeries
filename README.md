\# ML-TimeSeries: Machine Learning Based SSH Brute-Force Detection



\## Overview



This project analyzes \*\*SSH authentication logs\*\* to detect \*\*brute-force attacks\*\* using statistical time-series analysis and machine learning.



The objective is to understand how malicious login behavior differs from normal SSH traffic and build an automated system capable of detecting attacks in \*\*near real-time\*\*.



The project combines:



\* \*\*Time-series analysis\*\*

\* \*\*Signal processing techniques\*\*

\* \*\*Machine learning classification\*\*

\* \*\*Real-time intrusion detection\*\*



---



\# Project Motivation



SSH brute-force attacks attempt to gain unauthorized access by repeatedly trying different credentials.



These attacks typically produce \*\*distinct temporal patterns\*\* in authentication logs:



\* rapid bursts of login attempts

\* high failure ratios

\* multiple username guesses

\* abnormal login frequency



By analyzing these patterns statistically and training machine learning models, we can automatically detect malicious activity.



---



\# Dataset



The project uses the \*\*SSH Anomaly Dataset\*\* from Kaggle.



Dataset characteristics:



\* ~41,825 SSH authentication events

\* labeled network activity



Labels include:



\* `normal`

\* `brute\_force`

\* `brute\_force\_connection\_issue`

\* `config\_anomaly`



Each record contains:



| Column     | Description                 |

| ---------- | --------------------------- |

| timestamp  | time of SSH event           |

| source\_ip  | IP address attempting login |

| username   | username used               |

| event\_type | SSH authentication event    |

| status     | success / failure           |

| label      | ground truth class          |



---



\# System Architecture



The detection pipeline follows this workflow:



```

SSH Logs

&nbsp;  ↓

Log Parsing \& Cleaning

&nbsp;  ↓

Time-Series Analysis

&nbsp;  ↓

Sliding Window Feature Engineering

&nbsp;  ↓

Machine Learning Models

&nbsp;  ↓

Attack Detection

```



---



\# Part 1 — Statistical \& Time-Series Analysis



Before building machine learning models, the project investigates the \*\*temporal structure of SSH traffic\*\*.



\## Techniques Used



\### Descriptive Statistics



Basic statistical properties are analyzed including:



\* mean login frequency

\* variance

\* skewness

\* kurtosis

\* inter-arrival times



\### Stationarity Tests



Two statistical tests are applied:



\* Augmented Dickey-Fuller (ADF)

\* KPSS Test



These determine whether the login time series is stationary.



\### Temporal Pattern Analysis



The project examines login attempts per second and rolling averages to identify bursty attack behavior.



\### Autocorrelation Analysis



ACF and PACF plots reveal correlations in login activity across time lags.



---



\# Frequency Domain Analysis



Signal processing techniques are used to understand periodic attack patterns.



\## Power Spectral Density



Welch’s method identifies dominant frequencies in login attempts.



\## Fast Fourier Transform (FFT)



Detects repeating patterns in authentication attempts.



\## STFT Spectrogram



Shows how frequency content evolves over time.



---



\# Wavelet Multi-Resolution Analysis



Wavelet decomposition is used to analyze login activity at multiple time scales.



Key observation:



\* \*\*Brute-force traffic\*\*



&nbsp; \* large bursts of login attempts

&nbsp; \* strong energy in low-frequency components



\* \*\*Normal traffic\*\*



&nbsp; \* continuous low-intensity events

&nbsp; \* higher energy in fine-scale components



This finding motivates the use of \*\*multiple sliding windows\*\* in the detection model.



---



\# Part 2 — Feature Engineering



The detection system computes behavioral features for each IP address using sliding time windows.



Window sizes:



\* 30 seconds

\* 60 seconds

\* 300 seconds



Example engineered features:



| Feature      | Description                        |

| ------------ | ---------------------------------- |

| attempt\_rate | login attempts per second          |

| fail\_ratio   | fraction of failed logins          |

| unique\_users | number of usernames attempted      |

| iat\_mean     | mean inter-arrival time            |

| iat\_std      | variability of login timing        |

| n\_failed\_pw  | number of failed password attempts |



These features capture \*\*behavioral patterns of attackers\*\*.



---



\# Machine Learning Models



Two models are evaluated.



\## Isolation Forest



Unsupervised anomaly detection model.



Training strategy:



\* trained on \*\*normal traffic only\*\*

\* detects deviations from normal behavior.



Performance:



```

ROC-AUC ≈ 0.998

F1 Score ≈ 0.997

```



---



\## LightGBM Classifier



Gradient boosting model for supervised classification.



Advantages:



\* handles nonlinear relationships

\* robust to feature interactions

\* performs well on imbalanced datasets



Performance:



```

ROC-AUC ≈ 1.000

F1 Score ≈ 0.9998

```



LightGBM achieved near-perfect detection performance.



---



\# Model Evaluation



Evaluation metrics include:



\* ROC-AUC

\* Precision-Recall Curve

\* Confusion Matrix

\* F1 Score



LightGBM significantly outperformed the anomaly detection baseline.



---



\# Real-Time Detection System



The project implements a production-style \*\*online SSH brute-force detector\*\*.



Features:



\* real-time log ingestion

\* per-IP sliding window monitoring

\* automatic feature extraction

\* machine learning scoring

\* attack alert generation



Example usage:



```python

detector.ingest(log\_line)

```



The system returns:



\* attack probability

\* alert flag for suspicious activity.



---



\# Project Structure



```

ML-TimeSeries

│

├── auth.log

├── convert.py

├── log.csv

│

├── statistical\_analysis.ipynb

├── ssh\_bruteforce\_detector.ipynb

│

└── README.md

```



---



\# Technologies Used



Python ecosystem:



\* pandas

\* numpy

\* scipy

\* statsmodels

\* pywavelets

\* scikit-learn

\* lightgbm

\* matplotlib

\* seaborn



---



\# Applications



This project demonstrates techniques used in:



\* Intrusion Detection Systems (IDS)

\* Security Information and Event Management (SIEM)

\* Network anomaly detection

\* Cybersecurity monitoring platforms



---



\# Future Improvements



Possible extensions:



\* real-time streaming pipeline

\* deployment with Kafka / Elastic Stack

\* deep learning models (LSTM / Transformers)

\* integration with SIEM systems

\* adaptive detection thresholds



---



\# License



This project is provided for educational and research purposes.



---



\# Authors



Machine Learning project on \*\*SSH security analytics and intrusion detection\*\*.



