# MongoDB

MongoDB (Humongous), because it can store lots and lots of data.

## Characteristics
1. No Schema.
2. Speed, Flexible (No need for merge).

## How it works?

![](https://github.com/shamy1st/mongodb/blob/main/images/how-it-works.png)

## MongoDB Ecosystem

![](https://github.com/shamy1st/mongodb/blob/main/images/mongodb-ecosystem.png)

## Installation

1. www.mongodb.com
2. Software > Community Server > Download
3. Extract .tgz
4. Put extracted folder anywhere.
5. On macOS root create directory (data > db) (it won't work without this step!)
6. Start mongodb database server: /installation-directory/bin/mongod --dbpath "/data/db"

## Get Start

* Run: /installation-directory/bin/mongo

\> show dbs
- admin   0.000GB
- config  0.000GB
- local   0.000GB

\> use shop (it will create shop db on the fly if it not exist)
- switched to db shop

\> db.dropDatabase() (to drop current database)

\> db.stats() (show statistics about current database)

\> db.products.insertOne({name: "iPhone 11", price: 560.00}) (create collection products into shop db with one record)

{

  "acknowledged" : true,
  
  "insertedId" : ObjectId("60531579e7f08c26e2b3d063")

}

\> db.products.drop() (drop products collection from current database)

\> db.products.find() (if no argument then return all data in this collection)

{ "_id" : ObjectId("60531579e7f08c26e2b3d063"), "name" : "iPhone 11", "price" : 560 }

\> db.products.find().pretty() (the same as before but easier to read)

{

  "_id" : ObjectId("60531579e7f08c26e2b3d063"),

  "name" : "iPhone 11",

  "price" : 560

}

\> db.products.insertOne({name: "iPhone 12 Pro", price: 949.99, description: "APPLE iPhone 12 Pro 128GB"})

\> db.products.insertOne({name: "iPhone 12 Mini", price: 649.99, description: "APPLE iPhone 12 Mini", details: {storage: "128GB", color: "Black"}})

\> db.products.find().pretty()

## Drivers

* https://docs.mongodb.com/drivers/java/

## MongoDB In Action

![](https://github.com/shamy1st/mongodb/blob/main/images/mongodb-in-action-1.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/mongodb-in-action-2.png)

## Course Outline

![](https://github.com/shamy1st/mongodb/blob/main/images/course-outline.png)

## Basics

### Start server & shell

* start mongodb server
  * run: /installation-directory/bin/mongod --dbpath "/data/db" --port 27018
* start shell
  * run: /installation-directory/bin/mongo --port 27018

### Creating Databases & Collections

\> show dbs

\> use flights

\> db.flightData.insertOne({

"departureAirport": "MUC",

"arrivalAirport": "SFO",

"aircraft": "Airbus A380",

"distance": 12000,

"intercontinental": true

})

\> db.flightData.find().pretty()

### JSON vs BSON

* mongoDB actually store BSON, not JSON.

![](https://github.com/shamy1st/mongodb/blob/main/images/json-vs-bson.png)

### CRUD Operations

![](https://github.com/shamy1st/mongodb/blob/main/images/crud-usage.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/crud-operations.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/crud-example.png)

### Create

\> db.flightData.insertOne({

"departureAirport": "MUC",

"arrivalAirport": "SFO",

"aircraft": "Airbus A380",

"distance": 12000,

"intercontinental": true

})


\> db.flightData.insertMany([

  {

"departureAirport": "MUC",

"arrivalAirport": "SFO",

"aircraft": "Airbus A380",

"distance": 12000,

"intercontinental": true

},

{

"departureAirport": "LHR",

"arrivalAirport": "TXL",

"aircraft": "Airbus A320",

"distance": 950,

"intercontinental": false

}

])

### Read

\> db.flightData.find().pretty()

\> db.flightData.find({intercontinental: false}).pretty()

\> db.flightData.find({distance: {$gt: 900}}).pretty()

\> db.flightData.findOne({distance: {$gt: 900}}).pretty()

![](https://github.com/shamy1st/mongodb/blob/main/images/find-cursor.png)

* find only return cursor with about 20 records and not all records
* to get all records use: db.passengers.find().toArray()

### Update

* update one record where distance=12000 by adding new field marker=updated

\> db.flightData.updateOne({distance: 12000}, {$set: {marker: "updated"}})

* update all records by adding new field marker=toDelete

\> db.flightData.updateMany({}, {$set: {marker: "toDelete"}})

* replace only one occurance, and no need for $set

\> db.flightData.replaceOne({_id: ObjectId("60538c46e7f08c26e2b3d067")}, {marker: "toDelete"})

* replace old object by new one, and no need for $set

\> db.flightData.update({_id: ObjectId("60538c46e7f08c26e2b3d067")}, {marker: "toDelete"})

### Delete

* will delete first record with departureAirport=TXL

\> db.flightData.deleteOne({departureAirport: "TXL"})

* delete all records with marker=toDelete

\> db.flightData.deleteMany({marker: "toDelete"})

### Projection

![](https://github.com/shamy1st/mongodb/blob/main/images/projection.png)

\> db.passengers.find({}, {name: 1, age: 1, _id: 0}).pretty()

### Embedded Documents

![](https://github.com/shamy1st/mongodb/blob/main/images/embedded-documents.png)

### Arrays

![](https://github.com/shamy1st/mongodb/blob/main/images/arrays.png)

## Schemas & Relations: How to Structure Documents

### Schema or No Schema!

![](https://github.com/shamy1st/mongodb/blob/main/images/schema-or-no-1.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/schema-or-no-2.png)

### [Data Types](https://docs.mongodb.com/manual/reference/bson-types/)

![](https://github.com/shamy1st/mongodb/blob/main/images/data-types.png)

* [MongoDB Limits and Thresholds](https://docs.mongodb.com/manual/reference/limits/)
* MongoDB has a couple of hard limits - most importantly, a single document in a collection (including all embedded documents it might have) must be <= 16MB. Additionally, you may only have 100 levels of embedded documents.
* Normal integers (int32) can hold a maximum value of +-2,147,483,647
* Long integers (int64) can hold a maximum value of +-9,223,372,036,854,775,807
* Text can be as long as you want - the limit is the 16MB restriction for the overall document
* It's also important to understand the difference between int32 (NumberInt), int64 (NumberLong) and a normal number as you can enter it in the shell. The same goes for a normal double and NumberDecimal.
* NumberInt creates a int32 value => NumberInt(55)
* NumberLong creates a int64 value => NumberLong(7489729384792)
* If you just use a number (e.g. insertOne({a: 1}), this will get added as a normal double into the database. The reason for this is that the shell is based on JS which only knows float/ double values and doesn't differ between integers and floats.
* NumberDecimal creates a high-precision double value => NumberDecimal("12.99") => This can be helpful for cases where you need (many) exact decimal places for calculations.
* When not working with the shell but a MongoDB driver for your app programming language (e.g. Java, PHP, .NET, Node.js, ...), you can use the driver to create these specific numbers. https://mongodb.github.io/mongo-java-driver/4.2/apidocs/
* By browsing the API docs for the driver you're using, you'll be able to identify the methods for building int32s, int64s etc.

### Data Schemas & Data Modelling

![](https://github.com/shamy1st/mongodb/blob/main/images/schemas-and-modelling.png)

### Relations

![](https://github.com/shamy1st/mongodb/blob/main/images/relations.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/relations-summary.png)

### One To One Relations - Embedded

![](https://github.com/shamy1st/mongodb/blob/main/images/relations-example1.png)

### One To One - Using References

![](https://github.com/shamy1st/mongodb/blob/main/images/relations-example2.png)

### One To Many - Embedded

![](https://github.com/shamy1st/mongodb/blob/main/images/relations-example3.png)

### One To Many - Using References

![](https://github.com/shamy1st/mongodb/blob/main/images/relations-example4.png)

### Many To Many - Embedded

![](https://github.com/shamy1st/mongodb/blob/main/images/relations-example5.png)

### Many To Many - Using References

![](https://github.com/shamy1st/mongodb/blob/main/images/relations-example6.png)

### Join Reference Relations using lookup()

\> db.books.aggregate([{$lookup: {from: "authors", localField: "authors", foreignField: "_id", as: "creators"}}])

![](https://github.com/shamy1st/mongodb/blob/main/images/relations-join.png)

### Blog Project Example

![](https://github.com/shamy1st/mongodb/blob/main/images/blog-example.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/blog-example-sketch.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/blog-example-solution.png)

### [Schema Validation](https://docs.mongodb.com/manual/core/schema-validation/)

![](https://github.com/shamy1st/mongodb/blob/main/images/schema-validation-1.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/schema-validation-2.png)

    db.createCollection('posts', {
      validator: {
        $jsonSchema: {
          bsonType: 'object',
          required: ['title', 'text', 'creator', 'comments'],
          properties: {
            title: {
              bsonType: 'string',
              description: 'must be a string and is required'
            },
            text: {
              bsonType: 'string',
              description: 'must be a string and is required'
            },
            creator: {
              bsonType: 'objectId',
              description: 'must be an objectid and is required'
            },
            comments: {
              bsonType: 'array',
              description: 'must be an array and is required',
              items: {
                bsonType: 'object',
                required: ['text', 'author'],
                properties: {
                  text: {
                    bsonType: 'string',
                    description: 'must be a string and is required'
                  },
                  author: {
                    bsonType: 'objectId',
                    description: 'must be an objectid and is required'
                  }
                }
              }
            }
          }
        }
      }
    });

### Edit Validation Schema

    db.runCommand({
      collMod: 'posts',
      validator: {
        $jsonSchema: {
          bsonType: 'object',
          required: ['title', 'text', 'creator', 'comments'],
          properties: {
            title: {
              bsonType: 'string',
              description: 'must be a string and is required'
            },
            text: {
              bsonType: 'string',
              description: 'must be a string and is required'
            },
            creator: {
              bsonType: 'objectId',
              description: 'must be an objectid and is required'
            },
            comments: {
              bsonType: 'array',
              description: 'must be an array and is required',
              items: {
                bsonType: 'object',
                required: ['text', 'author'],
                properties: {
                  text: {
                    bsonType: 'string',
                    description: 'must be a string and is required'
                  },
                  author: {
                    bsonType: 'objectId',
                    description: 'must be an objectid and is required'
                  }
                }
              }
            }
          }
        }
      },
      validationAction: 'warn'
    });


## The Shell & The Server

### The Server

* check /installation-directory/bin/mongod --help

### dbpath & logpath

* create db/ directory and logs/ directory in the installation-directory/

\> /installation-directory/bin/mongod --dbpath /installation-directory/db --logpath /installation-directory/logs/log.log

### repair

* Run repair on all dbs, if you have any errors relate to your database.

\> /installation-directory/bin/mongod --repair

### directoryperdb

* Each database will be stored in a separate directory

\> /installation-directory/bin/mongod --directoryperdb

### storageEngine

* What storage engine to use - defaults to **wiredTiger** if no data files present

\> /installation-directory/bin/mongod --storageEngine

### fork

* Fork server process, and run as a background process "background service".
* Fork isn't available in Windows, but while installation check run as service.

\> /installation-directory/bin/mongod --fork

* how to quit? 
  * from mongo shell: /installation-directory/bin/mongo
  * use admin
  * db.shutdownServer()

### [Config File](https://docs.mongodb.com/manual/reference/configuration-options/)

* we can set all previous settings in config file
* in /installation-directory/bin/ create file "mongod.cfg"

        storage:
          dbPath: "/Users/elshamy/Documents/courses/mongodb/installation/mongodb-macos-x86_64-4.4.4/db"
        systemLog:
          destination: file
          path: "/Users/elshamy/Documents/courses/mongodb/installation/mongodb-macos-x86_64-4.4.4/logs/log.log"

* while start mongodb server set config file:

\> /installation-directory/bin/mongod -f /installation-directory/bin/mongod.cfg

### Shell

* check /installation-directory/bin/mongo --help
* after you connect to shell you can also check: help
* in database level check: db.help()
* in collection level check: db.test.help()


## MongoDB Compass to Explore Data Visually

* Download compass: https://www.mongodb.com/try/download/compass
* Explore the UI

## Create Operations


## Read Operations


## Update Operations


## Delete Operations


## Indexes

![](https://github.com/shamy1st/mongodb/blob/main/images/why-indexes.png)

* index come with cost in insertion

![](https://github.com/shamy1st/mongodb/blob/main/images/too-many-indexes.png)

### Adding a Single Field Index

* Download database tools: https://www.mongodb.com/try/download/database-tools

\> /path-to-database-tools/bin/mongoimport persons.json -d contactData -c contacts --jsonArray

* connect to shell and run the following

\> use contactData

\> db.contacts.explain("executionStats").find({"dob.age": {$gt: 60}})

* **"executionTimeMillis" : 12**, "totalKeysExamined" : 0, "totalDocsExamined" : 5000

* now create index (1 for asc, -1 for desc)

\> db.contacts.createIndex({"dob.age": 1})

* now run the command again with explain

\> db.contacts.explain("executionStats").find({"dob.age": {$gt: 60}})

* **"executionTimeMillis" : 4**, "totalKeysExamined" : 1222, "totalDocsExamined" : 1222

* to delete index

\> db.contacts.dropIndex({"dob.age": 1})

* if we changed age to 20 the index will slow down the speed of the query.
* then you should choose index depending on your query because sometimes it will slow down.

### Compound Indexes

* the order of the fields is important
* the second field is not supported by compound index

\> db.contacts.createIndex({"dob.age": 1, gender: 1})

* but in the query it doesn't matter which field comes first

\> db.contacts.explain("executionStats").find({"dob.age": 35, "gender": "male"})

* when you do query on "dob.age" it will use index scan, but if you will do query on "gender" it will do collection scan.

### Using Indexes for Sorting

\> db.contacts.explain("executionStats").find({"dob.age": 35}).sort({gender: 1})

* if you are making sorting without indexes it will take a long time and maybe it will timeout because mongodb will fetch and sort them in memory and mongodb has limiation 32 MB memory threshold.

### Default Index

* to list indexes

\> db.contacts.getIndexes()

* the first one in the list "_id" is the default index

### Configuring Indexes

* unique index will guarantee that this field is unique like email for example.

\> db.contacts.createIndex({email: 1}, {unique: true})

### Partial Filters

\> db.contacts.createIndex({"dob.age": 1}, {partialFilterExpression: {gender: "male"}})

* when you query on "dob.age" alone mongodb will do collection scan and will include females in the result.
* but when you query on "dob.age" with gender, then it will use index scan
* now what is the difference between compound index and partial filter?
* compound index take more space, but in the same time both fields are working alone or combined
* partial filter less space than compound index, but working only in the field of the index with filter

### Partial Index

* if you want to create unique index, but in the same time allow null

\> db.users.createIndex({email: 1}, {unique: true, partialFilterExpression: {email: {$exists: true}}})

* now you have unique index on email and you can insert user without email

### Time-To-Live (TTL) Index

\> db.sessions.createIndex({createdAt: 1}, {expireAfterSeconds: 10})

* any record create after setting this index will expire after 10 seconds and will be deleted
* if the record was created before this index, it will expire only if a new record is inserted and expired after the index is setted

### Query Diagnosis & Query Planning

![](https://github.com/shamy1st/mongodb/blob/main/images/query-planning.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/efficient-queries.png)

### Covered Queries

* is the query with "totalDocsExamined: 0" and you can reach this by the following
* only include the search field in the result and ignore "_id"

\> db.contacts.explain("executionStats").find({name: "Max"}, {_id:0, name: 1})

### How MongoDB Rejects a Plan?

![](https://github.com/shamy1st/mongodb/blob/main/images/winning-plans.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/clear-winning-plan.png)

### Multi-Key Indexes

\> db.contacts.drop()

\> db.contacts.insertOne({name: "Ahmed", hobbies: ["Cooking", "Sports"], addresses: [{street: "Main Street"}, {street: "Second Street"}]})

\> db.contacts.createIndex({hobbies: 1})

\> db.contacts.explain("executionStats").find({hobbies: "Sports"})

* used IXSCAN
* mongodb turn array to multi-key index **"isMultiKey: true"**
* multi-key index have a bigger size than single index
* multi-key index turn array values to a seperate index
* for example if you have array of 4 elements and you have 1000 documents, then it will stored in 4000 elements

\> db.contacts.createIndex({addresses: 1})

\> db.contacts.explain("executionStats").find({"addresses.street": "Main Street"})

* used COLLSCAN

\> db.contacts.explain("executionStats").find({addresses: {street: "Main Street"}})

* used IXSCAN

\> db.contacts.createIndex({"addresses.street": 1})

* now used IXSCAN, multi-key index
* multi-key of multi-key index will leads to performance issue

\> db.contacts.createIndex({name: 1, hobbies: 1})

* this compound index with only one multi-key is working good

\> db.contacts.createIndex({addresses: 1, hobbies: 1})

* cannot have compound index of more than one multi-key index
* because compound index do cartesian product of the two keys
* for example for each address will store all hobbies, which is very bad performance

### Text Indexes

![](https://github.com/shamy1st/mongodb/blob/main/images/text-index.png)

\> db.products.insertMany([{title: "A Book", description: "This is an awesome book about a young artist!"},{title: "Red T-Shirt", description: "This T-Shirt is red and it's pretty awesome!"}])

\> db.products.createIndex({description: "text"})

* mongodb will remove all stop words like (is, a, the, ...), and store key words in essential array

\> db.products.find({$text: {$search: "awesome"}}).pretty()

* only have one text index per collection, because text index is expensive
* then no need to specify the field name in the search
* everything is stored in lowercase
* if you search about "red book", the result will contain all products with word "red" + all products with word "book"
* to search about phrase you should surround it by double-quotes

\> db.products.find({$text: {$search: "\"red boook\""}}).pretty()
\> db.products.find({$text: {$search: "\"awesome boook\""}}).pretty()

* this is more faster than regular expression

### Text Indexes & Sorting

* sorting the search result is important

\> db.products.find({$text: {$search: "awesome t-shirt"}}).pretty()

* the result will show "A Book" product before "Red T-Shirt" product, while "Red T-Shirt" product contains both key words "awesome" and "t-shirt", but "A Book" product contains only one word "awesome"

\> db.products.find({$text: {$search: "awesome t-shirt"}}, {score: {$meta: "textScore"}}).sort({score: {$meta: "textScore"}}).pretty()

* now sorting the search result by score while mongodb provide

### Combined Text Indexes

\> db.products.getIndexes()

* drop text index by name: "description_text"

\> db.products.dropIndex("description_text")

\> db.products.createIndex({title: "text", description: "text"})

\> db.products.find({$text: {$search: "book"}}).pretty()

### Text Indexes to Exclude Words

* put minus "-" before the word which you want to exclude it's search result

### Setting the Default Language & Using Weights

\> db.products.getIndexes()

* drop text index by name: "title_text_description_text"

\> db.products.dropIndex("title_text_description_text")

\> db.products.createIndex({title: "text", description: "text"}, {default_language: "german", weights: {title: 1, description: 10}})

* now default_langauge is german
* description now is 10 times more important than title in calculating the score

\> db.products.find({$text: {$search: "red", $language: "german"}}, {score: {$meta: "textScore"}}).pretty()

* now you will find different score than before

### How to add Indexes?

![](https://github.com/shamy1st/mongodb/blob/main/images/building-indexes.png)

* creating index in foreground make lock on the collection while creating, which can cause problems
* all previous indexes which we already did was in foreground
* we will see how to add indexes in background

\> db.ratings.createIndex({age: 1}, {background: true})

* now it's created without lock on the collection

### Indexes Summary

![](https://github.com/shamy1st/mongodb/blob/main/images/indexes-summary.png)

* More on partialFilterExpressions: https://docs.mongodb.com/manual/core/index-partial/
* Supported default_languages: https://docs.mongodb.com/manual/reference/text-search-languages/#text-search-languages
* How to use different languages in the same index: https://docs.mongodb.com/manual/tutorial/specify-language-for-text-index/#create-a-text-index-for-a-collection-in-multiple-languages

## Geospatial Data

### [GeoJSON Objects](https://docs.mongodb.com/manual/reference/geojson/)

* format -> type: "one of supported types like Point", coordinates: \[logitude, latitude\]
* from google maps latitude comes first then latitude

\> db.places.insertOne({name: "Rathausplatz", location: {type: "Point", coordinates: \[16.3538882, 48.2205935\]}})

### Geo Queries

* provide your current location \[logitude, latitude\]

\> db.places.find({location: {$near: {$geometry: {type: "Point", coordinates: \[16.3611409, 48.2187821\]}}}})

* you will get error: unable to find index for $geoNear query
* need Geopatial index

\> db.places.createIndex({location: "2dsphere"})

* now repeat the find query again will success

\> db.places.find({location: {$near: {$geometry: {type: "Point", coordinates: \[16.3611409, 48.2187821\]}}}})

* how is near defined?!
* pass $maxDistance and $minDistance

\> db.places.find({location: {$near: {$geometry: {type: "Point", coordinates: \[16.3611409, 48.2187821\]}, $maxDistance: 600, $minDistance: 10}}})

* now you find the point is near to you within 600 meter

### Finding Places Inside a Certain Area

\> const p1 = \[logitude, latitude\]

\> const p2 = \[logitude, latitude\]

\> const p3 = \[logitude, latitude\]

\> const p4 = \[logitude, latitude\]

\> db.places.find({location: {$geoWithin: {$geometry: {type: "Polygon", coordinates: \[\[p1, p2, p3, p4, p1\]\]}}}})

* now if any point in the collection places is within this polygon will get in the result

### Finding Out If a User Is Inside a Specific Area

\> db.areas.insertOne({name: "A specific area", area: {type: "Polygon", coordinates: \[\[p1, p2, p3, p4, p1\]\]}})

\> db.areas.createIndex({area: "2dsphere"})

\> db.areas.find({area: {$geoIntersects: {$geometry: {type: "Point", coordinates: \[16.3611409, 48.2187821\]}}})

* also you can check intersect of area with another area

### Finding Places Within a Certain Radius

* [Calculate Distance Using Spherical Geometry](https://docs.mongodb.com/manual/tutorial/calculate-distances-using-spherical-geometry-with-2d-geospatial-indexes/) to calculate the radius 1km = 1 / 6,378.1

\> db.places.find({location: {$geoWithin: {$centerSphere: \[\[longitude, latitude\], radius\]}}})

### Geopatial Summary

![](https://github.com/shamy1st/mongodb/blob/main/images/geopatial-summary.png)

* Official Geospatial Docs: https://docs.mongodb.com/manual/geospatial-queries/
* Geospatial Query Operators: https://docs.mongodb.com/manual/reference/operator/query-geospatial/

## Aggregation Framework

![](https://github.com/shamy1st/mongodb/blob/main/images/aggregation-framework.png)

* [Aggregation Pipeline Stages](https://docs.mongodb.com/manual/reference/operator/aggregation-pipeline/#aggregation-pipeline-operator-reference)

\> mongoimport persons.json -d analytics -c persons --jsonArray

* go to mongo shell

\> use analytics

\> db.persons.findOne()

### [match stage](https://docs.mongodb.com/manual/reference/operator/aggregation/match/)

* return a cursor

\> db.persons.aggregate(\[
  {$match: {gender: "female"}}
\]).pretty()

### [group stage](https://docs.mongodb.com/manual/reference/operator/aggregation/group/)

db.persons.aggregate(\[
    {$match: {gender: "female"}},
    {$group: {_id: {state: "$location.state"}, totalPersons: {$sum: 1}}}
\]).pretty()



## Numeric Data

![](https://github.com/shamy1st/mongodb/blob/main/images/number-types.png)

## Security

![](https://github.com/shamy1st/mongodb/blob/main/images/security-checklist.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/authentication-authorization.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/role-based-access-control.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/why-roles.png)

### Creating User

![](https://github.com/shamy1st/mongodb/blob/main/images/creating-user.png)

* start server but with param --ath

\> mongod --auth

* now start shell without any user

\> mongo

* the problem how you can create user without having any user
* mongodb give exception for localhost user for creating only one user

\> use admin

\> db.createUser({user: "admin", pwd: "admin", roles: ["userAdminAnyDatabase"]})

\> db.auth("admin", "admin")

\> show dbs

* now you can show everything

### [Built-In Roles](https://docs.mongodb.com/manual/reference/built-in-roles/)

![](https://github.com/shamy1st/mongodb/blob/main/images/builtin-roles.png)

### Assigning Roles to Users & Databases

\> mongo -u admin -p admin

\> use shop

\> db.createUser({user: "appdev", pwd: "appdev", roles: ["readWrite"]})

\> db.auth("appdev", "appdev")

\> db.products.insertOne({name: "Book"})

* error: logical sessions can't have multiple authenticated users

\> mongo -u appdev -p appdev --authenticationDatabase shop

\> use shop

\> db.products.insertOne({name: "Book"})

### Updating & Extending Roles to Other Databases

* now "appdev" user only can access shop database
* we want to give him access to another databases

\> mongo -u admin -p admin

\> db.updateUser("appdev", {roles: ["readWrite", {role: "readWrite", db: "blog"}]})

\> db.getUser("appdev")

* now you can see that the user have readWrite role in blog database + old roles

\> mongo -u appdev -p appdev --authenticationDatabase shop  

\> use blog

\> db.posts.insertOne({title: "It works!"})

* something went wrong, please check it later!

### Adding SSL Transport Encryption

* https://docs.mongodb.com/manual/tutorial/configure-ssl/

![](https://github.com/shamy1st/mongodb/blob/main/images/transport-encryption.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/ssl-1.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/ssl-2.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/ssl-3.png)

* [How to create a self-signed certificate with OpenSSL](https://stackoverflow.com/questions/10175812/how-to-create-a-self-signed-certificate-with-openssl)

### Encryption at REST

* Storage Encryption is option allowed in mongoDB Enterprise
* But you can hash your passwords from your apps

![](https://github.com/shamy1st/mongodb/blob/main/images/encryption-rest.png)

### Links

![](https://github.com/shamy1st/mongodb/blob/main/images/security-summary.png)

* Official "Encryption at Rest" Docs: https://docs.mongodb.com/manual/core/security-encryption-at-rest/
* Official Security Checklist: https://docs.mongodb.com/manual/administration/security-checklist/
* What is SSL/ TLS? => https://www.acunetix.com/blog/articles/tls-security-what-is-tls-ssl-part-1/
* Official MongoDB SSL Setup Docs: https://docs.mongodb.com/manual/tutorial/configure-ssl/
* Official MongoDB Users & Auth Docs: https://docs.mongodb.com/manual/core/authentication/
* Official Built-in Roles Docs: https://docs.mongodb.com/manual/core/security-built-in-roles/
* Official Custom Roles Docs: https://docs.mongodb.com/manual/core/security-user-defined-roles/

## Performance, Fault Tolerancy & Deployment

### What Influences Performance?

![](https://github.com/shamy1st/mongodb/blob/main/images/what-influences-performance.png)

### Capped Collections

* is a special type of collection, which need to be created explicitly
* where you limit the data or documents can be stored in there
* old documents will be deleted when it's size is exceeded
* basically a store where an old data is deleted when a new data comes in
* efficient for high throughput application logs or caching
* by default it is 4 bytes

\> use performance

\> db.createCollection("capped", {capped: true, size: 10000, max: 3})

* size: 10000 -> maximum size in bytes
* max: 3 -> means 3 documents at most

\> db.capped.insertOne({name: "ahmed"})

\> db.capped.insertOne({name: "elshamy"})

\> db.capped.insertOne({name: "amr"})

\> db.capped.find().pretty()

* in default in capped collection you will get result in "insertion order"
* now insert another document then the oldest one should be deleted "ahmed"

\> db.capped.insertOne({name: "mona"})

\> db.capped.find().pretty()

### Replica Sets

![](https://github.com/shamy1st/mongodb/blob/main/images/replica-set-1.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/replica-set-2.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/replica-set-3.png)

### Sharding

![](https://github.com/shamy1st/mongodb/blob/main/images/sharding-1.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/sharding-2.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/sharding-3.png)

### Deploying a MongoDB Server

![](https://github.com/shamy1st/mongodb/blob/main/images/deploying.png)

### MongoDB Atlas

* https://www.mongodb.com/cloud/atlas
* start fro free > sign up
* create cluster

### Backups & Setting Alerts in MongoDB Atlas

* from MongoDB Atlas

### Connecting to our Cluster

* from UI > cluster0 > connect > create a database user admin
* add your IP to acces IPs
* chooose a connection method > shell > copy command

\> mongo "mongodb+srv://cluster0.mhvl9.mongodb.net/products" --username admin

\> show dbs

\> use products

### Links

* Official Docs on Replica Sets: https://docs.mongodb.com/manual/replication/
* Official Docs on Sharding: https://docs.mongodb.com/manual/sharding/

## Transactions

### Example

![](https://github.com/shamy1st/mongodb/blob/main/images/transaction-example.png)

\> use blog

\> db.users.insertOne({name: "ahmed"})

\> db.posts.insertMany(\[{title: "First Post", userId: ObjectId("6058edc471a69a1727e4f87c")}, {title: "Second Post", userId: ObjectId("6058edc471a69a1727e4f87c")}\])

\> db.users.deleteOne({_id: ObjectId("6058edc471a69a1727e4f87c")})

* now after we delete the user, this user have two posts
* if we delete these posts by userId and for any reason this operation failed, then it's not good state now
* the right thing is to delete user and his posts together in one transaction

### Transaction

\> const session = db.getMongo().startSession()

\> session.startTransaction()

\> const usersCollection = session.getDatabase("blog").users

\> const postsCollection = session.getDatabase("blog").posts

\> usersCollection.deleteOne({_id: ObjectId("6058ef7071a69a1727e4f87f")})

\> postsCollection.deleteMany({userId: ObjectId("6058ef7071a69a1727e4f87f")})

\> session.commitTransaction() - or session.abortTransaction()

* now check users collection after commit

\> db.users.find().pretty()

* Official Docs on Transactions: https://docs.mongodb.com/manual/core/transactions/

## From Shell to Driver

![](https://github.com/shamy1st/mongodb/blob/main/images/shell-and-driver.png)

## Realm (Stitch)

* Build better apps faster with edge-to-cloud sync and fully managed backend services including triggers, functions, and GraphQL.
* MongoDB Stitch is our new Backend as a Service (BaaS) that makes it easy for developers to create and launch applications across mobile and web platforms. Stitch provides a REST API on top of MongoDB with read, write, and validation rules built-in and full integration with the services you love. [link](https://www.mongodb.com/presentations/introduction-to-mongodb-stitch-drew-dipalma)
* MongoDB Realm is a serverless platform and mobile database. MongoDB Stitch and Realm Database are now part of MongoDB Realm.

![](https://github.com/shamy1st/mongodb/blob/main/images/what-is-stitch.png)

![](https://github.com/shamy1st/mongodb/blob/main/images/stitch-serverless.png)

## Ref
* https://www.udemy.com/course/mongodb-the-complete-developers-guide/
