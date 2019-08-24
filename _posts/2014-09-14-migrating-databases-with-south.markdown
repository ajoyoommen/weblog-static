---
layout: post
title: Migrating databases with South
date: 2014-09-14 17:15:12 +0530
category: Django
tags: South Python Database Django
author: Ajoy Oommen
published: true
---
Django is a powerful framework. What it lacks is database synchronisation when models are changed. This is done by South.

The advantage of South is that it can do the necessary modifications easily; no need for complex configurations. When I started with South, I found its documentation unnecessarily complex. This is how I use South with my Django apps.

For a new project, after adding south to your INSTALLED_APPS, run `syncdb`

This will create South’s database. When South is managing an app, it will create a directory called ‘migrations’ in the app’s directory.

To create this directory and tell South to write an initial state for the models, run

    ./manage.py schemamigration appname –initial

You can now run

    ./manage.py migrate appname

After any modification to the models, run

    ./manage.py schemamigration appname –auto , and then
    ./manage.py migrate appname

This is the general process. Tell south to create a migration file with schemamigration and then run the migration with migrate . Therefore, if you are working with different databases on your development as well as production servers, your work will be better if you add the migrations to git. The advantage is that after you create the migrations on your development server, and commit, push and pull them into your production server, you just have to run migrate and the database will be altered.

Those are the basics. South can do a lot more. When you run schemamigration –initial, south creates a migration 0001. When you make another change and create a migration with –auto, south numbers it 0002, and so on.

If you now run migrate appname 0001, South will run the migration and modify your database. Since you had run initial, 0001 will contain data to create tables reflecting your model.

Now, after migration 0001, if you run migrate appname 0002, South will run migrations that update the changes you introduced after 0001.

And the best part is that, if, in case you want to avoid unwanted migrations, you want to undo any database changes or delete the database, you can also run migrations in reverse.

So, if your database is currently synced to 0002, you can undo just 0002 by

    ./manage.py migrate appname 0001

If you want to delete all tables for appname, run

    ./manage.py migrate appname zero