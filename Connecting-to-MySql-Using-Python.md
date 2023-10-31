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

## Refactoring my code

I am going to start refactoring my code to stop repeating database routines.

I will create the following files.

### configuration.py

This file will have my connection details in it. This will minimise the number of times I have to repeat my connection strings. Now there will be only one place in my code that has usernames, passwords and the name of the database.

```python
def connect():
    user = "username"
    password = "password"
    database = "RecordDB"
    return user, password, database
```

This function creates an unnamed tuple. You can unpack the tuple to get the details.

Now to connect to my database routines all I have to do is unpack the tuple.

```python
import mysql.connector
import configuration as c

db_user, db_password, db_database = c.connect() # unpack the tuple
```

The next set of files I'll create will be database layers. These will hold my MySql database functions.

### album_db.py

These are a couple of my functions.

```python
import mysql.connector
import configuration as c


db_user, db_password, db_database = c.connect()


def get_all_artists():
    db = mysql.connector.connect(
        host="localhost", user=db_user, password=db_password, database=db_database
    )

    cursor = db.cursor()

    sql = "SELECT ArtistId, FirstName, LastName, Name, Biography FROM Artist ORDER BY LastName, FirstName;"

    cursor.execute(sql)

    artists = cursor.fetchall()

    cursor.close()
    db.close()

    return artists


def get_artist(artist_name):
    db = mysql.connector.connect(
        host="localhost", user=db_user, password=db_password, database=db_database
    )

    cursor = db.cursor()

    sql = f"SELECT ArtistId, Name, FirstName, LastName, Biography FROM Artist WHERE Name = '{artist_name}';"

    cursor.execute(sql)

    artist = cursor.fetchone()

    return artist

    cursor.close()
    db.close()
```

For example, when you return ``artists`` in the first function they are returned a list of tuples, with each tuple representing one artist and containing the mentioned fields.

To access these artist records you have to unpack them. E.g.

```python
    for artist in artists:
        artist_id, first_name, last_name, name, biography = artist

    ...
```

``artist`` in the second function is also a tuple and can be accessed by unpacking the tuple.

```python
artist = a.get_artist(name)

if artist:
    artist_id, artist_name, first_name, last_name, biography = artist  # Unpack the row

...
```

### record_db.py

Contains the record database calls.

```python
import mysql.connector
import configuration as c

db_user, db_password, db_database = c.connect()

def get_full_records():
    db = mysql.connector.connect(
        host="localhost", user=db_user, password=db_password, database=db_database
    )

    cursor = db.cursor()

    sql = "SELECT RecordId, ArtistId, Name, Field, Recorded, Label, Pressing, \
            Rating, Discs, Media, Bought, Cost, Review \
            FROM Record ORDER BY ArtistId, Recorded DESC;"

    cursor.execute(sql)

    records = cursor.fetchall()

    cursor.close()
    db.close()

    return records
```

Once again ```records``` are a list of tuples and can be accessed by unpacking them.

I can simplify my main python code by shifting the data functions into the following files.

### artist_data.py

```python
    def get_artist_ids(records):
        # Create a set to store distinct artist_ids
        artist_ids = set()

        for record in records:
            artist_id, name, recorded, media = record
            artist_ids.add(artist_id)

        # Convert the set to a list if needed
        artist_ids_list = list(artist_ids)

        return artist_ids_list

    def organize_records_by_artist(records):
        artist_records = {}

        for record in records:
            artist_id, name, recorded, media = record

            if artist_id not in artist_records:
                artist_records[artist_id] = []

            artist_records[artist_id].append((recorded, name, media))

        return artist_records
```

I can call and retrieve these records using the following code.

```python
    artist_ids_list = ad.get_artist_ids(records)
```

This returns a list of ``id's``.

**Note:** I could have left this list in a **set** and been able to retrieve the values just as easily. They were originally in a **set** because they were unique values.

### record_data.py

```
    def filter_records_by_artist(records, artist_id):
        filtered_records = [record for record in records if record[0] == artist_id]
        sorted_records = sorted(filtered_records, key=lambda record: record[1])

        return sorted_records
```

This function filters the ``records`` by artist_id to return a list of a particular artist's records. It also sorts the record names by alphabetical order. The function can be called with the following code.

```python
    filtered_records = rd.filter_records_by_artist(records, artist_id)

            for record in filtered_records:
                artist_id, name, recorded, media = record

                print(f"\t{name} ({media})")
```

## Create a list of all Artists and their Records

Uses the following modules.

* Configuration.py
* artist_db.py
* artist_data.py
* record_db.py

```python
    import artist_db as a
    import artist_data as ad
    import record_db as r

    artists = a.get_all_artists()

    records = r.get_records()


    def print_artist_records():
        artist_records = ad.organize_records_by_artist(records)

        for artist in artists:
            artist_id, first_name, last_name, artist_name, biography = artist

            if artist_id in artist_records:
                print(f"\n{artist_name}\n")

                for recorded, name, media in artist_records[artist_id]:
                    print(f"\t{recorded} - {name} ({media})")

        print()


    print_artist_records()
```

Returns:

```python
    ...
    Duane Allman

            1972 - Anthology (R)

    Greg Allman

            2011 - Low Country Blues (CD)
            1973 - Laid Back (R)

    The Allman Brothers Band

            1979 - Enlightened Rogues (R)
            1973 - Brothers And Sisters (R)
            1973 - Brothers And Sisters (CD)
            ...
```

### Print all albums for a particular year

Uses the following modules.

* Configuration.py
* artist_db.py
* artist_data.py
* record_db.py
* record_data.py

```python
    import artist_db as a
    import artist_data as ad
    import record_db as r
    import record_data as rd

    year = 1975

    records = r.get_records_by_year(year)

    if len(records) == 0:
        print(f"No records found for {year}.")
        exit()

    artists = a.get_all_artists()

    artist_ids_list = ad.get_artist_ids(records)

    artists = [artist for artist in artists if artist[0] in artist_ids_list]


    def print_records(artists, records):
        print(f"\nAlbums recorded in {year}:")

        for artist in artists:
            artist_id, first_name, last_name, name, biography = artist

            print(f"\n{name}\n")

            filtered_records = rd.filter_records_by_artist(records, artist_id)

            for record in filtered_records:
                artist_id, name, recorded, media = record

                print(f"\t{name} ({media})")

        print()


    print_records(artists, records)
```

Returns:

```python
    ...
    Loggins And Messina

            So Fine (R)

    Ellen McIlwaine

            The Real (R)

    Joni Mitchell

            The Hissing Of Summer Lawns (R)
            The Hissing Of Summer Lawns (CD)
    ...
```
