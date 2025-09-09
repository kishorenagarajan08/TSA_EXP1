# Ex.No: 01A PLOT A TIME SERIES DATA
### Developed By: KISHORE N
### Register Number: 212223240064

# AIM:
To Develop a python program to Plot a time series data (population/ market price of a commodity
/temperature.
# ALGORITHM:
1. Import the required packages like pandas and matplot
2. Read the dataset using the pandas
3. Calculate the mean for the respective column.
4. Plot the data according to need and can be altered monthly, or yearly.
5. Display the graph.
# PROGRAM:
```py

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

data = pd.read_csv("avocado.csv")
data['Date'] = pd.to_datetime(data['Date'])
data = data.groupby('Date')['Total Volume'].mean().reset_index()
data = data.sort_values('Date')
data.set_index('Date', inplace=True)
data_main = data[['Total Volume']]

data_main['volume_diff'] = data_main['Total Volume'] - data_main['Total Volume'].shift(1)

result = seasonal_decompose(data_main['Total Volume'], model='additive', period=52)
data_main['volume_sea_diff'] = result.resid

data_main['volume_log'] = np.log(data_main['Total Volume'])
data_main['volume_log_diff'] = data_main['volume_log'] - data_main['volume_log'].shift(1)

result_log = seasonal_decompose(data_main['volume_log_diff'].dropna(), model='additive', period=52)
resid_log = result_log.resid
resid_log.index = data_main['volume_log_diff'].dropna().index
data_main['volume_log_sea_diff'] = resid_log

plt.figure(figsize=(16, 18))

plt.subplot(6, 1, 1)
plt.plot(data_main['Total Volume'], label='Original')
plt.legend(loc='best')
plt.title('Original Data')
plt.xlabel('Year')
plt.ylabel('Total Volume')

plt.subplot(6, 1, 2)
plt.plot(data_main['volume_diff'], label='Regular Difference')
plt.legend(loc='best')
plt.title('Regular Differencing')
plt.xlabel('Year')
plt.ylabel('Differenced Volume')

plt.subplot(6, 1, 3)
plt.plot(data_main['volume_sea_diff'], label='Seasonal Adjustment')
plt.legend(loc='best')
plt.title('Seasonal Adjustment')
plt.xlabel('Year')
plt.ylabel('Seasonally Adjusted Volume')

plt.subplot(6, 1, 4)
plt.plot(data_main['volume_log'], label='Log Transformation')
plt.legend(loc='best')
plt.title('Log Transformation')
plt.xlabel('Year')
plt.ylabel('Log(Volume)')

plt.subplot(6, 1, 5)
plt.plot(data_main['volume_log_diff'], label='Log Transformation + Regular Differencing')
plt.legend(loc='best')
plt.title('Log Transformation and Regular Differencing')
plt.xlabel('Year')
plt.ylabel('RDiff(Log(Volume))')

plt.subplot(6, 1, 6)
plt.plot(data_main['volume_log_sea_diff'], label='Log + Regular Differencing + Seasonal Differencing')
plt.legend(loc='best')
plt.title('Log Transformation + Regular Differencing + Seasonal Differencing')
plt.xlabel('Year')
plt.ylabel('SDiff(RDiff(Log(Volume)))')

plt.tight_layout()
plt.show()

```

# OUTPUT:
<img width="1249" height="467" alt="image" src="https://github.com/user-attachments/assets/7adc2e6f-5469-4175-8fd8-4fa1ff51dc0f" />
<img width="1247" height="464" alt="image" src="https://github.com/user-attachments/assets/5ae2959f-7e6d-4612-92bb-f9b79bf5d092" />
<img width="1225" height="457" alt="image" src="https://github.com/user-attachments/assets/35d34d23-fc1e-460a-b76e-9bc15a958fd4" />


# RESULT:
Thus we have created the python code for plotting the time series of given data.
