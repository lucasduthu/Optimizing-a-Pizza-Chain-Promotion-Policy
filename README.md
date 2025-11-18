# Optimizing-a-Pizza-Chain-Promotion-Policy
Business use case : A pizza chain company ask us to optimize their promotion policy. 

# Project README

---


## Business Use Case

The primary goal of this project was to help a pizza chain company **optimize their promotion policy** to maximize revenue (approximated as profit).

The initial context and assumptions for this theoretical exercise, which uses a synthetic dataset, are:
* The company's past promotions were placed **randomly** in time with random intensity (e.g., 10%, 20%, 50%).
* **Profit is approximated as revenue** (selling price is the only cost considered).
* Effects of exceptional events (like holidays or sports) were **not included**.
* Effects of promotions in geographically close restaurants were **not included**.

---


## Dataset

The project uses a **synthetic retail dataset** (`simulated_retail_data.csv`).


### Variables

| Variable | Description |
| :--- | :--- |
| `product_id` | Panel identifier for the product. |
| `date` | Date of sale. |
| `sales` | Quantity sold (float, no units specified). |
| `promotion` | Promotion expressed as a portion of the price (e.g., 0.5 = 50% promotion). |
| `product_name` | Name of the product. |
| `initial_price` | Price before promotion. |
| `sell_price` | Price after reduction. |

### Pre-processing and Feature Engineering
* Negative `sales` values were set to 0.
* A binary variable, **`promo`**, was created (1 if a promotion was active, 0 otherwise).
* Time features (`year`, `month`, `day`, `dayofweek`) were extracted from the `date` column.
* **Seasonal components** (weekly, monthly, yearly) were extracted from the time series of non-promoted sales using **MSTL (Multiple Seasonal-Trend decomposition using Loess)**. These components were used as features in the regression models.

---


## Baseline

The initial model used was a **Linear Regression** approach. This model incorporated key features, including seasonal decomposition, to serve as a strong baseline.

| Element | Description |
| :--- | :--- |
| **Model** | Linear Regression (`statsmodels.formula.api.ols`). |
| **Features** | Sales prediction based on: `decomposition2:C(product_id)`, `promotion:C(product_id):C(month)`, and `C(product_id)`. |
| **Pre-processing** | MSTL decomposition was applied to non-promoted sales to isolate **seasonal patterns**. |
| **Validation Strategy** | Time-based cross-validation was used, splitting the data into **16-year training windows** and **5-year test windows**. |
| **Metrics Obtained** | Average **$R^2$ (test)**: **0.9168** ($\text{std} = 0.0004$). |
| | Average **MSE (test)**: **167.7957** ($\text{std} = 0.9479$). |

---


## First Iteration

The first iteration aimed to improve performance by switching to a tree-based model to capture more **complex non-linear interactions** than the Linear Regression model could.

| Element | Description |
| :--- | :--- |
| **Change** | Switched the model to a **Decision Tree Regressor**. |
| **Why** | To capture **non-linear relationships**. |
| **Features** | `product_id_int`, `year`, `month`, `day`, `dayofweek`, and `promotion`. *Note: Seasonal decomposition features were removed for this initial iteration of the tree-based model.* |
| **Pre-processing** | `product_id` was transformed to an **integer-encoded categorical variable** (`product_id_int`). |
| **Model Hyperparameters** | `max_depth=20`, `random_state=42`. |
| **Impact on Metrics** | The model showed a **significant performance increase** over the Linear Regression baseline: |
| | Average **$R^2$ (test)**: **0.9591** (vs. 0.9168 baseline). |
| | Average **MSE (test)**: **82.3647** (vs. 167.7957 baseline). |
