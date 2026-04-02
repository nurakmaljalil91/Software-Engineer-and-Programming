---
title: How to migrate data from csv or excel to database using Python
category: python
tags:
  - python
created: 2026-03-28
updated: 2026-03-28
status: active
---
## Overview

We have a excel file that contain the [[PKTBM]] data, where it save all the information for [[PKTBM]] member. We want to move the data to the [[database]], because we already create a new web application for moving the work of recording the [[PKTBM]] member from just an excel to our new web application. We will need to create a script call `pktbm_moving_data.py` using [[Python]].

## Steps

1. First step, we want to make sure the [[database]] is exists. To do this we will do migration for the project using our web api in [[Dot NET]] with [[Postgresql]] [[database]]. Create a new [[database]] using [[PGAdmin]] and then just configure the `appsetting.development.json` in the web api to use the [[database]] connection string.
![[Pasted image 20240519131323.png]]
Here we create a new [[database]] in the [[PGAdmin]].

```json
{
	"ConnectionStrings": {  
		  "DefaultConnection": "Server=localhost;port=5432;user id = postgres; password = admin4321; database = pktbmlocaldb; pooling = true; Minimum Pool Size=0;Maximum Pool Size=100;"
	},
}
```

Here is the simple local [[Postgresql]] [[database]] connection string in `appsettings.development.json`

2. Run ef core migration in [[Dot NET]] web api project by following this instruction 
3. Once the database ready, make the excel simple by making the simple column and data. Convert it to `csv` file using Excel.
4. [[Create virtual environment for Python]] project and install `pandas` library

```bash
pip install pandas
```

5. Import `pandas` and read the csv file and print the result

```python
import pandas as pd

# Load the CSV file  
file_path = './simple_cashbook.csv'  
df = pd.read_csv(file_path)  
  
# Set display options to show all rows and columns  
pd.set_option('display.max_rows', None)  
pd.set_option('display.max_columns', None)  
  
# Display the dataframe  
print(df)
```

6. To insert to [[Postgresql]] [[database]], install `psycopg2` library. This library allow you to connect to the localhost [[database]] and insert the parse data.

```bash
pip install psycopg2
```

7. Rename the columns of the csv file to match the [[database]] columns. Select only the columns that needed and handle missing value using `fillna('')` function to make all null value into empty string

```python
# Rename columns to match the database table's columns  
df = df.rename(columns={  
    'NAMA SYARIKAT': 'CompanyName',  
    'REGION': 'Region',  
    'STATE': 'State',  
    'JENIS KEAHLIAN': 'Type',  
    'H/P': 'HandPhone',  
    'NAMA': 'DirectorName',  
    'EMAIL': 'Email',  
    'NO OFIS': 'Address',  
    'STATUS': 'Status'  
})  
  
# Select only the columns that we need  
df = df[['CompanyName', 'Region', 'State', 'Type', 'HandPhone', 'DirectorName', 'Email', 'Address', 'Status']]  
  
# Handle missing values  
df = df.fillna('')
```

8. Connect to [[Postgresql]] database

```python
# Connect to the database  
connection = psycopg2.connect(  
    user="postgres",  
    password="admin4321",  
    host="157.230.248.77",  
    port="5432",  
    database="pktbmdb"  
)  
  
cursor = connection.cursor()
```

9. Iterate the excel data and create a query to insert it into the database

```python
# Insert the data into the database  
for index, row in df.iterrows():  
    # Insert the data into the database  
    query = (  
        "INSERT INTO public.\"Members\" (\"CompanyName\", \"Region\", \"State\", \"Type\", \"Address\", "  
        "\"PhoneNumber\", \"Status\", \"CreatedDate\", \"CreatedBy\", \"UpdatedDate\", \"UpdatedBy\") "  
        "VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s) RETURNING \"Id\";")  
    # print('Inserting data into the database for company:', row['CompanyName'])  
    variables = (  
        row['CompanyName'], get_region(row['Region']), get_malaysia_state(row['State']), get_type(row['Type']),  
        row['Address'], row['HandPhone'], get_status(row['Status']), datetime.now(timezone.utc), 'system',  
        datetime.now(timezone.utc), 'system')  
    print(variables)  
    cursor.execute(query, variables)  
    member_id = cursor.fetchone()[0]  
    print('Member ID:', member_id)  
  
connection.commit()  
  
# Close the connection  
cursor.close()  
connection.close()
```

10. Once insert all, close the connection and check the data in the database is inserted