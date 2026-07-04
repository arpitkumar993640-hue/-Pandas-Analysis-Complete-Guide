# -Pandas-Analysis-Complete-Guide
A beginner-friendly Pandas project containing notes, examples, and practice code for learning Data Analysis using Python.  ---  ## 📖 About  This repository covers the most commonly used Pandas functions with practical examples. It is designed for students and beginners who want to learn Data Analysis.  ---  
##### What is Pandas?
Python library used for working with structured data sets (tables). It has functions for analyzing, cleaning, exploring, and manipulating data. Similar to data tables in SQL and Excel.

#### 0 install pandas
"""

!pip install pandas

# import pandas
import pandas as pd

pd.__version__ # check installed pandas version

"""##### What is Dataframe?
It's a structured data constructed with rows and columns, similar to a SQL tables or Excel tables

#### 1 Create Dataframe
"""

# using a List
df = pd.DataFrame([11,22,33], columns=['Col_Name'])
print(df)

print(type(df)) # check data type

# using Dictionary of Lists
data = {
    'Name': ['Madhav', 'Vishakha', 'Lalita', 'Rishabh'],
    'Age': [16,17,18,19],
    'Salary': [90000, 70000, 50000, 30000]
}

df = pd.DataFrame(data)
print(df)

print(type(df))

"""#### 2 Basic Dataframe Understanding"""

df.head(2) # top rows

df.tail(2) # last rows

df.shape # returns a tuple containing the shape of the DataFrame - rows & columns

df.columns # list of column names in a dataframe

df.rename(columns={'Salary': 'Monthly_Salary'}, inplace=True) # rename column name/s

df

df.info()  # info method prints information about the DataFrame

df.describe() # describe method generates descriptive statistics of DataFrame, only for numerical-value columns

"""#### 3 Save and Load data from csv"""

df.to_csv('Test_data.csv', index=False) # save file - export data frame

load_df= pd.read_csv('Test_data.csv') # load file - import dataframe
load_df

"""#### 4 Rows & Columns - Selection"""

# select single column
df[['Name']]

# select multiple columns
df[['Name', 'Monthly_Salary']]

# select single row || loc - label based index
df.loc[df.Name=='Madhav']

# select multiple rows || loc - label based index
df.loc[(df.Name=='Madhav') & (df.Monthly_Salary>=50000)]

df.loc[0:2]

# Select Rows || iloc - index-value based
# df.iloc[0]
df.iloc[0:2] # [start:stop:step]

"""#### 5 Filter Dataframe - Filtering by column values"""

df

df_age_filter = df[df['Age'] >= 18] # filter and store dataframe in a new variable

df_age_filter

df[(df['Age'] >= 18) & (df['Monthly_Salary'] >= 50000)] # multiple filter conditions

df.where(df['Age'] >= 18) # where function replace values in a DataFrame based on a condition

df.where(df['Age'] >= 18, other = 'Not Eligible')

"""#### 6 Rows and Columns - Operation (Add, update, delete)"""

df

# Create a new column
df['Team'] = ['CEO', 'HR', 'CTO', 'DA']
df

# Add new columns using existing column/s
df['Bonus'] = df['Monthly_Salary'] * 0.20
df

# Add new row - at the end of dataframe
df.loc[len(df)] = ['ABC', 21, 21000, 'IT', 2000]
df

# update value in dataframe using index-name
df.loc[0, 'Monthly_Salary'] = 95000
df

# update value in dataframe using column-value
df.loc[df.Name=='Madhav','Monthly_Salary'] = 90000
df

# delete value - rows and columns

# delete row using column-value filter
df = df.drop(df[df.Name == 'ABC'].index) # delete row, axis= 0

# delete row using index-name filter
df.drop(1, axis=0)

df.drop('Bonus', axis=1, inplace=True) # delete one column
# df.drop(['Bonus', 'Team'], axis=1, inplace=True) # delete multiple columns

df

df.rename(columns={'Monthly_Salary': 'Salary'}, inplace=True) # rename column name

# sort values - order values in dataframe by asc or desc order
df.sort_values('Salary') #ascending order, by default

# sort values in descending order
df.sort_values('Salary', ascending=False)

"""#### 7 working with date value"""

# create a date column - date of joining
df['DOJ'] = ['2024-01-01', '2024-01-15', '2024-03-28', '2024-03-03']
df

df['DOJ'].dtype # object-type value

df['DOJ'] = pd.to_datetime(df['DOJ']) # change to date-time type

df['DOJ'].dtype # date-time type

df1 = df

# creating a new column with incorrect date format
df1['DOJ2'] = ['01-01-2025', '15-01-2025', '28-03-2025', '03-03-2025']
df1

df1['DOJ2'].dtype  # object-type value

df1['DOJ2'] = pd.to_datetime(df1['DOJ2'], format = '%d-%m-%Y') # change to date-time type using date-format

df1['DOJ2'].dtype

df = df.drop('DOJ2', axis=1)
df

# extract year, month, week, day
df['DOJ'].dt.year
df['DOJ'].dt.month
df['DOJ'].dt.day
df['DOJ'].dt.day_name()

# create new column using month extract function from DOJ column
df['Month'] = df['DOJ'].dt.month

df

df['DOJ'] + pd.Timedelta(days=90) # add 90 days to DOJ column

"""#### 8 Handling Missing Values"""

df

df.isnull() #  Detect missing values

import numpy as np  # to create null values below

df.loc[df.Name=='Rishabh', 'Salary'] = np.nan # adding a null value

df

df.isnull()

df.isnull().sum() # count of null values by columns

df.fillna(0) # fill null values with 0

df.loc[df.Name=='Rishabh', 'Salary'] = 30000

df

"""#### 9 Aggregation and Group By"""

df['Month'].value_counts() # frequency of values in month column

df[df['Month']==1].value_counts() # frequency of values in month column where month=1

# aggregation based on group by
df.groupby('Month')['Salary'].sum() # sum of salary by month

# different aggregation on different columns
df.groupby('Month').agg({'Salary': 'mean', 'Name': 'count'})

"""#### 10 Concatenate and Merge Dataframe (JOINS)"""

df1 = pd.DataFrame({'ID':[1,2,3],'Name':['A','B','C']})
df1

df2 = pd.DataFrame({'ID':[1,2,2,4],'Score':[88,96,77,79]})
df2

"""##### What is Concatenate:
Combine objects along a particular axis.
"""

# Concatenate: vertical / row level / top on top
pd.concat([df1, df2], axis=0)

# Concatenate: horizontal / column level / side on side
pd.concat([df1, df2], axis=1)

"""##### Merge in Pandas
Performs join operations similar to relational databases like SQL JOINS
"""

# merge = join
pd.merge(df1, df2, how='inner', on='ID')

pd.merge(df1, df2, how='inner', left_on='ID', right_on='ID')

"""#### 11 AI and ChatGPT"""

df

# Prompt to filter salary > 70000 and January employees

# Method-1: basic filter
df[(df['Month'] == 1) & (df['Salary'] >= 70000)]

# Method-2: query method
df.query("Month == 1 and Salary >= 70000")
