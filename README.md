# DB Assignment 3

## Made by:
- Chris Rosendorf
- Viktor Kim Christiansen
- William Pfaffe

## Queries:

### 1:
```
db.getCollection("tweets").distinct("user").length 
```
### 2:
```
aggregate([
            {
                $group: {
                    _id: '$user', 
                    count: {
                        $sum: 1
                    }
                }
            }, 
            {
                $sort: {
                    count: -1
                }
            },
            {
                $limit: 10
            }
        ])

        
```
### 3:
```
Five Most Grumpy
aggregate([
            {
                $match: {
                     polarity: 0
                }
            },
            {
                $group: {
                    _id: "$user",
                    count: {
                        $sum: 1
                    }
                }
            },
            {
                $project: {
                    count: "$count",
                    polarity: {
                        $literal: "0"
                    }
                }
            },
            {
                $sort: {
                    count: -1
                }
            },
            {
                $limit: 5
            }
        ]);

Five Most Happy

aggregate([
            {
                $match: {
                     polarity: 4 
                }
            },
            {
                $group: {
                    _id: "$user",
                    count: {
                        $sum: 1
                    }
                }
            },
            {
                $project: {
                    count: "$count",
                    polarity: {
                        $literal: "4"
                    }
                }
            },
            {
                $sort: {
                    count: -1
                }
            },
            {
                $limit: 5
            }
        ]);
```
### 4:
```
aggregate([
            {
                $match: {
                    text: /@\w+/
                }
            }, 
            {
                $group: {
                    _id: '$user', 
                    count: {
                        $sum: 1
                    }
                }
            }, 
            {
                $sort: {
                    count: -1
                }
            },
            {
                $limit: 10
            }
        ])
```
### 5:
```
aggregate([
    {'$match':{'text':{'$regex':"@\w+"}}}, 
    {'$addFields': {"mentions":1}}, 
    {'$group':{"_id":"$user", "mentions":{'$sum':1}}}, 
    {'$sort':{"mentions":-1}}, 
    {'$limit':5}
    ])
```


### #1
- Query 1
    - Yes it does solve the problem. Using distinct (https://docs.mongodb.com/manual/reference/method/db.collection.distinct/) it returns all unique elements in the collection, thus gives the right result.'
    - Using the MongoDB Connector developed by MongoDB (https://www.npmjs.com/package/mongodb), we successfully connected and were able to extract unique users using the distinct function.
    - Using the official MongoDB connector instead of Mongoose.
    - Not really.
- Query 2
    - Yes it does solve the issue. It searches for all users (Since each row is one post given the username) and groups them up by username. Using the operator $sort, we can reverse the sorting order since we want the most first. operator $limit limits the post amounts to the given number of 10.
    - Using the MongoDB Connector developed by MongoDB (https://www.npmjs.com/package/mongodb), we successfully connected and were able to extract the top 10 users.
    - Using the official MongoDB connector instead of Mongoose.
    - Not really.
- Query 3
    - Yes it does solve the issue. The results and queries have been split into 2, in accordance to the order of the project assignment info (Grumpy first, happy afterwards).
    - Using the MongoDB Connector developed by MongoDB (https://www.npmjs.com/package/mongodb), we successfully connected and were able to extract the top 5 of each the most happy and most grumpy users.
    - Using the official MongoDB connector instead of Mongoose.
    - Not really.
- Query 4
    - Yes it does solve the issue. 
    - Using the MongoDB Connector developed by MongoDB (https://www.npmjs.com/package/mongodb), we successfully connected and were able to extract the top 10 of the users who linked the most users.
    - Using the official MongoDB connector instead of Mongoose.
    - Not really.
- Query 5
    - Yes it does solve the issue.
    - Using the MongoDB Connector developed by MongoDB (https://www.npmjs.com/package/mongodb), we successfully connected and were able to extract the top 5 of most mentioned Twitter users.
    - Using the official MongoDB connector instead of Mongoose.
    - Not really.

### #2
|  Solutions |  Atomicity | Sharding  |  Indexes | Large Number of Collections  |  Collection Contains Large Number of Small Documents |
|---|---|---|---|---|---|
| Arrays of Ancestors  |X|   | X  | X  |   |
|  Materialized paths |   |  X |   | X  |  X |
|  Nested sets |  X | X  | X  |   |   |

Arrays of Ancestors, see https://docs.mongodb.com/manual/tutorial/model-tree-structures-with-ancestors-array/
- Atomicity
    - Because, when inserting into the collection, only the single document is changed/created, the operation is atomic.
- Indexes
    - One can index the parents on the specific document.
- Large Number of Collections
    - You get a large collection, as each document must refer to all the ancestors.


Materialized paths, see https://docs.mongodb.com/manual/tutorial/model-tree-structures-with-materialized-paths
- Sharding
    - Because you can get the partial path to a document.
- Large Number of Collections
    - You still store the path in each document.
- Collection Contains Large Number of Small Documents
    - Since they are split up, as smaller documents instead of one big, you will naturally have many smaller documents.


Nested sets, https://docs.mongodb.com/manual/tutorial/model-tree-structures-with-nested-sets/
- Atomicity
    - Because, when inserting into the collection, only the single document is changed/created, the operation is atomic.
- Sharding
    - The different categories can still be sharded
- Indexes
    - You can add indexes to the different categories, since they are “hard 
