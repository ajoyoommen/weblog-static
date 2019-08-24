---
layout: post
title: Forwarding port to a PostgreSQL server
date: 2014-09-04 17:07:16 +0530
category: PostgreSQL
tags: SSH forwarding Port PostgreSQL AWS EC2
author: Ajoy Oommen
published: true
---
I have a postgres database installed on an EBS drive in EC2. Recently I had to access this DB from the local machine. These are my setup notes. (There is nothing EC2 specific here. Except perhaps that you should ensure SSH access is allowed in your security group and that you have your secret key file.)

Ensure that your user postgres has permissions to access the database. These permissions can be added in pg_hba.conf.

Now you can forward a port, say 54320, on your client, to the port postgres is listening to on your EC2 instance. Like this

    ssh -L 54320:localhost:5432 -i key.pem ec2-user@host.ip.address

What -L does is that it listens on 54320. When a connection arrives on the port, SSH forwards it to localhost:5432 on host.ip.address as user ec2-user.

So now you can connect to your database from your local machine:

    psql -U postgres dbname -p 54320 -h localhost