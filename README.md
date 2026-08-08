# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION

### Name : Sanjay Balaji S
### Reg. No: 212223240149
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

data = pd.read_csv('AEP_hourly.csv',parse_dates=['Datetime'],index_col='Datetime')
data.head()

resampled_data = data['AEP_MW'].resample('Y').sum().to_frame()
resampled_data.head()
resampled_data.index = resampled_data.index.year

resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'Datetime': 'Year'}, inplace=True)
resampled_data.head()

years = resampled_data['Year'].tolist()
values = resampled_data['AEP_MW'].tolist()
X = [i - years[len(years) // 2] for i in years]
x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, values)]
n = len(years)

b = (n * sum(xy) - sum(values) * sum(X)) / (n * sum(x2) - (sum(X) ** 2))
a = (sum(values) - b * sum(X)) / n

linear_trend = [a + b * X[i] for i in range(n)]
x3 = [i ** 3 for i in X]
x4 = [i ** 4 for i in X]
x2y = [i * j for i, j in zip(x2, values)]

coeff = [
    [len(X), sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(values), sum(xy), sum(x2y)]
A = np.array(coeff)
B = np.array(Y)

solution = np.linalg.solve(A, B)

a_poly, b_poly, c_poly = solution

poly_trend = [
    a_poly + b_poly * X[i] + c_poly * (X[i] ** 2)
    for i in range(n)
]
print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")
print(f"Polynomial Trend: y = {a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x²")

resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend
resampled_data.set_index('Year', inplace=True)

plt.figure(figsize=(10,5))
resampled_data['AEP_MW'].plot(kind='line', color='blue', marker='o')
resampled_data['Linear Trend'].plot(kind='line', color='black', linestyle='--')
plt.title('Yearly AEP Energy Consumption with Linear Trend')
plt.ylabel('AEP_MW')
plt.show()

plt.figure(figsize=(10,5))
resampled_data['AEP_MW'].plot(kind='line', color='blue', marker='o')
resampled_data['Polynomial Trend'].plot(kind='line', color='black', marker='o')
plt.title('Yearly AEP Energy Consumption with Polynomial Trend')
plt.ylabel('AEP_MW')
plt.show()
```

### OUTPUT
A - LINEAR TREND ESTIMATION

<img width="906" height="467" alt="image" src="https://github.com/user-attachments/assets/cd05594f-9bc5-4071-aefb-d8f2171a09aa" />

B- POLYNOMIAL TREND ESTIMATION

<img width="911" height="470" alt="image" src="https://github.com/user-attachments/assets/13800aba-6727-4d5f-a1b1-f07bcb5c3659" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
