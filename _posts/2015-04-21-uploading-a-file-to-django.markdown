---
layout: post
title: Uploading a file to Django
date: 2015-04-21 10:53:34 +0530
category: Django
tags:
    - Python
    - Django
author: Ajoy Oommen
published: true
---
Though I am writing this post under Django, most of it is general and can be used on any server.

Uploading a file is a two-stage process. The first step is on the browser where the user selects a file and uploads it. The upload can be easily handled with a form's `POST` method. An input with `type='file'` will provide the interface to select the file. It should be noted that the form's [enctype][1] must be `multipart/form-data`. An example:

    <form action="/attach" method="POST" enctype="multipart/form-data">
      <input type="file" name="attachment" />
      <button type="submit">Submit</button>
    </form>

When the user clicks on submit, the form is submitted with the selected file to the webserver. If your server is Django, you can easily retrieve this file as:

    attachment = request.FILES.get("attachment", None)

Once you get this you can save it locally or upload it to another server. To save the file locally, according to the [docs][2]:

    def handle_uploaded_file(f):
        with open('some/file/name.txt', 'wb+') as destination:
            for chunk in f.chunks():
                destination.write(chunk)

    handle_uploaded_file(attachment)

Or if you want to attach it to an email with Mailgun:

    files = [("attachment", attachment), ]

    def send_complex_message():
        return requests.post(
            "https://api.mailgun.net/custom/api/url",
            auth=("api", "key-7777777777777777777777777777"),
            files=files,
            data={"from": "Excited User <excited@example.com>",
                      "to": "foo@example.com",
                      "subject": "Hello",
                      "html": "<html>HTML version of the body</html>"})

[1]: http://stackoverflow.com/a/4526286/1761793
[2]: https://docs.djangoproject.com/en/1.8/topics/http/file-uploads/