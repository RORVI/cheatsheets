## What is MongoDB?
MongoDB is a cross-platform document-oriented database program. Classified as a NoSQL database program, MongoDB uses JSON-like documents with schema. 

#

## Mongo tools
* Use **`mongodump`** for **storing  a copy of the data in the database in BSON**.
* Use **`mongorestore`** for **restoring data to the database from the exported BSON file**.
* **To convert the BSON backup to JSON**, use **`bsondump`**.
* Use **`mongostat`** to **get a status of the currently running instance of mongodb**.

#

## Mongo terms
* **`Collections`** are similar to tables in SQL databases, but they do not have a schema.
* **`Documents`** are the rows in the collections.
* **`Fields`** are the equivalent of columns in SQL databases. They have a key and  value.

#

## Creating a database

* Use `show dbs` for displaying the databases in the system.
* Connect to a database by running the command `use <db_name>`.
* To create a database, use the command `use <db_name>`.
* To interact with the currently selected database, we use the db object. We set the name of the db object with the `use <db_name>`  command, when we create the database.
* To create a collection, use `db.<collection_name>`.
* To view how many documents there are in the collection: `db.<collection_name>.count()`.
* To insert a document into the collection: `db.<collection_name>.insert({“key_name”:”value”})`.
* Data can be added the “Javascript” way:

```javascript
var doc={};

doc.title="Nettuts+";
doc.url="google.com";
doc.comment="plowing along";

doc.tags=["tut","mongo"];

doc.saved_on=new Date();

doc.meta.browser="Google Chrome 14";
doc.meta.OS="Windows 10";

doc.name="doc name";
```

which will output the following database content, when typing `doc` into the mongo shell:

```json
{
	"title" : "Nettuts+",
    "url" : "google.com",
    "comment" : "plowing along",
    "tags" : [
        "tut",
        "mongo"
    ],
    "saved_on" : ISODate("2017-12-17T12:48:02.586Z"),
    "meta" : {
        "browser" : "Google Chrome 24",
        "OS" : "Windows 10"
    },
    "name" : "tutorial document",
    "date" : ISODate("2017-12-17T13:18:19.978Z")
}
```

* Use db.links.save(doc);  to save the modifications you did into the database.
* To query the entire database: db.links.find().forEach(printjson);

#

## Object IDs:
* IDs are immutable and unique. But they can be set on insert by setting a value to the _id property of the document.
* To view the date at which a document was created, use: **`db.links.find()[0]._id.getTimestamp();`**
* To get a new randomly generated `_id`, use **`new ObjectId`**;

* To display all the registrations in a database:
```javascript
function counter(name)
{
	var ret=db.counters.findAndModify({query: {_id:name}, update:{$inc: {next:1}}, "new":true, upsert:true});
	return ret.next;
}
```

#

## Queries:
* To load a script into the database (execute a script to alter the structure of the database), use:
```bash
.\mongo.exe <db_name> <path_to_script.js>
```

* Select: 
```javascript
db.users.find({email:'johndoe@gmail.com'});
```

* To display the output of the select in a formatted manner, use: 
```javascript
db.users.find({email:'johndoe@gmail.com'}).forEach(printjson);
```

* To display the output of a single value select, use: 
```javascript
db.users.findOne({email:'johndoe@gmail.com'});
```

* To exclude fields from a select query, use, “false”, or the 0 or 1 values for that item in the query:
```javascript
db.links.find({favourites: {$gt:100}},{url: false},{title: 1});
```

* Document values can be used as `foreign keys`, when working with multiple collections.

#

## Operators:

* **`$gt`** which stands for **`"greater than"`**
```javascript
db.links.find({favourites: {$gt: 50}},{title: 1, favourites:1, _id:0});
```

* **`$lt`** which stands for **`"less than"`**
```javascript
db.links.find({favourites: {$lt: 250}},{title: 1, favourites:1, _id:0});
```

* **`$lte`** which stands for **`"less than or equal to"`**
```javascript
db.links.find({favourites: {$lte: 150}},{title: 1, favourites:1, _id:0});
```

* **`$gte`** which stands for **`"greater than or equal to"`**
```javascript
db.links.find({favourites: {$gte: 50}},{title: 1, favourites:1, _id:0});
```

* **These can be used grouped, in order to make more complex sorting criteria.**

* **`$in`** which tells the system that the **sorting criteria should be part of the array given in the query**
```javascript
db.links.find({"name.first": {$nin: ["John","Jane"]}}, {name:1});
```

* **`$nin`** which tells the system that **the sorting criteria should not be part of the array given in the query**

* **`$all`** which tells the system that **the resulting elements should contain all the elements in the array given in the query**
```javascript
db.links.find({tags: {$all: ["tutorials","code"]}}, {title:1, tags:1, _id:0});
```

* **`$ne`** which tells the system that the **resulting elements should not contain any of the elements in the array given in the query**
```javascript
db.links.find({tags: {$ne: ["code"]}}, {title:1, tags:1, _id:0});
```

* **`$or`** which tells the system that **the resulting elements should contain at least one of the elements in the array given in the query**
```javascript
db.getCollection('users').find({$or:[{'name.first':'John'},{'name.first':'Jane'}]});
```

* **`$nor`** which tells the system that **the resulting elements should not contain one of the elements in the array given in the query**
```javascript
db.getCollection('users').find({$nor:[{'name.first':'John'},{'name.first':'Jane'}]});
```

* **`$and`** which tells the system that **the resulting elements should do not one of the elements in the array given in the query**
```javascript
db.getCollection('users').find({$and:[{'name.first':'Bob'},{'name.last':'Smith'}]});
```

* **`$exists`** which tells the system that **the resulting elements should have a field defined for the `"$exists"` argument**
```javascript
db.getCollection('users').find({email: {$exists: true}});
```

* **`$mod`** **uses modulo** on the given parameter
```javascript
db.getCollection('links').find({favourites: {$mod: [5,0]}}, {title:1, favourites:1, _id:0});
```

* **`$not`** used to **negate an operation**
```javascript
db.getCollection('links').find({favourites: {$not: {$mod: [5,0]}}}, {title:1, favourites:1, _id:0});
```

* **`$elemMatch`** used to **match the values from the query with the value from the database**
```javascript
db.getCollection('users').find({logins: {$elemMatch: {minutes: 34}}});
```

* **`$where`** used to **match the given value from the query with the value from the database**
```javascript
db.users.find({$where: 'this.name.first==="John"'}).forEach(printjson);
```

* **`distinct`** used to **get the distinct field values from the database**
```javascript
db.links.distinct('favourites');
```

#

## Function usage for queries:

* Instead of using **`$where`**, the **code can be transplanted into a function and executed inside a query**:
```javascript
var f=function(){return this.name.first==='John'};
db.users.find(f).forEach(printjson);
```

* To use **regex**:
```javascript
db.links.find({title:{$regex: /tuts\+$/, $ne:“Mobiletuts+”}});
```

* To **count the results obtained from a query**:
```javascript
db.users.find({“name.first”:”John”}).count();
```

**or**

```javascript
db.users.count({“name.first”:”John”});
```

* To **sort the results** obtained from a query:
```javascript
db.links.find({},{title:1, favourites:1, _id:0}).sort({title:-1, title:1});
//this will sort descending by title and sort alphabetically by title
```

* Limit used to **get a restricted number of outputs for the query**
```javascript
db.links.find({},{title:1, favourites:1, _id:0}).sort({favourites:-1}).limit(1);
```

* Skip can be used to **implement pagination in the database content**
```javascript
db.links.find({},{title:1, favourites:1, _id:0}).sort({favourites:-1}).skip(0*2).limit(2);
```

**or**

```javascript
db.links.find({},{title:1, favourites:1, _id:0}).sort({favourites:-1}).skip(1*2).limit(2);
```

#

## Content updating:

* To **update an attribute**:
```javascript
db.users.update({'name.first':'John'},{'job':'developer'});
```

* To **update an attribute or create the document if it doesn’t exist**, set the last parameter of the query to **`"true"`**:
```javascript
db.users.update({'name':'Roman Ovidiu'},{'name':'Roman Ovidiu','job':'developer'},true);
```

* To **update attributes for one or for a set of documents**:
```javascript
var n={title:'Nettuts+'};
db.links.find(n, {title:1, favourites:1});
```

* **`$inc`** used to **increment document attributes**
```javascript
var n={title:'Nettuts+'};
db.links.update(n,{$inc:{favourites:5}});
```

* **`$unset`** used to **delete an attribute from a document**
```javascript
db.links.update(n,{$unset:{job:'web developer'}});
```

* **`$push`** used to **add an attribute to a document**
```javascript
var temp={title:"Nettuts+"};
db.links.update(temp,{$push: {tags:'blog'}});
```

* **`$pushAll`** used to **add an array of attributes to a document**
* **`$addToSet`** used to **add an attributes to a document, but it will not add duplicates**
* **`$pull`** used to **delete an attribute from a document**
* **`$pullAll`** used to **delete multiple attributes from a document**
* **`$pop`** will **remove either the first or the last item from a document**:
```javascript
var temp={title:"Nettuts+"};
db.links.update(temp,{$pop: {tags:1}});
```
