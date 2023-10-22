---
title: Running Emacs in server mode
date: 2023-10-22 01:40:00 0300
categories: [IDE, Emacs]
tags: [emacs, ide]
---

## Intro

I've been an [doom-]Emacs user for quite some time now and I love it to the point that there's no way back to any other IDE (if we can call Emacs an IDE). However, when using it on Mac, I've found a few issues.

First and foremost, when you install Emacs via homebrew, it is not available as an "Application", so you can't just hit CMD-Space and look for it. Usually you open it from a terminal. This is where sometimes I had problems since when you do so, it blocks your Terminal until Emacs is closed. 

I found myself sometimes hitting CTRL-c on the Terminal instead of on Emacs, which closed my entire Emacs application. Frustrating!

## Emacs server

It turns out you can run Emacs as a daemon and have multiple instances of Emacs clients connecting to it and the benefits to it are several. First of all, stupid mistakes like the one I was making quite often are avoided. Also, by having several Emacs clients sharing the same context, you can share other things such as command history and much more, as stated by the Emacs official documentation:

> Various programs can invoke your choice of editor to edit a particular piece of text. For instance, version control programs invoke an editor to enter version control logs (see Version Control), and the Unix mail utility invokes an editor to enter a message to send. By convention, your choice of editor is specified by the environment variable EDITOR. If you set EDITOR to ‘emacs’, Emacs would be invoked, but in an inconvenient way—by starting a new Emacs process. This is inconvenient because the new Emacs process doesn’t share buffers, a command history, or other kinds of information with any existing Emacs process.

## Starting server and clients

To start a new Emacs server, it is as easy as `emacs --daemon`. This will create a new Emacs process running in server mode and won't block your terminal.

Then, to connect to it, just create a new client:

```
emacsclient -r -n
```

I'm often using the -r and -n options, which do the following:

- -r: Reuse existing frame if available, otherwise creates a new one.
- -n: No-wait, which basically means that it won't block your terminal while Emacs is running, but rather give it back to you right away.

You can start the server in other ways, such as by typing `M-x server-start` or even add the Emacs server to your system initialization. 

For more information, I recommend checking the [Emacs official documentation](https://www.gnu.org/software/emacs/manual/html_node/emacs/Emacs-Server.html#:~:text=Run%20the%20command%20server%2Dstart,dies%20with%20the%20Emacs%20process.)
