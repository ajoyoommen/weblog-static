---
layout: post
title: PostgreSQL module ltree
date: 2015-04-02 06:02:14 +0530
category: PostgreSQL
tags: Ltree PostgreSQL
author: Ajoy Oommen
published: true
---
**This is a work (post) in progres.....**

Dealing with hierarchies is a really important skill. They crop up everywhere. There are many ways to represent trees in databases. There is the **adjacency model**, the **materialized path** and **nested sets**. I have used the adjacency model quite a lot before, but they are not good if your tree is huge. A single query won't be enough to construct a whole tree. Recently I had to query from a table that uses [Ltree](http://www.postgresql.org/docs/9.4/static/ltree.html).

`ltree` is a sort of materialized path. It is a data type for your column that will have a materialized path for each element in your tree. Consider a hierarchy of geographical places. A place can be child of another place.

* Continent A
    * Country A
        * State A
        * State B
    * Country B
* Continent B

We now assign them unique IDs and a materialized `parent_path`:

* Continent A
    ID: `1`
parent_id: `null`
parent_path: `root`

* Country A
    ID: `2`
parent_id: `1`
parent_path: `root.1`

* State A
    ID: `3`
parent_id: `2`
parent_path: `root.1`

* State B
    ID: `4`
parent_id: `null`
parent_path: `root`

* Country B
    ID: `5`
parent_id: `1`
parent_path: `root`

* Continent B
    ID: `6`
parent_id: `null`
parent_path: `root`