- https://github.blog/2023-02-06-the-technology-behind-githubs-new-code-search/

- Summary:
    - Own search engine from scratch, in Rust, specifically for the domain of code search.
    - Motivation to build from scratch as general text search products are inefficient to power **_code_** search. The user experience is poor, indexing is slow, and it’s expensive to host. There are some newer, code-specific open source projects out there, but they definitely don’t work at GitHub’s scale.
    - Interesting **Scale** point: `GitHub’s scale is truly a unique challenge. When we first deployed Elasticsearch, it took months to index all of the code on GitHub (about 8 million repositories at the time). Today, that number is north of 200 million, and that code isn’t static: it’s constantly changing and that’s quite challenging for search engines to handle. For the beta, you can currently search almost 45 million repositories, representing 115 TB of code and 15.5 billion documents.`
    - Example on how throwing resources to a problem doesn't always work: `[ripgrep](https://github.com/BurntSushi/ripgrep) on that 115 TB of content. On a machine with an eight core Intel CPU, ripgrep can run an exhaustive regular expression query on a 13 GB file cached in memory in 2.769 seconds, or about 0.6 GB/sec/core. This really isn’t going to work for the larger amount of data we have. Code search runs on 64 core, 32 machine clusters. Even if we managed to put 115 TB of code in memory and assume we can perfectly parallelize the work, we’re going to saturate 2,048 CPU cores for 96 seconds to serve a single query! Only that one query can run. Everybody else has to get in line. This comes out to a whopping 0.01 queries per second (QPS) and good luck doubling your QPS—that’s going to be a fun conversation with leadership about your infrastructure bill.
    - ==Indexing to rescue as always:== 
       - Precomputing informations such as maps from keys to sorted lists of document IDs (called “posting lists”) where that key appears. Scan each document to detect what programming language it’s written in, assign a document ID, and then create an inverted index where language is the key and the value is a posting list of document IDs. Example: ![[Pasted image 20230221165405.png]]   

    - ==Code Search==: Special type of inverted index called an [ngram](https://en.wikipedia.org/wiki/N-gram) index, which is useful for looking up substrings of content.

# Indexing 45 million repositories
- Utilise Git’s use of [content addressable hashing](https://en.wikipedia.org/wiki/Content-addressable_storage) and the fact that there’s actually quite a lot of duplicate content on GitHub. 
- **Shard by Git blob object ID** which gives us a nice way of evenly distributing documents between the shards while avoiding any duplication. There won’t be any hot servers due to special repositories and we can easily scale the number of shards as necessary.
- **Model the index as a tree** and use delta encoding to reduce the amount of crawling and to optimize the metadata in our index. For us, metadata are things like the list of locations where a document appears (which path, branch, and repository) and information about those objects (repository name, owner, visibility, etc.). This data can be quite large for popular content.
- **Eventual consistency followed:** Query results are consistent on a commit-level basis. If someone is pushing code while another person is searching indexes will point to prior codebase only. Results shouldn’t include documents from the new commit until it has been fully processed by the system.

# Building the index
![[Pasted image 20230221174325.png]]
- Based on events such as `git push` 

# Querying the data
- ![[Pasted image 20230221174445.png]]
- Blackbird query service where we parse the query into an abstract syntax tree and then rewrite it, resolving things like languages to their canonical [Linguist](https://github.com/github/linguist) language ID and tagging on extra clauses for permissions and scopes.
- Fan out and send _n_ concurrent requests: one to each shard in the search cluster. Query should go to each shard as sharded by Git blob SHA Ids.
- On each individual shard, further conversion of the query in order to lookup information in the indices. Like creating `ngram`  into substring queries.

# High availability and low latency
- p99 response times from individual shards are on the order of 100 ms, but total response times are a bit longer due to aggregating responses, checking permissions, and things like syntax highlighting. A query ties up a single CPU core on the index server for that 100 ms, so our 64 core hosts have an upper bound of something like 640 queries per second. Compared to the grep approach (0.01 QPS), that’s screaming fast with plenty of room for simultaneous user queries and future growth.