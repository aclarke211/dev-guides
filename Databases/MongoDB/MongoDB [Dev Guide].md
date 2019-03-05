# MongoDB [Dev Guide]

## General

### Loading Data from JS File
To load dataset from a Javascript file first navigate to the directory containing the dataset file using a terminal.

```shell
cd <**FILE_LOCATION**>
```

Connect to a database via MongoShell from within this directory.\
Then use the `load()` MongoShell command to add the datset to the database.

```shell
load("dataset.js")
```


<br><br>
### Cursor
Essentially, the current result being returned, when many results are being returned from a query.\
A pointer to the current location in a result set. Is returned by the find() command.

When many results are returned, they are retuned in batches.\
Around 20 reults are returned per batch in MongoDB Compass.

The cursor is the current result being returned in a batch.

When another batch is requested in Mongo Shell a [getMore()]([https://www.google.com](https://docs.mongodb.com/manual/reference/command/getMore/#dbcmd.getMore)) method is called.
<br>
```shell
it
```
<sup>*(it is short for iterate')*</sup>

<br><br>

### Projections
Limit the fields that are returned in results documents. Allows you to only return the data that you need.

By defualt, all fields are returned from a request.

Only the fields in the query are returned from a projection.

Second argument to the find() method.

By default, projects will also return "_id" fields from documents, along with the specified fields.

```shell
db.movies.find({"genre": "Action"}, {"title": 1})
```
<sup>*Will return all documents with the genre **"Action"**, however only the **"title"** and **"_id"** fileds will be included in the returned results for each document.*</sup>
<br><br>
The **1** stands for include this field. We can exclude a field by using a 0.
```shell
db.movies.find({"genre": "Action"}, {"title": 1, "_id": 0})
```
<sup>*The **"_id"** fields will now no longer be returned in the results.*</sup>

Giving just a 0 in the projection will limit the fields returned in each document by excluding the fields specified.
```shell
db.movies.find({"genre": "Action"}, {"year": 0})
```
<sup>*The returned results will include all fields except the "**year** field.*</sup>


<br><br>
## Adding Docuemnts
Insert documents into a database.

<br><br>
### insertOne()
When inserting docs, an ObjectId will be automatically generated, unless we specifiy our own ID in the "_id" field.

Add a single document to the database.
```shell
const doc = {"name": "Rocky", "year": 1976};
db.dbName.insertOne(doc);
```

#### Insert into a DB called "moviesScratch"
```shell
db.moviesScratch.insertOne({title: "Star Trek II: The Wrath of Khan", year: 1982: imdb: "tt0084726"})
```

#### Supply your own ID
```shell
db.moviesScratch.insertOne({_id: "tt0084726", title: "Star Trek II: The Wrath of Khan", year: 1982: imdb: "tt0084726"})
```

<br><br>
### insertMany()

Used to insert multiple documents into a collection.

Add multiple documents to the database.
```shell
db.dbName.insertMany([doc1, doc2]);
```

#### First Argument is an array
The array is an array of documents to insert.
```shell
db.insertMany([])
```

#### Can give an optional second argument to insertMany (an options object/document)
Options for the query of the type object/document.<br>
Saying ordered: false, will prevent an insertMany command from erroring and stopping when it encounters an error such as a duplicate ID key.
```shell
db.insertMany([], { "ordered": false })
```

<br><br>
## Updating Documents

### Operators & Modifiers
These are the operators we can use when updating a document.\
See the [MongoDB Update Operators](https://docs.mongodb.com/manual/reference/operator/update/) documentation.

#### Operators

##### $set
Fields to update/add in a document.

##### $unset
Remove a field from the document.\
If the field does not exist, then the command does nothing.
```shell
db.dbName.update({
  "title": "Title to be removed"
  }, {
      $unset: {
        "title": "",
       "anotherFieldToRemove": ""
      }
  })
```

##### $min
Will only update the field inside this object if the value is less than the current value in the field.

##### $max
Only updates a field if the value being updated is more than the current value.

##### $inc
Increments the value of the field by the amount specified.

<br><br>
#### Working with Arrays
There are various operators that are used specifically for Arrays. These can be found in the documentation for update operators.

##### $push
Add an item to an array.

<br><br>
### Modifiers

##### $each
Can be used with the $push and $addToSet operators to append multiple items.

<br><br>
### Update a Single Document
Documents within the same collection may not always have the same set of fields. Some may include fields that other documents do not have.

We can update a document using the `updateOne()` command.

First argument is a **filter** which defines the documents to target. When updating a single document, the first document in the collection matching the filter is returned.

The second argument specifies the fields and value being set (updated) in the document.

The `$set` operator is part ofthe `updateOne()` syntax which specifies the fields to be updated in the document.

```shell
db.moviesScratch.updateOne({
  "title": "Rocky"
}, {
  $set: {
    "director": "John G. Avildsen"
  }
})
```

<br><br>
### Update Multiple Documents
We can update multiple documents at a time by using the `updateMany()` command.

The command will target all docments in the db which match the filter <sup>*(first argument)*</sup>.

```shell
db.movieDetails.updateMany({
  "rated": null
}, {
  $unset: {
    "rated": ""
  }
})
```
<sup>*Remove the 'rated' fields from all of the documents in the db with a 'rated' value of 'null'.*</sup>


<br><br>
### Upserts
Used to create new documents from within update commands.

If `upsert: true` is defined as the third argument, a new document is created when no document matches the filter.

If a document does match the filter, then that document is updated with the fields in the $set operator.

```shell
db.dbName.updateOne({
  "name.last": data.lastName
}, {
  $set: data
}, {
  upsert: true
})
```
<sup>*If the 'lasName' value in 'data' does not match any of the 'name.last' values in the documents, then a new document is created with with the values from 'data' as its fields.*</sup>


<br><br>
### Replacing Data
We can [replace a document](https://docs.mongodb.com/manual/reference/method/db.collection.replaceOne/) by using the `replaceOne()` command.

* The replacement document cannot contain update operators.
* replaceOne will apply changes to only one document, the first found in the server that matches the filter expression, using the [$natural](https://docs.mongodb.com/manual/reference/method/cursor.sort/#return-natural-order) order of documents in the collection.

```shell
const filter = {"title": "Rocky"};
const doc = {
  "title": "Rocky",
  "year": "1976",
  "writer": "Sylvester Stallone"
}

db.dbName.replaceOne(filter, doc);
```
<sup>*Will replace the first document that matches the filter and replace it with the doc object.*</sup>

Can `find()` a document from the db, update some of its fields, then `replace()` the same document with the updated data.


<br><br>
## Deleting Documents
Remove documents which match the filter from a database.

Delete a single document.
```shell
db.reviews.deleteOne({"_id": ObjectId("5c7dc858ef24f6a7e5b0770f")});
```

Delete multiple documents.
```shell
db.reviews.deleteMany({"reviewer_id": 759723314});
```


<br><br>
## Commands

### find()

#### Find documents in a collection
```shell
db.movies.find({"mpaaRating": "PG-13"}).pretty()

db.movies.find({"mpaaRating": "PG-13", "year": 2009}).pretty()
```

#### Match all docs with "C" in wind.type
Where wind is a document/object within a document.
```
db.100YWeatherSmall.find({"wind.type": "C"}).pretty()
```

#### Match an array exactly (exact strings, exact order)
```shell
db.movies.find({"cast": ["Jeff Bridges", "Tim Robbins"]}).pretty()

db.movieDetails.find({"writers": ["Ethan Coen", "Joel Coen"]}).pretty()
```


#### Match a string in an array (any position)
```shell
db.movies.find({"cast": "Jeff Bridges"}).pretty()
```

<br><br>
### .pretty()
Get formatted results back.

<br><br>
### quit()
Stop connection in Mongo Shell


<br><br>
### use
Switch to a DB or Collection in the MongoDB.

```shell
use <**DATABASE_NAME**>
```


<br><br>
### show
List the collections in a database.
```shell
show collections
```

<br><br>
### .find()
Return documents that match a query.
```shell
db.collectionName.find(QUERY)
```

<br><br>
### count()
Return the number of documents that match a query.
```shell
db.collectionName.count(QUERY)
```


<br><br>
## MongoDB Compaass

### Filter
Filter Documents in a Collection.<br>
They are && by default so only docs matching all values in the filter will be shown
```shell
{"mpaaRating": "PG-13", "year": 2009}
```

