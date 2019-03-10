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

```shell
db.dbName.find({}, { "title": 1 })
```
<sup>*Due to the empty document as the filter, this query will the 'title' and '_id' fields from all documents.*</sup>

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

### insertOne()
When inserting docs, an ObjectId will be automatically generated, unless we specifiy our own ID in the "_id" field.

Add a single document to the database.
```shell
db.dbName.insertOne({ "name": "Rocky", "year": 1976 });
```

#### Insert into a DB called "moviesScratch"
```shell
db.moviesScratch.insertOne({ title: "Star Trek II: The Wrath of Khan", year: 1982: imdb: "tt0084726" })
```

#### Supply your own ID
```shell
db.moviesScratch.insertOne({ _id: "tt0084726", title: "Star Trek II: The Wrath of Khan", year: 1982: imdb: "tt0084726" })
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
## Operators
There are various operaters we can use.

<br><br>
### Update Operators
See the [MongoDB Update Operators](https://docs.mongodb.com/manual/reference/operator/update/) documentation.


#### $set
Fields to update/add in a document.

#### $unset
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

#### $min
Will only update the field inside this object if the value is less than the current value in the field.

#### $max
Only updates a field if the value being updated is more than the current value.

#### $inc
Increments the value of the field by the amount specified.

<br><br>
### Working with Arrays
There are various operators that are used specifically for Arrays. These can be found in the documentation for update operators.

#### $push
Add an item to an array.



<br><br>
### Query Operators
We can use operators within queries.\
See the [Query Operators](https://docs.mongodb.com/manual/reference/operator/query/) documentation.

#### Comparison Operators
`$eq`\
Matches values that are equal to a specified value.

`$gt`\
Matches values that are greater than a specified value.

`$gte`\
Matches values that are greater than or equal to a specified value.

`$in`\
Matches any of the values specified in an array.\
Must be an array, even if only using a single value.

`$lt`\
Matches values that are less than a specified value.

`$lte`\
Matches values that are less than or equal to a specified value.

`$ne`\
Matches all values that are not equal to a specified value.

`$nin`\
Matches none of the values specified in an array.

##### Examples
Query a single field.
```shell
db.dbName.find({
  "year": { $gte: 1970 }
})
```
<sup>*Will return the documents where the 'year' field is greater than or equal to 1970.*</sup>


Query a single field with two operators.
```shell
db.dbName.find({
  "year": { $gte: 1970, $lt: 1990 }
})
```
<sup>*Will return the documents where 'year' is greater than or equal to 1970, but is less than 1980.*</sup>


Query two fields in a document with multiple operators.
```shell
db.dbName.find({
  "year": { $gte: 1970, $lt: 1990 },
  "qty": { $in: [5, 10] }
})
```
<sup>*Returns all of the documents where 'year' is greater than or equal to 1970, but is less than 1980 and a field in the 'qty' array is either 5 or 10.*</sup>


Arrays that do not contain a field.
```shell
db.dbName.find({
  "colours": { $nin: ["Red"] }
})
```
<sup>*Will return all documents where 'Red' is not present in the 'colours' array.\
The value of `$nin` must be an array.*</sup>


<br><br>
### Logical Operators
#### $or
```shell
db.movieDetails.find(
  {
    $or: [
      { writers: { $in: ["Ethan Coen"] } },
      { writers: { $in: ["Joel Coen"] } }
    ]
  }
).count()
```
<sup>*Return all documents where the writers array includes a field of either "Ethan Coen" or "Joel Coen".*</sup>


<br><br>
### Element Operators

#### $exists
Check if a field exists within a document:
```shell
db.dbName.find({ title: { $exists: false } })
```
<sup>*Returns all documents that do not contain a 'title' field.*</sup>

Another way to check if a field does not exist, is to check for the value of a field being 'null'.\
However, this method will return all documents where the value of the field is explicitly set to 'null' and the documents that do not contain the field at all.

```shell
db.dbName.find({ title: null })
```


<br><br>
#### $type

Check if a field within a document is of a certain type.\
Looking at the "Alias" list in the [$type documentation](https://docs.mongodb.com/manual/reference/operator/query/type/) will help when determining which types of values we can filter by.

```shell
db.dbName.find({ viewerRating: { $type: "int" } })
```
<sup>*Will return all documents where the value type of the field 'viewerRating* is an 'int' value.</sup>


<br><br>
### Logical Operators
Additional info can be found in the [Logical Operataors](https://docs.mongodb.com/manual/reference/operator/query-logical/) documentation.

#### $or
Takes an array as its arguments, used to check if any of the filters match.

```shell
db.dbName.find({ $or: [
  { "year": { $gt: 1980 } },
  { "rating": { $gt: 8 } }
] })
```
<sup>*Returns all documents where either the 'year' field is greater than 1980, or the 'rating' field is greater than 8.*</sup>


#### $and
By default, different parts of a filter are AND by default.

```shell
db.dbName.find({ "year": 1980, "rating": 8 })
```
<sup>*Returns all documents where the year is 1980 and the rating is 8.*</sup>


The $and operator is used to filter the same field more than once in a query.

```shell
db.dbName.find({ $and: [
  { "year": null },
  { "year": { $exists: true } }
] })
```
<sup>*Returns documents where the 'year' field is 'null', but the 'year' field does actually exist.*</sup>

Without the $and operator, only the last query for a certain field will be used (the other before it will be ignored).


<br><br>
### Array Operators
These are helpful when querying fields within an array in a document.

#### $all
Matches fields within an array in a document.\
To match, all of the values in the $all query array must be included in the array for the chosen field within a document.\
They do not have to be in the specified order, or be the only values in the array, they just have to exist somewhere in the array.

```shell
db.movieDetails.find({ genres: { $all: ["Comedy", "Crime", "Drama"] } })
```
<sup>*Returns all documents that include 'Comedy', 'Crime', and 'Drama' within the 'genres' array field.*</sup>

#### $size
Used to match documents based on the length of an array.\
The value given to the size operator is the length of the array we are searching for.

```shell
db.dbName.find({ countries: { $size: 1 } })
```
<sup>*Returns all documents where the 'countries' array includes just one field (i.e. length of 1)*</sup>

#### $elemMatch
Used to search an element within an array field (in a document) for values that match certain criteria.

If we had an array of objects, this would be used over a regular find query as the find query would look in the array to see if any of the arguments match (regardless if they are in the same object or not) which is not what we want.

```shell
db.dbName.find({ "boxOffice.country": "Germany", "boxOffice.revenue": { $gt: 17 } })
```
<sup>*This would return any documents that have a country of 'Germany' in the 'boxOffice' array, and where the 'revenue' field is greater than 17, regardless if the 'revenue' field is in the same object (in the array) as the 'Germany' country field.*</sup>


We want to use the $elemMatch operator to make sure that the values 'Germany' and 17 are being matched in the same object within the array.

```shell
db.dbName.find({ "boxOffice": { $elemMatch: { "country": "Germany", "revenue": { $gt: 17 } } } })
```
<sup>*Now, only if the revenue field in the same object as country 'Germany' (in the array) is greater than 17, will the document be returned from the query.*</sup>


### Evaluation Operators
More detail on for these can be found in the [Evaluation Operators](https://docs.mongodb.com/manual/reference/operator/query-evaluation/) documentation.

#### $regex
We can use regular expressions in queries with the $regex operator.

```shell
db.dbName.find({ "title": { $regex: /^The .*/ } })
```
<sup>*Will return all documents where the first word in the 'title' field is 'The'. The regex means match the start of the string wth 'The' followed by any character, any number of times.*</sup>



<br><br>
## Modifiers

#### $each
Can be used with the $push and $addToSet operators to append multiple items.


<br><br>
## Updating Documents

### Update a Single Document
Documents within the same collection may not always have the same set of fields. Some may include fields that other documents do not have.

We can update a document using the `updateOne()` command.

First argument is a **filter** which defines the documents to target. When updating a single document, the first document in the collection matching the filter is returned.

The second argument specifies the fields and value being set (updated) in the document.

The `$set` operator is part of the `updateOne()` syntax which specifies the fields to be updated in the document.

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

or

```shell
ddb.dbName.find(QUERY).count()
```

<br><br>
### .count()
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

