+++
title = "Redis and Memcached"
author = ["Dony Ridani"]
date = 2023-09-17T00:00:00+08:00
lastmod = 2023-09-17T17:56:09+08:00
tags = ["redis", "memcached"]
draft = false
+++

## Main difference {#main-difference}

| Redis                                                                       | Memcached                                             |
|-----------------------------------------------------------------------------|-------------------------------------------------------|
| in-memory (except OOM or AOF/RDB enabled) data store and it is not volatile | in-memory cache and it is volatile                    |
| has data types                                                              | no data types                                         |
| keys with a maximum size of 512MB and also values up to 512MB               | keys with a maximum size of 250B and values up to 1MB |


## How Redis achieves persistence {#how-redis-achieves-persistence}

Redis supports persistence, thus it’s called a data store, in two different ways:

-   RDB snapshot: Is a point-in-time snapshot of all your dataset, that is stored in a file in disk and performed at specified intervals. This way, the dataset can be restored on startup.

-   AOF log: Is an Append Only File log of all the write commands performed in the Redis server. This file is also stored in disk, so by re-running all the commands in their order, a dataset can be restored on startup.

These files are handled by a child process and this is a key factor in deciding which kind of persistence to use.

-   If the dataset stored in Redis is too big, the RDB file will take some time to be created, which has an impact on the response time. On the other hand, it will be faster to load on boot up compared to the AOF log.

-   The AOF log is better if data loss is not acceptable at all, as it can be updated at every command. It also has no corruption issues since it's an append-only file. However, it can grow much larger than an RDB snapshot


## How to Choose {#how-to-choose}

Just it's important to consider its pros and cons right from the beginning to avoid changes and migrations during the project.

[Ref:Redis vs Memcached: which one to choose?](https://www.imaginarycloud.com/blog/redis-vs-memcached/#:~:text=While%20Redis%20is%20an%20in,the%20memory%20limit%20is%20reached)
