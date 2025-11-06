# Lecture 08 - Data Manipulation (`Pandas`)


## Overview 

**What is `pandas`?**

- `Pandas` is a powerful Python library used for **data manipulation** and **analysis**.
- It provides two main data structures: `Series` (1D) and `DataFrame` (2D), which are ideal for working with **structured data**, similar to Excel spreadsheets or SQL tables.

**Why `pandas` for Finance?**

- **Exploratory Data Analysis** (EDA): large and structured financial datasets that require cleaning, manipulation, and analysis.
- **Efficiency**: `Pandas` can handle millions of rows of financial data efficiently.
- **Integration with Data Science**: `Pandas` works seamlessly with other libraries like `NumPy` and `matplotlib` for numerical operations and visualization, essential for financial modeling.

This notebook covers:
- `DataFrame` and `Series` classes
- Basic operations with `Pandas`
- `GroupBy`, complex selection and data combinations


```python
import pandas as pd
import numpy as np 
```

## 1. The `DataFrame` Class

At the core of `Pandas` is the `DataFrame`, a class designed to **efficiently handle data in tabular form** —i.e., data characterized by a columnar organization. 

- A `DataFrame` in `pandas` is a **two-dimensional**, **labeled data structure** that organizes and manipulates structured data
- `DataFrames` consist of **rows** and **columns**, where each column can contain different types of data (`integers`, `floats`, `strings`, etc.).
    - Like a table in a relational database or an Excel sheet. 


### 1.1 Creating a `DataFrame`

The `pd.DataFrame()` function in `pandas` creates `DataFrames`.

```
pd.DataFrame(data=None, index=None, columns=None, dtype=None, copy=False)
```

- `data`: Data to populate the `DataFrame`
- `index`: Index labels for the rows (optional).
- `columns`: Column labels (optional).
- `dtype`: Data type for the elements (optional).
- `copy`: Whether to copy the input data (optional).

There are several ways to create a `DataFrame`. 
- from **scratc**h using `lists` or `dictionaries`
- from **reading** external files (CSV, Excel, SQL databases). 

#### From a `list`


```python
# Creating a DataFrame from a list
data = [10, 20, 30, 40]
df = pd.DataFrame(data, columns=['numbers'], index=['a', 'b', 'c', 'd'])
```


```python
# Displaying the DataFrame
print(df)
```

In this example:

- The `data` list contains values for a single column.
- The `columns` parameter labels the column, and the `index` parameter sets row labels.

Once a `DataFrame` is instantiated, one can observe its meta structure.


```python
df.index
```


```python
df.columns
```

#### From a `dictionary`

A `DataFrame` can be sourced from a `dictionary`, where `keys` are column `names` and values are lists of column data.


```python
data = {
    'Product': ['A', 'B', 'C', 'D'],
    'Price': [100, 150, 200, 250],
    'Stock': [50, 60, 70, 80]
}

df = pd.DataFrame(data)

# Displaying the DataFrame
print(df)
```

#### From external sources

`DataFrames` can also be created by reading data from **external files** such as CSVs, Excel files, or databases.

```python
# Reading from a CSV file
df = pd.read_csv('file.csv')

# Reading from an Excel file
df = pd.read_excel('file.xlsx')
```

*More on this in next lecture...*

### 1.2 Accessing a `DataFrame`

Once a `DataFrame` is created, one can access  
- columns
- rows 
- specific subsets of data

#### Accessing columns

Access to columns can be done **directly** by referring the column like in a `dictionary`.


```python
# Accessing a single column
df['Product']
```


```python
# Accessing multiple columns
df[['Product', 'Price']]
```

#### Accessing rows

Access to rows can be done by index (`.loc[]`) or position (`.iloc[]`)


```python
df
```


```python
# Accessing by label
df.loc[1]  # Retrieves the row with index 1
```


```python
# Accessing by position
df.iloc[2]  # Retrieves the third row (index 2)
```

#### Selecting multiple rows

A range of rows or a specific set of rows can be selected by combining `.loc[]` or `.iloc[]` with slicing or `lists`.


```python
# Accessing multiple rows by label
df.loc[1:3]
```


```python
# Accessing multiple rows by position
df.iloc[0:2]  
```


```python
# Accessing multiple rows by list
indeces = [1,3]
df.loc[indeces]
```

### 1.3 Editing a `DataFrame`

`DataFrames` are live objects which allow for adding, deleting or modifying data (columns or rows) on the fly.

###### Initialization


```python
data = {
    'Product': ['A', 'B', 'C', 'D'],
    'Price': [110, 160, 210, 260],
    'Stock': [50, 60, 70, 80],
}
df = pd.DataFrame(data)
```

#### Editing columns

- Adding
- Modifying
- Deleting

##### Adding columns

- **Internal product**


```python
# Adding a new column
df['Discounted_Price'] = df['Price'] * 0.9
print(df)
```

- **from `list`**
    - The length of the list should match the number of rows in the `DataFrame`.


```python
# Adding a new column 'Tax' using a list
df['Tax'] = [10.5, 15.0, 19.5, 24.0]
print(df)
```


```python
# Displaying the 'Tax' column
print(df['Tax'])
```

- **from `DataFrame`**
    - Indices must match between the original `DataFrame` and the new column.


```python
# Adding a new column 'Supplier' based on another DataFrame
df['Supplier'] = pd.DataFrame(['Supplier1', 'Supplier2', 'Supplier3', 'Supplier4'], 
                              index=[0, 1, 2, 3])
print(df)
```

After enlarging a `DataFrame`, it’s also essential to check the data types of each column to ensure everything is in order.


```python
# Checking the data types of the DataFrame columns
print(df.dtypes)
```

##### Modifying columns


```python
# Modifying an existing column
df['Price'] = df['Price'] + 10
print(df)
```

##### Deleting columns

Removing columns can be done using the `.drop()` method.


```python
# Dropping the 'Discounted_Price' column
df_dropped = df.drop(columns=['Discounted_Price'])
print(df_dropped)
```

#### Editing rows

- Adding
- Modifying
- Deleting

##### Adding rows

- `append()`
    - Single row
    - However, note that `append()` is deprecated, and should be replaced using `pd.concat()` instead in future `pandas` versions.


```python
# Appending a new row to the DataFrame
df_appended = df.append({'Product': 'E', 'Price': 300, 'Stock': 90, 
                'Discounted_Price': 270.0, 'Tax': 30.0, 'Supplier': 'Supplier5'}, 
               ignore_index=True)
print(df_appended)
```

- `concat()`


```python
# New row as a DataFrame
new_row = pd.DataFrame({
    'Product': ['E'],
    'Price': [300],
    'Stock': [90],
    'Discounted_Price': [270.0],
    'Tax': [30.0],
    'Supplier': ['Supplier5']
})

# Using pd.concat() to append the new row
df_concated = pd.concat([df, new_row], ignore_index=True)

# Display the updated DataFrame
print(df_concated)
```

More on `concat()` in section [6. Concatenation, Join, Merge](#6.-Concatenation,-Join,-Merge) and below on [indexing](#Indexing)

##### Modifying rows


```python
# Modifying an existing row
df.loc[1, 'Price'] = 180
df.loc[1, 'Stock'] = 200
```


```python
print(df)
```

##### Deleting rows

- Rows can be removed from a DataFrame using the `.drop()` method either by index or by condition.


```python
# Dropping a row by index
df_dropped = df.drop(index=2)  # Dropping the row with index 2 (Product C)
print(df_dropped)
```


```python
# Dropping rows where Stock is greater than 80
df_dropped = df[df['Stock'] <= 80]
print(df_dropped)
```

#### Indexing 

When appending rows to a `DataFrame`, it’s important to pay attention to the treatment of **indices**. 

- **Indexing with `concat()`**
    - When concatenating two or more `DataFrames`, the default behavior is to **retain the original index** of the `DataFrames`, which can lead to duplicate indices. 
    - The `ignore_index=True` parameter resets the index for the concatenated `DataFrame` to ensure a continuous sequence.


```python
# New DataFrame to concatenate
df_new = pd.DataFrame({
    'Product': ['F', 'G'],
    'Price': [350, 400],
    'Stock': [95, 100],
    'Discounted_Price': [315.0, 360.0],
    'Tax': [35.0, 40.0],
    'Supplier': ['Supplier6', 'Supplier7']
})
```


```python
# Concatenating the two DataFrames without resetting the index
df_concat = pd.concat([df, df_new])
print(df_concat)
```


```python
# Concatenating with index reset
df_concat_reset = pd.concat([df, df_new], ignore_index=True)
print(df_concat_reset)
```

- **Indexing with `append()`**
    - When appending, the new row needs to adopt the next available index by default (`ignore_index = True`), else does not proceed.


```python
# Appending a single row with a new product using append()
new_row = {
    'Product': 'H', 
    'Price': 450, 
    'Stock': 105, 
    'Price_Squared': 202500, 
    'Discounted_Price': 405.0, 
    'Tax': 45.0, 
    'Supplier': 'Supplier8'
}

df_appended = df.append(new_row, ignore_index = True)

print(df_appended)
```

### 1.4 Handling missing data

Missing data is common in real-world datasets, and `pandas` provides tools to handle them.


```python
# Adding missing values (NaN) to some cells
df.loc[1, 'Price'] = np.nan  # Missing price for product B
df.loc[2, 'Discounted_Price'] = np.nan  # Missing discounted price for product C
df.loc[3, 'Stock'] = np.nan  # Missing stock for product D
```


```python
print (df)
```

- **`df.isna()`:** Detects missing values


```python
df.isna()
```

- **`df.fillna(a)`:** Fils missing values with a default


```python
df.fillna(0)
```

- **`df.dropna()`**: Drops rows with missing values


```python
df.dropna()
```

#### Operations with missing data

`Pandas` method calls handle missing data


```python
df
```


```python
df[['Price', 'Stock']].mean()
```


```python
df[['Price', 'Stock']].std()
```

### 1.5 Sorting Data

`DataFrame` can be sorted based on any column using the `.sort_values()` method.


```python
# Sorting by 'Price' in ascending order
df_sorted = df.sort_values(by='Price')
```


```python
print (df_sorted)
```


```python
# Sorting by sequence of multiple columns: first stock, than price
df.loc[3,'Stock'] = df.loc[2,'Stock']
df_sorted = df.sort_values(by=['Stock', 'Price'], ascending=[False, True])
```


```python
print (df_sorted)
```

### 1.6 Libraries integration

`Pandas` integrates the manipulation of objects from other classes such as `numpy` and `datetime` objects.

- **`NumPy`**


```python
import numpy as np
np.random.seed(100)
a = np.random.standard_normal((9,4))
a
```


```python
df_n = pd.DataFrame(a)
df_n.columns = ['No1', 'No2', 'No3', 'No4']
df_n
```


```python
df_n['No2'].mean()
```

- **`DateTime`**


```python
dates = pd.date_range('2019-1-1', periods = 9, freq = 'M')
dates
```


```python
df_n.index = dates
df_n
```


```python
df_n.values
```

## 🚦Checkpoint 

**Task:**
1. Create a `DataFrame` with the following columns: `Item`, `Price`, `Quantity` using the following dictionary
```python
    data = {
    'Item': ['Apple', 'Banana', 'Orange'],
    'Price': [1.0, 0.5, 0.75],
    'Quantity': [10, 5, 8]
        }
```
2. Add a new column `Total` that calculates the total price by multiplying `Price` and `Quantity`.
3. Add a column called `Discount` with values `[0.1, 0.05, 0.2]` to the existing DataFrame.
4. Update the `Total` column to apply the discount to the total price.

## 2. Basic Analytics

When working with data in `pandas`, the library provides several **built-in methods** that make data exploration and analysis efficient. 

These methods help with
- Inspection
- Summaries
- Statistics
- Data operations
- Visualization 

### 2.1 Inspection and summary

- **`df.info()`**: Provides a concise summary of the `DataFrame`, including the number of entries, column names, data types, and memory usage. 


```python
df.info()
```

- **`df.head()`:** Returns the first few rows of the `DataFrame` (by default, the first 5 rows).


```python
df.head()  # By default, displays the first 5 rows
```


```python
df.head(10)  # Displays the first 10 rows
```

- **`df.tail()`:** Similar to `df.head()`, but returns the last few rows of the DataFrame.


```python
df.tail()  # By default, displays the last 5 rows
```

- **`df.describe()`:** Generates descriptive statistics for numerical columns, including count, mean, standard deviation, minimum, and maximum values, as well as the 25th, 50th, and 75th percentiles.


```python
df
```


```python
df.describe()
```

### 2.2 Statistics and operations

- **`df.sum()`**

- **`df.mean()`**


```python
df[['Price','Stock']].mean()  # By default, computes the mean of each column
```


```python
df[['Price','Stock']].mean(axis=0)  # Mean across columns (default behavior)
```


```python
df[['Price','Stock']].mean(axis=1)  # Mean across rows
```

- **`df.cumsum()`**


```python
df.cumsum()
```

- **`df.apply()`:** Custom functions can be applied to columns or rows using the `.apply()` method.


```python
# Applying a lambda function to square the values in 'Price' column
df['Price'].apply(lambda x: x ** 2)
```

### 2.3 Integrated `NumPy` Functions

`Pandas` integrates with `NumPy`, allowing the use of `NumPy` functions directly on `DataFrames`.

- **`np.mean(df)`**


```python
np.mean(df[['Price','Stock']])
```

- **`np.log(df)`**


```python
np.log(df)
```

- **`np.sqrt(abs(df))`**


```python
np.sqrt(abs(df))
```


```python
np.sqrt(abs(df)).sum()
```

### 2.4 Basic Visualization

`Pandas` integrates with visualization libraries like `matplotlib` to enable quick visualizations of data. 

#### Setting up `Matplotlib` for Visualization


```python
from pylab import plt, mpl

# Setting the style to 'seaborn' for better aesthetics
plt.style.use('seaborn')

# Setting the default font to 'serif' for a clean look
mpl.rcParams['font.family'] = 'serif'

# Ensure plots are displayed inline in Jupyter notebooks
%matplotlib inline
```

#### Examples


```python
df.cumsum().plot(lw=2.0, figsize=(10, 6))  # Line width set to 2.0 and figure size to 10x6
```


```python
df.plot(kind='bar', figsize=(10, 6))  # Generates a bar plot with a figure size of 10x6
```

## 3. The `Series` Class

The `Series` class in `pandas` is a **one-dimensional labeled array** capable of holding any data type. 

A `Series` is essentially a single column of data, making it a simpler, more specialized version of the `DataFrame` class. It shares many characteristics and methods with DataFrame and adds more specifics techniques.

### 3.1 Creating a `Series`

A `Series` object can be created directly or obtained by selecting a single column from a `DataFrame`.

- **From `list`**


```python
# Creating a Series with evenly spaced numbers between 0 and 15
s = pd.Series(np.linspace(0, 15, 7), name='series')
print(s)
```


```python
# Checking the type of the Series
type(s)
```

- **From `DataFrame`**

Selecting a single column from a `DataFrame` result in a `Series`.


```python
# Selecting a column from the DataFrame as a Series
s = df_n['No1']
print(s)
```


```python
# Checking the type
type(s)
```

### 3.2 Methods for `Series`

#### Inherited methods

Most of the methods available for `DataFrame` are also available for `Series`, such as `mean()`, or `plot()`.


```python
# Calculating the mean of the Series
s.mean()
```


```python
# Plotting the Series
s.plot(lw=2.0, figsize=(10, 6))  # Line width set to 2.0 and figure size to 10x6
```

#### `Series` specific methods

There are several methods that are specific to `Series` objects. 

These methods are designed to leverage the **one-dimensional** nature of a `Series` and simplify certain operations that are less intuitive or applicable in a two-dimensional `DataFrame`.

- **`Series.value_counts()`:** Returns the count of unique values in a `Series`. 
    - Useful when working with categorical data or needing to understand the frequency of certain values.


```python
# Example of value_counts
s = pd.Series(['A', 'B', 'A', 'C', 'B', 'A'])
s.value_counts()
```

- **`Series.unique()`**: Returns an array of the unique values in a `Series`. 
    - It helps identify distinct values in a column of data.


```python
# Example of unique
s = pd.Series([1, 2, 2, 3, 4, 4, 4])
s.unique()
```

- **`Series.nunique()`:** Returns the number of unique values in a `Series`. 
    - Similar to `value_counts()` but only provides the total number of unique elements, not their frequency.


```python
# Example of nunique
s = pd.Series([1, 2, 2, 3, 4, 4, 4])
s.nunique()
```

- **`Series.str` accessor:** performs string operations. 
    - This feature is unique to Series and makes it easy to manipulate text data in bulk.
    - These include `.str.upper()`, `.str.contains()`, `.str.replace()`, and `.str.len()`


```python
# Sample Series of strings
s = pd.Series(['apple', 'banana', 'pear'])
```


```python
# Convert all strings to uppercase
s_upper = s.str.upper()
print(s_upper)
```


```python
# Check if each string contains the letter 'a'
s_contains = s.str.contains('a')
print(s_contains)
```


```python
# Replace 'a' with 'o' in each string
s_replace = s.str.replace('a', 'o')
print(s_replace)
```


```python
# Get the length of each string
s_len = s.str.len()
print(s_len)
```

- **`Series.dt` accessor:** performs datetime-specific operations. 
    - This makes it easy to extract year, month, day, or other components from a datetime Series.
    - These include `dt.day`, `.dt.year`, `.dt.month`, and `.dt.weekday`.


```python
# Example of dt accessor
s = pd.Series(pd.date_range('2023-01-01', periods=3, freq='D'))
s.dt.day  # Extract the day from the datetime
```

- **`Series.isin()`:** Checks whether each element of the `Series` is in a given list of values and returns a boolean `Series`.


```python
# Example of isin
s = pd.Series(['A', 'B', 'C', 'D'])
s.isin(['B', 'C', 'E'])
```

- **`Series.idxmax()`** and **`Series.idxmin()`:** Return the index of the maximum or minimum value in the `Series`, respectively. 


```python
# Example of idxmax and idxmin
s = pd.Series([1, 5, 3, 9, 2])
print(s.idxmax())  # Index of the maximum value
print(s.idxmin())  # Index of the minimum value
```

## 🚦Checkpoint 

**Task:**
1. Create a `pandas` Series from a list of stock prices: `[150.25, 153.50, 2800.50, 2830.75, 3400.00, 3450.50]` and name the Series `Stock_Prices`.

2. Using the `Stock_Prices` series:
   - Find the maximum price in the series.
   - Find the minimum price in the series.
   - Calculate the mean (average) price.
   - Find how many stock prices are above the mean.
   - Create a new Series where each stock price is increased by 5%.
   - Normalize the Series so that all values are between 0 and 1 (i.e., subtract the minimum and divide by the range).

3. Create another `pandas` Series from the list `[0.05, 0.03, 0.02, 0.04, 0.01, 0.06]` and name it `Stock_Changes`.  
   This series represents the daily percentage changes for each stock (assume all changes are positive and range between 0.01 to 0.10). 
   - Multiply the `Stock_Prices` by the `Stock_Changes` Series.
   - Create a new Series called `Updated_Prices` which contains the new stock prices after applying the percentage changes.

4. Create a new `Series` that categorizes stock prices in `Updated_Prices` as either "High" if the price is above 3000, or "Low" if the price is below 3000.

5. Check if any of the original `Stock_Prices` are in this list: `[153.50, 2830.75, 5000.00]` and return a boolean Series indicating whether each price is present in this list.

6. Count how many times each category ("High" or "Low") appears in the `price_categories` Series from step 4.


## 4. `GroupBy` Operations

`Pandas` provides powerful and flexible **grouping** capabilities that function similarly to SQL groupings and pivot tables in Excel. 

Grouping is often used when performing **aggregations** or **applying specific operations** to subsets of data.

###### Initialization


```python
# Creating a sample financial DataFrame
data = {
    'Stock': ['AAPL', 'AAPL', 'GOOGL', 'GOOGL', 'AMZN', 'AMZN', 'AAPL', 'GOOGL', 'AMZN'],
    'Quarter': ['Q1', 'Q1', 'Q1', 'Q2', 'Q2', 'Q2', 'Q3', 'Q3', 'Q3'],
    'Price': [150.25, 153.50, 2800.50, 2830.75, 3400.00, 3450.50, 155.30, 2900.75, 3500.75],
    'Volume': [1000, 1100, 1500, 1600, 1700, 1800, 1200, 1700, 2000],
    'Market_Cap': [2.41e12, 2.45e12, 1.78e12, 1.82e12, 1.71e12, 1.75e12, 2.50e12, 1.85e12, 1.80e12]
}

df = pd.DataFrame(data)
df 
```

### 4.1 Simple grouping

Simple grouping of a DataFrame using `.groupby()` is done on a specific column which then allows to perform basic operations on each group. 


```python
groups = df.groupby('Quarter')
```

#### Inspection

- `.size()`: checks how many records belong to each group


```python
groups.size()
```

#### Basic Operations on Groups

Groups can perform simple aggregate functions
- `.mean()`: compute the average value of given columns for each group.


```python
groups[['Price', 'Volume']].mean()
```

- `.max()`: shows the maximum for given columns for each group.


```python
groups[['Price', 'Volume']].max()
```

#### Aggregating Multiple Functions at Once

The `.aggregate()` methods obtains multiple aggregate functions to summarize data. 


```python
groups[['Price', 'Volume']].aggregate(['min', 'max']).round(2)
```

### 4.2 Grouping by multiple columns


```python
groups = df.groupby(['Quarter', 'Stock'])
```


```python
groups.size()
```

### 4.3 Advanced grouping example

#### Aggregations on Multiple Groups

After grouping by multiple column, one can perform multiple operations, such as summing and calculating the mean for specific columns


```python
groups[['Price', 'Volume']].aggregate(['sum', 'mean'])
```

#### Calculating market share

**Goal:** calculate each stock’s market share in a specific quarter. 
- Compute the market share of each stock by dividing its market cap by the total market cap of all stocks in that quarter.


```python
df['Market_Share'] = df.groupby('Quarter')['Market_Cap'].transform(lambda x: x / x.sum())
df[['Stock', 'Quarter', 'Market_Share']]
```

##### Step by step

- Step 1: Grouping by `Quarter`

We are using the `groupby()` function to group the data by the `Quarter` column:

```python
df.groupby('Quarter')
```
This groups the rows in the `DataFrame` based on the values in the `Quarter` column. This means that all stocks from the same quarter will be grouped together.

- Step 2: Applying the Aggregation

Next, we focus on the `Market_Cap` column within each quarter. This is done using:
```python
df.groupby('Quarter')['Market_Cap']
```
Here, we are selecting the `Market_Cap` values for each group (each quarter). We now want to calculate the market share for each stock within its respective quarter.

- Step 3: Calculating Market Share with `transform()`

The custom operation is a `lambda` function that computes the market share:
```python
lambda x: x / x.sum()
```
This `lambda` function divides each stock’s market capitalization (`x`) by the total market capitalization of all stocks in the same quarter (`x.sum()`).

- `x` represents the `Market_Cap` values for the group (all stocks in a given quarter).
- `x.sum()` calculates the total market capitalization for that quarter.
- The `lambda` function then divides each stock’s market capitalization by the total, which gives the proportion (or market share) of that stock relative to the total market capitalization in the quarter.

- Step 4: Assigning the Market Share to a New Column

The result of this operation is assigned to a new column in the DataFrame called Market_Share:

```python
df['Market_Share'] = ...
```
Now, each row in the DataFrame will have a Market_Share value representing the percentage of the total market capitalization that each stock holds within its respective quarter.

## 🚦Checkpoint

**Task:**
1. Create a `DataFrame` with columns: `Employee`, `Department`, `Salary` from
```python
    data = {
    'Employee': ['John', 'Jane', 'Peter', 'Lucy'],
    'Department': ['HR', 'HR', 'IT', 'IT'],
    'Salary': [50000, 60000, 70000, 80000]
    }
```
2. Calculate the average salary per department.

## 5. Complex Selection

In `pandas`, **data selection** involves formulating **conditions** based on column values and combining multiple conditions logically. 

##### Initialization


```python
import numpy as np

# Creating a DataFrame with random financial data
data = {
    'Transaction_Amount': np.random.uniform(-500, 1500, 10),  # Random transaction amounts between -500 and 1500
    'Interest_Rate': np.random.uniform(0.01, 0.15, 10),       # Random interest rates between 1% and 15%
    'Loan_Amount': np.random.uniform(1000, 20000, 10),         # Random loan amounts between 1000 and 20000
}

df = pd.DataFrame(data)
```


```python
df.info()  # Display information about the DataFrame
```


```python
df.head()  # Display the first few rows
```

### 5.1 Conditions

Conditions based on the values in the `DataFrame` columns:

1. **Single Condition**: Selecting transactions greater than a specified amount:


```python
condition1 = df['Transaction_Amount'] > 0
```

2. **Multiple Conditions**: Using logical operators to combine conditions:
- **AND** Condition: Selecting rows where the transaction amount is positive and the interest rate is less than 0.1:


```python
condition2 = (df['Transaction_Amount'] > 0) & (df['Interest_Rate'] < 0.1)
```

- **OR** Condition: Selecting rows where the transaction amount is positive or the interest rate is less than 0.05:


```python
condition3 = (df['Transaction_Amount'] > 0) | (df['Interest_Rate'] < 0.05)
```

3. **Condition on All Values**: Checking for all positive values in the DataFrame:


```python
condition4 = df > 0
```

### 5.2 Conditional Selection

Condition-based selection of data from the DataFrame.

- **Select rows where `Transaction_Amount` is greater than 0**:


```python
positive_transactions = df[df['Transaction_Amount'] > 0]
print (positive_transactions)
```


```python
condition1
```

- **Select rows where `Transaction_Amount` is positive and `Interest_Rate` is less than 0.1**:


```python
filtered_transactions = df[(df['Transaction_Amount'] > 0) & (df['Interest_Rate'] < 0.1)]
print (filtered_transactions)
```


```python
df[condition2]
```

- **Select rows where either `Transaction_Amount` is positive or `Interest_Rate` is less than 0.05**:


```python
selected_transactions = df[(df['Transaction_Amount'] > 0) | (df['Interest_Rate'] < 0.05)]
```

- **Select all positive values from the DataFrame**:


```python
df[df > 0]
```

## 🚦Checkpoint

**Task:**
1. Create a `DataFrame` with columns `Age`, `Income` from
```python 
    data = {
        'Age': [25, 35, 45, 50],
        'Income': [40000, 60000, 70000, 30000]
    }
```
2. Select rows where `Age > 30` and `Income > 50000`.

## 6. Concatenation, Join, Merge

Data manipulation often involves the need to **combine multiple datasets**. 

#### Initialization


```python
loan_data = pd.DataFrame({
    'Customer_ID': ['A001', 'A002', 'A003', 'A004'],
    'Loan_Amount': [10000, 20000, 15000, 25000]
})

credit_scores = pd.DataFrame({
    'Customer_ID': ['A002', 'A004', 'A005'],
    'Credit_Score': [720, 680, 710]
})
```


```python
loan_data
```


```python
credit_scores
```

### 6.1 Concatenation

**Concatenation** or appending means adding rows from one DataFrame to another. 


```python
# Concatenating the DataFrames
pd.concat([loan_data, credit_scores], sort=False)
```

- Remember that when concatenating, the index values are maintained unless specified otherwise. They can be reset `ignore_index=True` (more on this [here](#Indexing)).


```python
pd.concat([loan_data, credit_scores], ignore_index=True, sort=False)
```

### 6.2 Join

**Joining** allows to **combine two DataFrames** based on their **indices**. 
- It is similar to SQL joins 
- useful when combining datasets with matching shared keys.

Let’s join the `loan_data` and `credit_scores` using `Customer_ID`.


```python
# Setting the 'Customer_ID' column as index for join
loan_data.set_index('Customer_ID', inplace=True)
credit_scores.set_index('Customer_ID', inplace=True)
```


```python
# Performing a left join
loan_data.join(credit_scores, how='left')
```

**Join methods**

There are **4 different approaches** to join `DataFrames`.

Each approach leads to a different behavior with regard to how index values and the corresponding data rows are handled:

| Join Method | Description                                             |
|-------------|---------------------------------------------------------|
| `left`      | Preserves index values from the left DataFrame.         |
| `right`     | Preserves index values from the right DataFrame.        |
| `inner`     | Preserves index values found in both DataFrames.        |
| `outer`     | Preserves all index values from both DataFrames.        |


```python
# Different types of joins
loan_data.join(credit_scores, how='right')
```


```python
loan_data.join(credit_scores, how='inner')
```


```python
loan_data.join(credit_scores, how='outer')
```

### 6.3 Merge

**Merging** `DataFrames` is similar to joining except it can be achieved on specific columns instead of only indeces. 
- Useful when financial datasets have overlapping columns.


```python
# Resetting index for merging
loan_data.reset_index(inplace=True)
credit_scores.reset_index(inplace=True)
```


```python
# Merging based on the 'Customer_ID' column
pd.merge(loan_data, credit_scores, on='Customer_ID', how='inner')
```

As for **joins**, mergers can be done with the four different options:
- `left`
- `right`
- `inner`
- `outer`


```python
# Merge with outer join
pd.merge(loan_data, credit_scores, on='Customer_ID', how='outer')
```

**Merging** can also be done using different columns from each `DataFrame`
- `left_on`
- `right_on`


```python
# Sample DataFrame for loan data
loan_data = pd.DataFrame({
    'Loan_ID': ['L001', 'L002', 'L003', 'L004'],
    'Amount': [5000, 7000, 8000, 6000],
    'Branch_ID': ['B001', 'B002', 'B003', 'B004']
})

# Sample DataFrame for branch data
branch_data = pd.DataFrame({
    'Branch_Name': ['Main Branch', 'East Branch', 'West Branch', 'North Branch'],
    'Manager': ['John', 'Sally', 'Mike', 'Anna'],
    'ID': ['B001', 'B002', 'B003', 'B005']  # Different name for Branch_ID column
})

# Merging using different columns from each DataFrame
# Merging on loan_data['Branch_ID'] and branch_data['ID']
merged_df = pd.merge(loan_data, branch_data, left_on='Branch_ID', right_on='ID', how='outer')

print(merged_df)
```

## 🚦Checkpoint

**Task:**
1. Create two `DataFrames`, one with `Customer_ID` and `Loan_Amount` and another with `Customer_ID` and `Credit_Score` from 
```python 
    loan_data = pd.DataFrame({
    'Customer_ID': ['A001', 'A002', 'A003'],
    'Loan_Amount': [10000, 15000, 20000]
    })

    credit_data = pd.DataFrame({
        'Customer_ID': ['A001', 'A002', 'A004'],
        'Credit_Score': [720, 650, 700]
    })
```
2. Merge the two `DataFrames`.

---
