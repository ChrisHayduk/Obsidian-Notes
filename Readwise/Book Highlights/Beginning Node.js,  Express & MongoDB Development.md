# Beginning Node.js,  Express & MongoDB Development

![rw-book-cover](https://m.media-amazon.com/images/I/611NwnA1KyL._SY160.jpg)

## Metadata
- Author: [[Greg Lim]]
- Full Title: Beginning Node.js,  Express & MongoDB Development
- Category: #books

## Highlights
- What benefits does choosing Node.js as a server-side language bring about? -          Firstly, the V8 JavaScript engine used in Google Chrome is fast and can execute thousands of instructions a second -          Secondly, Node.js encourages an asynchronous coding style making for faster code to manage concurrency while avoiding multithreaded problems. -          Thirdly, because of its popularity, JavaScript offers Node.js access to many useful libraries -          And of course, Node.js provides us the ability to share code between browser and server since they both use JavaScript code. ([Location 148](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=148))
    - Tags: [[blue]] 
- The general convention is that when you require a module, you put it in a variable that has the same name as the module itself. ([Location 198](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=198))
    - Tags: [[blue]] 
- The function provided to createServer is called a callback function. It will be called when the createServer function is completed. When it is called, it will be provided the request (req – request from browser) and response (res – response to give back to browser) object in the function. ([Location 216](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=216))
    - Tags: [[blue]] 
- A port is a specific gateway on the server to host a particular app. For example, if there are multiple apps running on the same server, we specify different port numbers for different apps. ([Location 228](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=228))
    - Tags: [[blue]] 
- Every Node.js application is just like this, a single request handler function responding to requests. For small sites, this might seem easy, but things quickly get huge and unmanageable quickly as you can imagine, for example a site like Amazon.com which includes rendering dynamic reusable HTML templates, rendering/uploading of images etc. ([Location 395](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=395))
    - Tags: [[blue]] 
- Express is a framework that acts as a light layer atop the Node.js web server making it easier to develop Node.js web applications. ([Location 417](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=417))
    - Tags: [[blue]] 
- Express takes care of the http, request and response objects behind the scenes. The callback function provided in the 2nd argument in app.listen() is executed when the server starts listening. ([Location 513](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=513))
    - Tags: [[blue]] 
- With Express, we can refactor one big request handler function into many smaller request handlers that each handle a specific function. This allows us to build our app in a more modular and maintainable way. ([Location 559](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=559))
    - Tags: [[blue]] 
- path is another built in module in Node. It helps us get the specific path to the file, because if we do res.sendFile(‘index.html’), there will be error thrown as an absolute path is needed. ([Location 570](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=570))
    - Tags: [[blue]] 
- We have been seeing a few examples of code with callback functions. Callback functions are an important aspect in Node.js that help support tasks to be done asynchronously. That is, rather than waiting for one task to complete before executing another e.g. in PHP:   Task 1 -> Task 2 -> Task 3 -> Task 4 -> Completion   Node.js allows the possibility to performs tasks in parallel, where no task is blocking another. ([Location 587](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=587))
    - Tags: [[blue]] 
- app.use is a special function to increase functionality with Express by adding a function to our application’s middleware stack. ([Location 639](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=639))
    - Tags: [[blue]] 
- express.static is a packaged shipped with Express that helps us serve static files. With express.static(‘public’), we specify that any request that ask for assets should get it from the ‘public’ directory. ([Location 642](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=642))
    - Tags: [[blue]] 
- The -dev option is to specify that we install nodemon for development purposes only. ([Location 730](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=730))
    - Tags: [[blue]] 
- instead of running our app with node index.js as we have done previously, we now run it with: npm start. npm start looks inside our package.json file, see that we have added a script called start, and run the associated command i.e. nodemon index.js. nodemon watches our files for changes and automatically restarts the server when there are code changes. ([Location 746](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=746))
    - Tags: [[blue]] 
- Templating engines help us dynamically render HTML pages in contrast to just having fixed static pages which in the earlier chapter gives us a problem that we have to duplicate the nav bar code for all static pages. ([Location 957](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=957))
    - Tags: [[blue]] 
- templating engine that allows us to abstract our app into different layout files so that we don’t repeat common code, yet still serve the same HTML file as before. ([Location 959](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=959))
    - Tags: [[blue]] 
- With app.set('view engine','ejs'), we tell Express to use EJS as our templating engine, that any file ending in .ejs should be rendered with the EJS package. ([Location 977](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=977))
    - Tags: [[blue]] 
- To solve the problem of repetitive code (e.g. nav bar, footer) appearing in each page, we will use the concept of a layout file. A layout file contains everything common in a page, for e.g., navbar layout, header layout, footer layout, scripts layout. Each page will then include these layout files in additional to their own content. This results in a much concise, readable and manageable file. ([Location 1020](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=1020))
    - Tags: [[blue]] 
- Templating engines allows us to abstract our app into different layout files so that we don’t repeat common code, yet still serve the same HTML file as before. ([Location 1201](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=1201))
    - Tags: [[blue]] 
- It might seem like NoSQL is a protest over SQL but it actually refers to a database not structured like a spreadsheet, i.e. less rigid than SQL databases. ([Location 1224](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=1224))
    - Tags: [[blue]] 
- the architecture of MongoDB is a NoSQL database which stores information in the form of collections and documents. MongoDB stores one or more collections. A collection represents a single entity in our app, for example in an e-commerce app, we need entities like categories, users, products. Each of these entity will be a single collection in our database.   A collection then contain documents. A document is an instance of the entity containing the various relevant fields to represent the document. For example, a product document will contain name, image and price fields. Each field is a key-value pair. Documents look a lot like JSON objects with various properties (though they are technically Binary JSON or BSON). ([Location 1229](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=1229))
    - Tags: [[blue]] 
- Models are defined through the Schema interface. Remember that a collection represents an entity in our app. e.g. users, products, blogposts. A schema represents how a collection looks like. This means that each document in the collection would have the fields specified in the schema. ([Location 1316](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=1316))
    - Tags: [[blue]] 
- Here, we handle a POST request which is generally used to request an addition to the state of the server unlike GET where we simply get resources. A user POSTs a blog entry, a photo, signing up for an account, buying an item etc. POST is used to create records on servers. For modifying existing records, we use the PUT request. ([Location 1633](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=1633))
    - Tags: [[blue]] 
- To resolve this, we can alternatively use a feature in ES8 called async and await for asynchronous method calling as shown in the following code:   app.post('/posts/store', async (req,res)=>{                await BlogPost.create(req.body)     res.redirect('/') })   With async, we specify that the following method is an asynchronous call. And using await for BlogPost.create, we are saying that we will await the completion of the current line before the below line can be executed. This thus lets us have more readable code. ([Location 1676](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=1676))
    - Tags: [[blue]] 
- Middleware are functions that Express executes in the middle after the incoming request which then produces an output which could either be the final output or be used by the next middleware. We can have more than one middleware and they will execute in the order they are declared. In the process, middlewares might make changes to the request and response objects. ([Location 2103](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=2103))
    - Tags: [[blue]] 
- The use function registers a middleware with our Express app. So, when a browser makes a request to a page for example, Express will execute all the ‘use’ statements sequentially before handling the request. ([Location 2114](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=2114))
    - Tags: [[blue]] 
- A good use case of using a middleware is in form validation, where we check for the validity of form field values (field not blank etc.) Without form validation, our app currently crashes if we submit an empty new post form. We get an error TypeError: Cannot read property 'image' of null because the image property cannot be null.   With a validation middleware, Express checks to see if data filled in the fields is valid before sending the request to the server. ([Location 2139](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=2139))
    - Tags: [[blue]] 
- Till now, we have been putting all our code into index.js. For example, all the request handlers app.get, app.post are in the same file. This is not the best approach when our app grows or if we are building a more complex app as index.js becomes too complicated and unmanageable. Thus, we will refactor our code adhering to a common pattern called the Model-View-Controller or MVC pattern.   That is, we divide our application into three interconnected parts, Model, View and Controller: - Model represents the structure of the data, the format and the constraints with which it is stored. In essence, it is the database part of the application. As you might already notice, we already have that in our models folder currently storing the BlogPost model. - View is what is presented to the user. Views make use of the Model and present data in a manner which the user wants. From the view, user can make changes to the data presented to them. In our app, the View consist of static or dynamic pages rendered to users. The pages are stored in a views folder storing various EJS files to render static and dynamic HTML websites. - Lastly, we have Controller which controls the requests of the user and then generates appropriate response rendered back to the user. That is, a user interacts with the View which generates the appropriate request which is handled by the Controller which then renders the appropriate view with the Model data as a response. ([Location 2178](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=2178))
    - Tags: [[blue]] 
- This is flushing, where the validation errors are only available for the next request life cycle and in the following cycle, is deleted. ([Location 3209](https://readwise.io/to_kindle?action=open&asin=B07TWDNMHJ&location=3209))
    - Tags: [[blue]] 
