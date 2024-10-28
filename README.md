# Weather Data Prediction Using Linear Regression

## Project Overview

The **Weather Data Prediction Using Linear Regression** project aims to predict future temperature values based on historical weather data, utilizing a linear regression model. By analyzing temperature trends in conjunction with humidity and pressure, the model aims to establish a relationship that allows for accurate predictions of temperature.

## Introduction to Linear Regression

**Linear Regression** is a statistical method used to model the relationship between a dependent variable (target) and one or more independent variables (features). In this project, the dependent variable is the temperature, while the independent variables are previous hour’s temperature, humidity, and pressure.

### Key Concepts

- **Synthetic Data Generation**: Creating artificial data that resembles real-world data to simulate conditions and test models.
- **Feature Engineering**: The process of creating new input features from existing data to improve model performance.
- **Train-Test Split**: Dividing the dataset into training and testing subsets to validate the model's performance.
- **Model Evaluation Metrics**:
  - **Mean Squared Error (MSE)**: Measures the average squared difference between actual and predicted values. Lower values indicate better model performance.
  - **R² Score**: Indicates the proportion of variance in the dependent variable that can be explained by the independent variables. Values range from 0 to 1, with higher values signifying better model fit.

## Code Explanation

The project is implemented in Python and leverages various libraries, including **Pandas**, **NumPy**, **Matplotlib**, and **Scikit-learn**. Below are the key components of the code.

### Imports

```python
pip install pandas numpy matplotlib scikit-learn

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score
```

### Synthetic Data Generation

The project begins by generating synthetic weather data to simulate hourly temperature, humidity, and pressure readings.

```python
# Generate synthetic weather data
np.random.seed(42)  # For reproducibility
data_size = 1000
dates = pd.date_range(start='2020-01-01', periods=data_size, freq='H')
temperature = np.random.normal(loc=20, scale=5, size=data_size)  # Average temperature of 20 with some noise
humidity = np.random.uniform(low=30, high=90, size=data_size)  # Humidity between 30% and 90%
pressure = np.random.uniform(low=980, high=1020, size=data_size)  # Pressure in hPa
```

- `np.random.seed(42)`: Ensures the reproducibility of random data generation.
- `pd.date_range()`: Creates a range of dates starting from January 1, 2020, with a frequency of one hour.
- Synthetic data for temperature, humidity, and pressure is generated using normal and uniform distributions.

### Creating the DataFrame

The synthetic data is organized into a Pandas DataFrame for easy manipulation and analysis.

```python
# Create a DataFrame
weather_data = pd.DataFrame({
    'Date': dates,
    'Temperature': temperature,
    'Humidity': humidity,
    'Pressure': pressure
})
```

### Feature Engineering

Previous hour’s data is used as features for predicting current temperature. This helps the model learn patterns based on recent observations.

```python
# Feature engineering: Use previous hours' data as features
weather_data['Temperature_Lag1'] = weather_data['Temperature'].shift(1)
weather_data['Humidity_Lag1'] = weather_data['Humidity'].shift(1)
weather_data['Pressure_Lag1'] = weather_data['Pressure'].shift(1)

# Drop NaN values generated by lagging
weather_data = weather_data.dropna()
```

- **Lagged Features**: New columns are created by shifting the original temperature, humidity, and pressure data to reference the previous hour's data.
- **NaN Handling**: The first row will contain NaN values due to the shift, which is removed using `dropna()`.

### Defining Features and Target

The independent variables (features) and the dependent variable (target) are defined for modeling.

```python
# Define features and target
X = weather_data[['Temperature_Lag1', 'Humidity_Lag1', 'Pressure_Lag1']]
y = weather_data['Temperature']
```

### Train-Test Split

The dataset is split into training and testing sets to evaluate the model's performance.

```python
# Split the dataset into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

- **Test Size**: 20% of the data is reserved for testing.
- **Random State**: Ensures the split is reproducible.

### Model Initialization and Training

A linear regression model is initialized and trained using the training data.

```python
# Initialize and train the Linear Regression model
model = LinearRegression()
model.fit(X_train, y_train)
```

### Making Predictions

Predictions are made on the test set using the trained model.

```python
# Make predictions
y_pred = model.predict(X_test)
```

### Model Evaluation

The performance of the model is evaluated using MSE and R² score.

```python
# Evaluate the model
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

# Output the evaluation metrics
print("Mean Squared Error:", mse)
print("R^2 Score:", r2)
```

### Visualization

A scatter plot is generated to compare the actual and predicted temperature values, providing a visual representation of the model's performance.

```python
# Visualize the predictions vs actual values
plt.figure(figsize=(12, 6))
plt.scatter(y_test, y_pred, alpha=0.5)
plt.plot([y.min(), y.max()], [y.min(), y.max()], 'r--', lw=2)
plt.xlabel("Actual Temperature")
plt.ylabel("Predicted Temperature")
plt.title("Actual vs Predicted Temperature")
plt.grid()
plt.show()
```

- The plot displays the predicted temperatures against the actual temperatures. The closer the points are to the red dashed line, the better the model’s predictions.

## Conclusion

The **Weather Data Prediction Using Linear Regression** project demonstrates the use of linear regression for predicting temperature based on historical weather data. The project showcases key steps in data preparation, feature engineering, model training, and evaluation. By visualizing the predictions, users can gain insights into the model's effectiveness, laying the groundwork for more complex weather prediction models in the future.