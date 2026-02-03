# Ex.No: 1B      CONVERTING NON-STATIONARY SERIES INTO STATIONARY FORM

## Developed By : NIJANTH K

## Register Number: 212223042186

# Date: 03/01/026

### AIM:

To convert the international airline passenger dataset into a stationary time series by applying differencing, seasonal modification, and logarithmic transformation.

### PROCEDURE:

1. Load essential Python libraries such as pandas and numpy.
2. Import the dataset using pandas.
3. If necessary, clean the data and then apply the following transformations:

   * Regular differencing
   * Seasonal adjustment
   * Logarithmic transformation
4. Visualize the dataset before and after each transformation step.
5. Summarize and interpret the changes observed.

### PROGRAM:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

df = pd.read_csv("weatherHistory.csv")
df['Formatted Date'] = pd.to_datetime(df['Formatted Date'], utc=True)
df = df.set_index('Formatted Date')


daily = df.resample('D').mean(numeric_only=True)

daily['Temp_diff1'] = daily['Temperature (C)'].diff()

decomp1 = seasonal_decompose(daily['Temperature (C)'].dropna(), model='additive', period=365)
daily['Temp_season_adj'] = decomp1.resid

daily['Temp_log'] = np.log1p(daily['Temperature (C)'] - daily['Temperature (C)'].min())

daily['Temp_log_diff'] = daily['Temp_log'].diff()

decomp2 = seasonal_decompose(daily['Temp_log_diff'].dropna(), model='additive', period=365)
daily['Temp_log_season_adj'] = decomp2.resid


fig, axes = plt.subplots(3, 2, figsize=(15, 15))

axes[0, 0].plot(daily['Temperature (C)'], color='tab:blue')
axes[0, 0].set_title("Original Temperature Series")

axes[0, 1].plot(daily['Temp_diff1'], color='tab:orange')
axes[0, 1].set_title("First Difference")

axes[1, 0].plot(daily['Temp_season_adj'], color='tab:green')
axes[1, 0].set_title("Seasonal Component Removed")

axes[1, 1].plot(daily['Temp_log'], color='tab:red')
axes[1, 1].set_title("Log Transformation")

axes[2, 0].plot(daily['Temp_log_diff'], color='tab:purple')
axes[2, 0].set_title("Log Differenced")

axes[2, 1].plot(daily['Temp_log_season_adj'], color='tab:brown')
axes[2, 1].set_title("Log + Seasonal Differencing")

for ax in axes.flat:
    ax.set_xlabel("Date")
    ax.set_ylabel("Temperature")

plt.tight_layout()
plt.show()
```

### OUTPUT:

**After Regular Differencing:**
<img width="612" height="406" alt="Screenshot 2025-08-18 161443" src="https://github.com/user-attachments/assets/9813d0c7-0baa-48bb-a951-1f0500fbf562" />
<img width="600" height="394" alt="Screenshot 2025-08-18 161501" src="https://github.com/user-attachments/assets/ad5e4aa0-bf46-41a3-bc5d-dfba4e188e4d" />
<img width="609" height="401" alt="Screenshot 2025-08-18 161511" src="https://github.com/user-attachments/assets/2369a9e6-fb19-4c4e-a626-8bd1068c7791" />
<img width="585" height="403" alt="Screenshot 2025-08-18 161522" src="https://github.com/user-attachments/assets/eb294dff-0c14-4994-9c75-5a58205fef36" />
<img width="600" height="407" alt="Screenshot 2025-08-18 161532" src="https://github.com/user-attachments/assets/3c366723-df48-4911-96da-35d7cd8ebae8" />
<img width="600" height="396" alt="Screenshot 2025-08-18 161548" src="https://github.com/user-attachments/assets/f43d5f50-2eee-4bdc-af20-3fdd3cefc696" />


### RESULT:

The non-stationary airline passenger dataset was successfully transformed into stationary form using differencing, seasonal adjustment, and logarithmic conversion techniques.



