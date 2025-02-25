
# Using SQL with Pandas - Lab

## Introduction

In this lab, you will practice using SQL statements and the `.query()` method provided by pandas to manipulate datasets.

## Objectives

You will be able to:

* Query DataFrames with SQL using the `pandasql` library
* Query DataFrames by slicing with conditional logic
* Use the `.query()` method to access data

## The Dataset

In this lab, we will continue working with the _Titanic Survivors_ dataset.

Begin by importing `pandas` as `pd`, `numpy` as `np`, and `matplotlib.pyplot` as `plt`, and set the appropriate alias for each. Additionally, set `%matplotlib inline`.


```python
!pip install pandasql;
```

    Requirement already satisfied: pandasql in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (0.7.3)
    Requirement already satisfied: numpy in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from pandasql) (1.16.4)
    Requirement already satisfied: pandas in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from pandasql) (0.23.4)
    Requirement already satisfied: sqlalchemy in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from pandasql) (1.3.8)
    Requirement already satisfied: python-dateutil>=2.5.0 in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from pandas->pandasql) (2.8.0)
    Requirement already satisfied: pytz>=2011k in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from pandas->pandasql) (2018.5)
    Requirement already satisfied: six>=1.5 in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from python-dateutil>=2.5.0->pandas->pandasql) (1.11.0)
    

    pexpect 4.6.0 requires ptyprocess>=0.5, which is not installed.
    You are using pip version 10.0.1, however version 19.2.3 is available.
    You should consider upgrading via the 'python -m pip install --upgrade pip' command.
    


```python
import sqlite3
import pandas as pd
from pandasql import sqldf
import matplotlib.pyplot as plt

import numpy as np

```

#### Functions


```python
pysqldf = lambda q: sqldf(q, globals())
```

Next, read in the data from `titanic.csv` and store it as a DataFrame in `df`. Display the `.head()` to ensure that everything loaded correctly.


```python
df = pd.read_csv('titanic.csv', index_col=0)
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



## Slicing DataFrames Using Conditional Logic

One of the most common ways to query data with pandas is to simply slice the DataFrame so that the object returned contains only the data you're interested in.  

In the cell below, slice the DataFrame so that it only contains passengers with 2nd or 3rd class tickets (denoted by the `Pclass` column). 

Be sure to preview values first to ensure proper encoding when slicing

- **_Hint_**: Remember, your conditional logic must be passed into the slicing operator to return a slice of the DataFrame--otherwise, it will just return a table of boolean values based on the conditional statement!


```python
df.columns
#df.Pclass.unique()

```




    Index(['PassengerId', 'Survived', 'Pclass', 'Name', 'Sex', 'Age', 'SibSp',
           'Parch', 'Ticket', 'Fare', 'Cabin', 'Embarked'],
          dtype='object')




```python
no_first_class_df = df.query("Pclass == '3' | Pclass == '2'")
no_first_class_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>0</td>
      <td>3</td>
      <td>Moran, Mr. James</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>330877</td>
      <td>8.4583</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>0</td>
      <td>3</td>
      <td>Palsson, Master. Gosta Leonard</td>
      <td>male</td>
      <td>2.0</td>
      <td>3</td>
      <td>1</td>
      <td>349909</td>
      <td>21.0750</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



We can also chain conditional statements together by wrapping them in parenthesis and making use of the `&` and `|` operators ('and' and 'or' operators, respectively).

In the cell below, slice the DataFrame so that it only contains passengers with a `Fare` value between 50 and 100, inclusive.  


```python
fares_50_to_100_df = df.query("Fare >= 50.0 & Fare < 100")

```

We could go further and then preview the Fare column of this new subsetted DataFrame:


```python
fares_50_to_100_df.Fare.hist()
plt.xlabel('Fare', color='red')
plt.ylabel('Frequency', fontsize=12) 
plt.title('Distribution of Fares')
```




    Text(0.5, 1.0, 'Distribution of Fares')




![png](index_files/index_13_1.png)


Remember that there are two syntactically correct ways to access a column in a DataFrame.  For instance, `df['Name']` and `df.Name` return the same thing.  

In the cell below, use the dot notation syntax and slice a DataFrame that contains male passengers that survived that also belong to Pclass 2 or 3. Be sure to preview the column names and content of the `Sex` column.


```python
# Checking column names for reference
df.columns
```




    Index(['PassengerId', 'Survived', 'Pclass', 'Name', 'Sex', 'Age', 'SibSp',
           'Parch', 'Ticket', 'Fare', 'Cabin', 'Embarked'],
          dtype='object')




```python
# Checking Column values to hardcode query below
df.Sex.unique()
```




    array(['male', 'female'], dtype=object)




```python
poor_male_survivors_df = df.query("Sex == 'male'").query("Pclass == '3' | Pclass == '2'")
poor_male_survivors_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>0</td>
      <td>3</td>
      <td>Moran, Mr. James</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>330877</td>
      <td>8.4583</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>0</td>
      <td>3</td>
      <td>Palsson, Master. Gosta Leonard</td>
      <td>male</td>
      <td>2.0</td>
      <td>3</td>
      <td>1</td>
      <td>349909</td>
      <td>21.0750</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>12</th>
      <td>13</td>
      <td>0</td>
      <td>3</td>
      <td>Saundercock, Mr. William Henry</td>
      <td>male</td>
      <td>20.0</td>
      <td>0</td>
      <td>0</td>
      <td>A/5. 2151</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



Great! Now that you've reviewed the methods for slicing a DataFrame for querying our data, let's explore a sample use case.  


## Practical Example: Slicing DataFrames

In this section, you're looking to investigate whether women and children survived more than men, or that rich passengers were more likely to survive than poor passengers.  The easiest way to confirm this is to slice the data into DataFrames that contain each subgroup, and then quickly visualize the survival rate of each subgroup with histograms.

In the cell below, create a DataFrame that contains passengers that are female, as well as children (males included) ages 15 and under.   

Additionally, create a DataFrame that contains only adult male passengers over the age of 15.  


```python
women_and_children_df = df.query("Sex == 'female' | Age <= 15")
                         
male_all_ages_df =  df.query("Sex == 'male' | Age > 15")

```

Great! Now, you can use the `matplotlib` functionality built into the DataFrame objects to quickly create visualizations of the `Survived` column for each DataFrame.  

In the cell below, create histogram visualizations of the `Survived` column for both DataFrames.  Bonus points if you use `plt.title()` to label them correctly and make it easy to tell them apart!


```python
plt.subplot(121)
women_and_children_df.Survived.hist();
plt.title('Women and Childred');
plt.xticks(ticks=[0,1])

plt.subplot(122)
male_all_ages_df.Survived.hist();
plt.title('Adult Men');
plt.xticks(ticks=[0,1])

```




    ([<matplotlib.axis.XTick at 0x26cc32dcc50>,
      <matplotlib.axis.XTick at 0x26cc32dc588>],
     <a list of 2 Text xticklabel objects>)




![png](index_files/index_21_1.png)


Well that seems like a pretty stark difference--it seems that there was drastically different behavior between the groups!  Now, let's repeat the same process, but separating rich and poor passengers.  

In the cell below, create one DataFrame containing First Class passengers (`Pclass == 1`), and another DataFrame containing everyone else.


```python
first_class_df = df.query("Pclass == '1'")
second_third_class_df = df.query("Pclass != '1'")
```

Now, create histograms of the surivival for each subgroup, just as you did above.  


```python
plt.subplot(121)
first_class_df.Survived.hist();
plt.title('First');
plt.xticks(ticks=[0,1])

plt.subplot(122)
second_third_class_df.Survived.hist();
plt.title('Poor');
plt.xticks(ticks=[0,1])
```




    ([<matplotlib.axis.XTick at 0x26cc3396d68>,
      <matplotlib.axis.XTick at 0x26cc33966a0>],
     <a list of 2 Text xticklabel objects>)




![png](index_files/index_25_1.png)


To the surprise of absolutely no one, it seems like First Class passengers were more likely to survive than not, while 2nd and 3rd class passengers were more likely to die than not.  However, don't read too far into these graphs, as these aren't at the same scale, so they aren't fair comparisons.  

Slicing is a useful method for quickly getting DataFrames that contain only the examples we're looking for.  It's a quick, easy method that feels intuitive in Python, since we can rely on the same conditional logic that we would if we were just writing `if/else` statements.  

## Using the `.query()` method

Instead of slicing, you can also make use of the DataFrame's built-in `.query()` method.  This method reads a bit more cleanly and allows us to pass in our arguments as a string.  For more information or example code on how to use this method, see the [pandas documentation](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.query.html).

In the cell below, use the `.query()` method to slice a DataFrame that contains only passengers who have a `PassengerId` greater than or equal to 500. 


```python
query_string = "PassengerId >= 500"
high_passenger_number_df = df.query(query_string)
high_passenger_number_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>499</th>
      <td>500</td>
      <td>0</td>
      <td>3</td>
      <td>Svensson, Mr. Olof</td>
      <td>male</td>
      <td>24.0</td>
      <td>0</td>
      <td>0</td>
      <td>350035</td>
      <td>7.7958</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>500</th>
      <td>501</td>
      <td>0</td>
      <td>3</td>
      <td>Calic, Mr. Petar</td>
      <td>male</td>
      <td>17.0</td>
      <td>0</td>
      <td>0</td>
      <td>315086</td>
      <td>8.6625</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>501</th>
      <td>502</td>
      <td>0</td>
      <td>3</td>
      <td>Canavan, Miss. Mary</td>
      <td>female</td>
      <td>21.0</td>
      <td>0</td>
      <td>0</td>
      <td>364846</td>
      <td>7.7500</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>502</th>
      <td>503</td>
      <td>0</td>
      <td>3</td>
      <td>O'Sullivan, Miss. Bridget Mary</td>
      <td>female</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>330909</td>
      <td>7.6292</td>
      <td>NaN</td>
      <td>Q</td>
    </tr>
    <tr>
      <th>503</th>
      <td>504</td>
      <td>0</td>
      <td>3</td>
      <td>Laitinen, Miss. Kristina Sofia</td>
      <td>female</td>
      <td>37.0</td>
      <td>0</td>
      <td>0</td>
      <td>4135</td>
      <td>9.5875</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



Just as with slicing, you can pass in queries with multiple conditions.  One unique difference between using the `.query()` method and conditional slicing is that you can use `and` or `&` as well as `or` or `|` (for fun, try reading this last sentence out loud), while you are limited to the `&` and `|` symbols to denote and/or operations with conditional slicing.  

In the cell below, use the `query()` method to return a DataFrame that contains only female passengers of ages 15 and under. 

**_Hint_**: Although the entire query is a string, you'll still need to denote that `female` is also a string, within the string.  (_String-Ception?_)


```python
female_children_df = df.query("Sex == 'female' or Age <= 15")
female_children_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>0</td>
      <td>3</td>
      <td>Palsson, Master. Gosta Leonard</td>
      <td>male</td>
      <td>2.0</td>
      <td>3</td>
      <td>1</td>
      <td>349909</td>
      <td>21.0750</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>1</td>
      <td>3</td>
      <td>Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)</td>
      <td>female</td>
      <td>27.0</td>
      <td>0</td>
      <td>2</td>
      <td>347742</td>
      <td>11.1333</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>



A cousin of the `query()` method, `eval()` allows you to use the same string-filled syntax as querying for creating new columns.  For instance:

```
some_df.eval('C = A + B')
```

would return a copy of the `some_df` dataframe, but will now include a column `C` where all values are equal to the sum of the `A` and `B` values for any given row.  This method also allows the user to specify if the operation should be done in place or not, providing a quick, easy syntax for simple feature engineering.  

In the cell below, use the DataFrame's `eval()` method in place to add a column called `Age_x_Fare`, and set it equal to `Age` multiplied by `Fare`.  


```python
df = df.eval("Age_x_Fare = Age* Fare")
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>Age_x_Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
      <td>159.5000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
      <td>2708.7654</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
      <td>206.0500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
      <td>1858.5000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>NaN</td>
      <td>S</td>
      <td>281.7500</td>
    </tr>
  </tbody>
</table>
</div>



Great! Now, let's move on the coolest part of this lab--querying DataFrames with SQL!

## Querying DataFrames With SQL

For the final section of the lab, you'll make use of the `pandasql` library.  Pandasql is a library designed to make it easy to query DataFrames directly with SQL syntax, which was open-sourced by the company Yhat in late 2016.  It's very straightforward to use, but you are still encouraged to take a look at the [documentation](https://github.com/yhat/pandasql) as needed.  

If you're using the prebuilt virtual environment, you should already have the package ready to import. If not, uncomment and run the cell below to pip install pandasql so that it is available to import.


```python
!pip install pandasql
```

    Requirement already satisfied: pandasql in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (0.7.3)
    Requirement already satisfied: pandas in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from pandasql) (0.23.4)
    Requirement already satisfied: numpy in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from pandasql) (1.16.4)
    Requirement already satisfied: sqlalchemy in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from pandasql) (1.3.8)
    Requirement already satisfied: python-dateutil>=2.5.0 in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from pandas->pandasql) (2.8.0)
    Requirement already satisfied: pytz>=2011k in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from pandas->pandasql) (2018.5)
    Requirement already satisfied: six>=1.5 in c:\users\flatiron_user\.conda\envs\learn-env\lib\site-packages (from python-dateutil>=2.5.0->pandas->pandasql) (1.11.0)
    

    pexpect 4.6.0 requires ptyprocess>=0.5, which is not installed.
    You are using pip version 10.0.1, however version 19.2.3 is available.
    You should consider upgrading via the 'python -m pip install --upgrade pip' command.
    

That should have installed everything correctly. This library has a few dependencies, which you should already have installed. If you don't, just `pip install` them in your terminal and you'll be good to go!

In the cell below, import `sqldf` from `pandasql`.


```python
from pandasql import sqldf

```

Great! Now, it's time to get some practice with this handy library.

`pandasql` allows you to pass in SQL queries in the form of a string to directly query your database.  Each time you make a query, you need to pass an additional parameter that gives it access to the other variables in the session/environment. You can use a lambda function to pass `locals()` or `globals()` so that you don't have to type this every time.  

In the cell below, create a variable called `pysqldf` and set it equal to a lambda function `q` that returns `sqldf(q, globals())`.  If you're unsure of how to do this, see the example in the [documentation](https://github.com/yhat/pandasql).


```python
pysqldf = lambda q: sqldf(q, globals())
```

Great! That will save you from having to pass `globals()` as an argument every time you query, which can get a bit tedious.  

Now write a basic query to get a list of passenger names from `df`, limit 10.  If you would prefer to format your query on multiple lines and style it as canonical SQL, that's fine--remember that multi-line strings in python are denoted by `"""`--for example:
```
"""
This is a 
Multi-Line String
"""
```

In the cell below, write a SQL query that returns the names of the first 10 passengers.


```python
q = """
Select * from df limit 10
"""

passenger_names = pysqldf(q)
passenger_names
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
      <th>Age_x_Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>None</td>
      <td>S</td>
      <td>159.5000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
      <td>2708.7654</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>None</td>
      <td>S</td>
      <td>206.0500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
      <td>1</td>
      <td>0</td>
      <td>113803</td>
      <td>53.1000</td>
      <td>C123</td>
      <td>S</td>
      <td>1858.5000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>373450</td>
      <td>8.0500</td>
      <td>None</td>
      <td>S</td>
      <td>281.7500</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>0</td>
      <td>3</td>
      <td>Moran, Mr. James</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>330877</td>
      <td>8.4583</td>
      <td>None</td>
      <td>Q</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>McCarthy, Mr. Timothy J</td>
      <td>male</td>
      <td>54.0</td>
      <td>0</td>
      <td>0</td>
      <td>17463</td>
      <td>51.8625</td>
      <td>E46</td>
      <td>S</td>
      <td>2800.5750</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>0</td>
      <td>3</td>
      <td>Palsson, Master. Gosta Leonard</td>
      <td>male</td>
      <td>2.0</td>
      <td>3</td>
      <td>1</td>
      <td>349909</td>
      <td>21.0750</td>
      <td>None</td>
      <td>S</td>
      <td>42.1500</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>1</td>
      <td>3</td>
      <td>Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)</td>
      <td>female</td>
      <td>27.0</td>
      <td>0</td>
      <td>2</td>
      <td>347742</td>
      <td>11.1333</td>
      <td>None</td>
      <td>S</td>
      <td>300.5991</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>1</td>
      <td>2</td>
      <td>Nasser, Mrs. Nicholas (Adele Achem)</td>
      <td>female</td>
      <td>14.0</td>
      <td>1</td>
      <td>0</td>
      <td>237736</td>
      <td>30.0708</td>
      <td>None</td>
      <td>C</td>
      <td>420.9912</td>
    </tr>
  </tbody>
</table>
</div>



Great! Now, for a harder one:

In the cell below, query the DataFrame for names and fares of any male passengers that survived, limit 30.  


```python
q2 = """
select Name from df where Survived == 1 and Sex ='male'
"""

sql_surviving_males = pysqldf(q2)
sql_surviving_males
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Williams, Mr. Charles Eugene</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Beesley, Mr. Lawrence</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sloper, Mr. William Thompson</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mamee, Mr. Hanna</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Woolner, Mr. Hugh</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Moubarek, Master. Gerios</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Bing, Mr. Lee</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Caldwell, Master. Alden Gates</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Sheerlinck, Mr. Jan Baptist</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Greenfield, Mr. William Bertram</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Moss, Mr. Albert Johan</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Nicola-Yarred, Master. Elias</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Madsen, Mr. Fridtjof Arne</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Andersson, Mr. August Edvard ("Wennerstrom")</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Goldsmith, Master. Frank John William "Frankie"</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Becker, Master. Richard F</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Romaine, Mr. Charles Hallace ("Mr C Rolmane")</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Navratil, Master. Michel M</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Cohen, Mr. Gurshon "Gus"</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Albimona, Mr. Nassef Cassem</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Blank, Mr. Henry</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Sunderland, Mr. Victor Francis</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Hoyt, Mr. Frederick Maxfield</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Mellors, Mr. William John</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Beckwith, Mr. Richard Leonard</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Asplund, Master. Edvin Rojj Felix</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Persson, Mr. Ernst Ulrik</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Tornquist, Mr. William Henry</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Dorking, Mr. Edward Arthur</td>
    </tr>
    <tr>
      <th>29</th>
      <td>de Mulder, Mr. Theodore</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>79</th>
      <td>Lindqvist, Mr. Eino William</td>
    </tr>
    <tr>
      <th>80</th>
      <td>Wilhelms, Mr. Charles</td>
    </tr>
    <tr>
      <th>81</th>
      <td>Cardeza, Mr. Thomas Drake Martinez</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Hassab, Mr. Hammad</td>
    </tr>
    <tr>
      <th>83</th>
      <td>Dick, Mr. Albert Adrian</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Lam, Mr. Ali</td>
    </tr>
    <tr>
      <th>85</th>
      <td>Silverthorne, Mr. Spencer Victor</td>
    </tr>
    <tr>
      <th>86</th>
      <td>Calderhead, Mr. Edward Pennington</td>
    </tr>
    <tr>
      <th>87</th>
      <td>Moubarek, Master. Halim Gonios ("William George")</td>
    </tr>
    <tr>
      <th>88</th>
      <td>Taylor, Mr. Elmer Zebley</td>
    </tr>
    <tr>
      <th>89</th>
      <td>Chambers, Mr. Norman Campbell</td>
    </tr>
    <tr>
      <th>90</th>
      <td>Lesurer, Mr. Gustave J</td>
    </tr>
    <tr>
      <th>91</th>
      <td>Hawksford, Mr. Walter James</td>
    </tr>
    <tr>
      <th>92</th>
      <td>Stranden, Mr. Juho</td>
    </tr>
    <tr>
      <th>93</th>
      <td>Moor, Master. Meier</td>
    </tr>
    <tr>
      <th>94</th>
      <td>Hamalainen, Master. Viljo</td>
    </tr>
    <tr>
      <th>95</th>
      <td>Barah, Mr. Hanna Assi</td>
    </tr>
    <tr>
      <th>96</th>
      <td>Dean, Master. Bertram Vere</td>
    </tr>
    <tr>
      <th>97</th>
      <td>Carter, Master. William Thornton II</td>
    </tr>
    <tr>
      <th>98</th>
      <td>Thomas, Master. Assad Alexander</td>
    </tr>
    <tr>
      <th>99</th>
      <td>Hedman, Mr. Oskar Arvid</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Lulic, Mr. Nikola</td>
    </tr>
    <tr>
      <th>101</th>
      <td>Mallet, Master. Andre</td>
    </tr>
    <tr>
      <th>102</th>
      <td>McCormack, Mr. Thomas Joseph</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Richards, Master. George Sibley</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Chip, Mr. Chang</td>
    </tr>
    <tr>
      <th>105</th>
      <td>Marechal, Mr. Pierre</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Daly, Mr. Peter Denis</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Johnson, Master. Harold Theodor</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Behr, Mr. Karl Howell</td>
    </tr>
  </tbody>
</table>
<p>109 rows × 1 columns</p>
</div>



This library is really powerful! This makes it easy for us to leverage all of your SQL knowledge to quickly query any DataFrame, especially when you only want to select certain columns.  This saves from having to slice/query the DataFrame and then slice the columns you want (or drop the ones you don't want).

Although it's outside the scope of this lab, it's also worth noting that both `pandas` and `pandasql` provide built-in functionality for join operations, too!


## Practical Example: SQL in Pandas

In the cell below, create 2 separate DataFrames using `pandasql`.  One should contain the Pclass of all female passengers that survived, and the other should contain the Pclass of all female passengers that died.  

Then, create a horizontal bar graph visualizations of the `Pclass` column for each DataFrame to compare the two.  Bonus points for taking the time to make the graphs extra readable by adding titles, labeling each axis, and cleaning up the number of ticks on the X-axis! 


```python
# Write your queries in these variables to keep your code well-formatted and readable
q3 = """
Select * from df where Sex == 'female' and Survived == 1
"""
q4 = """
Select * from df where Sex == 'female' and Survived == 0
"""

survived_females_by_pclass_df = pysqldf(q3)
died_females_by_pclass_df = pysqldf(q4)

plt.figure(figsize=(13,8));
plt.subplot(121);
survived_females_by_pclass_df.Pclass.value_counts().plot(kind= 'bar');
plt.title('Female Survivors');
plt.xlabel('Class');
plt.ylabel('Count');

plt.subplot(122);
died_females_by_pclass_df.Pclass.value_counts().plot(kind= 'bar');
plt.title('Female Victim');
plt.xlabel('Class');
plt.ylabel('Count');


```


![png](index_files/index_45_0.png)


## Summary

In this lab, you practiced how to query Pandas DataFrames using SQL.


```python
!jupyter nbconvert --to markdown index.ipynb

!mv README.md README_old.md

!mv index.md README.md

```
