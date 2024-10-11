# HR_Analytics-Job_Change_of_Data_Scientists
A company operating in the field of Big Data and Data Science is looking to recruit data professionals from a pool of people who have successfully completed courses organized by the company. In order to optimize the recruitment and training process, the company wants to clearly identify candidates who are truly intending to work for the company after completing the course, and those who are simply looking for new job opportunities. This classification is important in reducing costs and time, while improving the quality of the training program as well as planning future courses.

The company has collected data related to the personal information, education and work experience of candidates when they register and take courses. By analyzing this data, the company will be able to categorize candidates into different groups based on their desires and motivations. This helps the company not only focus on candidates with long-term potential but also improve the quality of training and course planning, ensuring that the candidates joining are in line with the company's vision and goals.

This project will make an important contribution to the company's talent development strategy, helping to create a team of high-quality and most suitable data experts, contributing to taking the company further in the field of data technology.


# **1. Extract Data**
The dataset used in the project comes from a company that offers training in Data Science. It contains personal information about candidates who enrolled in these courses, with the goal of determining which individuals are genuinely interested in working for the company after completing the training. The data includes demographic details, education, work experience, and relevant feature such as: city, gender, education level, company size, relevant experience, training hours completed,...

The data includes both categorical and numerical features, and the goal is to analyze, preprocess, and build a predictive model to determine whether candidates will seek job changes after training.

The data is collected from multiple sources, including Google Sheets, Excel files, and CSV files. These are loaded using Python libraries like pandas, requests, and SQLAlchemy for database connections. Ensuring proper loading of this data is essential, and a preliminary inspection is done by checking the first few rows of the datasets.

Let's start with importing pandas to our notebook
```
import pandas as pd
import requests
```
**Loading data by Google Sheets**

We have the Google Sheet that stores data about enrolled students:
https://www.google.com/url?q=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1VCkHwBjJGRJ21asd9pxW4_0z2PWuKhbLR3gUHm-p4GI%2Fedit%3Fusp%3Dsharing
```
google_sheet_id = '1VCkHwBjJGRJ21asd9pxW4_0z2PWuKhbLR3gUHm-p4GI'
url = 'https://docs.google.com/spreadsheets/d/' + google_sheet_id + '/export?format=xlsx'
enrollies_data = pd.read_excel(url, sheet_name = 'enrollies')
```
The first 5 rows

Before performing any operation with data, you need to take a look to ensure that everything is okay with the loaded data.

In the cell below, write a code to output the first 5 rows of the dataframe.
```
enrollies_data.head(5)
```
**Loading data by Excel**

We have information about the educational background of the registered individuals in this file: https://www.google.com/url?q=https%3A%2F%2Fassets.swisscoding.edu.vn%2Fcompany_course%2Fenrollies_education.xlsx
```
url = 'https://github.com/NgaLam1703/HR_Analytics-Job_Change_of_Data_Scientists/raw/refs/heads/main/enrollies_education.xlsx'
file_name = 'enrollies_education.xlsx'
response = requests.get(url)
with open(file_name, 'wb') as file:
    file.write(response.content)
```
```
enrollies_education = pd.read_excel(f'./{file_name}')
```
The first 5 rows

Before performing any operation with data, you need to take a look to ensure that everything is okay with the loaded data.

In the cell below, write a code to output the first 5 rows of the dataframe.
```
enrollies_education.head()
```
**Loading data by CSV**

The data to work is located in CSV file by the following link: https://www.google.com/url?q=https%3A%2F%2Fassets.swisscoding.edu.vn%2Fcompany_course%2Fwork_experience.csv
```
url = 'https://github.com/NgaLam1703/HR_Analytics-Job_Change_of_Data_Scientists/raw/refs/heads/main/work_experience.csv'
file_name = 'work_experience.csv'
response = requests.get(url)
with open(file_name, 'wb') as file:
    file.write(response.content)
```
```
work_experience = pd.read_csv(f'./{file_name}')
```
The first 5 rows
Before performing any operation with data, you need to take a look to ensure that everything is okay with the loaded data.
In the cell below, write a code to output the first 5 rows of the dataframe.
```
work_experience.head()
```
**Loading data by SQL databases**

Let's import this:
```
from sqlalchemy import create_engine
```
Then install and import the MySQL connector for Python:

Module installation and import connector
```
!pip install pymysql
import pymysql
```
Connect to the database via SQL Alchemy tool and save the connection into a variable
```
engine = create_engine("mysql+pymysql://etl_practice:550814@112.213.86.31:3360/company_course")
training_hours = pd.read_sql("SELECT * FROM training_hours", engine)
```
The first 5 rows
Before performing any operation with data, you need to take a look to ensure that everything is okay with the loaded data.
In the cell below, write a code to output the first 5 rows of the dataframe.
```
training_hours.head(5)
```
**Loading data by Web Page**

So let's retrieve data from the table placed by the following link: https://www.google.com/url?q=https%3A%2F%2Fsca-programming-school.github.io%2Fcity_development_index%2Findex.html
```
# Use pandas to read the HTML table
tables = pd.read_html('https://sca-programming-school.github.io/city_development_index/index.html')
```
```
# Assuming the first table on the webpage is the one you want (indexing starts from 0)
cities = tables[0]
```
The first 5 rows
Before performing any operation with data, you need to take a look to ensure that everything is okay with the loaded data.
In the cell below, write a code to output the first 5 rows of the dataframe.
```
cities.head(5)
```
**Loading data by LMS databases**

Let's import this:
```
from sqlalchemy import create_engine
```
Connect to the database via SQL Alchemy tool and save the connection into a variable
```
engine = create_engine('mysql+pymysql://etl_practice:550814@112.213.86.31:3360/company_course')
employment = pd.read_sql('SELECT * FROM employment', engine)
```
The first 5 rows
Before performing any operation with data, you need to take a look to ensure that everything is okay with the loaded data.
In the cell below, write a code to output the first 5 rows of the dataframe.
```
employment.head(5)
```
# **2. Transfrom data**

## **2.1. Enrollies' data**

Show the columns of the dataframe and their types:
```
enrollies_data.info()
```
Looking at the table we can see that the city and gender columns are not of the same data type.

Let's fix their types and make them category:
```
enrollies_data['full_name'] = enrollies_data['full_name'].astype('string')
```
```
cat_cols = ['city', 'gender']
enrollies_data[cat_cols] = enrollies_data[cat_cols].astype('category')
```
Check again (output columns and their types):

```
enrollies_data.info()
```
After converting the data type to category, the gender column has a lot of missing data.
```
enrollies_data['city'] = enrollies_data['city'].str.lower()
enrollies_data['gender'] = enrollies_data['gender'].str.lower()
enrollies_data['full_name'] = enrollies_data['full_name'].str.lower()
```
```
gender_mode = enrollies_data['gender'].mode()[0]
enrollies_data['gender'] = enrollies_data['gender'].fillna(gender_mode).astype('category')
```
Check whether everything is okay. Output a test sample (5 random rows):
```
enrollies_data.sample(5)
```
Let's check again if the dataframe is correct.
```
enrollies_data.info()
```
## **2.2. Enrollies' education**

Show the columns of the dataframe and their types:
```
enrollies_education.info()
```
Check the sum of the error values
```
enrollies_education.isnull().sum()
```
We have category columns considered as objects and missing data.

Let's fix their types and make them category and fill missing data
```
enrollies_education['enrolled_university'] = enrollies_education['enrolled_university'].fillna('Missing')
enrollies_education['education_level'] = enrollies_education['education_level'].fillna('Missing')
enrollies_education['major_discipline'] = enrollies_education['major_discipline'].fillna('Missing')
```
```
tow_cols = ['education_level', 'major_discipline', 'enrolled_university']
enrollies_education[tow_cols] = enrollies_education[tow_cols].astype('category')
```
Let's check again if the dataframe is correct
```
enrollies_education.info()
```
Check whether everything is okay. Output a test sample (5 random rows):
```
enrollies_education.sample(5)
```
## **2.3. Enrollies' working experience**

Show the columns of the dataframe and their types:
```
work_experience.info()
```
We have category columns considered as objects and missing data.

Let's fix their types and make them category and fill missing data
```
experience_mode = work_experience['experience'].mode()[0]
work_experience['experience'] = work_experience['experience'].fillna(experience_mode)
```
```
work_experience['company_size'] = work_experience['company_size'].fillna('Missing')
work_experience['company_type'] = work_experience['company_type'].fillna('Missing')
work_experience['last_new_job'] = work_experience['last_new_job'].fillna('Missing')
```
Convert columns to category data type
```
cat_cols = ['company_size', 'company_type', 'last_new_job', 'relevent_experience', 'experience']
work_experience[cat_cols] = work_experience[cat_cols].astype('category')
```
Check whether everything is okay. Output a test sample (5 random rows):
```
work_experience.sample(5)
```
Check again (output columns and their types):
```
work_experience.info()
```
## **2.4. Training hours**

Show the columns of the dataframe and their types:
```
training_hours.info()
```
Looks like the data has no missing values, the data is complete

## **2.5. City development index**

Show the columns of the dataframe and their types:
```
cities.info()
```
```
cities.duplicated().sum()
```
Looks like the data has no missing values ​​and no duplicate rows. Dataset is clean

## **2.6. Employment**

Show the columns of the dataframe and their types:
```
employment.info()
```
Looks like the data has no missing values, dataset is clean

# **3. Load data**
```
from sqlalchemy import create_engine
```
```
# Path to the SQLite database
db_path = 'data_warehouse.db'
```
```
# Create an SQLAlchemy engine
engine = create_engine(f'sqlite:///{db_path}')
```
```
employment.to_sql('Fact_Employment', engine, if_exists= 'replace', index=False)
enrollies_data.to_sql('Dim_Enrollies', engine, if_exists= 'replace', index=False)
enrollies_education.to_sql('Dim_Education', engine, if_exists= 'replace', index=False)
work_experience.to_sql('Dim_Working_Experience', engine, if_exists= 'replace', index=False)
training_hours.to_sql('Dim_Training_Hours', engine, if_exists= 'replace', index=False)
cities.to_sql('Dim_Cities', engine, if_exists= 'replace', index=False)
```
