http-template
=============

node module for sending http request using files containing raw HTTP

Installation
------------

    npm install http-template

Explanation and Usage
---------------------

http-template is a simple module that reads http templates and uses them to send HTTP requests. An http template is jsut a file with the extension '.http' that contains raw http. Here's an example file (a request for google):

    GET /search?hl=en&output=search&sclient=psy-ab&q={{query}}&btnK= HTTP/1.1
    Host: www.google.com
    User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/534.57.2 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
    Referer: http://www.google.com/
    Accept-Language: en-us
    Accept-Encoding: gzip, deflate
    Connection: keep-alive




to send this request we can write the following simple app. Let's assume the the file shown above is named *google.http* and is in the *templates* directory

	// require the module and set the path to where templates are stored
    var httpTemplate = require('http-template')(__dirname + '/templates');

    // send a request using google.http, pass in empty object, nothing to fill in in template
    httpTemplate('google', {query: 'nodejs+api'}, function(response, body) {
    	console.log(body);
    });



