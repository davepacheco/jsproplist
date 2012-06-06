# jsproplist

jsproplist is a command-line tool that reads JavaScript source files and
attempts to find properties used with the "." operator (as in "this.propname").
This can be useful to find cases where property names are misspelled.  When
using prefixes on properties (as is common in many coding styles), this also
makes it easy to construct a comprehensive list of the properties for objects
of a given class, for reference or to audit that they're all initialized in the
constructor, for example.

Here's the prototypical Node program (from nodejs.org):

    var http = require('http');
    http.createServer(function (req, res) {
      res.writeHead(200, {'Content-Type': 'text/plain'});
      res.end('Hello World\n');
    }).listen(1337, '127.0.0.1');
    console.log('Server running at http://127.0.0.1:1337/');

Here's the output of jsproplist on this file:

    # jsproplist example.js
    .createServer
    .end
    .listen
    .log
    .writeHead

With "-l", jsproplist also shows the line where each property is used:

    # jsproplist -l example.js
    .createServer 2
    .end 4
    .listen 5
    .log 6
    .writeHead 3

This is more useful more complicated programs.  Here's a run on jsproplist
itself:

    # jsproplist -l $(which jsproplist)
    .BasicParser 45
    .argv 45,64
    .basename 14
    .carry 89
    .createReadStream 78
    .error 39,51,81
    .exit 119,40
    .forEach 102,108,166,70
    .getopt 50
    .hasOwnProperty 167
    .join 115,28
    .keys 106,114
    .length 103,129,66,69
    .log 104,110,114
    .match 161
    .message 81
    .on 80,86,94
    .optarg 59
    .optind 64
    .option 54
    .removeAllListeners 87
    .slice 64
    .sort 107,115
    .sprintf 15

You can also use the "-p" flag to specify a prefix, which is especially useful
when your coding style uses unique prefixes for each class.  For example, this
prints out all fields started with the prefix "js_":

    # jsproplist -l -p js_ mycode.js
    .js_init 234,535
    .js_log 225,279,288,316,327,331,387
    .js_server 226,307,333,443,481
    .js_state 230,259,278,291,317,318,377,378,495,496

which can be used to make sure field names are spelled consistently,
initialized correctly in the constructor, and so on.
