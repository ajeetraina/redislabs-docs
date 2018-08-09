---
Title: RediSearch Quick Start Tutorial
description: $description
weight: $weight
alwaysopen: false
---
For this quick start, you will need the following:

-   [A Redis Enterprise Software cluster with set up already
    complete](/redis-enterprise-documentation/getting-started/quick-setup/)
-   redis-cli

### Create a new database that uses the Module

1.  Navigate to **databases** tab
2.  Click on the **+** sign, if necessary, then **create database**
3.  On the create database screen, check the box for Redis Modules and
    select the module you want to use for this database.\
    ![](/wp-content/uploads/2017/10/create_database-1.png){.alignnone
    .size-full .wp-image-30752 width="794" height="554"}
4.  Click **Show advanced options** and put **12544** for the port.
5.  Click the **Activate** button

Creating Indexes
----------------

Let's create a new index called "myidx". When you define the index, you
must pass in the structure of the data you will be adding to the index.
In this example, we have four things we input, the title, body, url, and
value. In this example, we have three TEXT and one NUMERIC values. The
title has a weight of 5.0.

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
127.0.0.1:12544> FT.CREATE myIdx SCHEMA title TEXT WEIGHT 5.0 body TEXT url TEXT value NUMERIC
```

### Add info to test index

Now add some data to this index. We will add an object which key will be
doc1 and then adds a title of "hello world", body of "my favorite
object", and url of "https://redislabs.com/" to the object as follows:

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
127.0.0.1:12544> FT.ADD myIdx doc1 1.0 FIELDS title "hello world" body "My first object" url "https://redislabs.com/"
OK
```

### Search the Index

Do a search on this index for any object with the word "first":

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
127.0.0.1:12544> FT.SEARCH myIdx "first" LIMIT 0 10
1) (integer) 1
2) "doc1"
3) 1) "title"
2) "hello world"
3) "body"
4) "My first object"
5) "url"
6) "https://redislabs.com/"
```

### Drop the Index

Now that we are done with it, we can drop the index.

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
127.0.0.1:12544> FT.DROP myIdx
OK
```

### Auto-Complete and Search Engine Suggestions

Let's add a suggestion for the search engine to use

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
127.0.0.1:12544> FT.SUGADD autocomplete "hello world" 100
"(integer)" 1
```

Make sure the suggestion is there:

``` {style="border: 2px solid #ddd; background-color: #333; color: #fff; padding: 10px; -webkit-font-smoothing: auto;"}
127.0.0.1:12544> FT.SUGGET autocomplete "he"
1) "hello world"
```