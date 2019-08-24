---
layout: post
title: Postgres backup and recovery
date: 2015-04-20 15:43:41 +0530
category: PostgreSQL
tags: Database PostgreSQL
author: Ajoy Oommen
published: true
---
It is very easy to backup a postgres database and restore it. The commands used are [pg_dump][1] and [pg_restore][2]. A backup can be created in 4 formats:

1. Plain SQL script file
2. Custom-format
3. Directory-format
4. Tar-format archive

I usually prefer the custom-format as it *"is the most flexible output format"* and *"is also compressed by default"*. The commands are:

    pg_dump -U postgres -W dbname -Fc -f filename.db.backup

This file can be restore to a new database with:

    pg_restore -U postgres -Fc -d dbname filename.db.backup


[1]: http://www.postgresql.org/docs/9.4/static/app-pgdump.html
[2]: http://www.postgresql.org/docs/9.4/static/app-pgrestore.html