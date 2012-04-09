# Node.js: Step by Step - Blogging Application

## Introduction

Hi guys, it's Christopher Roach again with the second installment of my series on web development with Node. In the
previous installment we covered the basics of Node by playing around a bit with it's REPL and creating a very simple
HTTP server. In this episode, we are going to begin developing our blog engine by adding a few routes to the server we
created in the previous lesson.

As I mentioned in the last episode, we'll be creating the very same blog application that you've most likely seen if
you've ever watched the now famous Ruby on Rails introductory video. Unlike the Ruby on Rails video, however, we'll not
be putting our application together in a matter of minutes. Sexy videos that slap together an app in near impossible
times are great for creating hype, but not necessarily the best mechanism for truly learning a new technology. Instead
we'll be progressing through the development of the app at a somewhat slower pace. The reason for this is that I want
to walk you through the creation of the app from the lowest level possible where we will essentially develop our own
very simple set of libraries and eventually work are way all the way up to using a full on popular web development
framework.

The goal here is that you'll learn how to develop an application in Node just like you would in the real world; using
plenty of libraries and frameworks that abstract away much of the mundane details and allow you to concentrate solely
on what's unique to your app, but you'll also develop a good understanding---at least at a very high level---of what
exactly these libraries are doing behind the scenes. In essence, I hope to pull back the curtain and expose the tech
behind the magic, because an engineer really needs to truly understand their tools to be as effective as possible.

So, let's not waste any more time talking about where we're headed, we've got a lot of ground to cover today so let's
go ahead and get right down to it.

## Our First Route

Let's begin by cd'ing into the blog directory that we created in the last screencast. You should have a single file in
your directory called app.js which is the simple HTTP server that we created last time. Go ahead and open that up now
in your favorite text editor and navigate to the server code that we created last time. We're going to start out small,
so let's add some code now that will check the URL being called, and, if it matches a particular pattern, displays the
same hello world message that we displayed last time, otherwise we'll issue a 404 error.

To do so, we'll start off with a little refactoring. Highlight all of the code that we wrote inside of the createServer
function last time and cut it out. With that code, we'll create a new function that renders our 'New Post' form. So, go
ahead and create that function now, call it 'renderNewPostForm' and we'll add some parameters so that it will take both
the request and response objects that are passed into the createServer function. Then, paste the code that we just cut
into our new function. Once we get the routing working properly we'll change this function to return some HTML rather
than the current text message, but for the time being let's just leave it as is.

Now, go ahead and call that new function from the createServer function and remember to pass in the request and
response objects as well. Save that and we'll go ahead and test it now to make sure that we haven't broken anything
with our simple refactoring. Remember we run our application by calling the node command and passing in the name of the
app's main file, which, in our case, will be `node app.js`. Then copy the URL that's printed out and pop that into your
browser to confirm that everything is still working properly.

Let's flip back over now to the command line, stop our app, and jump back into our text editor. Back in the
createServer function we want to add a new variable which will be the regular expression representing our first
route. Now I'm not guaranteeing strict adherence to the REST protocol (especially since we will only be using the GET
and POST methods during the course of this tutorial), but I will try my best to stick to a fairly RESTful set of routes
given this constraint. With that in mind, let's use the URL "/posts/new" for our 'New Post' form.

So now that we've got a regular expression object representing our first route, we need to add some code to match that
object to the incoming URL. Before we can do so though, we need to extract the document filepath from the URL.
Luckily, Node comes with a library that makes that easy for us and it's called...  What else? The 'url' library. So
let's go ahead and require that now. On a related side note, I'll be using the terms 'module' and 'library' throughout
this series to represent the same concept which is essentially just a method of packaging up a set of interrelated
functions and objects.

The 'url' module that we've just required has a function called 'parse' that takes a URL string and returns an object
representing that URL. Within that object is an attribute called 'pathname' that we can use to get everything in the
URL after the hostname and up to, but not including, the querystring. This is essentially the document filepath. Once
we have that filepath, we can compare it to our regular expression object to see if the user has hit the New Post
URL. Let's add that code now. We will call the `test` method on our `newPostFormRegex` object which will return true if
there is a match and false otherwise. If a match was found we want to call our `renderNewPostForm` function, otherwise,
we want to send a 404 result. Let's go ahead and create a new function for handling the 404 result now; call it
`render404`. This function is really quite simple, as we essentially just need to call the response' `writeHead` method
with the status code 404, then call the `end` method with a simple message explaining that the requested file was not
found on this server. Now, we just need to call that from our `else` case and we should be ready to test our first
route.

To do so, let's get back to the command line and fire up node with our `app.js` file. Then we'll flip back over to the
browser and let's try out the 404 error first. Just go ahead and hit the server again at the base address, and we
should see a 404 error returned. Now, let's try out our new route and make sure that we see our Hello World
message. Excellent! everything seems to be working perfectly, so let's add the HTML for the form now.

## Our First View

Now we could go ahead and copy our HTML for the New Post form into the current file and keep everything in one
place. However, that's going to start to get real messy, real fast. So, at this point it's probably a good idea to add
a bit of structure to our application. So, skip back over to the command line and go ahead and create a new
folder. Let's call it 'views' since this will, of course, be where we keep all of the views for our application.

Once we've created our views folder, we need to add a new file to hold the markup for our New Post form. Now we could
shove everything directly into the views folder, but since we're currently dealing with blog posts and you may want to
create other model objects later such as comments, we'll go ahead and create another folder inside of views that we'll
call 'post' and this will be where we keep all of our views specific to blog posts.

Now that we've got our views directory structure settled, let's go ahead and create a file called new.html inside of
the 'post' directory we just created. I've got some basic HTML that I'm going to copy and paste into this file just to
keep things moving along. I've also added the same HTML to the transcipt of this screencast so you can simply copy and
paste the markup over to your view as well.

    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
    <html
       xmlns="http://www.w3.org/1999/xhtml">
      <body>
        <h1> New Post
        </h1>
        <form method="post" action="/posts"
              id="new_post" class="new_post">
          <div class="field">
            <label for="post_title">Title
            </label>
            <br />
            <input
               type="text" name="title" id="post_title" size="30" />
          </div>
          <div class="field">
            <label
               for="post_content">Content
            </label>
            <br />
            <textarea name="content" cols="40" rows="20" id="post_content">
            </textarea>
          </div>
          <div class="actions">
            <input type="submit" value="Create Post" id="post_submit" />
          </div>
        </form>
        <p>
          <br />
          <a href="/posts">Back
          </a>
        </p>
      </body>
    </html>

Now that we've created our view, we need to get it loaded into our application before we can deliver it to the user. To
do that, we are going to break one of the cardinal rules of Node development---we're going to perform synchronous
I/O. Now, if you remember in the first screencast, I mentioned that Node's strength came from it's non-blocking
event-driven nature. Unlike other types of HTTP servers, that use threads for each active connection, Node performs all
actions on a single thread so it is absolutely paramount that you not block this thread by performing any synchronous
actions, and, for its part, Node makes this nearly impossible to do by simply not providing any synchronous
functions. However, doing everything asynchronously, while an excellent idea for creating servers, is not the best way
to do other things like writing scripts, and because of this, some functions have synchronous counterparts as we will
see now when we use the [`readFileSync`][readFileSync] function from the File System module to read in and cache our
New Post view.

Let's do that now by first requiring the File System module which is actually called `fs` and calling the
`readFileSync` method.

Something to take notice of here is that, by default, all functions in Node are asynchronous unless otherwise specified
in the name of the function. So, for example, the asynchronous counterpart to the function we just called would be
called simply, `readFile`. Another thing to notice about many of Node's functions is that they tend to work with data
buffers by default for performance reasons. So, for instance, if we were to call the `readFileSync` function, just as
we've done here, with only the name of the file we want to read in, we would get back a data buffer instead of a string
like you might expect. I'll demonstrate that to you now at the REPL.

If we want to get back a string instead, we'll need to add a second, optional, parameter that specifies the encoding to
use; which in our case will be 'utf8' to read the data in as a Unicode string.

With that little interlude out of the way, let's jump back into our editor and finish up our first view. Now, we could
go ahead and alter our `readFileSync` call by adding the 'utf8' encoding parameter to it, but this isn't really
necessary since, by default, the response sent to the browser will be UTF8. So let's leave that alone and instead
concentrate on our `renderNewPostForm` function. We'll want to change that to return our new HTML form rather than the
text we are currently returning.

First, we'll need to change the content type that we are returning to be 'text/html', otherwise we'll continue to
return plain text to the browser. To be absolutely sure that the browser interprets the data just the way that we want
it to, I'll go ahead and also add a charset parameter to the Content-type as well. So let's add that now
(`charset=utf-8`). Finally, rather than return the string 'Hello World', we'll pass in our view's variable to the
Response object's `end` function.

If we step back into the command line again real quick and rerun our app, we should now be seeing the form for creating
a New Post being sent back to the browser. And, there it is. Congratulations you've created your first route and also
your very first view.

## Conclusion

So that pretty much wraps up this lesson. To recap, you've learned a bit about Node's File System module, and we've
used it to read in a view file and cache it. We've covered how text is represented internally in Node (as a data buffer
rather than a string). We've also created our very first view and slapped together an extremely simple routing system.

Now, if you've ever developed any kind of software before, I'm sure you're already seeing all kinds of issues with the
code we've written today. First off, having a separate function for each view we want to display is not a very DRY way
of doing things and probably won't scale very far. Also, unless you happen to like spaghetti code, having one long `if`
statement as our entire routing system seems like a crime against humanity at the very least. Finally, our view system
is not very useful right now since it is essentially nothing more than static text. We'll soon need a full-fledged
templating system if we want any type of dynamic behavior in our web pages. These are all issues that we'll be
addressing in the lessons still to come, so please stay tuned and be sure to join me again in the next episode.

[readFileSync]: http://nodejs.org/docs/v0.4.5/api/fs.html#fs.readFileSync
