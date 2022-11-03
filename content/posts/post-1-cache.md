---
title: "Cache - a story of saving time"
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


### Caching web resources

There are different ways content can be cached. Let's walk through them.

### Browser cache (private cache)
Cache stored in browser's memory. It can be controlled through HTTP headers.

When you visit a website for the first time, the browser does not have any files cached, so it will fetch everything from the web server.
For every next visit, the browser will get the HTML page from a server and will look for the static assets - images, css files, JavaScript files - in its cache.
This helps reduce transferred data size.

#### So the HTTP headers?


There are a few headers commonly used in caching: 

* `Cache-Control` - holds instructions - in both requests and responses that control caching in browsers and shared caches (eg. Proxies, CDNs). Response instructions:
  * `private` - response can be stored only in private caches.
  * `max-age=N` - response remains fresh until N seconds after the response is generated. Tells cache that it can store the response and reuse it as long as it's fresh.
  * `no-cache` - means that the response can be stored in cache, but it must be validated with the origin server before each reuse. It does not mean `don't store`.
  * `must-revalidate` - response can be stored in cache and reused while fresh, but should validate with server if stale.
  * `no-store` - cache should not be stored for this response.
* `Etag` - entity tag, response header, an identifier for a specific resource - if the response for a URL has changes, a new ETag value must be generated.
* `Vary` - describes the parts of the request message aside from method and URL that influenced the content of the response. It takes comma separated header names as directives.


### Shared cache

Shared cache is a cache that exists between server and clients, so eg. Proxies, CDNs. It stores a single response and reuses it with multiple users.

Shared cache also works with HTTP headers. Setting the `Cache-Control` header to `public` means that the response can be stored in shared caches.

#### So what exactly is a CDN? 

**Content Delivery Network (CDN)**  is a group of geographically distributed servers, which work together to provide fast delivery of Internet content. It does not host content, but helps to cache content at the network edge, which improves website performance.

CDN pulls static content files from the origin server into the distributed CDN networks. Some CDNs have features allowing for selective caching of dynamic content.


Examples of CDN are CloudFront from AWS and Cloud CDN from Google.

#### Why use CDN?

* Improves latency:
  * **reduces physical distance**,
  * **hardware/software optimization** - SSD drives, load balancing,
  * **reduced data transfer** - minification, file compression.
* Improves reliability and redundancy:
  * **load balancing**,
  * **intelligent failover** - uninterrupted service even if some servers go offline, traffic is redistributed,
  * **anycast routing** - if an entire data server is having technical issues, anycast transfers the traffic to another available data center.
* Protects data security:
  * CDN can keep a site secured with TLS/SSL.




#### Examples

##### Varnish
**Varnish** is a caching HTTP **reverse proxy**. It can be used in front of a server and configured to cache the content. It's highly configurable, offering its own language.

After receiving a request, Varnish can decide to look for an answer in the cache, and like other caching systems does not bother backend, when the data is there. Cache hits in Varnish can take less than a millisecond to deliver.

Varnish understands the `Cache-Control` HTTP header, but in the end Varnish configuration decides about what and how long to cache, it also allows to send different `Cache-Control` header to the clients.

##### Fastly
**Fastly** is a CDN, built on top of Varnish. It allows to cache event-driven content and programmatic purges, when content changes.


Fastly offers an alternative to `Cache-Control` response header, called `Surrogate-Control`. It is proprietary to Fastly and works in a similar way to `Cache-Control`.
If both are available, Fastly will prefer `Surrogate-Control`.

Another HTTP header proprietary to Fastly is `Surrogate-Key`. When a server response has this header, Fastly indexes the response against the specific key, in addition to the regular cache key.
Surrogate keys cannot be used to find and serve content, but they allow to purge the content associated with them. Purging with a surrogate key will purge all pages associated with a key.

### Redis and the like
Redis is a key-value store, that can be used as an in-memory cache working *in front of* the server.


