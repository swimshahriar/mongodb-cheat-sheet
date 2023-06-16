# MongoDB Cheat Sheet

## Terminology

- **Collection**: A grouping of documents inside of a database. This is the same as a table in SQL and usually each type of data (users, posts, products) will have its own collection.

- **Document**:
  A record inside of a collection. This is the same as a row in SQL and usually there will be one document per object in the collection. A document is also essentially just a JSON object.

- **Field**: A key value pair within a document. This is the same as a column in SQL. Each document will have some number of fields that contain information such as name, address, hobbies, etc. An important difference between SQL and MongoDB is that a field can contain values such as JSON objects, and arrays instead of just strings, number, booleans, etc.

# Basic

## Show All Databases

```
show dbs
```

## Show Current Database

```
db
```

## Create Or Switch Database

```
use blog
```

## Drop

```
db.dropDatabase()
```

## Create Collection

```
db.createCollection('posts')
```

### Show Collections

```
show collections
```

# Create

## Insert Row

```
db.posts.insert({
  title: 'Post One',
  body: 'Body of post one',
  tags: ['news', 'events'],
  reactions: 2,
  views: 20,
  userId: 1,
})
```

## Insert Multiple Rows

```
db.posts.insertMany([
  {
    title: 'Post Two',
    body: 'Body of post two',
    category: 'Technology',
    tags: ['news', 'events'],
    reactions: 20,
    views: 30,
    userId: 1,
    date: Date()
  },
  {
    title: 'Post Three',
    body: 'Body of post three',
    category: 'News',
    tags: ['news', 'events'],
    reactions: 12,
    views: 10,
    userId: 1,
    date: Date()
  },
  {
    title: 'Post Four',
    body: 'Body of post three',
    category: 'Entertainment',
    tags: ['news', 'events'],
    reactions: 10,
    views: 3,
    userId: 1,
    date: Date()
  }
])
```

# Read

## Get All Rows

```
db.posts.find()
```

## Get All Rows Formatted

```
db.posts.find().pretty()
```

## Find Rows

```
db.posts.find({ userId: 1 })
```

## Find One Row

```
db.posts.findOne({ id: 1 })
```

## Find Specific Fields

```
db.posts.find({ title: 'Post One' }, {
  title: 1,
  userId: 1
})
```

## Count Rows

```
db.posts.find().count()
db.posts.find({ reactions: 2 }).count()
```

# Read Modifiers

## Sort Rows

```
# asc
db.posts.find().sort({ title: 1 }).pretty()
# desc
db.posts.find().sort({ title: -1 }).pretty()
```

## Limit Rows

```
db.posts.find().limit(2).pretty()
```

## Chaining

```
db.posts.find().limit(2).sort({ title: 1 }).pretty()
```

## Greater & Less Than

```
db.posts.find({ views: { $gt: 2 } })
db.posts.find({ views: { $gte: 7 } })
db.posts.find({ views: { $lt: 7 } })
db.posts.find({ views: { $lte: 7 } })
```

## Complex Filter

```
db.posts.find({ name: { $eq: "shahriar" } })
db.posts.find({ name: { $ne: "shahriar" } })
db.posts.find({ name: { $in: ["shahriar", "swim"] } })
db.posts.find({ name: { $nin: ["shahriar", "swim"] } })
db.posts.find({$or: [{name: "shahriar"}, {views: 3}]})
db.posts.find({name: {$not: {$eq: "shahriar"}}})
db.posts.find({name: {$exists: true}})
db.posts.find({ $expr: { $gt: ["$views", "$likes"] } })
```

# Update

## Update Row

```
db.posts.update({ title: 'Post Two' },
{
  title: 'Post Two',
  body: 'New body for post 2',
  date: Date()
})
```

## Update Specific Field

```
db.posts.update({ title: 'Post Two' },
{
  $set: {
    body: 'Body for post 2',
    userId: 2
  }
})
```

## Increment Field ($inc)

```
db.posts.update({ title: 'Post Two' },
{
  $inc: {
    reactions: 5
  }
})
```

## Rename Field

```
db.posts.update({ title: 'Post Two' },
{
  $rename: {
    reactions: 'favorites'
  }
})
```

## Rename Field

```
db.posts.update({ title: 'Post Two' },
{
  $rename: {
    reactions: 'favorites'
  }
})
```

## Remove a Field

```
db.posts.replaceOne({ title: 'Post Two' },
{
  $unset: {title: ""}
})
```

## Push into a Field

```
db.posts.replaceOne({ title: 'Post Two' },
{
  $push: {tags: "new tag"}
})
```

## Pull from a Field

```
db.posts.replaceOne({ title: 'Post Two' },
{
  $pull: {tags: "new tag"}
})
```

# Delete

## Delete Row

```
db.posts.deleteOne({ title: 'Post Four' })
```

## Delete Multiple Row

```
db.posts.deleteMany({ userId: 1 })
```
