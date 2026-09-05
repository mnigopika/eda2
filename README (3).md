# Sales Data Analysis (Superstore EDA)

Exploratory Data Analysis (EDA) on the **Sample Superstore** dataset using Python, Pandas, Matplotlib, and Seaborn. The notebook (`sales.ipynb`) walks through data loading, cleaning, feature engineering, and visualizations to uncover trends in sales, profit, and discounting behavior.

## 📌 Overview

This notebook walks through a complete EDA workflow on retail sales data:

- Loading and inspecting the dataset
- Data cleaning (parsing `Order Date` and `Ship Date` as datetime)
- Feature engineering (`Delivery Days` — time between order and shipment)
- Checking for missing values and unique categories
- Aggregating total sales by product category
- Visualizing sales by category and overall sales distribution
- Analyzing profit by category (bar plots, box plots)
- Examining the impact of discounts on profit (scatter plot)
- Correlation analysis across numeric features (heatmap)

## 📂 Project Structure

```
├── sales.ipynb              # Main analysis notebook
├── samplesuperstore.csv     # Dataset (place in project root or update path)
└── README.md
```

## 🗂️ Dataset

The notebook expects a CSV file named `samplesuperstore.csv` (the "Sample Superstore" dataset, commonly used for retail analytics practice) at:

```
/content/samplesuperstore.csv
```

> **Note:** This path is set up for Google Colab. If running locally or elsewhere, update the path in the notebook, e.g.:
> ```python
> df = pd.read_csv("samplesuperstore.csv")
> ```

Typical columns include: `Order Date`, `Ship Date`, `Category`, `Sub-Category`, `Sales`, `Profit`, `Discount`, `Region`, etc.

## 🛠️ Tech Stack / Requirements

- Python 3.x
- pandas
- numpy
- matplotlib
- seaborn

Install dependencies:

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

## 🚀 Usage

1. Clone this repository:
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   cd <repo-name>
   ```
2. Place `samplesuperstore.csv` in the appropriate directory (update the path in the notebook if needed).
3. Launch Jupyter Notebook / Jupyter Lab:
   ```bash
   jupyter notebook sales.ipynb
   ```
4. Run all cells to reproduce the analysis.

## 📊 Key Steps in the Notebook

| Step | Description |
|------|-------------|
| Data Loading | Reads the CSV into a Pandas DataFrame |
| Data Inspection | `head()`, `info()`, `shape`, `describe()` |
| Date Parsing | Converts `Order Date` and `Ship Date` to datetime |
| Feature Engineering | Calculates `Delivery Days` |
| Data Quality Check | Checks unique categories and null values |
| Sales Aggregation | Total sales grouped by `Category` |
| Sales Visualization | Bar chart of sales by category; histogram of sales distribution |
| Profit Analysis | Bar plot and box plots of profit by category |
| Discount Impact | Scatter plot of discount vs. profit |
| Correlation Analysis | Correlation matrix and heatmap of numeric features |

## 📈 Example: Code & Output

Below is a sample run showing the notebook's logic in action (numbers below are from a small demo dataset used to illustrate the workflow — your real output will reflect your own `samplesuperstore.csv`).

### 1. Load and inspect the data

```python
import pandas as pd
df = pd.read_csv("/content/samplesuperstore.csv")
df.info()
```

```
<class 'pandas.DataFrame'>
RangeIndex: 200 entries, 0 to 199
Data columns (total 5 columns):
 #   Column      Non-Null Count  Dtype  
---  ------      --------------  -----  
 0   Order Date  200 non-null    object
 1   Ship Date   200 non-null    object
 2   Category    200 non-null    object
 3   Region      200 non-null    object
 4   Sales       200 non-null    float64
dtypes: float64(1), object(4)
memory usage: 7.9 KB
```

### 2. Parse dates and engineer `Delivery Days`

```python
df['Order Date'] = pd.to_datetime(df['Order Date'], format="mixed")
df['Ship Date'] = pd.to_datetime(df['Ship Date'], format="mixed")
df['Delivery Days'] = (df['Ship Date'] - df['Order Date']).dt.days
df.head()
```

```
  Order Date  Ship Date         Category Region   Sales  Delivery Days
0 2023-01-01 2023-01-05        Furniture   West  164.34              4
1 2023-01-04 2023-01-09  Office Supplies   West   68.55              5
2 2023-01-07 2023-01-10  Office Supplies   West   51.77              3
3 2023-01-10 2023-01-15        Furniture  South  265.61              5
4 2023-01-13 2023-01-18  Office Supplies   East  193.44              5
```

### 3. Aggregate sales by category

```python
category_sales = df.groupby('Category')['Sales'].sum()
category_sales
```

```
Category
Furniture          15419.67
Office Supplies    28919.40
Technology         18756.14
Name: Sales, dtype: float64
```

### 4. Visualize sales

```python
import matplotlib.pyplot as plt

category_sales.plot(kind='bar', figsize=(8,5))
plt.title("Sales by Category")
plt.ylabel("Total Sales")
plt.show()
```

**Output:**

<img width="721" height="560" alt="image" src="https://github.com/user-attachments/assets/1eed901d-9595-4674-8ca3-4c8fba26e4e2" />

```python
import seaborn as sns

plt.figure(figsize=(8,5))
sns.histplot(df['Sales'], bins=30)
plt.title("Sales Distribution")
plt.show()
```

This produces a histogram showing how individual sale amounts are distributed (right-skewed, with most sales clustered at lower values and a long tail of higher-value orders).

### 5. Profit by category

```python
sns.barplot(data=df, x="Category", y="Profit")
plt.title("Profit by Category")
plt.show()

sns.boxplot(data=df, x="Category", y="Profit")
plt.title("Profit Variation Across Categories")
plt.show()
```

These plots reveal which categories are most/least profitable and how spread out profit values are within each category (including outliers).

### 6. Discount vs. Profit

```python
sns.scatterplot(data=df, x="Discount", y="Profit")
plt.title("Impact of Discount on Profit")
plt.show()
```

Helps visualize whether higher discounts correlate with reduced (or negative) profit.

### 7. Correlation heatmap

```python
numeric_df = df.select_dtypes(include="number")
corr = numeric_df.corr()

sns.heatmap(corr, annot=True)
plt.title("Correlation Heatmap")
plt.show()
```

Shows how numeric features (Sales, Profit, Discount, Quantity, etc.) relate to one another.

## 📈 Key Insights

- Identifies which product categories drive the most sales and profit.
- Highlights how discounts can erode — or even reverse — profit margins.
- Surfaces correlations between sales, profit, discount, and quantity.
- Flags categories with high profit variability/outliers via box plots.

## 🤝 Contributing

Feel free to fork this repo and submit a pull request with improvements or additional analysis (e.g., regional breakdowns, time-series trends, customer segmentation).

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
