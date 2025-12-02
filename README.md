# Optimizing-a-Pizza-Chain-Promotion-Policy
Business use case : A pizza chain company ask us to optimize their promotion policy. 

# Project README

---
<img width="1014" height="528" alt="Pres" src="https://github.com/user-attachments/assets/1d649a08-0cf8-4aaf-8b15-8f895f22b6ba" />

Here is a comprehensive README file based on the analysis of your Jupyter Notebook. You can copy and paste this directly into your GitHub repository as `README.md`.

-----

# 🍕 Pizza Chain Promotion Optimization

## Overview

This project focuses on optimizing the promotion policy for a fictional pizza restaurant chain. Using a synthetic dataset spanning historical sales from 1995 to 2024, the project applies Time Series Analysis and Machine Learning to determine the optimal discount level (0% to 50%) to apply on any given day to maximize revenue.

The goal is to transition from a "random" promotion strategy to a data-driven "optimized" strategy for the year 2025.

## Business Context & Assumptions

**Objective:** A pizza chain has asked for an optimization of their promotion policy to maximize profit.

**Key Constraints & Assumptions:**

  * **Data Source:** The dataset is synthetic and theoretical.
  * **Profit Proxy:** Profit is approximated as Revenue (Selling Price $\times$ Quantity Sold). Cost of Goods Sold (COGS) is not included in this specific iteration.
  * **Historical Context:** The company previously placed promotions randomly with varying intensities (10%, 20%, 50%).
  * **Exclusions:** Exceptional events (holidays, sports finals) and geographic cannibalization between store locations are excluded from this model.

## Dataset Description

The analysis uses `simulated_retail_data.csv`, containing **219,000 entries** representing daily sales data for various pizza products.

| Feature | Description |
| :--- | :--- |
| `product_id` | Unique identifier for the product panel |
| `date` | Transaction date (daily) |
| `sales` | Quantity sold (float) |
| `promotion` | Discount intensity (e.g., 0.3 = 30% off) |
| `product_name` | Human-readable name (e.g., Margherita) |
| `initial_price`| Base price before discount |
| `sell_price` | Final price after promotion applied |

## Methodology

### 1\. Exploratory Data Analysis (EDA) & Feature Engineering

  * Analysis of sales distribution and promotion frequency.
  * Feature extraction from timestamps (Year, Month, Day, Day of Week).
  * Generation of binary flags for active promotions.

### 2\. Time Series Analysis

  * **FFT (Fast Fourier Transform):** Applied to identify dominant frequencies in sales signals.
  * **MSTL Decomposition:** Decomposed time series into Trend, Seasonality (Weekly, Monthly, Yearly), and Residual components to understand underlying patterns.

### 3\. Sales Forecasting Model

  * **Algorithm:** Random Forest Regressor.
  * **Features Used:** Product encoding, Temporal features (Year, Month, Day, DayOfWeek), and Promotion intensity.
  * **Training:** The model was trained on historical data to learn the elasticity of demand relative to price reductions and seasonal trends.

### 4\. Optimization Engine (Simulation)

  * **Scenario Generation:** For the target year (2025), the model simulated sales scenarios for every product for every day under different promotion regimes (0%, 5%, 10% ... 50%).
  * **Maximization:** Calculated projected Revenue for each scenario and selected the promotion level that yielded the highest return.

## Results

The project compares the historical average profit (Non-optimized) against the projected 2025 profit (Optimized).

  * **Strategy:** The model identifies specific days where demand elasticity suggests a promotion will drive enough volume to offset the lower margin, and days where full price yields better returns.
  * **Outcome:** The final visualization demonstrates the potential growth percentage by switching to the optimized schedule.

## Dependencies

To run this project, you will need the following libraries:

```bash
pip install pandas numpy matplotlib seaborn statsmodels scikit-learn scipy tqdm
```

## Disclaimer

This work is applied to a synthetic dataset for educational and theoretical demonstration purposes. Real-world application would require further validation against actual operational costs and constraints.
