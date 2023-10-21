# Connecting to SQL Server using Python

## Getting started

**Note:** Microsoft recommends using the ``pyodbc`` driver. 

## Configure development environment for pyodbc Python development

Install ODBC Driver 18 for SQL Server (these should already be installed).

If they aren't, download the drivers and install with,

```bash
    msiexec /i msodbcsql.msi ADDLOCAL=ALL
```

Add the licence,

```bash
    msiexec /quiet /passive /qn /i msodbcsql.msi IACCEPTMSODBCSQLLICENSETERMS=YES ADDLOCAL=ALL
```

## Install the pyodbc package

```bash
    pip install pyodbc
```

Check the list of installed packages.

'''bash
    pip list
'''
Now, you can connect to a database with the following code.

```python
"""
Connects to a SQL database using pyodbc
"""

import pyodbc

SERVER = "TIGER"
DATABASE = "RecordDB"

connectionString = f"DRIVER={{ODBC Driver 18 for SQL Server}};SERVER={SERVER};DATABASE={DATABASE};Encrypt=No;Trusted_Connection=yes;"

conn = pyodbc.connect(connectionString)

SQL_QUERY = """
SELECT 
a.Name AS Artist, r.Name AS Album, r.Recorded, r.Media
FROM 
Artist a INNER JOIN
Record r ON a.ArtistId = r.ArtistId
ORDER BY 
a.LastName, a.FirstName, r.Recorded DESC
"""

cursor = conn.cursor()
cursor.execute(SQL_QUERY)

artists = cursor.fetchall()

for a in artists:
    print(f"{a.Artist}:\t{a.Recorded} - {a.Album} ({a.Media})")
```

Output:

> ...       
> Neil Young:     2005 - Prairie Wind (CD)      
> Neil Young:     2005 - Chrome Dreams II (CD)      
> Neil Young:     2004 - Greatest Hits (CD)     
> ...

### Gotchas

I had a couple of issues getting this code to work. The first was the original connection string I was using.

```python
connectionString = f"DRIVER={{ODBC Driver 18 for SQL Server}};SERVER={SERVER};DATABASE={DATABASE};Trusted_Connection=yes;"
```

This gave me a connection error with a bad certificate warning. To get around this.

```python
connectionString = f"DRIVER={{ODBC Driver 18 for SQL Server}};SERVER={SERVER};DATABASE={DATABASE};Encrypt=No;Trusted_Connection=yes;"
```

I added the ``Encrypt=No;`` to get around this issue.

The next problem I had was that I had this working in Visual Studio Code with a Virtual Environment and then tried to use it with a Powershell command prompt. It didn't work.

Activating the Virtual Environment with the following command got the code working.

```bash
    lpp3ed\Scripts\activate
```

**Note:** this assumes that I had already installed the virtual environment.

## Insert a row as a transaction

In this example, you execute an ``INSERT`` statement safely and pass parameters. Passing parameters as values protects your application from SQL injection attacks.

```python
"""
Connects to a SQL database using pyodbc
"""
import pyodbc
from random import randrange

SERVER = "TIGER"
DATABASE = "AdventureWorksLT"

connectionString = f"DRIVER={{ODBC Driver 18 for SQL Server}};SERVER={SERVER};DATABASE={DATABASE};Encrypt=No;Trusted_Connection=yes;"

conn = pyodbc.connect(connectionString)

productNumber = randrange(1000)

SQL_QUERY = """
INSERT SalesLT.Product (
Name, 
ProductNumber, 
StandardCost, 
ListPrice, 
SellStartDate
) OUTPUT INSERTED.ProductID 
VALUES (?, ?, ?, ?, CURRENT_TIMESTAMP)
"""

cursor = conn.cursor()
cursor.execute(
    SQL_QUERY, f"Example Product {productNumber}", f"EXAMPLE-{productNumber}", 100, 200
)

resultId = cursor.fetchval()
print(f"Inserted Product ID : {resultId}")
conn.commit()
```

Returns:

> Inserted Product ID : 1000
