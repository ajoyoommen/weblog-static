---
layout: post
title: Useful EMACS shortcuts
date: 2015-02-21 04:20:01 +0530
category: Editors
tags:
    - EMACS
author: Ajoy Oommen
published: true
---
Some very useful shorcuts

    C-x C-f # Open a file
    C-x k   # Close buffer
    C-x C-s # Save single file
    C-x C-c # Exit

    C-x b   # Cycle buffers

    M-x untabify # Remove all tab characters in the selected region

**Encoding**

    C-x RET f unix # Set encoding to unix

**Selection and editting**

    C-spc direction   # Start selection
    C-w   # Cut
    M-w   # Copy
    C-y   # Paste

    M-x replace-string  # Replace all occurences of a string
    M-x query-replace   # Replace matched strings one-by-one

    C-q C-j # Insert a newline in the search or replace string

**Rectangles**

    C-x r t   # Set text in the selected rectangle
    C-x r d   # Delete text in the selected rectangle
    C-x r k   # Cut the rectangular region
    C-x r M-w # Copy the rectangular region
    C-x r y   # Paste the rectangular region (yank)

**Webmode shortcuts**

    C-c C-i    # Indent all code
    C-c C-f    # Toggle folding

**Keyboard Macro**

Eg: [Appending characters to the end of each line in Emacs](http://stackoverflow.com/a/4870840/1761793)

    C-x ( C-e a C-n C-x )