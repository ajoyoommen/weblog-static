---
layout: post
title: Installing PostgreSQL
date: 2016-07-11 09:49:36 +0530
category: PostgreSQL
tags: PostgreSQL
author: Ajoy Oommen
published: true
---
These are the steps for installing PostgreSQL on an Ubuntu 14.04 server

    sudo apt-get install postgresql postgresql-contrib

    sudo su - postgres
    export PGDATA=/var/lib/postgresql/9.3/main
    /usr/lib/postgresql/9.3/bin/initdb
    exit

    sudo nano /etc/postgresql/9.3/main/pg_hba.conf # Change auth for user `postgres` to trust
    sudo service postgresql restart

    sudo su postgres
    psql
    alter user postgres with password 'secure_password';
    \q
    exit

    sudo nano /etc/postgresql/9.3/main/pg_hba.conf # Change auth for user `postgres` to md5
    sudo service postgresql restart