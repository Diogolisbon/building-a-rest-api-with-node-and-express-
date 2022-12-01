# **Building a REST API with Node and Express**

## **Introduction**

REST APIs are an industry-standard way for web services to send and receive data. They use HTTP request methods to facilitate the request-response cycle and typically transfer data using JSON, and more rarely - HTML, XML and other formats.


### **What We are Going to Build**

Let's create a simple app to store information about books. In this app, we will store information about the ISBN of the book, title, author, published date, publisher and number of pages.

Naturally, the basic functionality of the API will be CRUD functionality. We'll want to be able to send requests to it to create, read, update and delete Book entities. Of course, an API may do much more than this - provide users with an enpoint to get statistical data, summaries, call other APIs, etc.

Non-CRUD functionalities are application-dependent, and based on your project's nature, you'll probably have other endpoints. However, practically no project can go without CRUD.

To avoid making up book data - let's use a dataset from GitHub to get some sample details about books.

**Setting Up the Project**

First, let's initialize a new Node.js project:

1 **$ npm init**

2 **$ npm test**

3 **$ test npm start** 

4 **$ npm install --save express**

Now, let's start building a simple "Hello World" app. It'll have a single simple endpoint that just returns a message as a response to our request to get the home page.

5 **$ nano hello-world.js**

6 Then, let's import the Express framework within it:

**const express = require('express');**
Next, we'll want to instantiate the Express app:

**const app = express();**
And set our port:

**const port = 3000;**

7 Now, we can create a simple GET endpoint right beneath the boilerplate. When a user hits the endpoint with a GET request, the message "Hello World, from express" will be returned (and rendered in the browser or displayed on the console).

We'd like to set it to be on the home page, so the URL for the endpoint is /:

8 **app.get('/', (req, res) => {**
    **res.send('Hello World, from express');**
**});**

**app.listen(port, () => console.log(`Hello world app listening on port ${port}!`));**

9 Let's run the application and visit the only endpoint we have via our browser:

**$ node hello-world.js**
*Hello world app listening on port 3000!*

10 For now, to get started, we need to install a middleware called body-parser, which helps us decode the body from an HTTP request:
**$ npm install --save body-parser**
**$ npm install --save cors**

11 Now we can start building our app. Create a new file called book-api.js

copy-paste :

const express = require('express')
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
const port = 3000;

// Where we will keep books
let books = [];

app.use(cors());

// Configuring body parser middleware
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

app.post('/book', (req, res) => {
    ==**// We will be coding here**==
});

app.listen(port, () => console.log(`Hello world app listening on port ${port}!`));


As you can see, we can configure body-parser by importing it and passing it to the app.use method, which enables it as middleware to the Express app instance.

We will be using the books array to store our collection of books, simulating a database.

There are a few types of HTTP request body types. For an example, application/x-www-form-urlencoded is the default body type for forms, whereas application/json is something we'd use when requesting a resource using jQuery or backend REST client.

What the body-parser middleware will be doing is grabbing the HTTP body, decoding the information, and appending it to the req.body. From there, we can easily retrieve the information from the form - in our case, a book's information.

12 Inside the app.post method let's add the book to the book array:
app.post('/book', (req, res) => {
    const book = req.body;

    // Output the book to the console for debugging
    console.log(book);
    books.push(book);

    res.send('Book is added to the database');
});

13 Now, let's create a simple HTML form with the fields: ISBN, title, author, published date, publisher, and number of pages in a new file, say new-book.html.

We'll be sending the data to the API using this HTML form's action attribute:

<div class="container">
    <hr>
    <h1>Create New Book</h1>
    <hr>

    <form action="http://localhost:3000/book" method="POST">
        <div class="form-group">
            <label for="ISBN">ISBN</label>
            <input class="form-control" name="isbn">
        </div>

        <div class="form-group">
            <label for="Title">Title</label>
            <input class="form-control" name="title">
        </div>

        <!--Other fields-->
        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
</div>

Here, our **<form>** tag's attribute corresponds to our endpoint and the information we send with the submit button is the information our method parses and adds to the array. Note that the method parameter is ==POST==, just like in our API.

Clicking =="Submit"==, we're greeted with the our applications **console.log(book)** statement.

14 Now let's create an endpoint to get all the books from the API:

app.post('/book', (req, res) => {
    const book = req.body;

    // Output the book to the console for debugging
    console.log(book);
    books.push(book);

    res.send('Book is added to the database');
});

 ADD HERE :
 **app.get('/book', (req, res) => {**
    **res.json(books);**
**});** 

app.listen(port, () => console.log(`Hello world app listening on port ${port}!`));


15 Now let's create an HTML page to display these books in a user-friendly way.

This time around, we'll create two files **- book-list.html** which we'll use as a template and a **book-list.js** file which will hold the logic to updating/deleting books and displaying them on the page:

Let's start off with the template: 

<div class="container">
    <hr>
    <h1>List of books</h1>
    <hr>
    <div>
        <div class="row" id="books">
        </div>
    </div>
</div>

<div id="editBookModal" class="modal" tabindex="-1" role="dialog">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title">Edit Book</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
            </div>

            <div class="modal-body">
                <form id="editForm" method="POST">
                    <div class="form-group">
                        <label for="ISBN">ISBN</label>
                        <input class="form-control" name="isbn" id="isbn">
                    </div>

                    <div class="form-group">
                        <label for="Title">Title</label>
                        <input class="form-control" name="title" id="title">
                    </div>

                    <!--Other fields-->

                    <button type="submit" class="btn btn-primary">Submit</button>
                </form>
            </div>
        </div>
    </div>
</div>
<!--Our JS file-->
<script src="book-list.js"></script> 

With the template done, we can implement the actual logic to retrieve all books using browser-side JavaScript and our REST API:

const setEditModal = (isbn) => {
    // We will implement this later
}

const deleteBook = (isbn) => {
    // We will implement this later
}

const loadBooks = () => {
    const xhttp = new XMLHttpRequest();

    xhttp.open("GET", "http://localhost:3000/books", false);
    xhttp.send();

    const books = JSON.parse(xhttp.responseText);

    for (let book of books) {
        const x = `
            <div class="col-4">
                <div class="card">
                    <div class="card-body">
                        <h5 class="card-title">${book.title}</h5>
                        <h6 class="card-subtitle mb-2 text-muted">${book.isbn}</h6>

                        <div>Author: ${book.author}</div>
                        <div>Publisher: ${book.publisher}</div>
                        <div>Number Of Pages: ${book.numOfPages}</div>

                        <hr>

                        <button type="button" class="btn btn-danger">Delete</button>
                        <button types="button" class="btn btn-primary" data-toggle="modal"
                            data-target="#editBookModal" onClick="setEditModal(${book.isbn})">
                            Edit
                        </button>
                    </div>
                </div>
            </div>
        `

        document.getElementById('books').innerHTML = document.getElementById('books').innerHTML + x;
    }
}

loadBooks();
In the above script, we are sending a GET request to the endpoint http://localhost:3000/books to retrieve the books and then creating a Bootstrap card for every book to display it.


16  **Retrieving a Book by ISBN**

If we'd like to display a specific book to the user, we'll need a way to retrieve it from the database (or the array, in our case). This is always done by a key specific to that entity. In most cases, each entity has a unique id that helps us identify them.

In our case, each book has an ISBN which is unique by nature, so there's no need for another id value.

This is typically done by parsing the URL parameter for an id and searching for the book with the corresponding id.

For an example, if the ISBN is 9781593275846 the URL would look like, http://localhost:3000/book/9781593275846:

Lets add in **book-api.js**

app.get('/book', (req, res) => {
    res.json(books);
});

 **app.get('/book/:isbn', (req, res) => {**
    **// Reading isbn from the URL**
    **const isbn = req.params.isbn;**
});** 

Here, we're introduced to parametrized URLs. Since the ISBN depends on the book, there's potentially an infinite number of endpoints here. By adding a colon (:) to the path, we can define a variable, mapped to the variable isbn. So, if a user visits localhost:3000/book/5 the isbn parameter will be 5.

Now, using our endpoint, we can retrieve a single book:

app.get('/book/:isbn', (req, res) => {
    // Reading isbn from the URL
    const isbn = req.params.isbn;

    // Searching books for the isbn
    for (let book of books) {
        if (book.isbn === isbn) {
            res.json(book);
            return;
        }
    }

    // Sending 404 when not found something is a good practice
    res.status(404).send('Book not found');
});

Again restart the server, add a new book, and open localhost/3000/{your_isbn} and the application will return the book's information.

**Deleting Books**

When deleting entities, we typically delete them one by one to avoid big accidental data loss. To delete items, we use the HTTP DELETE method and specify a book using its ISBN number, just like how we retrieved it:

