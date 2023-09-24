# sqlite_database_operations
## Details on the datasets 
The first dataset I used is from NewYork Presbyterian Hospital. The second dataset is from Stony Brook Hospital. Each dataset provides information about the code, description, the type of setting, and the charges of the hospital. Both datasets also provide charges for different insurances that are accepted by the respective hospital.
## An account of the exploratory data analysis process
For each dataset, my first step is to load in the data into a pandas dataframe. Then, I performed data cleansing by converting the column names into machine readable format. I also identified and then removed missing values. After that, I calculated the descriptive statistics for numerical columns, and frequency counts for categorical columns. I also performed data visualization using a histogram.
## Instructions to replicate SQLite database setup
1. Load in the packages
```
import pandas as pd
from sqlalchemy import create_engine
import sqlite3
```
The subsequent steps are to manually create a SQLite database and a table.
2. Create local SQLite DB
```
conn = sqlite3.connect('health.db')
c = conn.cursor()
```
3. Create new table called health_data
```
c.execute("""
            CREATE TABLE health_data
                (
                    hospital_name text,
                    insurance_type text,
                    code text,
                    code_description text,
                    cost_negotiated real,
                    cost_minimum real,
                    cost_maximum real
                );
          """)
conn.commit()
```
4. Insert new data
```
sql_query = """
INSERT INTO health_data (
  'hospital_name',
  'insurance_type',
  'code',
  'code_description',
  'cost_negotiated',
  'cost_minimum',
  'cost_maximum'
  )
  values (
    'southampton hospital',
    'aetna',
    '99214',
    'cost visit',
    100.0,
    1.00,
    10000.0
  );
"""
c.execute(sql_query)
conn.commit()
```
5. Create engine to connect to our sqlite DB
```
engine = create_engine('sqlite:///costs.db')
```
6. Check using pandas
```
healthdata = pd.read_sql("select * from health_data;", conn)
healthdata
```
The subsequent steps are to automatically create a SQLite database and a table.

7. Load in data
```
df = pd.read_csv('https://raw.githubusercontent.com/stephe-hu/sqlite_database_operations/main/data/stonybrook.csv')
```
8. Push pandas dataframe into sql table
```
df.to_sql('health_data', conn, if_exists='replace')
```
9. Check for table
```
query = """
  select *
  from health_data
  where type = 'Outpatient'
  limit 100;
"""
```
10. Create new dataframe called response
```
response = pd.read_sql(query, conn)
response
```