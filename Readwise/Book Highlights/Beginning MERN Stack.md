# Beginning MERN Stack

![rw-book-cover](https://m.media-amazon.com/images/I/513KxWagreS._SY160.jpg)

## Metadata
- Author: [[Greg Lim]]
- Full Title: Beginning MERN Stack
- Category: #books

## Highlights
- The architecture of MongoDB is a NoSQL database which stores information in the form of collections and documents. MongoDB stores one or more collections. A collection represents a single entity in our app, for example in an e-commerce app, we need entities like categories, users, products. Each of these entities will be a single collection in our database. ([Location 187](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=187))
    - Tags: [[blue]] 
- Express is a framework that acts as a light layer atop the Node.js web server making it easier to develop Node.js web applications. It simplifies the APIs of Node.js, adds helpful features, helps organizes our application’s functionality with middleware and routing and many others. ([Location 358](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=358))
    - Tags: [[blue]] 
- CORS stands for Cross-Origin Resource Sharing. By default, modern browsers don’t allow frontend clients to talk to REST APIs. ([Location 361](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=361))
    - Tags: [[blue]] 
- The cors package we are installing provides an Express middleware that can enable CORS with different options so we can make the right connections on the network. ([Location 366](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=366))
    - Tags: [[blue]] 
- The mongodb dependency allows us to interact with our MongoDB database. ([Location 369](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=369))
    - Tags: [[blue]] 
- The dotenv dependency loads environmental variables from the process.env file instead of setting environment variables on our development machine which simplifies development. ([Location 371](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=371))
    - Tags: [[blue]] 
- Next, we will install a package called nodemon (https://www.npmjs.com/package/nodemon) that automatically detects code changes and restart the Node server so we don’t have to manually stop and restart it whenever we make a code change. Install nodemon with the following command:   npm install -g nodemon ([Location 410](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=410))
    - Tags: [[blue]] 
- express.json is the JSON parsing middleware to enable the server to read and accept JSON in a request’s body. ([Location 456](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=456))
    - Tags: [[blue]] 
- Middleware are functions that Express executes in the middle after the incoming request and before the output. ([Location 459](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=459))
    - Tags: [[blue]] 
- The general convention for API urls is to begin it with: /api/<version number>. And since our API is about movies, the main URL for our app will be i.e. localhost:5000/api/v1/movies. The subsequent specific routes are specified in the 2nd argument movies. ([Location 471](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=471))
    - Tags: [[blue]] 
- Our app lives in the src folder. All React components, CSS styles, images (e.g. logo.svg) and anything else our app needs go here. Any other files outside of this folder are meant to support building your app (the app folder is where we will work 99% of the time!). ([Location 1686](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=1686))
    - Tags: [[blue]] 
- In index.js, we import both React and ReactDOM which we need to work with React in the browser. React is the library for creating views. ReactDOM is the library used to render the UI in the browser. ([Location 1705](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=1705))
    - Tags: [[blue]] 
- A component has to return a single React element. In our case, App returns a single <div />. The element can be a representation of a native DOM component, such as <div />, or another composite component that you've defined yourself. ([Location 1767](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=1767))
    - Tags: [[blue]] 
- React.useState is a ‘hook’ that lets us add some local state to functional components. useState declares a ‘state variable’. React preserves this state between re-renders of the component. In our case, our state consists of a user variable to represent either a logged in or logged out state. When we pass null to useState, i.e. useState(null), we specify null to be the initial value for user. ([Location 2081](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=2081))
    - Tags: [[blue]] 
- Note that we use render instead of component because render allows us to pass in props into a component rendered by React Router. In this case, we are passing user (the logged-in user information) as props to the AddReview component. We can pass data into a component by passing in a object called props. ([Location 2162](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=2162))
    - Tags: [[blue]] 
- To retrieve the list of movies from the database, we will need to connect to our backend server. We will create a service class for that. A service is a class with a well-defined specific function your app needs. In our case, our service is responsible for talking to the backend to get and save data. Service classes provide their functionality to be consumed by components. ([Location 2194](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=2194))
    - Tags: [[blue]] 
- Components are meant to be responsible for mainly rendering views supported by application logic for better user experience. They don’t fetch data from the backend but rather delegate such tasks to services. ([Location 2274](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=2274))
    - Tags: [[blue]] 
- In essence, if the 2nd argument of useEffect contains an array of variables and any of these variables change, useEffect will be called. ([Location 2785](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=2785))
    - Tags: [[blue]] 
- useEffect without a second argument, is called each time a state change occurs in its body. useEffect with an empty array in its second argument gets called only the first time the component renders. useEffect with a state variable in the array gets called each time the state variable changes. ([Location 2789](https://readwise.io/to_kindle?action=open&asin=B097JTNBNG&location=2789))
    - Tags: [[blue]] 
