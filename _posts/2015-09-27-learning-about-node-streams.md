---

layout:     post
title:      Learning About Node Streams
date:       2015-09-27 01:00:00
summary: 	Recently, I was working on my Give Me The Time project and ran into an error when...

---

The purpose of this blog post is to explain what a stream is in the simplest way
possible. There are already many articles and videos online about this topic;
however, I wanted to explain it in the simplest way that I could.
This is how I would explain Node streams to a five year old.

**ELI5**: In essence, a stream connects programs together. Streams can be read and written to.
These programs are small and specialized to do a single thing.

Recently, I was working on my Give Me The Time project and ran into
an error when working on my Gulp file. The error looked something like
this:

		stream.js:94
      		throw er; // Unhandled stream error in pipe.
            	    	^

I have to admit, when I first ran into this error. I had no idea what a "stream"
was. I did what any developer should do, turn to Google.

My favorite search pattern of all time:

		ELI5 <insert topic here>

This time it was:

		ELI5 node streams

ELI5 = Explain Like I'm 5

The first result was a Reddit thread suggesting to install the nodeschool program
and complete the exercises in the stream-adventure series.

Before installing the Stream Adventure, I started to read the Node.js documentation which states:

> A stream is an abstract interface implemented by various objects in Node.js. For example a request to an  HTTP server is a stream, as is stdout. Streams are readable, writable, or both. All streams are instances of EventEmitter.

What does this even mean? In essence, a stream is something that allows you to connect
other objects, or programs, together. You take some input and then _stream_ it,
or pass it into another program. I like the plumber analogy that has been around
for a long time. Connect a bunch of tiny pipes (programs) that do a specific thing
really well to achieve a specific end goal.

The piping system has been around since the UNIX days; you read this article for more information:
[Unix Pipelines](https://en.wikipedia.org/wiki/Pipeline_(Unix))

Streams can be: readable, writable and duplex.

**Readable**: Read a file (Input)

**Writable**: Write a file (Output)

**Duplex**: Read and write a file (Input/Output)

Max Ogden goes into more detail in his blog post, which you can find below.

For now, let's make a file that takes input and prints it out into the terminal.

		//  The process object will take any input that it gets and print it out as an Output
		//  This code was stolen from Hack Reactor's video which can be found below.
		//  Oh and also taken from nodeschool's Stream Adventure exercise.
		//  (also linked below)

		process.stdin.pipe(process.stdout);


This is what it looks like when the file is running (via the node command):

![The console's output](http://i.imgur.com/XOCzFE2.png)

Let's get even more advanced and take a look at the fs, or File System, module.
The fs module allows you to read, write or even change permission levels of file(s)
and directories. Very nice!

		fs.createReadStream(path[, options])

We are going to write a program that read the content of a hello.txt file which
contains the following word:

		hello

Here's a Node program that reads the file and, when executed, spits it out
into the terminal.

		//get node's file system module

		var fs = require('fs');

		//  Next, let's assign the reading stream, which will read the hello.txt file.
		//  The first arguement takes a string, a file.
		//  We are going to pass an option, which is an object, and set the encoding
		//  to UTF-8, just in case.

		var file = fs.createReadStream('hello.txt', {encoding:'utf8'})

		// Since a stream is an EventEmitter, we can attach an event listener
		// to listen and run when the file is opened. The 'open' event can also
		// be accessed by calling the .open() method on the fs object.

		file.on('open', function() {

			// We are going to pipe, or connect to the process object and dump
			// the file's content into the terminal. Process is a global object
			// in Node that is also an EventEmitter

			this.pipe(process.stdout);

		});

Just a simple program but hope that it makes sense now. As you can see, we used
the pipe() method take the input from the file system module (fs) and passed it
into the process object. The process object then just prints it out into the
terminal.

After going through some of the exercises via nodeschool's Stream Adventure, I will write
another blog post and see if I can explain a little bit more about how streams work.
I recommend doing some of the exercises, they are fun and you can learn a lot about
streams. Remember, practice makes perfect!

**Disclaimer**: If you find any errors in this blog post, feel free to send me
an email at kevin@tokyoincode.com. Questions? Send them as well!

##References

[Hack Reactor's Video About Node Streams](https://www.youtube.com/watch?v=OeqnIuTMod4)

[Stream (Node.js)](https://nodejs.org/api/stream.html)

[File System (Node.js)](https://nodejs.org/api/fs.html)

[Node Streams Article by Max Ogden](http://maxogden.com/node-streams.html)
