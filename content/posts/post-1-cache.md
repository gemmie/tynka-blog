---
title: "Cache - what is it? Part I"
description: "meta description"
date: 2022-11-02T05:00:00Z
image: "/tynka-blog/images/posts/01.jpg"
categories: ["cache", "web development", "browser"]
authors: ["Kat Tynka"]
tags: ["cache", "redis", "fastly"]
draft: false
---

Cache is a hardware or a software that is used to temporarily store data. Cached data can be reused and accessed much faster than getting it from the primary data location, thus increasing efficiency. Cache clients include web browsers, DNS servers operating systems and CPUs.

### How does it work?

When a client requests a website's resources, first they check if the resources already exist in cache.  If the data is there, that's a **cache hit** and data is returned from cache. If the data is not there, we get a **cache miss**, so the client download data from the main storage and puts in cache for later use.



### Caching strategies

But what happens when the data in primary data location changes? What happens to cache then?
Are we served out-of-date content? There are different techniques to handle such situations.

#### Cache invalidation

Cache invalidation is a process where a system marks cached entries as invalid and removes or replaces them - to ensure the client gets the latest version.

There are three approaches to cache invalidation:
1. **Write-through cache**
2. **Write-around cache**
3. **Write-back cache**


#### Cache eviction

For cache eviction, if there is still space, data will be inserted. But when the cache is full, some data needs to be evicted.
Some of the most popular eviction techniques are:
- **First In First Out (FIFO)** - the first accessed block is removed, without checking how many times the block has been accessed.
- **Last In First Out (LIFO)** - the last accessed block is removed, without checking how many times the block has been accessed.
- **Least Recently Used (LRU)** - the least recently used items are removed first.
- **Most Recently Used (MRU)** - the most recently used items are removed first.
- **Least Frequently Used (LFU)** - cache counts how often each item is used, and removes the least frequently used.
- **Random Replacement (RR)** - a random item is chosen to be evicted.

## In-memory cache

### cache-control
#### max-age
#### etag
#### vary

## Varnish, Fastly
### surrogate key


## CDN


## Redis
