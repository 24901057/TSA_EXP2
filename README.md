# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date: 28-04-2026
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# 1. Load and Setup (Assuming your CSV now has 'Date' and 'Weekly_Sales')
data = pd.read_csv('/content/Walmart_Sales.csv')
data=data.head(100)
data['Date'] = pd.to_datetime(data['Date'], format='%d-%m-%Y')
# Set the 'Date' column as the DataFrame's index
data.set_index('Date', inplace=True)
resampled_data = data['Weekly_Sales'].resample('W').sum().to_frame()
resampled_data.reset_index(inplace=True)

# 3. Prepare X (Time indices) for Trend Estimation
# Instead of years, we use the row index to represent time steps
n = len(resampled_data)
# Centering X helps keep the numbers manageable for manual math
X = [i - (n // 2) for i in range(n)]
sales = resampled_data['Weekly_Sales'].tolist()

# 4. Linear Trend Estimation
x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, sales)]

b = (n * sum(xy) - sum(sales) * sum(X)) / (n * sum(x2) - (sum(X) ** 2))
a = (sum(sales) - b * sum(X)) / n
linear_trend = [a + b * X[i] for i in range(n)]

# 5. Polynomial Trend Estimation (Degree 2)
x3 = [i ** 3 for i in X]
x4 = [i ** 4 for i in X]
x2y = [i * j for i, j in zip(x2, sales)]

coeff = [[n, sum(X), sum(x2)],
         [sum(X), sum(x2), sum(x3)],
         [sum(x2), sum(x3), sum(x4)]]
Y_vals = [sum(sales), sum(xy), sum(x2y)]

A = np.array(coeff)
B = np.array(Y_vals)
a_poly, b_poly, c_poly = np.linalg.solve(A, B)
poly_trend = [a_poly + b_poly * X[i] + c_poly * (X[i] ** 2) for i in range(n)]

# 6. Visualising Results
print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")
print(f"Polynomial Trend: y = {a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x²")

resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend
resampled_data.set_index('Date', inplace=True)

plt.figure(figsize=(12, 6))
plt.plot(resampled_data.index, resampled_data['Weekly_Sales'], label='Weekly Sales', color='blue')
resampled_data['Linear Trend'].plot(label='Linear Trend', color='black', linestyle='--')
resampled_data['Polynomial Trend'].plot(label='Polynomial Trend', color='red', linewidth=2)
plt.legend()
plt.show()
```

### OUTPUT

<img width="1326" height="748" alt="image" src="https://github.com/user-attachments/assets/a9b1fe66-985a-440e-a513-1cef63259907" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
