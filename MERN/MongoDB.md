# MERN — MongoDB (NoSQL)

---

## MongoDB (NoSQL)

### NoSQL vs SQL
**Q: What are the primary differences between MongoDB (NoSQL) and Relational Databases (SQL)?**

| Feature       | MongoDB (NoSQL)                      | SQL (Relational)               |
|---------------|--------------------------------------|--------------------------------|
| Data model    | Flexible documents (JSON/BSON)       | Fixed schema (tables/rows)     |
| Schema        | Dynamic                              | Strict                         |
| Joins         | Embedded docs / `$lookup`            | Native JOINs                   |
| Scaling       | Horizontal (sharding)                | Vertical (more powerful server)|
| Transactions  | Supported (multi-doc, v4.0+)         | ACID by default                |
| Best for      | Unstructured/variable data, scale    | Complex relationships, integrity|

---

### Document vs Collection
**Q: Explain the difference between a Document and a Collection.**

- **Document** — a single record stored as BSON (Binary JSON). Equivalent to a row in SQL.
- **Collection** — a group of documents. Equivalent to a table in SQL.

```js
// A document (single user record)
{
  _id: ObjectId("64f1a..."),
  name: "Alice",
  email: "alice@example.com",
  age: 28
}

// A collection is named, e.g., "users", and holds many such documents
db.users.find(); // queries the "users" collection
```

---

### Object ID
**Q: What is the role of the `_id` field and ObjectId in MongoDB?**

Every document in MongoDB has a unique `_id` field. By default, MongoDB generates a 12-byte `ObjectId` automatically.

**ObjectId structure:**
- 4 bytes — Unix timestamp
- 5 bytes — random value (machine + process)
- 3 bytes — incrementing counter

```js
const { ObjectId } = require("mongodb");
const id = new ObjectId();
console.log(id.toString());        // "64f1a2b3c4d5e6f7a8b9c0d1"
console.log(id.getTimestamp());    // creation date embedded in ID

// Query by ID
db.users.findOne({ _id: new ObjectId("64f1a2b3c4d5e6f7a8b9c0d1") });
```

---

### Replica Sets
**Q: How do Replica Sets provide data redundancy and failover support?**

A **Replica Set** is a group of MongoDB instances that maintain the same data.

- **Primary** — receives all write operations.
- **Secondaries** — replicate data from the primary asynchronously.
- **Arbiter** (optional) — participates in elections but holds no data.

**Failover:** If the primary goes down, secondaries elect a new primary automatically (within seconds). Clients reconnect to the new primary seamlessly.

```
Primary  →  Secondary 1
         →  Secondary 2
```

```js
// Connection string for replica set
mongoose.connect("mongodb://host1,host2,host3/mydb?replicaSet=rs0");
```

---

### Sharding
**Q: How does sharding work to distribute data across multiple servers?**

**Sharding** is MongoDB's horizontal scaling strategy — it partitions data across multiple servers called **shards**.

**Components:**
- **Shard** — stores a subset of the data.
- **Config Server** — stores cluster metadata (which data is on which shard).
- **Mongos** — query router; directs client requests to the correct shard.

**Shard Key** — a field used to determine data distribution.

```js
// Enable sharding on a collection using "region" as the shard key
sh.shardCollection("mydb.orders", { region: 1 });
```

Data with the same shard key range lives on the same shard. A well-chosen shard key ensures even distribution and avoids "hot spots."

---

### Aggregation Framework
**Q: How do you perform complex data transformations using MongoDB's Aggregation Framework?**

The aggregation pipeline processes documents through a series of stages.

**Common stages:**
- `$match` — filter documents (like `WHERE` in SQL)
- `$group` — group and aggregate (like `GROUP BY`)
- `$project` — reshape documents (select/rename fields)
- `$sort` — sort results
- `$lookup` — join with another collection

```js
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: {
      _id: "$customerId",
      totalSpent: { $sum: "$amount" },
      orderCount: { $count: {} }
  }},
  { $sort: { totalSpent: -1 } },
  { $lookup: {
      from: "customers",
      localField: "_id",
      foreignField: "_id",
      as: "customerInfo"
  }}
]);
```

---

### Transactions
**Q: How does MongoDB handle multi-document transactions and data consistency?**

Since MongoDB 4.0, **multi-document ACID transactions** are supported across replica sets, and since 4.2, across sharded clusters.

- **Atomicity** — all operations in a transaction succeed or all are rolled back.
- **Consistency** — data remains valid before and after.
- **Isolation** — snapshot isolation (reads see a consistent view).
- **Durability** — committed data persists even after failures.

```js
const session = await mongoose.startSession();
session.startTransaction();
try {
  await Account.findByIdAndUpdate(senderId, { $inc: { balance: -100 } }, { session });
  await Account.findByIdAndUpdate(receiverId, { $inc: { balance: +100 } }, { session });
  await session.commitTransaction();
} catch (err) {
  await session.abortTransaction();
} finally {
  session.endSession();
}
```

**Note:** Transactions have higher overhead in MongoDB vs traditional SQL. Design schemas to minimise cross-document transactions by embedding related data where possible.
