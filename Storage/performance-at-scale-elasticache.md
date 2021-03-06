# **Performance at Scale with Amazon ElastiCache**

# Sections
- [**Performance at Scale with Amazon ElastiCache**](#performance-at-scale-with-amazon-elasticache)
- [Sections](#sections)
- [Overview](#overview)
  - [Architecture Overview](#architecture-overview)
  - [Alternatives to ElastiCache](#alternatives-to-elasticache)
  - [Memcached vs. Redis](#memcached-vs-redis)
- [ElastiCache for Memcached](#elasticache-for-memcached)
  - [Architecture](#architecture)
  - [Selecting the Right Cache Node Size](#selecting-the-right-cache-node-size)
  - [Security Groups and VPC](#security-groups-and-vpc)
- [Caching Design Patterns](#caching-design-patterns)
  - [How to Apply Caching](#how-to-apply-caching)
  - [Consistent Hashing (Sharding)](#consistent-hashing-sharding)
  - [Client Libraries](#client-libraries)
  - [Be Lazy](#be-lazy)
  - [Write On Through](#write-on-through)
  - [Expiration Date](#expiration-date)
  - [The Thundering Herd](#the-thundering-herd)
- [ElastiCache for Redis](#elasticache-for-redis)
  - [Architecture with ElastiCache for Redis](#architecture-with-elasticache-for-redis)
  - [Multi-AZ with Auto-Failover](#multi-az-with-auto-failover)
  - [Sharding with Redis](#sharding-with-redis)
- [Advanced Datasets with Redis](#advanced-datasets-with-redis)
  - [Game Leaderboards](#game-leaderboards)
  - [Recommendation Engines](#recommendation-engines)
  - [Chat and Messaging](#chat-and-messaging)
  - [Client Libraries and Consistent Hashing](#client-libraries-and-consistent-hashing)
- [Monitoring and Tuning](#monitoring-and-tuning)
  - [Monitoring Cache Efficiency](#monitoring-cache-efficiency)
  - [Watching for Hot Spots](#watching-for-hot-spots)
  - [Redis Backup and Restore](#redis-backup-and-restore)
- [Cluster Scaling and Auto Discovery](#cluster-scaling-and-auto-discovery)
  - [Auto Scaling Cluster Nodes](#auto-scaling-cluster-nodes)
  - [Auto Discovery of Memcached Nodes](#auto-discovery-of-memcached-nodes)
- [Conclusion](#conclusion)
- [References](#references)


# Overview
- [Source](https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf)

This summary is based off of the July 2019 revision of the **Performance at Scale with Amazon ElastiCache** whitepaper. This whitepaper highlights the [Amazon ElastiCache](https://aws.amazon.com/elasticache/) service as a tool for maintaining the performance of web applications at scale. It compares its two in-memory caching engines, [Memcached](https://memcached.org/) and [Redis](https://redis.io/), demonstrates in-memory design patterns, and explains best practices for using ElastiCache with real-world application architectures.

## Architecture Overview
ElastiCache works by deploying one or more cache clusters in the AWS cloud. Once deployed and running, the service automates administrative tasks such as resource provisioning, failure detection and recovery, and software patching.

ElastiCache is independent of the database tier, so it's possible to use it as an in-memory layer between the application(s) and one database, multiple databases, or none at all. Below displays an example architecture with one cache cluster, 3 cache nodes, and one primary [Amazon RDS](https://aws.amazon.com/rds/) database to serve applications running on [Amazon EC2](https://aws.amazon.com/ec2/).

![Architecture](../Diagrams/ElastiCacheArchitecture.png)

## Alternatives to ElastiCache

### Amazon CloudFront
- [Amazon CloudFront](https://aws.amazon.com/cloudfront/) is a content-delivery network used to cache static web content such as media files and webpages close to end users
- It's valuable for scaling a website, but isn't a replacement for ElastiCache
- It can be beneficial to use the two services together

### RDS Read Replicas
- RDS supports the ability for read replicas of the primary database
- This limits the data to a copy of the database format, and would not be a substitute for caching calculations, aggregates, or custom keys
- Read replicas are also not as fast as in-memory caches

### On-host caching (Not Recommended)
- The simplest approach is to store data on each EC2 application instance, but there are a wide host of problems with it:
  - The cache is tied to the instance's life span, reducing efficiency
    - If rebooted or replaced, the cache is emptied out
  - Cache invalidation is infeasible at scale
  - Synchronizing cache data across a fleet of servers is infeasible at scale
- In short, **don't use this approach**

## Memcached vs. Redis
Each ElastiCache cluster is selected between one of two popular open-source, in-memory key-value engines: [Memcached](https://memcached.org/) and [Redis](https://redis.io/). ElastiCache is protocol compliant with both, so existing applications that already use either caching engine will work seamlessly with the service.

<html>
    <table>
        <tr>
            <th align="center" width="400">ElastiCache for Memcached</th>
            <th align="center" width="400">ElastiCache for Redis</th>
        </tr>
        <tr>
            <td>
                <ul>
                    <li>Simple, easy-to-use caching model</li>
                    <li>Multithreaded nodes</li>
                    <li>Easily scales out by adding more nodes to the cluster</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Supports advanced data types, such as lists, hashes, and sets</li>
                    <li>Built-in disk persistence</li>
                    <li>Can run in multiple Availability Zones with failover support</li>
                    <li>Publish and subscribe capabilities</li>
                    <li>Supports multiple compliance standards (ex. PCI DSS, HIPAA, FedRAMP)</li>
                </ul>
            </td>
        </tr>
    </table>
</html>

It may be tempting to look at Redis as a more evolved Memcached due to its advanced features, but Memcached has a longer track record and the ability to leverage multiple CPU cores. The two engines are also very different in practice, so the next few sections will address each of them separately.

# ElastiCache for Memcached

## Architecture
ElastiCache for Memcached clusters sit in a separate tier alongside a database in an application architecture. ElastiCache does not directly communicate with the database, nor have any particular knowledge of the database. As requests come into the application, the application is responsible for communicating between the caching and database tiers. As more nodes are added to the cache cluster, the cache keys are distributed among them, resulting in linear scaling of the cache pool in relation to the number of its nodes.

![MemcachedArchitecture](../Diagrams/ElastiCacheMemcachedArchitecture.png)

When launching a cluster in a region, its availability zones must be specified. In the example above, we have an ElastiCache cluster spanning two availability zones.

### Best Practices
- Launch nodes across multiple availability zones to ensure high availability of the cache cluster
- Launch nodes in the same zones as application servers to achieve best performance

## Selecting the Right Cache Node Size
- To get an approximate amount of cache memory needed, multiply the size of the items (needing to be cached) by the number of items to be cached at once
- Memcached also adds 50-60 bytes of internal bookkeeping data to each element
- The cache key itself also consumes space, at 2 bytes per character
- Every cache node type and its specifications are [listed here](https://aws.amazon.com/elasticache/pricing/#Available_node_types)

### Cache Size Approximation Example
- The application is a social media website
- We want to cache user profile data, which is approximately 1KB
- We want to support at minimum 10 million profiles cached at any one time
- Our cache key per user will be 50 characters in length
- We have:
  - Raw Size per item = `1,000 bytes`
  - Cache key per item = `(50 * 2 bytes) = 100 bytes`
  - Bookkeeping per item = `50 bytes`
  - Total Size per item = `1,000 + 100 + 50 = 1,150 bytes`
- In total, we have:
  - Minumum cache memory required = `Item Size x Number of items`<br>
  `= 1,150 * 10,000,000`<br>
  `= 11,500,000,000 bytes`<br>
  `= 11.5 GB`

## Security Groups and VPC
- Like other AWS services, ElastiCache supports [security groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html) which define rules that limit access to compute instances based on IP addresses and ports
- It is a best practice to launch ElastiCache clusters into private subnets with no public connectivity for best security
- Only allow connectivity from the application tier, only the applications need  connectivity to ElastiCache
- The diagram below shows a simplified architecture diagram to achieve these practices:

![MemcachedVPC](../Diagrams/ElastiCacheMemcachedVPC.png)


# Caching Design Patterns

## How to Apply Caching
These questions can be used as a guideline to apply caching in an application:
- **Is it safe to use a cached value?**
  - The same piece of data can have different consistency requirements in different contexts
  - Ex. Price may be cached for the online checkout service, but on other pages it may be minutes out of date
- **Is caching effective for that data?**
  - Some applications have data access patterns not suitable for caching
  - Ex. A large, frequently changing dataset
- **Is the data structured well for caching?**
  - Sometimes, data is best cached in a format that combines multiple records together
  - Caching data in different formats allows it to be accessed by different attributes in the record

## Consistent Hashing (Sharding)
- A simple approach to spread cache keys across cache nodes would be to apply a hash function to the key, and use math modulo the number of cache nodes to determine which node to be cached to
- This approach has serious issues when adding more nodes to scale
  - The hash modulo of the new number of nodes will remap most cache keys to new nodes, effectively invalidating those keys
  - The number of keys remapped = `old node count / new node count`
    - Ex. A website receives heavy traffic and needs to increase 9 nodes to 10
      - `old node count / new node count = 9/10 = 90%`
      - 90% of the cache is wiped when the website needs it most, degrading performance for a while
- **Consistent Hashing** solves this problem by creating an internal hash ring with a preset number of random integers, and nodes are assigned to those random integers
  - As new nodes are added, they are slotted to a random integer on the hash ring
  - Cache keys are assigned by first finding it's closest integer on the ring, then being assigned to the node associated with that integer
- Many modern client libraries already support consistent hashing, so it likely won't need to be manually implemented

## Client Libraries
- As mentioned above, most popular client libraries for Memcached support consistent hashing
- For Java, .NET, or PHP, there are [ElastiCache clients with Auto Discovery](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Clients.html) to support auto discovery of new nodes as they are added to the cluster
  - Only works for Memcached, not Redis
  - Recommended if planning to use dynamically scaling clusters for applications with significant load fluctuation 

## Be Lazy
- **Lazy caching** is the most prevalent approach to caching, and the basic idea is to populate the cache only when an object is actually requested by the application
- The diagram below shows an example lazy caching flow for the cache key `my_cache_key`

![LazyLoading](../Diagrams/ElastiCacheLazyLoading.png)

### Advantages
- The cache only contains objects that the application actually requests, which helps keeps the cache size manageable
- As new nodes come online, the lazy caching method automaically adds objects to the nodes when the application first requests them
- Cache expiration is easily handled by simply deleting the cached object

### Best Practices
- For areas in the application where data is read often, a lazy caching strategy is a good choice
  - Ex. Personal profiles on social media apps don't change often, but are read by many users many times a day, depending on the profile
    - This is a perfect caching candidate

## Write On Through
- **Write-through caching** updates the cache in real-time as the database is updated
- The diagram below shows the same architecture as before, but now implementing the write-through strategy

![WriteThrough](../Diagrams/ElastiCacheWriteThrough.png)

### Advantages
- Avoids cache misses, which can help the app's performance
- Simplifies cache expiration since the cache is always up-to-date
- Shifts any application delay to the user updating data, mapping better to user expectations

### Best Practices
- Write-through caching should be used as a proactive measure for data that is certainly going to be accessed, or any type of aggregate
  - Ex. Top 100 game leaderboards, top 10 most popular news stories, product recommendations
- It's best to think of lazy caching as a foundation that can be used throughout the app, and write-through caching for targeted optimization for specific situations

## Expiration Date
- **Always apply a time to live (TTL)**
  - Apply a TTL to cache keys, except those updated with write-through caching
  - Catches application bugs when the cache key is forgotten to be updated or deleted when its underying record has been
    - The cache key will expire anyway and be refreshed
- **Set a short TTL for rapidly changing data**
  - Ex. comments, activity streams, leaderboards
  - If a database query is getting hammered, a TTL of a few seconds is a good hotfix to keep the application running
- **Russion doll caching pattern**
  - Nested records are managed with the ir own cache keys, and the top-level resource is a collection of those keys
    - Ex. a news webpage containing users, stories, and comments
      - Users, stories, and comments have their own cache keys
      - The webpage is the collection of those cache keys

## The Thundering Herd
- The **thundering herd effect** is what happens when a cache key is simultaneously requested by multiple application processes, missed, and then each process runs the same database query in parallel
  - Results in a heavier impact on the primary database, and the effect increases as the query becomes more expensive

### Causes
- **TTL**
  - Ex. a celebrity's profile on a social media app expires in the cache
    - Millions of users loading their profile page will suddenly hammer the database
- **Adding new nodes**
  - The new cache node's memory will be empty

### Solution
- The solution is to prewarm the cache by using these str:
  - **1.** Write an automated script to simulate requests that the application will expect
    - Cache misses will result in the cache keys filling up
  - **2.** When adding new cache nodes, the script should be run
    - Reconfigures the app to add a new node to the consistent hashing ring


# ElastiCache for Redis
Redis makes use of familiar concepts such as clusters and nodes, but has a few important differences compared to Memcached:
- Redis data structures cannot be horizontally sharded, so Redis clusters can only contain a single primary node
- Redis supports replication for both availability (failover) and to separate reads from writes (similar to RDS Read Replicas)
- Redis supports persistence, including backup and recovery

Other differences were highlighted [earlier in this summary](#memcached-vs-redis). Since Redis supports persistence, it's possible to use it as a primary data store. In practice however, long-term database solutions such as [RDS](https://aws.amazon.com/rds/) or [DynamoDB](https://aws.amazon.com/dynamodb) are a better fit for this purpose.

## Architecture with ElastiCache for Redis
ElastiCache clusters for Redis only contain a single primary node, and may have up to five read replicas that the primary nodes asynchronously writes to. An example architecture involving Redis is displayed below.

![RedisArchitecture](../Diagrams/ElastiCacheRedisArchitecture.png)
- The primary node is only used for writes, while the replicas offload the read traffic
- The primary node asynchronously updates its replicas when written to
- Each application server reads from the replica in it's own Availability Zone, for maximum performance

### Managing Replica Data
- Since replication is asynchronous, the replica data may be slightly out of date
- To decide whether data should be read from a replica, here are some questions to consider:
  - **Is the value being used only for display purposes?**
    - If so, being slightly out of date is likely not a concern
  - **Is the value going to be displayed on a screen where the user just edited it?**
    - May look like an application bug to the user
  - **Is the value being used for application logic?**
    - If so, using an old value can be risky
  - **Are multiple processes using the value simulataneously?**
    - If so, the value must be kept up-to-date, and must be read from the primary node

## Multi-AZ with Auto-Failover
During planned maintenance or an unlikely node or Availability zone failure, ElastiCache can automatically detect the failure of the primary node, select a replica, and promote it to become the new primary node. The DNS name of the new primary will be set to the same endpoint as the old primary, so no application change will be needed.

Depending on how in-sync the replica was with the previous primary, this process can take several minutes, and ElastiCache will prevent writes to the cluster until it is complete.

## Sharding with Redis
Redis has two categories of data structures:
- **1.** Simple keys and counters
- **2.** Advanced datasets such as lists, sets, and hashes

Only data of the first group can be horizontally sharded. This is done by using multiple Redis clusters, in a similar manner as using multiple Memcached nodes. Each Redis cluster would be responsible for part of the sharded dataset. Each cluster would also have its own set of replicas that could be spanned across Availability Zones. 

This is the most advanced configuration of Redis possible, and may be overkill for most applications, but allows for the potential to scale in the future if needed.


# Advanced Datasets with Redis
This section examines use cases for which ElastiCache for Redis can support.

## Game Leaderboards
- Re-sorting and re-assigning numeric positions to users on a leaderboard is computationally expensive
- Sorted sets can be useful in this case, because they simultaneously guarantee both the uniqueness and ordering of elements
- In a Redis sorted set, when an element is added, it is reranked in real-time and assigned a numeric position
- Redis sorted set commands all begin with "Z", such as [ZADD](https://redis.io/commands/zadd) 
- Below is a complete game leaderboard example:
```
> ZADD "topscores" 100 "Alice"
> ZADD "topscores" 77 "Bob"
> ZADD "topscores" 124 "Carl"
> ZADD "topscores" 30 "Dave"

> ZREVRANGE "topscores" 0 -1
1) "Carl"
2) "Alice"
3) "Bob"
4) "Dave"

> ZREVRANK "topscores" "Bob"
2

> ZADD "topscores" 99 "Fred"
(integer) 1

> ZREVRANGE "topscores" 0 -1
1) "Carl"
2) "Alice"
3) "Fred"
4) "Bob"
5) "Dave"

> ZREVRANK "topscores" "Bob"
3
```

## Recommendation Engines
- Calculating recommendations for users based on other items they've liked requires very fast access to a large dataset
- Redis data structures are a great fit for this data:
  - Counters can be used to increment or decrement the likes or dislikes for a given item
    - [INCR](https://redis.io/commands/incr) and [DECR](https://redis.io/commands/decr) are common counter commands
  - Redis hashes can maintain everyone who has liked or disliked a given item
    - [HSET](https://redis.io/commands/hset) and related hash commands are used here
- Below is an example that stores recommendation data for movies:
```
> INCR "movie:1222:likes"
> HSET "movie:1222:ratings" "Alice" 1

> INCR "movie:1222:dislikes"
> HSET "movie:1222:ratings" "Bob" -1

> HGETALL "movie:1222:ratings"
1) "Alice"
2) "1"
3) "Bob"
4) "-1"
```

## Chat and Messaging
- Redis provides pub/sub capabilities suited to in-app messaging, web chat windows, online game invites, and real-time comment streams
- [PUBLISH](https://redis.io/commands/publish), [SUBSCRIBE](https://redis.io/commands/subscribe) and related commands are used here:

```
> SUBSCRIBE "channel:main"
Reading messages...

> PUBLISH "channel:main" "Greetings!"

> Reading messages...
1) "message"
2) "channel:main"
3) "Greetings!"

> UNSUBSCRIBE "channel:main"
```
- Unlike other Redis data structures, pub/sub messaging doesn't get persisted to disk

## Client Libraries and Consistent Hashing
- Like Memcached, most popular client Redis libraries will work with ElastiCache for Redis
- Unlike Memcached, it is uncommon for Redis libraries to support [consistent hashing](#consistent-hashing-sharding)
  - Redis' advanced datasets simply cannot be sharded horizontally across multiple Redis nodes
- In general, Redis does not easily scale horizontally
- Redis can only scale up to larger node sizes so that its data structures can still function properly

# Monitoring and Tuning

## Monitoring Cache Efficiency
To recognize which metrics to monitor to understand the health and performance of ElastiCache clusters, the ElastiCache User Guide contains a list of them for each engine:
- [Memcached metrics to monitor](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/CacheMetrics.WhichShouldIMonitor.html)
- [Redis metrics to monitor](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheMetrics.WhichShouldIMonitor.html)

The most important metric to watch is CPU usage. A consistently high CPU usage implies that a node is overworked, either by too many concurrent requests, or by performing dataset operations (in the case of Redis).

In addition to CPU, here are some other metrics for monitoring cache memory utilization:
- **BytesUsedForCacheItems**
  - This value is the actual amount of cache memory being used
- **Evictions**
  - When memory starts to fill up, the unused cache keys will be evicted (deleted) to free up space
  - A large number of evictions indicates that the cache is running out of space
- **CacheMisses**
  - The number of times a key was requested, but not found in the cache
  - A large amount of CacheMisses combined with a large amount of Evictions indicates that the cache is thrashing due to lack of memory
- **SwapUsage**
  - This metric should stay at 0; there should be no swaps being performed
- **CurConnections**
  - Represents the number of clients connected to the engine
  - An increasing number of CurrConnections may indicate a problem with how the application manages connections

A well-tuned cache node will show the number of cache bytes used to be almost equal to the [maxmemory](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ParameterGroups.Redis.html#ParameterGroups.Redis.NodeSpecific) parameter in Redis, or the [max_cache_memory](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/ParameterGroups.Memcached.html#ParameterGroups.Memcached.NodeSpecific) parameter in Memcached. In steady state, cache hits should increase faster than cache misses, and there should be a low number of evictions.

## Watching for Hot Spots
**Hot spots** are nodes in the cache that receive higher load than other nodes. Uneven CPU usage among cache nodes likely indicates a hot spot. They are caused by **hot keys** which are used more frequently than other keys, usually occurring in apps of significant scale. 

In general, consistent hashing should distribute cache keys fairly evenly across nodes, but hot spots can still occur. A few hot keys may not create significant hot spots, but in extreme cases, a single hot key may overwhelm an entire cache node. There are two solutions for this:
- **1.** Create a mapping table to remap very hot keys to a separate set of cache nodes
  - There is still the challenge of scaling these new nodes, but at least the hot cache keys won't compromise the other keys
- **2.** Add a secondary layer of cache nodes in front of the main cache nodes
  - This can act as a buffer, but introduces additional latency

## Redis Backup and Restore
When [Redis Backup and Restore](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/backups.html) is enabled, ElastiCache can automatically take snapshots of the Redis cluster and save them to S3. Redis forks a background secondary process to write the backup data, so the memory that should be allocated to the node should be more than what the application's dataset actually consumes.

In a production environment, Redis backups should always be enabled and retained for a minimum of 7 days. Retaining backups provides a safety net in case an application bug corrupts the cache data.


# Cluster Scaling and Auto Discovery

## Auto Scaling Cluster Nodes
- ElastiCache currently does not support auto scaling of nodes in a cluster
- The number of cache nodes can be manually changed using the AWS console or AWS API
- As [mentioned earlier](#consistent-hashing-sharding), be aware that changing the number of cluster nodes can result in remapping cache keys and impacting performance

## Auto Discovery of Memcached Nodes
- As [mentioned earlier](#client-libraries), there are client libraries for Java, .NET, and PHP to support auto discovery of new ElastiCache Memcached nodes
- There are open-source libraries [dalli-elasticache](https://github.com/ktheory/dalli-elasticache) and [django-elasticache](https://github.com/gusdan/django-elasticache) for Ruby and Python respectively to provide auto discovery support
- Other languages will need auto discovery custom implemented, and the overall mechanism is found in the [How Auto Discovery Works](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/AutoDiscovery.HowAutoDiscoveryWorks.html) topic in the ElastiCache user guide

# Conclusion
Proper use of in-memory caching can result in an application that performs better and costs less at scale. ElastiCache allows for the easy deployment, managment, and monitoring of Memcached or Redis clusters, and the strategies in this whitepaper can increase the performance and resiliency of applications that use them.

# References
- [Whitepaper](https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf)
- [Amazon ElastiCache for Memcached User Guide](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/WhatIs.html)
- [Amazon ElastiCache for Redis User Guide](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/WhatIs.html)
