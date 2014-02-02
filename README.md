External Comments
=================

This repository contains a simple server used to store comments, and
an associated Javascript file which allows you to include comments
in your static-pages.

This is a self-hosted alternative to using disqus.


Comment Server
--------------

The comment server is written using [sinatra](http://www.sinatrarb.com/),
and has two choices for storing the actual comment data:

* A [Redis server](http://redis.io/).
* An SQLite database.

The server implements a simple API in two methods:

* `GET /comments/ID`
   * This retrieves the comments associated with the given ID.
   * The return value is an array of hashes.
   * The hashes have keys such as  `author`, `body` & `ip`.
* `POST /comments/ID`
   * Store a new comment against the given ID
   * The submission should have the fields `author` and `body`.

Assuming you have the appropriate library available you should specify
your preferred storage like so:

     $ STORAGE=redis ./server/comments.rb

Or:

     $ STORAGE=sqlite ./server/comments.rb

When SQLite is chosen the database can be set to an explicit path via the
DB variable:

     $ DB=/tmp/comments.db STORAGE=sqlite ./server/comments.rb


Client-Side Inclusion
---------------------

Including comments in a static-page on your site is a simple as including the
following in your HTML HEAD section:

    <script src="http://code.jquery.com/jquery-1.10.1.min.js"></script>
    <script src="/js/e-comments.js" type="text/javascript"></script>
    <link rel="stylesheet" type="text/css" href="/css/e-comments.css" media="screen" />
    <script type="text/javascript">
       $( document ).ready(function() {
        discussion( "http://server.name/comments/id" );
    });
    </script>

The `discussion()` method accepts a single argument, the URL of the comment-server you've got running.  Including the ID of the discussions to include.

So one page might have:

        discussion( "http://server.name/comments/home" );

And a different page might include:

        discussion( "http://server.name/comments/about" );

This ensures that both pages show distinct comments, and there is no confusion.


Online Demo
-----------

There is a live online demo you can view here:

* http://www.steve.org.uk/Software/e-comments/demo.html

Steve
--
