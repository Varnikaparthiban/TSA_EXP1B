# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 21.4.2026

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data

### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
   
### PROGRAM:
```PYTHON
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose
data = pd.read_csv('/content/weather_2013_to_2024.csv')
data['DATE'] = pd.to_datetime(data['DATE'])
data.set_index('DATE', inplace=True)
col = 'temp'
data[col] = data[col].ffill()
data['diff'] = data[col] - data[col].shift(1)

result = seasonal_decompose(data[col].dropna(), model='additive', period=365)
data['seasonal_diff'] = result.resid
data['log'] = np.log(data[col].replace(0, np.nan))
data['log_diff'] = data['log'] - data['log'].shift(1)
if not data.index.is_unique:
    data = data[~data.index.duplicated(keep='first')]
result = seasonal_decompose(data['log_diff'].dropna(), model='additive', period=365)
data.loc[:, 'log_seasonal_diff'] = result.resid

plt.figure(figsize=(16, 18))
plt.subplot(6, 1, 1)
plt.plot(data[col], label='Original Data')
plt.title('Original Data')
plt.xlabel('Date')
plt.ylabel(col)
plt.legend(loc='best')

plt.figure(figsize=(16, 18))
plt.subplot(6, 1, 2)
plt.plot(data['diff'], label='Regular Differencing')
plt.title('Regular Differencing')
plt.xlabel('Date')
plt.ylabel('Differenced Values')
plt.legend(loc='best')

plt.figure(figsize=(16, 18))
plt.subplot(6, 1, 3)
plt.plot(data['seasonal_diff'], label='Seasonal Adjustment')
plt.title('Seasonal Adjustment')
plt.xlabel('Date')
plt.ylabel('Seasonally Adjusted Values')
plt.legend(loc='best')

plt.figure(figsize=(16, 18))
plt.subplot(6, 1, 5)
plt.plot(data['log_diff'], label='Log + Differencing')
plt.title('Log Transformation with Differencing')
plt.xlabel('Date')
plt.ylabel('Differenced Log Values')
plt.legend(loc='best')

plt.figure(figsize=(16, 18))
plt.subplot(6, 1, 6)
plt.plot(data['log_seasonal_diff'], label='Log with Seasonal Differencing')
plt.title('Log with Seasonal Differencing')
plt.xlabel('Date')
plt.ylabel('Final Stationary Data')
plt.legend(loc='best')
```

### OUTPUT:

ORIGINAL DATA:
<img width="1402" height="313" alt="image" src="https://github.com/user-attachments/assets/a49e3f5f-1d58-4e25-a9b3-57eec6c18b0d" />

REGULAR DIFFERENCING:
<img width="1404" height="292" alt="image" src="https://github.com/user-attachments/assets/d789df64-e4de-4c3f-ad30-0b5253a8a47b" />


SEASONAL ADJUSTMENT:
<img width="1377" height="295" alt="image" src="https://github.com/user-attachments/assets/2bf14940-41f4-4f68-9192-fabc12e604bc" />


LOG TRANSFORMATION:
<img width="1369" height="301" alt="image" src="https://github.com/user-attachments/assets/6cb4e566-a1c6-49ac-aa9e-c684d63915ee" />

LOG TRANSFORMATION WITH DIFFERENCING:
<img width="1356" height="285" alt="image" src="https://github.com/user-attachments/assets/406424bb-2c6e-47c6-a1ec-b769daadb7c5" />

### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
