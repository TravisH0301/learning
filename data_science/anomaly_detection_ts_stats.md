# Anomaly Detection in Time Series using Statistics

## Statistical way to detect outliers
- For time series data with normal distribution: Use of z-score threshold (e.g., |Z| > 2 => outlier)
- For time series data without normal distribution: Use of IQR whiskers (e.g., x < (Q1 – 1.5 x IQR) => outlier)

### How to reduce noise
- Use rolling average smoothing for n periods (e.g., 4 weeks)
  - Longer n reduces the impact of recent temporal spikes
  - Shorter n increases responsiveness to recent trends

### How to alter outlier sensitivity
- Z-score: Increase threshold
- IQR: Increase IQR multiplier (e.g., Q1 – 2 x IQR)

### How to react to seasonality
- Ensure period range captures at least 1 seasonal cycle
