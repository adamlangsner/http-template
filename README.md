http-template
=============

node module for sending http request using files containing raw HTTP

Installation
------------

    npm install http-template

Explanation and Usage
---------------------

http-template is a simple module that reads http templates and uses them to send HTTP requests. An http template is just a file with the extension '.http' that contains raw http. Here's an example file (a google search):

    GET /search?hl=en&output=search&sclient=psy-ab&q={{query}}&btnK= HTTP/1.1
    Host: www.google.com
    User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/534.57.2 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
    Referer: http://www.google.com/
    Accept-Language: en-us
    Accept-Encoding: gzip, deflate
    Connection: keep-alive


notice the **{{query}}** on the first line. This is how we can modify the request with dynamic data. One good use for this is inserting session IDs into a request:

    ...
    Cookie: {{session_id}}; path=/;
    ...


to send this request we can write the following app. Let's assume the the file shown above is named **google.http** and is in the **templates** directory

```js
// require the module and set the path to where templates are stored
var httpTemplate = require('http-template')(__dirname + '/templates');

// send a request using google.http, pass in object with query to search for
httpTemplate('google', {query: 'nodejs+api'}, function(response, body) {
    console.log(body);
});
```

Arguments
---------

### template name
the first argument specifies the name of the file to use without the **.http** extension. If you want to add sub-directories in your templates folder you can simply pass in 'sub_folder/file_name', once again leave off the **.http** extension.

### data
the second argument is an object containing the data to render the template with. In the above example, we passed in **{query: 'nodejs+api'}** to fill in the **{{query}}** variable in the template. http-template uses underscore with mustache style interpolation tokens (**{{ }}**) to render templates.

### options 
the third argument can be an options object or the callback. the current supported options are as follows:

    - https: boolean (default: false) - if set to true request will be sent using https
    - autoContentLength: boolean (default: true) - when true, httpTemplate will fill in the 'Content-Length' header for you.

```js
// send request over https
httpTemplate.('google', {query: 'garbage'}, {https: true}, function(res, body) {
	
});
```

### callback
the last argument (3rd or 4th if you pass in options) is a callback which takes two arguments, the response object and the body string. the body is provided in addition to the response because if the response is gzipped, http-template will deflate it and pass the deflated body to the callback along with the response object.



