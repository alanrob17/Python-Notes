# Connecting to MySql using Python

Install the MySQL connector.

```bash
    python -m pip install mysql-connector-python
```

## Creating a database

```python
    import mysql.connector

    db = mysql.connector.connect(
        host="localhost", user="root", password="london@1"
    )

    cursor = db.cursor()

    cursor.execute("CREATE DATABASE RecordDB")
```

You can now change your database connection to be more specific.

```python
    import mysql.connector

    db = mysql.connector.connect(
        host="localhost", user="root", password="london@1", database="RecordDB"
    )

    print(db)
```

Returns:

> <mysql.connector.connection_cext.CMySQLConnection object at 0x0000011BEFA4F090>

## Creating a table

```python
    import mysql.connector

    db = mysql.connector.connect(
        host="localhost", user="root", password="london@1", database="RecordDB"
    )

    cursor = db.cursor()

    cursor.execute(
        "CREATE TABLE Artist (ArtistId INT AUTO_INCREMENT PRIMARY KEY, FirstName VARCHAR(50), LastName VARCHAR(50), Name VARCHAR (100), Biography LONGTEXT)"
    )
```

Now, create a Record table that will be related to the Artist table we created previously on the ``ArtistId`` field.

```python
    import mysql.connector

    db = mysql.connector.connect(
        host="localhost", user="root", password="london@1", database="RecordDB"
    )

    cursor = db.cursor()

    cursor.execute(
        "CREATE TABLE Record (RecordId INT AUTO_INCREMENT PRIMARY KEY, ArtistId INT, \
            Name VARCHAR(100), Field VARCHAR(50), Recorded INT, Label VARCHAR(50), \
            Pressing VARCHAR(50), Rating VARCHAR(4), Discs INT, Media VARCHAR(50), \
            Bought DATETIME, Cost DECIMAL(10,4), CoverName VARCHAR(50), \
            Review LONGTEXT, FreeDbId INT)"
    )
```

**Note:** we can use the ``\`` character to continue and save us from writing long lines of text.

Now, check your database.

```python
    import mysql.connector

    # connect to a database, if the database doesn't exist you will get an error.
    db = mysql.connector.connect(
        host="localhost", user="root", password="london@1", database="RecordDB"
    )

    cursor = db.cursor()

    cursor.execute("SHOW TABLES")

    for table in cursor:
        print(table)

```

Returns:

> ('artist',)       
> ('record',)

## Inserting records into a database

Create the **Artist** table.

```python
    import mysql.connector

    db = mysql.connector.connect(
    	host="localhost", user="root", password="london@1", database="RecordDB"
    )

    cursor = db.cursor()

    sql = "INSERT INTO Artist (ArtistId, FirstName, LastName, Name, Biography) VALUES (%s, %s, %s, %s, %s)"

    items = [
    	("528", "Mick", "Abrahams", "Mick Abrahams", "<p>Mick Abraham\'s ...</p>"),
    	("821", "Geoff", "Achison", "Geoff Achison", "<p>Australian-born...</p>"),
    	("1", "William", "Ackerman", "William Ackerman", "<p>Will Ackerman..."),
        ...
    ]

    cursor.executemany(sql, items)

    db.commit()

    print(f"{cursor.rowcount} were inserted.")
```

**Note:** the data has been abbreviated. Also note that the last record can have a comma on the end and this won't cause any problems in MySql.

We can do the same for the **Record** table.

```python
import mysql.connector

db = mysql.connector.connect(
	host="localhost", user="root", password="london@1", database="RecordDB"
)

cursor = db.cursor()

sql = "INSERT INTO Record (RecordId, ArtistId, Name, Field, Recorded, Label, Pressing, Rating, Discs, Media, Bought, Cost, Review) VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s)"

items = [
	("1431","528", "All Said And Done", "Rock", "1991", "Elite", "Eng", "*", "1", "CD", "2003-03-03 12:00:00", "0.0000", ""),
	("5236","821", "Sovereign Town", "Rock", "2018", "Landslide Records", "Aus", "****", "1", "CD", "2019-06-06 12:00:00", "20.0000", "<p>With the list...<p>"),
	("1","1", "It Takes A Year", "Acoustic", "1977", "Windham Hill", "Am", "**", "1", "R", "1990-07-15 12:00:00", "1.0000", ""),
    ...
]

cursor.executemany(sql, items)

db.commit()

print(f"{cursor.rowcount} were inserted.")
```

**Important:** all integers, decimals and datetime values are entered as strings. MySql will do the conversions provided that your data is in the correct format. Also note that the DateTime text is in the format, ``2003-03-03 12:00:00``.

## Create a list of all Artists and their Records

```python

```

Returns:

```python
```

## Search for an Artist

```python
    import mysql.connector

    db = mysql.connector.connect(
        host="localhost", user="root", password="london@1", database="RecordDB"
    )

    cursor = db.cursor()

    artistname = "Bob Dylan"

    sql = f"SELECT ArtistId, Name, FirstName, LastName FROM Artist WHERE Name = '{artistname}';"

    cursor.execute(sql)

    artist = cursor.fetchone()

    if artist:
        artist_id, artist_name, first_name, last_name = artist  # Unpack the row
        print(f"ArtistId: {artist_id}")
        print(f"Artist name: {artist_name}")
        print(f"First name: {first_name}")
        print(f"Last name: {last_name}")
    else:
        print(f"Artist '{artistname}' not found in the 'Artist' table.")

    cursor.close()
    db.close()
```

Returns:

> ArtistId: 114     
> Artist name: Bob Dylan        
> First name: Bob       
> Last name: Dylan

## Create a list of all Artists

```python
    import mysql.connector

    db = mysql.connector.connect(
        host="localhost", user="root", password="london@1", database="RecordDB"
    )

    cursor = db.cursor()

    sql = "SELECT ArtistId, FirstName, LastName,Name FROM Artist;"

    cursor.execute(sql)

    artists = cursor.fetchall()

    for artist in artists:
        artist_id, first_name, last_name, artist_name = artist  # Unpack the row
        print(f"Id: {artist_id}: {artist_name}")

    cursor.close()
    db.close()
```

Returns:

> Id: 1: William Ackerman       
> Id: 2: The Allman Brothers Band       
> Id: 3: Duane Allman       
> Id: 4: Greg Allman        
> Id: 5: Amazing Rhythm Aces        
> ...
