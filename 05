import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import statsmodels.api as sm
from scipy.optimize import minimize

# Step 1: Generate 100 random values between 10 to 30
np.random.seed(0)  # For reproducibility
X = np.random.uniform(10, 30, 100)

# Generate error term
e = np.random.normal(0, 5, 100)  # Mean 0, Standard deviation 5

# Compute Y using the function y = 10 + 4x + e
Y = 10 + 4 * X + e

# Step 2: Create DataFrame
data = pd.DataFrame({'X': X, 'Y': Y})

# Step 3: Plot the generated values
plt.figure(figsize=(10, 6))
sns.regplot(x='X', y='Y', data=data, ci=None, line_kws={"color": "red"})
plt.title('Scatter Plot with Regression Line')
plt.xlabel('X')
plt.ylabel('Y')
plt.show()

# Step 4: OLS Regression
X_with_const = sm.add_constant(X)  # Add constant term for intercept
ols_model = sm.OLS(Y, X_with_const).fit()
print("\nOLS Regression Results:")
print(ols_model.summary())

# Step 5: Calculate SD for residuals
residuals = ols_model.resid
sd_residuals = np.std(residuals)
print(f"\nStandard Deviation of Residuals: {sd_residuals:.2f}")

# Step 6: MLE Model using L-BFGS-B
def negative_log_likelihood(params, X, Y):
    beta0, beta1, sigma = params
    Y_pred = beta0 + beta1 * X
    residuals = Y - Y_pred
    # Log-Likelihood for normal distribution
    return np.sum(np.log(sigma) + 0.5 * ((residuals / sigma) ** 2))

# Initial parameter guesses
initial_params = [10, 4, 5]  # Initial guesses for beta0, beta1, and sigma

# Optimize parameters using L-BFGS-B
result = minimize(
    negative_log_likelihood,
    initial_params,
    args=(X, Y),
    method='L-BFGS-B',
    bounds=[(None, None), (None, None), (1e-5, None)]  # Bounds for beta0, beta1, sigma
)

# Extract results
mle_params = result.x
print("\nMLE Results:")
print(f"Intercept (beta0): {mle_params[0]:.2f}")
print(f"Slope (beta1): {mle_params[1]:.2f}")
print(f"Standard Deviation (sigma): {mle_params[2]:.2f}")

# Output detailed result of optimization
print("\nOptimization Details:")
print(f"Message: {result.message}")
print(f"Success: {result.success}")
print(f"Status: {result.status}")
print(f"Function Value (fun): {result.fun:.2f}")
print(f"Parameters (x): {result.x}")
print(f"Number of Iterations (nit): {result.nit}")
print(f"Jacobian (jac): {result.jac}")
print(f"Number of Function Evaluations (nfev): {result.nfev}")
print(f"Number of Jacobian Evaluations (njev): {result.njev}")
print(f"Hessian Inverse (hess_inv): {result.hess_inv}")

# Step 7: Compare MLE Parameters with OLS Parameters
print("\nComparison of OLS and MLE Parameters:")
print(f"OLS Intercept: {ols_model.params[0]:.2f}, MLE Intercept: {mle_params[0]:.2f}")
print(f"OLS Slope: {ols_model.params[1]:.2f}, MLE Slope: {mle_params[1]:.2f}")
print(f"OLS Standard Deviation of Residuals: {sd_residuals:.2f}, MLE Standard Deviation: {mle_params[2]:.2f}")
