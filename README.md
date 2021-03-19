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



## MongoDB Compass to Explore Data Visually


## Indexes





