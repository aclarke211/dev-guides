# MongoDB [Dev Guide]

## General

### Cursor
Essentially, the current result being returned, when many results are being returned from a query.

A pointer to the current location in a result set. Is returned by the find() command.

When many results are returned, they are retuned in batches.

Around 20 reults are returned per batch in MongoDB Compass.

The cursor is the current result being returned in a batch.

When another batch is requested in Mongo Shell a [getMore()]([https://www.google.com](https://docs.mongodb.com/manual/reference/command/getMore/#dbcmd.getMore)) method is called.
<br>
```js
it
```
<sub>*(it is short for iterate')*</sub>

<br><br>

### Projections
Limit the fields that are returned in results documents. Allows you to only return the data that you need.

By defualt, all fields are returned from a request.

Only the fields in the query are returned from a projection.


<br><br>
## Commands

### find()

#### Find documents in a collection
```js
db.movies.find({"mpaaRating": "PG-13"}).pretty()

db.movies.find({"mpaaRating": "PG-13", "year": 2009}).pretty()
```

#### Match all docs with "C" in wind.type
Where wind is a document/object within a document.
```js
db.100YWeatherSmall.find({"wind.type": "C"}).pretty()
```

#### Match an array exactly (exact strings, exact order)
```js
db.movies.find({"cast": ["Jeff Bridges", "Tim Robbins"]}).pretty()

db.movieDetails.find({"writers": ["Ethan Coen", "Joel Coen"]}).pretty()
```


#### Match a string in an array (any position)
```js
db.movies.find({"cast": "Jeff Bridges"}).pretty()
```

<br><br>
### insertOne()
When inserting docs, an ObjectId will be automatically generated, unless we specifiy our own ID in the "_id" field.

#### Insert into a DB called "moviesScratch"
```js
db.moviesScratch.insertOne({title: "Star Trek II: The Wrath of Khan", year: 1982: imdb: "tt0084726"})
```

#### Supply our own ID
```js
db.moviesScratch.insertOne({_id: "tt0084726", title: "Star Trek II: The Wrath of Khan", year: 1982: imdb: "tt0084726"})
```

<br><br>
### insertMany()

Used to insert multiple documents into a collection.

#### First Argument is an array
The array is an array of documents to insert.
```js
db.insertMany([])
```

#### Can give an optional second argument to insertMany (an options object/document)
Options for the query of the type object/document.<br>
Saying ordered: false, will prevent an insertMany command from erroring and stopping when it encounters an error such as a duplicate ID key.
```js
db.insertMany([], { "ordered": false })
```

<br><br>
### quit()
Stop connection in Mongo Shell


<br><br>
### use
Use a DB in the MongoDB.

```js
use <**DATABASE_NAME**>
```


<br><br>
### show
List the collections in a database.
```js
show collections
```

<br><br>
### .pretty()
Get formatted results back.


<br><br>
### .find()
Return documents that match a query.
```js
db.collectionName.find(QUERY)
```

<br><br>
### count()
Return the number of documents that match a query.
```js
db.collectionName.count(QUERY)
```


<br><br>
## MongoDB Compaass

### Filter
Filter Documents in a Collection.<br>
They are && by default so only docs matching all values in the filter will be shown
```js
{"mpaaRating": "PG-13", "year": 2009}
```

