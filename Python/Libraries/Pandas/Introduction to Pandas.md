![Pandas](https://pandas.pydata.org/static/img/pandas_mark.svg)

# <u> Pandas in Python</u>

**For depth understanding**
[Pandas for better uderstanding](https://www.datacamp.com/tutorial/pandas) 👈click here

## What is pandas?
![Pandas](https://s3.eu-west-1.amazonaws.com/redsys-prod/articles/be762273ee94f184046111c3/images/20_22_Infografik_pandas.png)
- pandas is a data manipulation package in Python for tabular data. 
![Dataframe](https://storage.googleapis.com/lds-media/images/series-and-dataframe.width-1200.png)

*That is, data in the form of **rows** and **columns***.
also known as ***DataFrames.** 

- Intuitively(Assume), you can think of a DataFrame as an Excel sheet. 

## What is pandas used for?
pandas is used throughout the data analysis workflow. With pandas, you can:

- Import datasets from databases, spreadsheets, comma-separated values (CSV) files, and more.
- Clean datasets, for example, by dealing with missing values.
- Tidy datasets by reshaping their structure into a suitable format for analysis.
- Aggregate data by calculating summary statistics such as the mean of columns, correlation between them, and more.
- Visualize datasets and uncover insights

pandas also contains functionality for time series analysis and analyzing text data.

![Pandas](https://files.realpython.com/media/Pandas-Project-Make-a-Gradebook-With-Pandas_Watermarked.6cf148621988.jpg)

## Key benefits of the pandas package
1. <u>**Intuitive view of data:**</u> pandas offers exceptionally intuitive data representation that facilitates easier data understanding and analysis.

2. <u>**Extensive feature set:**</u> It supports an extensive set of operations from exploratory data analysis, dealing with missing values, calculating statistics, visualizing univariate and bivariate data, and much more.

3. <u>**Works with large data:**</u>  pandas handles large data sets with ease. It offers speed and efficiency while working with datasets of the order of millions of records and hundreds of columns, depending on the machine

## How to install pandas?

### Install pandas
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`pip install pandas`

## 🔰 Introduction to Pandas – Real-Life Analogy

- **Scenario:** Think of a grocery store manager.

The manager tracks the sales of fruits and vegetables over time. They maintain a spreadsheet with:

- Rows as daily entries (one per day),

- Columns like Date, Item Name, Quantity Sold, Price, Store Location, etc.

Pandas works just like Excel, but it's much faster, and used in Python. It helps people handle large datasets easily, perform analysis, and gain insights.

## 🧠 Interview Questions (Real-Life Non-IT Examples)
1. **What is Pandas?**
*Pandas is a Python library used for data analysis and manipulation. It provides two main data structures: Series and DataFrame.*
---
2. **What is the difference between Series and DataFrame?**
- *`Series`: 1D labeled array*

- *`DataFrame`: 2D table with rows and columns*

***Example:-***
```
import pandas as pd

s = pd.Series([1, 2, 3])
df = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
```
---
3. **How to create a DataFrame from a dictionary?**
```
data = {'Name': ['Tom', 'Jerry'], 'Age': [25, 22]}
df = pd.DataFrame(data)
```
---
4. **How to read and write CSV files?**
```
df = pd.read_csv('file.csv')      # Read
df.to_csv('output.csv', index=False)  # Write
```
---**
5. **How to select rows and columns?**
```
df['Name']         # Select column
df.loc[0]          # Select row by label
df.iloc[0]         # Select row by index
```
---
**What is a DataFrame and Series in Pandas?**

Example: 
In a school, a DataFrame is like a full class report card with rows for students and columns for subjects. A Series is one column—say, just the Math scores.

