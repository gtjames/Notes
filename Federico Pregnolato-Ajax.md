>Ajax is a technique that allows web pages to communicate asynchronously with a server, and it dynamically updates web pages without reloading.

The use of Ajax revolutionized how websites worked, and ushered in a new age of web applications. Web pages were no longer static, but dynamic applications.

## Client and Server

+ Client: request a reseource
+ Server: process the request and send back data

Ajax allows JavaScript (originally client-side scripting language) to request resources from a server on behalf of the client. The resources requested are usually JSON data or small fragments of text or HTML rather than a whole web page.

Remember to allow CORS in a dev server or make requests to servers with COSR enabled
> The same-origin policy in browsers blocks all requests from a domain that is different from the page making the request. This policy is enforced by all modern browsers and is to stop any malicious JavaScript being run from an external source. The problem is that the APIs of many websites rely on data being transferred across domains. [Cross-origin resource sharing (CORS)](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) is a solution to this problem as it allows resources to be requested from another website outside the original domain. The CORS standard works by using HTTP headers to indicate which domains can receive data. A website can have the necessary information in its headers to allow external sites access to its API data. Most modern browsers support this method and respect the restrictions specified in the headers.

## A Brief History of Ajax
+ 1990 >> Microsoft implemented the XMLHTTP ActiveX control in Internet Explorer 5. It allowed data to be sent asynchronously in the background using JavaScript
+ 2004/2005 >> Google uses asynchronous loading techniques. 
+ 2005 >> Term Ajax was coined (Asynchronous JavaScript and XML) by Jesse James Garrett in 2005 in the article [“Ajax: A New Approach to Web Applications,”](https://web.archive.org/web/20080702075113/http://www.adaptivepath.com/ideas/essays/archives/000385.php)

+ **Asynchronous**: Program continues to run after a request is sent. Use callbacks to manage this.
+ **JavaScript**: Ajax enabled JavaScript to send requests and receive responses from a server, allowing content to be updated in real time.
+ **XML**: When the term Ajax was originally coined, XML documents were often used to return data. Many different types of data can be sent, but by far the most commonly used in Ajax nowadays is JSON, which is more lightweight and easier to parse than XML. 

>An application programming interface (API) is a collection of methods that allows external access to another program or service. Many websites allow controlled access to their data via public APIs. This means that developers are able to interact with the data and create mashups of third-party services. A weather site, for example, might have an API that provides methods that return information about the weather in a given location, such as temperature, wind speed, and so on. This can then be used to display local weather data on a web page. The information that’s returned by APIs is often serialized as JSON. Since the data is being provided by an external site, CORS will have to be enabled in order to access information from an API. Some services may also require authentication in order to access their APIs.


## The Fetch API
Currently the living standard for requesting and sending data asynchronously across networks.
The Fetch API uses promises to avoid callback hell, and also streamlines a number of concepts that had become cumbersome when using the XMLHttpRequest object.

### Basic Usage
The Fetch API provides a global `fetch()` method that only has one mandatory argument, which is the URL of the resource you wish to fetch. A very basic example would look something like the following piece of code:

```js
fetch('https://example.com/data')
.then( /*code that handles the response*/ )
.catch( /** code that runs if the server returns an error */ )
```

As you can see, the `fetch()` method returns a promise that resolves to the response returned from the URL that was provided as an argument.

### Response Interface
Response objects have a number of properties and methods that allow us to process the response effectively.

Properties of the Response object:
+ `ok` - Checks to see if the response is successful. It is based on the [HTTP status code](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes), which can be accessed using the `status` property. 
-   `headers` – A Headers object (see later section) containing any headers associated with the response
-   `url` – A string containing the URL of response
-   `redirected` – A boolean value that specifies if the response is the result of a redirect
-   `type` – A string value of 'basic', 'cors', 'error' or 'opaque'. A value of 'basic' is used for a response from the same domain. A value of 'cors' means the data was received from a valid cross-origin request from a different domain. A value of 'opaque' is used for a response received from 'no-cors' request from another domain, which means access to the data will be severely restricted. A value of 'error' is used when a network error occurs.

Response codes:
1. `200` - Response is successful
2. `201` - Resource was created
3. `204` - Request is successful but no content is returned

Example:
```js
const url = 'https:example.com/data';

fetch(url)
.then((response) => {
    if(response.ok) {
        return response;
    }
    throw Error(response.statusText);
})
.then( response => // do something with response )
.catch( error => console.log('There was an error!') )
```

Notice that the error thrown refers to the `statusText` property of the response object and specifies the status message that corresponds to the code returned, for example it might be 'Forbidden' for a status code of 403.

### Redirects

The `redirect()` method can be used to redirect to another URL. It creates a new promise that resolves to the response from the redirected URL.

Example:

```js
fetch(url)
.then( response => response.redirect(newURL)); // redirects to another URL.then( // do something else )
.catch( error => console.log('There was an error: ', error))
```

### Text Responses
The `text()` method takes a stream of text from the response, reads it to completion and then returns a promise that resolves to a USVSting object that can be treated as a string in JavaScript.

Example:

```js
fetch(url).then( response => response.text() ); // transforms the text stream into a JavaScript string
.then( text => console.log(text) )
.catch( error => console.log('There was an error: ', error))
```

In this example, once the promise has been resolved, we use the `string()` method to return a promise that resolves with a string representation of the text that was returned

### File Responses

The `blob()` method is used to read a file of raw data, such as an image or a spreadsheet. Once it has read the whole file, it returns a promise that resolves with a `blob` object.

Example:

```js
fetch(url).then( response => response.blob() ); // transforms the data into a blob object
.then( blob => console.log(blob.type) )
.catch( error => console.log('There was an error: ', error))
```

This example is similar to the text example above, but we use the `blob()` method to return a blob object. We then use the `type` property to log the MIME-type to log what type of file we have received.

### JSON Responses
JSON is probably the most common format for AJAX responses. The `json()` method is used to deal with these by transforming a stream of JSON data into a promise that resolves to a JavaScript object.

Example

```js
fetch(url).then( response => response.json() ); // transforms the JSON data into a JavaScript object
.then( data => console.log(Object.entries(data)) )
.catch( error => console.log('There was an error: ', error))
```

Again, this is very similar to the earlier examples, except this response returns some JSON data that is then resolved as a JavaScript object. This means we can manipulate the object using JavaScript. In the example below, the `Object.entries()` method is used to view the key and value pairs in the returned object.

### Creating Response Objects

How to create your own response object
```js
const response = new Response( 'Hello!', {
    ok: true,
    status: 200,
    statusText: 'OK',
    type: 'cors',
    url: '/api'
});
```
First argument: data to be returned
Second argument: Object that provides values for the properties specified

### Request Interface
We can receive better info if we provide a `Request` object as an argument.
This allows a number of important options to be set

Request objects are created using the `Request()` constructor, and include the following properties:

-   `url` – The URL of the requested resource (the only property that is required).
-   `method` – a string that specifies which HTTP method should be used for the request. By default, this is 'GET'.
-   `headers` – This is a `Headers` object (see later section) that provides details of the request's headers.
-   `mode` – Allows you to specify if CORS is used or not. CORS is enabled by default.
-   `cache` – Allows you to specify how the request will use the browser's cache. For example, you can force it to request a resource and update the cache with the result, or you can force it to only look in the cache for the resource.
-   `credentials` – Lets you specify if cookies should be allowed with the request.
-   `redirect` – Specifies what to do if the response returns a redirect. There’s a choice of three values: 'follow' (the redirect is followed), 'error' (an error is thrown) or 'manual' (the user has to click on a link to follow the redirect).
---
#### Hypertext Transfer Protocol
The Web is built upon the Hypertext Transfer Protocol, or HTTP. When a client (usually a browser) makes a request to a server, it contains information about which HTTP verb to use. [HTTP verbs, also known as HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) are the what HTTP uses to tell the server what type of request is being made, which then determines the server will deal with the request.

The five most commonly used verbs when dealing with resources on the web are:

-   GET requests to retrieve resources
-   POST requests, usually used to create a resource but can actually perform any task
-   PUT requests to _upsert_, which means insert a resource or update it entirely
-   PATCH requests to make partial updates to a resource
-   DELETE requests to delete a resources.

By default, a link in a web page will make a GET request. Forms are also submitted using a GET request by default, but they will often use a POST request.

This [excellent blog post](https://robm.me.uk/web-development/2013/09/20/http-verbs.html) by Rob Miller explains each of these verbs in more depth if you're interested in learning more about them.

---
Example of creating a `Request` object
```js
const request = new Request('https://example.com/data', {
    method: 'GET',
    mode: 'cors',
    redirect: 'follow',
    cache: 'no-cache'
});

// Then you can use it in the Fetch API

fetch(request)
.then( // do something with the response )
.catch( // handle any errors)

// OR

fetch('https://example.com/data', {
    method: 'GET',
    mode: 'cors',
    redirect: 'follow',
    cache: 'no-cache'
})
.then( // do something with the response )
.catch( // handle any errors)
```

### Headers Interface
HTTP **headers** are used to pass on any additional information about a request or response.
For example:
+ File-type of the resource
+ Cookie information
+ Authentication information
+ Last modified

The Fetch API introduced a `Headers` interface, which can be used to create a Headers object, which can then be added as a property of Request and Response objects.

Example:

```js
const headers = new Headers({ 
	'Content-Type': 'text/plain',
	'Accept-Charset' : 'utf-8', 
	'Accept-Encoding':'gzip,deflate' 
})
```

A `Headers` object includes the following properties and methods
1. `has()` – Can be used to check if the headers object contains the header provided as an argument.
```js
headers.has('Content-Type');
<< true
```

2. `get()` - Returns the value of the header provided as an argument
```js
headers.get('Content-Type');
<< 'text/plain'
```

3. `set()` - Can be used to set a value of an already existing header, or create a new header with the value provided as an argument if it does not already exist.
```js
headers.set('Content-Type', 'application/json');
```

4. `append()` – Adds a new header to the headers object.
```js
headers.append('Accept-Encoding','gzip,deflate');
```

5. `delete()` – Removes the header provided as an argument.
```js
headers.delete('Accept-Encoding')
```

6. `keys()`, `values()`, `entries()` – Iterators that can be used to iterate over the headers key, values or entries (key and value pairs).
```js
for(const entry of headers.entries(){
console.log(entry);
}
<< [ 'Content-Type', 'application/json' ]
```

### Putting it All Together
We can use the Headers, Request and Response objects to put together a typical example that sets up the URL, Request and Headers before calling the `fetch()` method:

```js
const url = 'https:example.com/data';
const headers = new Headers({ 'Content-Type': 'text/plain', 'Accept-Charset' : 'utf-8', 'Accept-Encoding':'gzip,deflate' })

const request = (url,{
    headers: headers
})

fetch(request)
.then( function(response) {
    if(response.ok) {
        return response;
    }
    throw Error(response.statusText);
})
.then( response => // do something with response )
.catch( error => console.log('There was an error!') )
```

## Receiving Information
Check out the code example at [my website](https://fedpre.github.io/wdd330-frontend-dev-II/work/ajax/ajax.html)

>In the previous example we displayed a message to say we were waiting for a response. It is common for sites to use spinners (or egg timers in the old days!) to indicate that the site is waiting for something to happen. [Ajax Load](http://www.ajaxload.info/) and [Preloaders.net](http://preloaders.net/) are both good resources for creating a spinner graphic for your site.

## Sending Information
Check out the code example at [my website](https://fedpre.github.io/wdd330-frontend-dev-II/work/ajax/ajax-todo.html)

## FormData
The Fetch API includes the `FormData` interface, which makes it much easier to submit information in forms using Ajax.

A `FormData` instance is created using a constructor function:

```js
const data = new FormData();
```

If a form is passed to this constructor function as an argument, the form data instance will serialize all the data automatically, ready to be sent using Ajax. In our last example, we created the task manually based on the data provided in the form. The `FormData` interface helps to reduce the amount of code needed when submitting forms.

We can use this to cut down the amount of code in `main.js` by changing it to the following:

```js
const form = document.forms['todo'];

form.addEventListener('submit', addTask, false);

function addTask(event) {
    event.preventDefault();
    const task = new FormData(form);
    const url = `http://echo.jsontest.com/id/1/title/${form.task.value}`;
    const headers = new Headers({
        'Accept': 'application/json',
        'Content-Type': 'application/json'
    });
    const request = new Request(url,
    {
        method: 'POST',
        mode: 'cors',
        header: headers,
        body: JSON.stringify(task)
    }
    )

    fetch(request)
    .then( response => response.json() )
    .then( data => console.log(`${data.title} saved with an id of ${data.id}`) )
    .catch( error => console.log('There was an error:', error))

}
```

The `FormData` interface really comes into its own when a form contains files to upload. This was a notoriously difficult task in the past, often requiring the use of Flash or another third-party browser plugin to handle the upload process. The `FormData` instance will automatically create the necessary settings required, and take care of all the hard work if any file uploads are present in the form.

You can find more information about the `FormData` interface in [this SitePoint article by Craig Buckler](http://www.sitepoint.com/easier-ajax-html5-formdata-interface/) and on the [Mozilla Developer Network.](https://developer.mozilla.org/en-US/docs/Web/API/FormData)

## A Living Standard
If you don't want to 'live on the edge', you could consider using a library to take care of Ajax requests. The advantage of this approach is that the library will take care of any implementation details behind the scenes – it will use the most up-to-date methods, such as the `fetch` API, if it's supported, and fallback on using older methods, if required.

The jQuery library is a good option for this – it has the generic `ajax()` method that can be used in a very similar way to the `fetch()` method. For example, if you want to get the data from the number API, you would use the following code:


