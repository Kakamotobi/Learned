# Redis

## Table of Contents
- [What is Redis?](#what-is-redis)
- [Getting Started](#getting-started)
  - [Installation](#installation)
  - [Starting Redis](#starting-redis)
  - [Connecting to Redis](#connecting-to-redis)
  - [Using Redis from your Application](#using-redis-from-your-application)
    - [Example with Express](#example-with-express)
- [Redis Commands](#redis-commands)

## What is Redis?
- An in-memory, key-value data store for use as database, cache, message broker, and queue.
- Since it runs on main memory, it enables millions of requests per second for real-time applications.
- It is a popular choice for caching, session management, gaming, leaderboards, real-time analytics, geospatial, ride-hailing, chat/messaging, media streaming, and pub/sub apps.
- *Redis is built on top of a traditional database. It is not meant to replace traditional databases.*
  - For example, Redis "sits in front of" MongoDB.
  - Any exhaustive queries to the DB, or data that needs to be frequently accessed and doesn't change often, are stored in Redis as well.
  - Therefore, upon the request, if the data is in Redis (up and ready), there is no need to visit the DB (find and compute).
- Important Notes
  - All Redis data resides in memory.
    - This enables low latency and high throughput data access (save a trip to disk).
    - If the system crashes, everything in Redis is lost (unless backedup consistently). Therefore, Redis is not used as a persistent database store (like MongoDB).
### Supported Data Types in Redis
- Strings, (Linked) Lists, Sets, Sorted Sets, Hashes, Bitmaps, HyperLogLogs, Streams, Geospatial, JSON.
### Key Expiration
- Set a timeout for a key (“time to live”) at which the key is automatically destroyed.
- Expirations are replicated and persisted on disk (Redis saves the date at which a key will expire).

## Getting Started
### Installation
```zsh
brew install redis
```
### Starting Redis
- Opening up access to Redis database.
#### Option 1
- Run Redis in the foreground.
```zsh
redis-server
```
#### Option 2
- Run Redis in the background.
```zsh
brew services start redis
```
- Related commands
  - Check the status.
    ```zsh
    brew services info redis
    ```
  - Stop running Redis.
    ```zsh
    brew services stop redis
    ```
### Connecting to Redis
- Open up the Redis REPL.
  ```zsh
  redis-cli
  ```
- Execute a Redis command but don't open up the REPL.
  ```zsh
  redis-cli <some-command>
  ```
### Using Redis from your Application
- Download and install a Redis client library.
  ```zsh
  npm install redis
  ```
- Create an instance of Redis.
  ```js
  import { createClient } from 'redis';

  const redisClient = createClient();

  redisClient.on('error', (err) => console.log('Redis Client Error', err));

  await redisClient.connect();

  await redisClient.set('key', 'value');
  const value = await redisClient.get('key');
  ```
#### Example with Express
```js
import express from "express";
import { createClient } from 'redis';
import axios from "axios";
import cors from "cors";

const app = express();

const redisClient = createClient();
await client.connect();

app.use(express.urlencoded({ extended: true }));
app.use(cors());

app.get("/photos", async (req, res) => {
  const albumId = req.query.albumId;
  
  // Check Redis.
  const photos = await getOrSetCache(`photos?albumId=${albumId}`, async () => {
    const { data } = await axios.get(
      "https://jsonplaceholder.typicode.com/photos",
      { params: { albumId }}
    );
    return data;
  });
  
  return res.json(photos);
});

app.get("/photos/:id", async (req, res) => {
  // Check Redis.
  const photo = await getOrSetCache(`photos:${req.params.id}`, async () => {
    const { data } = await axios.get(
      "https://jsonplaceholder.typicode.com/photos/${req.params.id}"
    );
    return data;
  });
  
  return res.json(photo);
});

function getOrSetCache(key, cb) {
  return new Promise((resolve, reject) => {
    redisClient.get(key, async (err, data) => {
      if (error) return reject(error);
      // If data in cache.
      if (data !== null) return resolve(JSON.parse(data));
      // If no data in cache.
      const freshData = await cb();
      redisClient.setex(key, 36000, JSON.stringify(freshData));
      return resolve(freshData);
    });
  }) 
}

app.listen(3000);
```

## Redis Commands
### String Commands
- **`set <key> <value>`**
  - If the key already exists, it replaces the existing value.
  - Values can be strings (incl. binary data) of every kind (Ex: jpeg image) that is less than 512 MB.
  - **`set <key> <value> nx`**
    - Don't replace the value if the key already exists.
  - **`set <key> <value> xx`**
    - Only set if the key already exists.
- **`get <key>`**
- **`getset <key> <value>`**
  - Sets a key to a new value, and returns the old value.
- **`mset <key1> <value1> <key2> <value2>`**
  - Set the value of multiple keys in a single command.
- **`mget <key1> <key2>`**
  - Get the value of multiple keys in a single command (returns an array of values).
- **`incr <key> [<value>]`**
  - Increment the value of the provided key.
  - Ex: `incr counter` adds 1 to `counter.
  - Ex: `incr counter 50` adds 50 to `counter`.
  - Similar commands: `incrby`, `decr`, `decrby`.
- **`exists <key>`**
- **`keys *`**
  - Show all keys in the Redis database.
- **`del <key>`**
- **`flushall`**
  - Delete all keys in the Redis database.
- **`expire <key> <seconds>`**
- **`ttl <key>`**
  - Check the expiration of the key.
- **`persist <key>`**
  - Remove expiration and make the key persistent forever.
### Linked List Commands
- **`lpush <value>`**
- **`rpush <value>`**
- **`lpop`**
- **`rpop`**
- **`lrange <key> <indexfrom> <indexto>`**
  - Ex: `lrange mylist 0 -1` for the whole linked list.
### Set Commands
- **`sadd <key> <values>`**
  - Add the values to the set.
- **`srem <key> <value>`**
  - Remove the value from the set.
- **`smembers <key>`**
  - Show all elements in the set.
### Hash Commands
- Hashes are key-value pairs that cannot be nested.
- **`hset <key> <field> <value>`**
- **`hget <key> [<field>]`**
- **`hgetall <key>`**
  - Get all the key(field) and value pairs of the hash.
- **`hdel <key> <field>`**
  - Delete the particular pair.
- **`hexists`**

## Reference
[Redis](https://redis.io/)  
[redis/node-redis: A high-performance Node.js Redis client.](https://github.com/redis/node-redis)  
[Redis Crash Course - YouTube](https://www.youtube.com/watch?v=jgpVdJB2sKQ&ab_channel=WebDevSimplified)  
