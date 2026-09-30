# 🍃 MongoDB Beginner Study Kit

> A beginner-friendly MongoDB reference based on the provided **Getting Started with MongoDB** notes.

<div align="center">

**MongoDB → Database → Collection → Document → Field**

![MongoDB](https://img.shields.io/badge/MongoDB-Beginner%20Guide-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Mermaid](https://img.shields.io/badge/Mermaid-Diagrams-FF3670?style=for-the-badge&logo=mermaid&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner-2F80ED?style=for-the-badge)

</div>

---

## 📚 What this kit contains

| File | Purpose |
|---|---|
| [README.md](README.md) | Main MongoDB beginner notes and quick reference |
| [01-mongodb-ecosystem.mmd](diagrams/01-mongodb-ecosystem.mmd) | MongoDB / Atlas / Cluster / Database hierarchy |
| [02-mongodb-learning-path.mmd](diagrams/02-mongodb-learning-path.mmd) | Beginner learning flow |
| [03-crud-flow.mmd](diagrams/03-crud-flow.mmd) | CRUD operations flow |
| [04-query-toolkit.mmd](diagrams/04-query-toolkit.mmd) | Filtering, projection, operators, sorting and pagination |
| [05-aggregation-pipeline.mmd](diagrams/05-aggregation-pipeline.mmd) | Aggregation and pipeline flow |
| [06-document-structure.mmd](diagrams/06-document-structure.mmd) | Document, nested document and array structure |

> 💡 The `.mmd` files are standalone Mermaid diagrams. Open them with a Mermaid-compatible VS Code extension or Mermaid editor to view and zoom/pan the diagrams.

---

# 1. 🌱 MongoDB in simple words

**MongoDB is a document-oriented database.** Instead of storing data in rows and columns like a relational database, MongoDB stores data as documents inside collections.

### SQL vs MongoDB

| SQL concept | MongoDB concept |
|---|---|
| Database | Database |
| Table | Collection |
| Row | Document |
| Column | Field |
| Primary Key | `_id` |
| `INSERT` | `insertOne()` / `insertMany()` |
| `SELECT` | `find()` |
| `UPDATE` | `updateMany()` |
| `DELETE` | `deleteMany()` |
| `ORDER BY` | `sort()` |
| `LIMIT` | `limit()` |
| `OFFSET` | `skip()` |
| `GROUP BY` | `$group` |
| `WHERE` filtering | `find()` conditions / `$match` |

> 🟢 **Easy memory trick:** SQL has **tables + rows**. MongoDB has **collections + documents**.

---

# 2. ☁️ MongoDB terms you should know

| Term | Common meaning |
|---|---|
| **MongoDB** | The database technology |
| **MongoDB Atlas** | Cloud service used to create and manage MongoDB deployments |
| **MongoDB Cluster** | The MongoDB deployment/environment you connect to |
| **Database** | Logical container for collections |
| **Collection** | Similar to a SQL table |
| **Document** | Similar to a SQL row; stores JSON-like data |
| **Field** | Similar to a SQL column |
| **`_id`** | Unique identifier automatically generated for a document when not supplied |
| **MongoDB Shell (`mongosh`)** | Command-line tool for interacting with MongoDB |
| **Connection String** | String used to connect to a MongoDB deployment |
| **CRUD** | Create, Read, Update, Delete |
| **Projection** | Selecting which fields should appear in results |
| **Operator** | Special MongoDB keyword such as `$gt`, `$lte`, `$or`, `$set` |
| **Aggregation** | Processing and calculating data from documents |
| **Aggregation Pipeline** | Multiple processing stages executed in sequence |
| **Nested / Embedded Document** | A document stored inside another document |
| **Array Field** | A field containing a list of values or documents |

### The mental model

```text
MongoDB
  └── Deployment / Cluster
       └── Database
            └── Collection
                 └── Document
                      └── Field
```

See: [MongoDB ecosystem diagram](diagrams/01-mongodb-ecosystem.mmd)

---

# 3. 🛠️ Getting started

The source notes use this basic setup sequence:

1. Create a MongoDB cluster.
2. Install MongoDB Shell.
3. Connect to MongoDB.
4. Create/use a database.
5. Create a collection.
6. Start querying documents.

## MongoDB Shell

Check the shell installation with:

```bash
mongosh --version
```

Connect using the connection string supplied by the MongoDB deployment.

Useful command:

```javascript
show dbs
```

## Create / switch to a database

```javascript
use myDatabase
```

The notes explain that `use <databaseName>` switches to the database and a new database is created when data is subsequently stored there.

## Create a collection

```javascript
db.createCollection("product")
```

MongoDB does not require the fields to be predefined before documents are inserted.

---

# 4. 🟩 CRUD Operations

CRUD means:

- **C — Create**
- **R — Read**
- **U — Update**
- **D — Delete**

## Create — `insertOne()`

```javascript
db.product.insertOne({
  name: "Bat",
  price: 2999,
  brand: "Yonex",
  category: "Sport"
})
```

You can also insert multiple documents:

```javascript
db.product.insertMany([
  { name: "Bat", price: 2999 },
  { name: "Ball", price: 499 }
])
```

## Read — `find()`

Read all documents:

```javascript
db.product.find()
```

Read documents matching a condition:

```javascript
db.product.find({ category: "Sport" })
```

## Update — `updateMany()` + `$set`

The notes use `$set` for updating field values.

```javascript
db.product.updateMany(
  { category: "Sport" },
  { $set: { discount: 10 } }
)
```

## Delete — `deleteMany()`

```javascript
db.product.deleteMany({ category: "Sport" })
```

To target all documents, the notes show an empty condition:

```javascript
db.product.deleteMany({})
```

### `_id`

MongoDB documents use `_id` as their unique identifier. MongoDB can automatically generate it when you do not provide one.

See: [CRUD flow diagram](diagrams/03-crud-flow.mmd)

---

# 5. 🎯 Projection — choose the fields you want

Projection lets you control which fields appear in the result.

### Include fields with `1`

```javascript
db.product.find(
  { category: "Electronics" },
  { name: 1, price: 1, _id: 1 }
)
```

### Exclude fields with `0`

```javascript
db.product.find(
  { category: "Electronics" },
  { category: 0 }
)
```

### Important rule

You generally cannot mix inclusion and exclusion in the same projection document, except for `_id`.

```text
1 → include
0 → exclude
```

---

# 6. 🔎 Conditional Operators

Conditional operators are used to filter documents.

| SQL | MongoDB |
|---|---|
| `=` | `$eq` |
| `>` | `$gt` |
| `>=` | `$gte` |
| `<` | `$lt` |
| `<=` | `$lte` |

Examples:

```javascript
// Equal
{ category: { $eq: "Clothing" } }

// Less than or equal
{ price: { $lte: 1000 } }

// Greater than
{ discount_in_percentage: { $gt: 10 } }
```

For equality, the notes also show the shorter form:

```javascript
{ category: "Sport" }
```

---

# 7. ↕️ Sorting

Use `sort()` to order results.

```javascript
db.product.find().sort({ price: 1 })
```

| Value | Meaning |
|---:|---|
| `1` | Ascending |
| `-1` | Descending |

### Multiple fields

```javascript
db.product.find().sort({
  price: -1,
  name: 1
})
```

The fields are considered in the order provided.

---

# 8. 📄 Pagination — `skip()` + `limit()`

MongoDB uses:

- `skip(n)` → skip `n` documents
- `limit(m)` → return at most `m` documents

```javascript
db.product.find()
  .skip(5)
  .limit(10)
```

### SQL comparison

```text
MongoDB                  SQL
skip(5)          →       OFFSET 5
limit(10)        →       LIMIT 10
```

### Combined example

```javascript
db.product.find({
  discount_in_percentage: { $gt: 5 }
})
.sort({ price: 1 })
.skip(10)
.limit(5)
```

---

# 9. 🧠 Logical Operators

The source notes cover:

- `$and`
- `$or`
- `$not`

### `$and`

```javascript
db.product.find({
  $and: [
    { brand: "Puma" },
    { price: { $lte: 1500 } }
  ]
})
```

### `$or` inside `$and`

```javascript
db.product.find({
  $and: [
    {
      $or: [
        { brand: "Puma" },
        { brand: "Denim" }
      ]
    },
    { price: { $lte: 1500 } }
  ]
})
```

Think of it as:

```text
(Puma OR Denim) AND price <= 1500
```

---

# 10. 📊 Aggregation

Aggregation is used when you need to **process, group, filter, or calculate values across documents**.

Basic syntax:

```javascript
db.collection.aggregate([
  stage1,
  stage2
])
```

The notes cover these aggregation operators:

| Purpose | Operator |
|---|---|
| Filter documents | `$match` |
| Group and aggregate | `$group` |
| Sum | `$sum` |
| Average | `$avg` |
| Maximum | `$max` |
| Minimum | `$min` |

### `$group` example

Get the total score for each player:

```javascript
db.playerMatchDetails.aggregate([
  {
    $group: {
      _id: "$name",
      totalScore: { $sum: "$score" }
    }
  }
])
```

### `$avg` example

```javascript
db.players.aggregate([
  {
    $group: {
      _id: null,
      avgScore: { $avg: "$score" }
    }
  }
])
```

---

# 11. 🔗 Aggregation Pipeline

An aggregation pipeline is a sequence of stages.

```text
Input documents
      ↓
   Stage 1
      ↓
   Stage 2
      ↓
   Stage 3
      ↓
 Final result
```

The output of one stage becomes the input for the next stage.

Example:

```javascript
db.playerMatchDetails.aggregate([
  {
    $match: {
      year: 2021,
      matchType: "T20"
    }
  },
  {
    $group: {
      _id: "$name",
      totalSixes: { $sum: "$noOfSixes" }
    }
  },
  {
    $match: {
      totalSixes: { $gt: 5 }
    }
  }
])
```

### Read the pipeline like English

```text
1. Keep only 2021 T20 matches
          ↓
2. Group by player name
          ↓
3. Add the number of sixes
          ↓
4. Keep players with totalSixes > 5
```

See: [aggregation pipeline diagram](diagrams/05-aggregation-pipeline.mmd)

---

# 12. 🧩 Nested / Embedded Documents

MongoDB documents can contain another document inside them.

Example:

```javascript
{
  name: "Smart Watch",
  price: 22995,
  category: "Watches",
  ratings: {
    avgRating: 3.5,
    noOfUsersRated: 1000
  }
}
```

The notes refer to nested documents as **embedded documents**.

### Dot notation

Dot notation is used to access a field inside a nested document.

```javascript
db.product.find({
  "ratings.avgRating": 3.5
})
```

This means:

```text
product
  └── ratings
       └── avgRating
```

See: [document structure diagram](diagrams/06-document-structure.mmd)

---

# 13. 📦 Arrays

MongoDB documents can contain arrays.

The source notes cover:

- `$all`
- `$in`
- `$size`
- Arrays containing nested objects
- `$elemMatch`

### Example array

```javascript
{
  name: "Product A",
  colors: ["red", "blue", "black"]
}
```

### `$in`

Useful when a field can match one of several values.

```javascript
{ category: { $in: ["Sport", "Clothing"] } }
```

### `$all`

Used when an array should contain all specified values.

```javascript
{ tags: { $all: ["sports", "sale"] } }
```

### `$size`

Used to match an array by its number of elements.

```javascript
{ tags: { $size: 3 } }
```

### `$elemMatch`

The notes use `$elemMatch` when working with arrays containing nested objects and when multiple conditions need to apply to the same array element.

See: [document structure diagram](diagrams/06-document-structure.mmd)

---

# 14. 🧭 Beginner Learning Order

For a beginner, follow this order instead of trying to memorize everything at once:

```text
MongoDB terminology
        ↓
Atlas / Cluster / Shell
        ↓
Database / Collection / Document / Field
        ↓
CRUD
        ↓
Filtering
        ↓
Projection
        ↓
Conditional operators
        ↓
Sorting
        ↓
Pagination
        ↓
Logical operators
        ↓
Nested documents
        ↓
Arrays
        ↓
Aggregation
        ↓
Aggregation pipelines
```

See: [beginner learning path diagram](diagrams/02-mongodb-learning-path.mmd)

---

# 15. 🧪 Query Practice Checklist

Use this checklist while practicing:

- [ ] Create/switch to a database with `use`
- [ ] Create a collection
- [ ] Insert one document
- [ ] Insert many documents
- [ ] Read all documents with `find()`
- [ ] Read documents using a condition
- [ ] Update fields with `$set`
- [ ] Delete matching documents
- [ ] Practice `_id`
- [ ] Select only required fields using projection
- [ ] Exclude fields using projection
- [ ] Practice `$eq`, `$gt`, `$gte`, `$lt`, `$lte`
- [ ] Sort ascending and descending
- [ ] Use `skip()` and `limit()`
- [ ] Practice `$and`, `$or`, `$not`
- [ ] Create a nested document
- [ ] Query nested fields with dot notation
- [ ] Practice `$in`, `$all`, `$size`
- [ ] Practice arrays of nested objects
- [ ] Practice `$elemMatch`
- [ ] Write a simple `$group` aggregation
- [ ] Combine `$match` + `$group`
- [ ] Understand the flow of an aggregation pipeline

---

# 16. ⚡ Quick Cheat Sheet

```text
DATABASE
use myDatabase

COLLECTION
db.createCollection("product")

CREATE
db.product.insertOne({...})
db.product.insertMany([...])

READ
db.product.find()
db.product.find({field: value})

UPDATE
db.product.updateMany(
  {condition},
  {$set: {field: value}}
)

DELETE
db.product.deleteMany({condition})

PROJECTION
db.product.find({}, {name: 1, price: 1})

FILTERING
$eq  $gt  $gte  $lt  $lte

SORTING
.sort({price: 1})
.sort({price: -1})

PAGINATION
.skip(10).limit(5)

LOGICAL
$and  $or  $not

AGGREGATION
.aggregate([...])

AGGREGATION STAGES
$match  $group

AGGREGATION FUNCTIONS
$sum  $avg  $max  $min

NESTED FIELDS
"ratings.avgRating"

ARRAYS
$all  $in  $size  $elemMatch
```

---

# 17. 🧠 The one-minute mental model

If you remember only one flow, remember this:

```text
MongoDB
   ↓
Cluster / Deployment
   ↓
Database
   ↓
Collection
   ↓
Document
   ↓
Fields + Arrays + Nested Documents
   ↓
CRUD
   ↓
Queries / Operators
   ↓
Sort + Pagination + Projection
   ↓
Aggregation Pipeline
```

### For your MERN learning

Once these MongoDB fundamentals are comfortable, the next practical layer is connecting MongoDB with Node.js/Express and then using Mongoose. This README itself intentionally stays focused on the MongoDB concepts covered in the provided beginner notes.

---

## 🎨 Diagram files

Open the `.mmd` files inside `diagrams/` with a Mermaid-compatible editor/extension. They use custom `classDef` colors and a dark-friendly Mermaid theme configuration. Their node layout is intentionally spaced so Mermaid-compatible viewers can zoom/pan without one giant diagram becoming difficult to read.

| Diagram | What it teaches |
|---|---|
| [01-mongodb-ecosystem.mmd](diagrams/01-mongodb-ecosystem.mmd) | MongoDB ecosystem and hierarchy |
| [02-mongodb-learning-path.mmd](diagrams/02-mongodb-learning-path.mmd) | Beginner learning sequence |
| [03-crud-flow.mmd](diagrams/03-crud-flow.mmd) | Create → Read → Update → Delete |
| [04-query-toolkit.mmd](diagrams/04-query-toolkit.mmd) | Filtering → Projection → Operators → Sort → Pagination |
| [05-aggregation-pipeline.mmd](diagrams/05-aggregation-pipeline.mmd) | `$match` → `$group` → next stages |
| [06-document-structure.mmd](diagrams/06-document-structure.mmd) | Fields → nested documents → arrays → `$elemMatch` |

---

<div align="center">

**🍃 Learn the structure first. Then practice the queries. Then build with it.**

</div>
