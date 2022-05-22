
**

## Readme Contents

**

1. About the Project
2. Features and Functionality
   2.1 Data Storage and Retrieval
   2.2 API
   2.3 Admin Functionality
   2.4 Images
   2.5 Categories
   2.6 Test Suite

3. Deployment / Installation
   3.1 Glitch
   3.2 Heroku
4. Reference List
5. Image & Sample Content Credits



**1.	About the project**

This project implements a node.js/hapi web application with API (openAPI 2.0 compliant) that allows users of the system to record and share information about unusual places of interest.

The application is built on version 14.16.0 of Node.js and the hapi Framework version 20.2.1 to implement a documented OpenAPI 2.0 compliant API with JWT authentication (hapi-swagger and hapi-auth-jwt2).

The API and data models are accompanied by extensive test suites using mocha.js testing framework and the chai assertion library.
The initial project commit was based on the following archive:

- https://wit-hdip-comp-sci-2021-full-stack.netlify.app/topic-05-models/unit-2/book-playtime-0-5-0/archives/playtime-0.5.0.zip

**2. Functionality & Features**


***mongoStore*** – This store implements a MongoDB based version of the Store object. The Mongoose library is used to interface between a mongo server (credentials defined as an environment variable). This interface relies on the definition of a Data Schema and model for the Collection which is declared in a js file that accompanies the export file for the store Object. The mongo storage type is unique among the four database types implemented in that a native mongo Object objectId() is used as a unique identifier and not a String. It also returns a versioning property that is relied on by mongo servers to resolve potential issues where clusters/shards are in use. This presented challenges for the current project in terms of maintaining the interoperability between data storage types and this was resolved by updating the Validation framework (joi-schemas.js) to accept this new ObjectId data type in addition to the string ids used in the other systems and also by permitting the version control property v as an optional value.

**2.2 API**
In parallel with the browser accessible version of PlaceMark there is a set of API endpoints which mirror the operations possible via the browser version of the app.
These are as follows:
Place Endpoints
-	GET - /api/placemark - Get all Places
-	POST - /api/placemark - Creates a new Place
-	DELETE - /api/placemark - Deletes all places.
-	GET - /api/placemark/{id} - Gets details related to a Place
-	DELETE - /api/placemark/{id} - Deletes a place.

Category Endpoints
-	GET - /api/placemark/category - Get all Categories
-	POST - /api/placemark/category - Creates a new Category
-	DELETE - /api/placemark/category - Deletes all categories.
-	POST - /api/placemark/category/{categoryId}/places - Adds a place to a category
-	DELETE - /api/placemark/category/{categoryId}/places/{placeId} - Deletes a place from a category.
-	GET - /api/placemark/category/{id} - Gets details related to a Category
-	DELETE - /api/placemark/category/{id} - Deletes a category.
-	GET - /api/placemark/category/{id}/places - Get Places tagged with Category

User Endpoints
-	GET - /api/users - Get all userApi
-	POST - /api/users - Create a new User
-	DELETE - /api/users - Deletes all users
-	GET - /api/users/{id} - Get a User by ID
-	POST - /api/users/authenticate

The Hapi-swagger library is used to provide user documentation of the requests formats expected and response payloads which will be returned. This is available at /documentation once the project is running. The Hapi-swagger generated documentation also provides a tool for exploring the API functionality. To avail of this in a live environment you must first obtain a JSON Web token (JWT) by supplying a valid set of user credentials via the POST /api/users/authenticate endpoint.

    {
      "email": "string",
      "password": "string"
    }

A demo live environment is available at: https://lit-plains-40353.herokuapp.com/documentation

To create an account use the following endpoint https://lit-plains-40353.herokuapp.com/documentation#/api/postApiUsers

**API Authentication**

With the exception of the `POST /api/users/authenticate` and the `POST /api/users`, routes are secured and require a valid JWT to obtain a successful response.

Implementing this functionality in the project relies on two packages:
-	https://github.com/dwyl/hapi-auth-jwt2
-	https://github.com/auth0/node-jsonwebtoken

The content of the JWT used mirrors the content available in the front-end cookie, storing the users email and their user id. Additional parameters can be added to the JWT by editing the /api/jwt-utils.js file to include them in the createToken() and decodeToken() methods.

To secure a route to require JWT authentication the JWT auth strategy must be included for the controller method associated with the route. This is accomplished by adding a new auth property to the controller method with a value of the following:

    auth: {
      strategy: "jwt",
    },



**2.4 Images**
The place model allows users to store image URLs related to individual placemarks.
To store these images a cloud-based hosting service, Cloudinary was selected. The implementation of the feature was guided by the process outlined in the following tutorial

>  https://reader.tutors.dev/#/lab/wit-hdip-comp-sci-2021-full-stack.netlify.app/topic-11-deployment/unit-1-deployment/book-3-cloudinary



**2.6 Test suite**

The project is accompanied by 2 test suites one focused on testing the API functionality and another focused on unit testing the integrity of the model

test
│
├── api\
│   ├── auth-api-test.js
│   ├── category-api-test.js
│   ├── place-api-test.js
│   ├── placemark-service.js
│   └── user-api-test.js
│
├── model\
│   ├── category-model-test.js
│   ├── places-model-test.js
│   └── users-model-test.js
│
├── fixtures.js
└── test-utils.js

The node `package.json` file is configured to run the test suites using: npm run test
Note that only test driven scripts are compatible and behaviour driven scripts if these are required will need to be refactored to a separate folder.

The testing suites in place rely on sample data provided in `fixtures.js`, this file should be modified according to your projects needs.

Further information on the mocha.js testing framework and chai assertion library is available in the respective documentation. Reference below.


**3.  Deployment / Installation**

**3.1 To deploy your own version of this repository on Glitch.**
1. Log into your Glitch account or create a new one (https://www.glitch.com)
2. Click [New Project] and [import from GitHub]
3. In the dialog that appears enter the repo address for this project: https://github.com/StephenSwantonIRL/PlaceMarkAssignment.git
4. Edit the Glitch project .env file to create the following variables adding your own API keys for the relevant services:

> cookie_name
> cookie_password
> db
> firebase_apiKey
> firebase_projectId
> firebase_storageBucket
> firebase_messaging
> SenderId firebase_appId
> cloudinary_name
> cloudinary_key
> cloudinary_secret

The `db` variable is the `mongodb+srv://` connection url for your mongoDB hosting. Firebase and cloudinary variables are available in your user account on these respective services.
5. Thats it!




**3.1 To deploy your own version of this repository on Heroku**
1. Use git clone https://github.com/StephenSwantonIRL/PlaceMarkAssignment.git to create your own  copy of the project.
2. If you don’t have the Heroku CLI install it now (Available here: https://devcenter.heroku.com/categories/command-line )
3. Log into your Heroku account by opening a terminal in your project folder and entering
   heroku login
4. Follow the login process
5. Once logged in type: heroku create
6. A new project will be created
7. In your browser navigate to the Heroku dashboard, find your newly created project and click it.
8. On the Heroku project settings tab enter your .env details in the Config Vars section
9. The variables required are as follows

> cookie_name
> cookie_password
> db
> firebase_apiKey
> firebase_projectId
> firebase_storageBucket
> firebase_messaging
> SenderId firebase_appId
> cloudinary_name
> cloudinary_key
> cloudinary_secret=


The `db` variable is the `mongodb+srv://` connection url for your mongoDB hosting. Firebase and cloudinary variables are available in your user account on these respective services.
10. Returning to your command line use the following command to push your project to the Heroku server:

    git push heroku master

Note that if you have previously changed your master branch to main the command will be

    git push heroku main

11. That’s it.




**4. References**
- Bulma - Documentation - https://bulma.io/documentation/
- Chai Assertion Library - Documentation, Assert - https://www.chaijs.com/api/assert/
- CKeditor - CKEditor Documentation - Getting Started - https://ckeditor.com/docs/ckeditor5/latest/installation/index.html
- Dotenv - dotenv - https://www.npmjs.com/package/dotenv
- GeeksforGeeks - Lodash _.cloneDeep() Method - https://www.geeksforgeeks.org/lodash-_-clonedeep-method/
- GeeksforGeeks - Lodash _.orderBy() Method - https://www.geeksforgeeks.org/lodash-_-orderby-method/
- Glitch Support Center - Can I change the version of node.js my project uses? - https://glitch.happyfox.com/kb/article/59-can-i-change-the-version-of-node-js-my-project-uses/
- Google - Firebase Documentation - Perform Simple and Compound Queries in Cloud FireStore - https://firebase.google.com/docs/firestore/query-data/queries
- Google - Firebase Documentation - Get Started with Cloud Firestore - https://firebase.google.com/docs/firestore/quickstart
- Handlebars - Handlebars API reference - https://handlebarsjs.com/api-reference/
- Handlebars - Handlebars Guide - https://handlebarsjs.com/guide/
- Heroku - Deploying with Git - https://devcenter.heroku.com/articles/git
- John Papa - Node.js Everywhere with Environment Variables! - https://medium.com/the-node-js-collection/making-your-node-js-work-everywhere-with-environment-variables-2da8cdf6e786
- Lowdb - https://github.com/typicode/lowdb
- Mastering JS - Updating Documents in Mongoose - https://masteringjs.io/tutorials/mongoose/update
- Mongoose JS - Documentation - Schema Types  - https://mongoosejs.com/docs/schematypes.html
- W3schools - JavaScript Fetch API - https://www.w3schools.com/js/js_api_fetch.asp
- W3schools - onclick Event - https://www.w3schools.com/jsref/event_onclick.asp
- WIT CompSci2021 - PlayTime version 5 - https://wit-hdip-comp-sci-2021-full-stack.netlify.app/topic-05-models/unit-2/book-playtime-0-5-0/archives/playtime-0.5.0.zip
- WIT CompSci2021 - Full Stack Web Development Course Material - Lab 5 - https://reader.tutors.dev/#/lab/wit-hdip-comp-sci-2021-full-stack.netlify.app/topic-05-models/unit-2/book-playtime-0-5-0
- WIT CompSci2021 - Full Stack Web Development Course Material - Lab 6 - https://reader.tutors.dev/#/lab/wit-hdip-comp-sci-2021-full-stack.netlify.app/topic-06-apis/unit-2/book-playtime-0-6-0
- WIT CompSci2021 - Full Stack Web Development Course Material - Lab 7 -https://reader.tutors.dev/#/lab/wit-hdip-comp-sci-2021-full-stack.netlify.app/topic-07-rest/unit-2/book-playtime-0-7-0
- WIT CompSci2021 - Full Stack Web Development Course Material - Lab 8 - https://reader.tutors.dev/#/lab/wit-hdip-comp-sci-2021-full-stack.netlify.app/topic-08-openapi/unit-2/book-playtime-0-8-0
- WIT CompSci2021 - Full Stack Web Development Course Material - Lab 9 - https://reader.tutors.dev/#/lab/wit-hdip-comp-sci-2021-full-stack.netlify.app/topic-09-jwt/unit-1/book-1
- WIT CompSci2021 - Full Stack Web Development Course Material - Lab 10 - https://reader.tutors.dev/#/lab/wit-hdip-comp-sci-2021-full-stack.netlify.app/topic-11-deployment/unit-1-deployment/book-1-mongo-atlas
- WIT CompSci2021 - Full Stack Web Development Course Material - Lab 11 - https://reader.tutors.dev/#/lab/wit-hdip-comp-sci-2021-full-stack.netlify.app/topic-11-deployment/unit-1-deployment/book-2-deployment
- WIT CompSci2021 - Full Stack Web Development Course Material - Lab 12 - https://reader.tutors.dev/#/lab/wit-hdip-comp-sci-2021-full-stack.netlify.app/topic-11-deployment/unit-1-deployment/book-3-cloudinary
- WIT CompSci2021 - Glitch Playlist version 5 - https://github.com/wit-hdip-comp-sci-2021/playlist-5
- YairEO –  Tagify - https://github.com/yairEO/tagify


**5.	Image & Content Credits**
J Comp on FreePiK  - Happy travel photo  - https://www.freepik.com/photos/happy-travel
Foer, J., Morton, E., Thuras, D., & Obscura, A. (2019). Atlas Obscura: An Explorer's Guide to the World's Hidden Wonders. Workman Publishing.






