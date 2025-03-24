# Anomaly Detection System for Industrial Dosing and Heating Systems

## Overview

This project implements an unsupervised anomaly detection system using autoencoders to identify abnormal behavior in industrial dosing and heating control systems. The model learns normal operational patterns from historical data and flags deviations that might indicate equipment failures, calibration issues, or other anomalies requiring attention.

## Features

- **Unsupervised Learning**: Detects anomalies without labeled training data
- **Autoencoder Architecture**: Uses deep learning to identify complex patterns
- **Visualization Tools**: Includes various plots to analyze system behavior
- **Feature Importance Analysis**: Identifies which sensors contribute most to anomalies
- **Production-Ready Functions**: Includes deployable code for real-time anomaly detection

## Data Description

The system monitors multiple components:

- 8 dosing systems (setpoints and actual values)
- Heater control systems (multiple cross and long configurations)

## How It Works

### 1. Data Preprocessing

- Converts all features to numeric format
- Normalizes data using Min-Max scaling
- Removes non-informative columns

### 2. Model Architecture

The autoencoder consists of:

- Input layer matching feature dimensions
- Encoder: Dense layers (32 → 10 neurons)
- Decoder: Dense layers (10 → 32 → output dimension)
- Activation functions: ReLU for hidden layers, Sigmoid for output

### 3. Training Process

- Split data into training (80%) and validation (20%) sets
- Uses Mean Squared Error (MSE) as loss function
- Implements Early Stopping to prevent overfitting
- Adam optimizer for efficient training

### 4. Anomaly Detection

- Calculates reconstruction error for each sample
- Sets threshold at 95th percentile of training errors
- Flags samples with errors above threshold as anomalies

### 5. Result Analysis

- Visualizes reconstruction errors across the dataset
- Identifies the most problematic features
- Provides detailed anomaly reports

## Usage

### Loading the Model

```python
from tensorflow.keras.models import load_model
import joblib

# Load the trained model and scaler
model = load_model('anomaly_autoencoder_model.h5')
scaler = joblib.load('anomaly_scaler.pkl')
```

### Detecting Anomalies in New Data

```python
import pandas as pd

# Load new data
new_df = pd.read_csv('new_data.csv')

# Detect anomalies
results = detect_anomalies(new_df, model, scaler, threshold)

# View anomalies
anomalies = results[results['is_anomaly'] == True]
```

## Related Research

This implementation is based on techniques described in the following research papers:

1. Sakurada, M., & Yairi, T. (2014). "Anomaly detection using autoencoders with nonlinear dimensionality reduction." Proceedings of the MLSDA 2014 2nd Workshop on Machine Learning for Sensory Data Analysis.

2. Malhotra, P., Vig, L., Shroff, G., & Agarwal, P. (2016). "LSTM-based encoder-decoder for multi-sensor anomaly detection." arXiv preprint arXiv:1607.00148.

3. An, J., & Cho, S. (2015). "Variational autoencoder based anomaly detection using reconstruction probability." Special Lecture on IE, 2(1), 1-18.

## Requirements

- Python 3.7+
- TensorFlow 2.x
- NumPy
- Pandas
- Matplotlib
- Seaborn
- Scikit-learn
- Joblib

## Future Improvements

- Implement time-series specific models (LSTM Autoencoders)
- Add real-time monitoring capabilities
- Develop explanation module for root cause analysis
- Integrate with notification systems
