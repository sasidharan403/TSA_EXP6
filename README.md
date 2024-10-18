#### Developed by A.Sasidharan
#### Register no: 212221240049
#### Date: 
# Ex.No: 6               HOLT WINTERS METHOD
### AIM:

To implement Holt-Winters model on Global Temperature Change Data Set.
### ALGORITHM:
1. You import the necessary libraries
2. You load a CSV file containing daily sales data into a DataFrame, parse the 'date' column as
datetime, and perform some initial data exploration
3. You group the data by date and resample it to a monthly frequency (beginning of the month
4. You plot the time series data
5. You import the necessary 'statsmodels' libraries for time series analysis
6. You decompose the time series data into its additive components and plot them:
7. You calculate the root mean squared error (RMSE) to evaluate the model's performance
8. You calculate the mean and standard deviation of the entire sales dataset, then fit a Holt-
Winters model to the entire dataset 
9. You plot the original  Global Temperature Change Data
### PROGRAM:
```
# Import the necessary libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.holtwinters import ExponentialSmoothing

# Load the dataset
data = pd.read_csv('/content/globaltemper.csv')

# Convert the 'dt' column to datetime format and set it as the index
# The error was due to an incorrect date format. 
# The format was likely 'DD-MM-YYYY' or similar, not '%m-%d-%Y'.
# Changed the format to '%d-%m-%Y' to reflect the actual date format in the CSV file.
data['dt'] = pd.to_datetime(data['dt'], format='%d-%m-%Y')  
data.set_index('dt', inplace=True)  

# Filter the data to include only records from 2015 onward (optional)
#data = data[data.index >= '2015-01-01']

# Convert 'AverageTemperature' column to numeric (removing invalid values)
data['AverageTemperature'] = pd.to_numeric(data['AverageTemperature'], errors='coerce')

# Drop rows with missing values in 'AverageTemperature' column
clean_data = data.dropna(subset=['AverageTemperature'])

# Extract 'AverageTemperature' column for time series forecasting
temperature_data = clean_data['AverageTemperature']

# Perform Holt-Winters exponential smoothing
model = ExponentialSmoothing(temperature_data, trend="add", seasonal="add", seasonal_periods=12) # Assuming yearly seasonality
fit = model.fit()

# Forecast for the next 30 steps (e.g., months)
n_steps = 30
forecast = fit.forecast(steps=n_steps)

# Plot the original data and the forecast
plt.figure(figsize=(12, 6))
plt.plot(temperature_data.index, temperature_data, label='Historical Average Temperature')
plt.plot(pd.date_range(start=temperature_data.index[-1], periods=n_steps + 1, freq='M')[1:], forecast, label='Forecast')  # 'M' for monthly frequency
plt.xlabel('Year')
plt.ylabel('Average Temperature')  
plt.title('Holt-Winters Forecast for Global Temperature Change')
plt.legend()
plt.grid(True)  # Added grid for better visualization
plt.show()
```

### OUTPUT:
![image](https://github.com/user-attachments/assets/51d63c9a-6319-439b-8e5f-d6d2b5632383)


### RESULT:
The program was successfully completed based on the Holt Winters Method model
