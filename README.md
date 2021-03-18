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

* Run: /Users/elshamy/Documents/courses/mongodb/installation/mongodb-macos-x86_64-4.4.4/bin/mongo

\> show dbs
- admin   0.000GB
- config  0.000GB
- local   0.000GB

\> use shop (it will create shop db on the fly if it not exist)
- switched to db shop

\> db.products.insertOne({name: "iPhone 11", price: 560.00}) (create collection products into shop db with one record)

{

  "acknowledged" : true,
  
  "insertedId" : ObjectId("60531579e7f08c26e2b3d063")

}

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

##
