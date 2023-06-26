# MongoDB Commands Tutorial

## Introduction
In this document, we will cover essential MongoDB commands with brief explanations. These commands include querying, updating, and deleting data from MongoDB collections.

## Basic Commands

- `mongosh`: The MongoDB Shell command, used to start the MongoDB interactive shell.

- `user cooker;`: Switch to the user "cooker".

- `show dbs;`: Show all databases.

- `db.getName();`: Get the name of the current database.

- `show collections;`: Show all collections in the current database.


## Inserting Data

1. `doc = {"title":"Tacos", "desc":"Yummie tacos", cook_time:20};`
   - This command creates a new document named `doc` with the given properties.
   
2. `db.tacos.insertOne(doc);`
   - This command inserts the document `doc` into the `tacos` collection.


## Reading Data

1. `db.tacos.find();`
   - This command retrieves all documents from the `tacos` collection.

2. `db.recipes.find({"title":"Tacos"});`
   - This command retrieves documents from the `recipes` collection with a title of "Tacos".

3. `db.recipes.find({"title":"Tacos", "cook_time":20}).pretty();`
   - This command retrieves documents from the `recipes` collection with a title of "Tacos" and a `cook_time` of 20, and formats the output to be more readable.

4. `db.recipes.find({"title":"Tacos"},{"title":1}).pretty();`
   - This command retrieves only the "title" field from documents in the `recipes` collection with a title of "Tacos", and formats the output to be more readable.

5. `db.recipes.find({"title":"Tacos"},{"title":0}).pretty();`
   - This command retrieves all fields except the "title" from documents in the `recipes` collection with a title of "Tacos", and formats the output to be more readable.

6. `db.recipes.find({"title": {$regex: /taco/i}});`
   - This command retrieves documents from the `recipes` collection where the "title" field matches the regular expression /taco/i.

7. `db.recipes.find({"title": {$regex: /taco/i}},{"title":1});`
   - This command retrieves the "title" field from documents in the `recipes` collection where the "title" matches the regular expression /taco/i.

8. `db.recipes.find({},{"title":1});`
   - This command retrieves only the "title" field from all documents in the `recipes` collection.

9. `db.recipes.find({},{"title":1}).sort({"title":1}).skip(1).limit(1);`
   - This command retrieves the "title" field from all documents in the `recipes` collection, sorts the results by "title" in ascending order, skips the first result, and limits the output to 1 document.


## Updating Data

1. `db.examples.updateOne({"title":"Pizza"}, {$set:{"title":"Thin crust pizza"}});`
   - This command updates the "title" field of the first document in the `examples` collection where the "title" is "Pizza", setting the new title to "Thin crust pizza".

2. `db.examples.updateOne({"title":"Thin crust pizza"}, {$set:{"vegan":false}});`
   - This command updates the "vegan" field of the first document in the `examples` collection where the "title" is "Thin crust pizza", setting the new value to `false`.

3. `db.examples.updateOne({"title":"Thin crust pizza"}, {$unset:{"vegan":1}});`
   - This command removes the "vegan" field from the first document in the `examples` collection where the "title" is "Thin crust pizza".

4. `db.examples.updateOne({"title":"Tacos"},{$inc: {"likes_count":1}});`
   - This command increments the "likes_count" field by 1 in the first document in the `examples` collection where the "title" is "Tacos".

5. `db.examples.updateOne({"title":"Tacos"}, {$push:{"likes":60}});`
   - This command adds the number 60 to the "likes" array in the first document in the `examples` collection where the "title" is "Tacos".

6. `db.examples.updateOne({"title":"Tacos"}, {$pull:{"likes":60}});`
   - This command removes the number 60 from the "likes" array in the first document in the `examples` collection where the "title" is "Tacos".

   
## Deleting Data

- `db.examples.deleteOne({"_id": ObjectId("5ee69e393260aab97ea0d58e")});`
   - This command deletes the document with the specified ObjectId from the `examples` collection.


## Advanced Queries

1. `db.recipes.find({ $or :[{"cook_time":{$lte:30}}, {"prep_time":{$lte:10}}]},{"title":1});`
   - This command finds documents in the `recipes` collection where `cook_time` is less than or equal to 30 or `prep_time` is less than or equal to 10, and returns only the "title" field.

2. `db.recipes.find({"tags":{$all:["easy","quick"]}},{"title":1, "tags":1});`
   - This command finds documents in the `recipes` collection where the "tags" array contains both "easy" and "quick", and returns the "title" and "tags" fields.

3. `db.recipes.find({"tags":{$in:["easy","quick"]}},{"title":1, "tags":1});`
   - This command finds documents in the `recipes` collection where the "tags" array contains either "easy" or "quick", and returns the "title" and "tags" fields.

4. `db.recipes.find({"ingredients.name":"egg"}, {"title":1});`
   - This command finds documents in the `recipes` collection where the "ingredients.name" field is "egg", and returns only the "title" field.

5. `const results = db.recipes.find({},{title:1, prep_time:1,_id:0}).toArray(); console.table(results);`
   - This command finds all documents in the `recipes` collection, returning only the "title" and "prep_time" fields, converts the results to an array, and prints the results in a tabular format.

6. `console.table(results)`
    -Print the table *results*

## MongoDB Command Explanation

#### 1. List Indexes
```javascript
db.recipes.getIndexes()
```

## Drop Index
```javascript
db.recipes.dropIndex("cook_time_-1")
```
This command removes the 'cook_time_-1' index from the 'recipes' collection.

##  Explain Query Execution Stats
```javascript
db.recipes.find({ "cook_time": 10 }, { "title": 1 }).explain("executionStats")
```
This command runs a find operation on the 'recipes' collection to fetch documents where 'cook_time' is 10, returning only the 'title' field. The '.explain("executionStats")' provides statistics about the execution of the operation.

## Create Capped Collection
```javascript
db.createCollection("error_log",{capped:true,size:10000, max:10000});

```
This command creates a new capped collection named 'error_log'. A capped collection is a fixed-size collection that automatically overwrites its oldest entries when it reaches its maximum size. In this case, the size and the max number of documents are set to 10000.

## List Files in MongoDB
```bash
mongofiles list --db=files --quiet
```
This command lists all files stored in the MongoDB 'files' database, using the GridFS storage specification. The '--quiet' option suppresses the output of the underlying mongo shell that 'mongofiles' runs.

## Upload Files to MongoDB
```bash
mongofiles put .\apple-pie.jpg --db=files
mongofiles put ozma.pdf --db=files
```
These commands upload the files 'apple-pie.jpg' and 'ozma.pdf' to the 'files' database in MongoDB, using GridFS.

## Delete File from MongoDB
```bash
mongofiles delete ozma.pdf --db=files

```
This command deletes the 'ozma.pdf' file from the 'files' database in MongoDB.

##  Download File from MongoDB

```bash
mongofiles get apple-pie.jpg --db=files
```
This command downloads the 'apple-pie.jpg' file from the 'files' database in MongoDB.


## Start MongoDB with Replication
```bash
mongod --replSet cookingSet --dbpath=/store/data/rs2 --port 27018 --oplogSize 200

```
This command starts the MongoDB daemon with a replication set named 'cookingSet'. The database files are stored at '/store/data/rs2', the daemon listens on port 27018, and the operation log size is set to 200 MB.


