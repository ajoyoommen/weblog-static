---
layout: post
title: Using Signals in Django
date: 2016-07-16 04:44:41 +0530
category: Django
tags: Signals Django
author: Ajoy Oommen
published: true
---
Signals are a great design pattern to connect actions within your project to each other. The concept is as simple as real world signals and receivers.

To understand signals in Django, you only need to go through the documentation. This post is based on [Signals in Django 1.9](https://docs.djangoproject.com/en/1.9/topics/signals/). Signals have these basic elements:

* *Sender* - Sends signals. Any method can be a sender by sending a Signal with the required arguments.
* *Signal* - The actual information instance. A Signal is a type of information that any sender can send and any receiver can receive.
* *Receiver* - Receives a particular signal sent by a sender. A receiver can only receive signal when sent by a Signal it is connected to.

## Basic signal configuration

Define your receivers (that connect to and handle your signals) in `signals.py` in your app module. For example, from the docs:

    # myapp/signals.py
    from django.db.models.signals import pre_save
    from django.dispatch import receiver
    from myapp.models import MyModel


    @receiver(pre_save, sender=MyModel)
    def my_handler(sender, **kwargs):
        ...

Add a [ready](https://docs.djangoproject.com/en/1.9/ref/applications/#django.apps.AppConfig.ready) method to your `apps.py` in the app module

    # myapp/apps.py
    from __future__ import unicode_literals 
                                            
    from django.apps import AppConfig       
                                            
                                            
    class MyAppConfig(AppConfig):       
        name = 'myapp'                  
                                            
        def ready(self):                    
            import signals  # noqa

Ensure default_app_config is set in `__init__.py`

    # myapp/__init__.py
    default_app_config = 'myapp.apps.MyAppConfig'

## Signals defined and sent by Django

Django, by default, provides and sends some Signals. These include many Model Signals like `pre_save`, `post_save`, etc. View the [Signals Reference](https://docs.djangoproject.com/en/1.9/ref/signals/) for more details.

The configuration above uses a pre-defined signal sent by Django. To connect to these signals, you need to add receivers in your `signals.py` as shown above.

## Defining a custom Signal and sending it

To define a Signal, all you need to do is this:

    import django.dispatch

    pizza_done = django.dispatch.Signal(providing_args=["toppings", "size"])

To send a signal, you can do the following:

    def send_pizza(self, toppings, size):
        pizza_done.send(sender=self.__class__, toppings=toppings, size=size)
        ...