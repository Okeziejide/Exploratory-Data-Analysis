```python
# Import the libraries
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px

import mysql.connector
from mysql.connector import Error
from IPython.display import clear_output

# set up the function for database connection

def connect_database(user_name,host_name,db_name,db_password):
    connection = None
    try:
        connection = mysql.connector.connect(
            host = host_name,
            user = user_name,
            passwd = db_password,
            database = db_name
        )
        print(f'Connected to database {db_name} successfully')

    except Error as err:
        print(f'Error: {err}')

    return connection


host_name = input()
user_name = input()
db_password = input()
db_name =  input()

clear_output()

connection = connect_database(user_name,host_name,db_name,db_password)

# create a streaming pipeline

def read_query(query, connection):
    result = None
    cursor = connection.cursor()
    try:
        cursor.execute(query)
        result = cursor.fetchall()
    except Error as err:
        print(f'Error: {err}')

    return result

test_query = '''
select firstname, lastname, salary
from employees;
'''

data = read_query(query=test_query, connection=connection)

data_list = []

for row in data:
    row = list(row)
    data_list.append(row)

employee_df = pd.DataFrame(data=data_list, columns=['firstname','lastname','salary'])
employee_df
