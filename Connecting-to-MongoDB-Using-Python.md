# Connecting to MongoDB using Python

## Getting started

Install the driver for MongoDB

```bash
    pip install pymongo
```

**Important Note:** I have had problems with this command. When I run a script with pymongo it gets an error.

You can force an install with.

```bash
    python -m pip install pymongo
```

And this seems to fix the problem.

Check with.

```bash
    pip list
```

Returns.

> pymongo 4.5.0

Now, you can connect to a database with the following code.

## Creating documents in MongoDB

```python
    import pymongo

    def createMany(client, documents):
        db = client["TestDB"]
        collection = db["Test"]
        collection.insert_many(documents)

    client = pymongo.MongoClient("mongodb://localhost:27017")

    documents = [
        {"id": 11, "item": "Americanos", "size": "Short", "quantity": 22},
        {"id": 12, "item": "Latte", "size": "Long", "quantity": 12},
        {"id": 13, "item": "Cappuccino", "size": "Long", "quantity": 42},
        {"id": 14, "item": "Mocha", "size": "Short", "quantity": 25},
        {"id": 15, "item": "Americanos", "size": "Short", "quantity": 34},
    ]

    createMany(client, documents)
```

This code allows you to create a number of documents in the ``Test`` collection in the **TestDB** database.

## Reading documents in MongoDB

```python
    import pymongo

    def readDocuments(client, condition):
        db = client["TestDB"]
        collection = db["Test"]
        results = collection.find(condition)

        for row in results:
            print(row)

    client = pymongo.MongoClient("mongodb://localhost:27017")

    condition = {"item": {"$eq": "Latte"}}

    readDocuments(client, condition)
```

Returns:

> {'_id': ObjectId('653616265e54db92a3c68579'), 'id': 2, 'item': 'Latte', 'size': 'Short', 'quantity': 34}      
> {'_id': ObjectId('653616fb5e54db92a3c6857e'), 'id': 7, 'item': 'Latte', 'size': 'Short', 'quantity': 12}      
> {'_id': ObjectId('65361c9832fa9340e9e44153'), 'id': 12, 'item': 'Latte', 'size': 'Long', 'quantity': 12}      
> {'_id': ObjectId('65371907180d6d1178b96358'), 'id': 14, 'item': 'Latte', 'size': 'Long', 'quantity': 12}

## Update documents in MongoDB

We want to change documents with item = *Americanos* to *Americano*.

```python
    import pymongo

    def updateDoc(client, condition, operation):
        db = client["TestDB"]
        collection = db["Test"]
        results = collection.update_many(condition, operation)

        print(results.modified_count)

    client = pymongo.MongoClient("mongodb://localhost:27017")

    condition = {"item": {"$eq": "Americanos"}}
    operation = {"$set": {"item": "Americano"}}

    updateDoc(client, condition, operation)
```

Returns:

> 6

Meaning 6 documents were modified.

## Deleting documents in MongoDB

```python
    import pymongo

    def deleteDocument(client, condition):
        db = client["TestDB"]
        collection = db["Test"]
        results = collection.delete_many(condition)

        print(results.deleted_count)


    client = pymongo.MongoClient("mongodb://localhost:27017")

    condition = {"id": {"$eq": 15}}

    deleteDocument(client, condition)
```

Returns:

> 1

Meaning one document was deleted.

## Querying two related collections

I am querying the **artists** collection to find the **artistid** and then querying the **records** collection to find all documents with that ``artistid``.

```python
import pymongo

client = pymongo.MongoClient("mongodb://localhost:27017")

db = client["record-db"]

artist_name = "Bob Dylan"

artist_document = db["artists"].find_one({"name": artist_name})

if artist_document:
    artistid = artist_document["artistid"]
else:
    print(f"Artist '{artist_name}' not found in the 'artists' collection.")
    artistid = None

if artistid is not None:
    records = (
        db["records"]
        .find({"artistid": artistid})
        .sort([("recorded", pymongo.DESCENDING)])
    )

    album_names = [
        [record["name"], record["recorded"], record["media"]] for record in records
    ]

    if album_names:
        print(f"Albums by {artist_name}:")

        for album_name in album_names:
            if album_name[2] == "CD-Audio":
                album_name[2] = "CD"
            if album_name[2] == "Record":
                album_name[2] = "R"

            print(f"\t{album_name[1]}: {album_name[0]} ({album_name[2]})")
    else:
        print(f"No albums found for {artist_name}.")
```

Returns:

> Albums by Bob Dylan:      
>         2005: Modern Times (CD)       
>         2001: Love And Theft (CD)     
>         2001: The Essential Bob Dylan (CD)        
>         1997: Time Out Of Mind (CD)       
>         ...

## Getting albums by year recorded

This code will select all records for a particular year. It then gets a list of artists who released albums in that year.

I iterate through each artist and then get a list of their albums to print out.

```python
    import pymongo

    client = pymongo.MongoClient("mongodb://localhost:27017")

    db = client["record-db"]

    recorded_year = 1974

    records_by_year = db["records"].find({"recorded": recorded_year})

    artist_ids = list({record["artistid"] for record in records_by_year})

    artists_in_year = db["artists"].find({"artistid": {"$in": artist_ids}})

    artists_in_year = artists_in_year.sort(
        [("lastname", pymongo.ASCENDING), ("firstname", pymongo.ASCENDING)]
    )

    print(f"\nAlbums by year: {recorded_year}\n")

    for artist in artists_in_year:
        print(f"\t{artist['name']}")

        # Create a new cursor for records by the year for each artist
        records_by_year = db["records"].find(
            {"recorded": recorded_year, "artistid": artist["artistid"]}
        )

        for record in records_by_year:
            print(f"\t\t{record['name']} ({record['media']})")

    print()
```

Returns:

```bash
        ...
        Ry Cooder
                Paradise And Lunch (R)
                Paradise And Lunch (CD)
        Miles Davis
                Big Fun (CD)
        John Denver
                Back Home Again (R)
        The Dingoes
                The Dingoes (R)
        Julie Driscoll
                Season Of The Witch (CD)
        Bob Dylan
                Before The Flood (R)
                Before The Flood (CD)
                Blood On The Tracks (CD)
                Blood On The Tracks - New York Sessions (CD)
                Planet Waves (CD)
                Planet Waves (R)
                St Valentines Day Massacre (R)
        ...
```

**Note:** I had a problem getting this code to work. I found out that once I had iterated through each artist I lost the records for that artist.

I fixed the problem by creating a new cursor for that artist's records.

```python
    for artist in artists_in_year:
        print(f"\t{artist['name']}")

        # Create a new cursor for records by the year for each artist
        records_by_year = db["records"].find(
            {"recorded": recorded_year, "artistid": artist["artistid"]}
        )

        for record in records_by_year:
            print(f"\t\t{record['name']} ({record['media']})")
```
