---
layout: post
title: Provisioning an ubuntu server for Django, Postgres, NGINX
date: 2017-02-27 06:29:05 +0530
category: NGINX
tags:
    - PostgreSQL
    - Ubuntu
    - AWS
    - AWS EC2
    - NGINX
    - Django
author: Ajoy Oommen
published: true
---
1. Install `virtualenv`

        sudo apt-get install python-pip
        sudo pip install virtualenv

2. Install NGINX

        sudo apt-get install nginx
        sudo rm /etc/nginx/sites-available/default
        sudo rm /etc/nginx/sites-enabled/default

3. Generate SSH public key

        ssh-keygen
        cat .ssh/id_rsa.pub

**Install and configure PostgreSQL**

1. Install Postgres

        sudo apt-get install postgresql

2. Change postgres auth to `trust`.

        sudo nano /etc/postgresql/9.5/main/pg_hba.conf

    Modify this line

        local   all             postgres                                peer

    to

        local   all             postgres                                trust

    Restart postgres

        sudo service postgresql restart

3. Set password for user postgres

        psql -U postgres
        alter user postgres with password 's3cretPass';
        \q

4. Change auth for postgres to `md5`

        sudo nano /etc/postgresql/9.5/main/pg_hba.conf

    Modify

        local   all             postgres                                trust

    to

        local   all             postgres                                md5

        sudo service postgresql restart


**Configure extra packages**

1. Install `libpq-dev` (for psycopg2)

        sudo apt-get install libpq-dev