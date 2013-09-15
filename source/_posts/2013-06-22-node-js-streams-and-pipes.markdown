---
author: sunilkumarn
comments: true
date: 2013-06-22 18:57:26+00:00
layout: post
slug: node-js-streams-and-pipes
title: 'Node.js: Streams and pipes.'
wordpress_id: 1156
categories:
- Node.js
---

Node.js is used for building a lot of network applications and there's a lot of data being passed around. This could well be huge in size. In node, all this data is processed the moment its received, piece by piece. This is done with the help of streams. Here we discuss the usage of streams by writing a small node script that handles file upload.

Here's the actual piece of code that handles a file upload and responds back to the client with the progress of the upload.

{% highlight javascript %}
  var http = require('http'),
  sys = require('sys'),
  fs = require('fs');

  var server = http.createServer();
  console.log("Starting up the server");
  server.listen(8000);

  server.on('request', function(request, response) {
    var file = fs.createWriteStream('copy.csv');
    var fileSize = request.headers['content-length'];
    var uploadedSize = 0;

    request.on('data', function (chunk) {
      uploadedSize += chunk.length;
      uploadProgress = (uploadedSize/fileSize) * 100;
      response.write(Math.round(uploadProgress) + "%" + " uploaded\n" );
      var bufferStore = file.write(chunk);
      if(bufferStore == false)
        request.pause();
    });

    file.on('drain', function() {
      request.resume();
    })

    request.on('end', function() {
      response.write('Upload done!');
      response.end();
    })

  });

{% endhighlight %}

The basics: We create a node server that listens on port 8000. Upon receival of a request, we create a write stream ( the destination file path ). Each chunk of data received is written on to the destination path, the upload progress is calculated and responded back.

Lets break up the above snippet into pieces and make an analysis of whats happening.

A writeStream is created and 'copy.csv' is the destination path to which the received data will be written.

{% highlight javascript %}
  var file = fs.createWriteStream('copy.csv');
{% endhighlight %}

The following piece forms the core of the upload process.

{% highlight javascript %}
  request.on('data', function (chunk) {
    var bufferStore = file.write(chunk);
    if(bufferStore == false)
      request.pause();
    uploadedSize += chunk.length;
    uploadProgress = (uploadedSize/fileSize) * 100;
    response.write(Math.round(uploadProgress) + "%" + " uploaded\n" );
  });

  file.on('drain', function() {
    request.resume();
  })
{% endhighlight %}

Looking at the code - on receiving each chunk of data ( via the read stream ), its written to the write stream as
`file.write(chunk);`

Right now, we need to pause a bit to check whether there might be a cause of worry in this whole read-write streaming process. The answer is yes, and is very obvious. **There exists a real possibility that the rate at which the data is written to the writeStream is less than the rate at which its read from the readStream**. This is a genuine cause of concern and hence cannot be ignored. How we handle this forms our next two lines of code.

`file.write(chunk)` stores the data onto a buffer. It returns true if the write was performed and returns false if the write failed due to the buffer being full. So, we need to handle this by pausing the readStream if the buffer storage is full.

{% highlight javascript %}
  var bufferStore = file.write(chunk);
  if(bufferStore == false)
    request.pause();
{% endhighlight %}

Also, we need to re-start streaming data from the read stream once the buffer is drained out. The following lines of code does just that.

{% highlight javascript %}
  file.on('drain', function() {
    request.resume();
  })
{% endhighlight %}

**Pipes in node**: Here, we have handled the logic of keeping the read - write rate to be in sync. Node.js provides us with pipes which has this logic already encapsulated in it.

The following line,

{% highlight javascript %}
request.pipe(file) // The notion is quite similar to UNIX pipes. Pipes the input into an output stream.

{% endhighlight %}

would be equivalent to

{% highlight javascript %}
  request.on('data', function(chunk) {
    var bufferStore = file.write(chunk);
    if(bufferStore == false)
      request.pause();
  })

  file.on('drain', function() {
    request.resume();
  })
{% endhighlight %}

Pipe by itself maintains the read write rate to be in sync by pausing and resuming when necessary.

Now since we have handled our cause of concern, all that is left is to calculate the upload percentage upon receiving each chunk of data and respond back with the calculated percentage.

{% highlight javascript %}
  uploadedSize += chunk.length;
  uploadProgress = (uploadedSize/fileSize) * 100;
  response.write(Math.round(uploadProgress) + "%" + " uploaded\n" );
{% endhighlight %}

Do note that the actual size of the upload file is calculated from the request headers.

{% highlight javascript %}
var fileSize = request.headers['content-length'];

{% endhighlight %}

Now, when the request ends ( i.e the 'end' event is emitted by the request ), the final chunk of response is given back to the client indicating that our file upload has been done successfully.

To test this, run the node server and try making a request, something like this:

`curl -v --upload-file "upload_file.csv" "http://localhost:8000"`

and the upload progress could be tracked. 
