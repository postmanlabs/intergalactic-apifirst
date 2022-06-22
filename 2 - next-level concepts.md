# Next Level Concepts

This session will be a work-along / follow-along workshop where your instructors will lead you through the design of an API using OpenAPI, while staying centered around the API-First mindset.

The information we present are the recommendations of the Postman team, and you can adapt them as applicable for your team in everyday practice.

If you find that you're working ahead on the material, feel free to explore additional concepts about OpenAPI that you could also build into your API specification.

### We Recommend Typing Things

We are providing samples here which you can copy/paste if you like, but for the best learning experience, we recommend typing the specification along with us. If you're concerned about losing track of what we're typing, we'll do our best to slow down the pace of our own typing for people attending the event today.

### If you get stuck, that's okay!

If you have trouble implementing something, [please ask for help](./getting-help.md).


### This Work Will Carry into the Afternoon Sessions

The afternoon sessions will be based on a copy of this OpenAPI specification that we're about to bulid.

If you feel behind or stuck on something, we'll provide everyone with an OpenPAI Specification to use in the afternoon so we're all starting at the same place.

---

## The Theme of the API: Books

We're going to build an API about books. 

We need to think about what a RESTful API would look like for endpoints that will fetch one book, many books, maybe a search feature, adding a new book, maybe even deleting a book.

### Attributes

When you think of a book, any book that is a favorite, what attributes come to mind? Title, author, description ... what else could you imagine as an attribute?

Are there limitations on those attributes that we should be concerned with, such as minimum or maximum lengths on string data, or range limits on numeric data? What about data which doesn't have a simple data type, something like the price of the book which may need to be stored in a currency that isn't easily converted to/from a floating point number?

Which of these attributes are *required*, and which ones are "nice to have"? What do we do if a user does not enter something that is required, or tries to send information we're not expecting?

### The Design

We need to design things using "natural language" so we can have good communication with our teams, without technology getting in the way.

One way to design this would be a "canvas" or "map" of the inputs, outputs, resources, and considerations for which user can access something.

This will inform us of the kinds of functions and operations we need to define, such as fetching a book, or deleting a book.

#### Think about what kind of URI path we might need to fetch all books, or to fetch one book

If you were designing an API, or expecting to use an API, for fetching a list of books, or for fetching a single book, what would that URI path look like?

Examples:

- /books
- /books/:id

## Building an OpenAPI Specification for our API

Now that we have a few things planned for our API, we can start to form our OpenAPI Specification file.

We're going to use Postman to write the OpenAPI Specification, but you can use whichever plaintext editor you like, including code editors or text document editors. As long as you can save the file as "plain text" later, it should work fine. If you're not sure, ask for help.

### Getting Started: the API, version, and information block

The title or name of our API should define the "boundary" or "scope" of the API. We want it specific enough that we know what the API can do, but flexible enough to add onto it later, as long as it fits within the overall "theme" of the API.

We also need to definte the servers, with API versioning in the path for best practice.

```yaml
openapi: 3.0.3

info:
  title: Bookshop
  version: "1.0"
  description: |
    ![OpenAPI Logo](https://avatars3.githubusercontent.com/u/16343502?v=3&s=100)

    A demo _API_ to **discover** the **_[OpenAPI Specification](https://www.openapis.org/)_**.

servers:
  - description: The base path for all resources
    url: /bookshop/v1

paths: {}
```
(full description of these portions of the file can be found in [this file](./openapi-books/01-bookshop-initiating-api.openapi.yaml))

### Building a path to fetch all books, and its response

Questions to consider:
- Which path should we start with?
- What kind of HTTP method (GET, POST, etc) should we use?
- How should we structure the response?
- Which status code is more applicable here?

```yaml
paths:
  /books:
    get:
      summary: Get/Search books
      operationId: searchBooks
      responses:
        "200":
          description: Books matching search parameters
```
(full description of these portions of the file can be found in [this file](./openapi-books/02-bookshop-search-books.openapi.yaml))

Other considerations, for example:
- should the endpoint to fetch all books contain every detail about every book, or could we send a smaller summary?
- what are the benefits/implications of this choice?

#### Can/Should we extend this path for a search operator?

What do we send back if no results are found?

What are best practices around status codes to return for a scenario where the endpoint was called successfully but the result list was empty? What about a scenario where we were trying to find one specific book?

```yaml
paths:
  /books:
    get:
      summary: Search books
      operationId: searchBooks
      responses:
        "200":
          description: Books matching search parameters
          content:
            application/json:
                schema:
                  type: object  
                  properties:
                    data:
                      type: array
                      minItems: 0
                      maxItems: 100
                      items:
                        properties:
                          id:
                            type: string
                            pattern: (978|979)([0-9]{10})
                            description: Book's [ISBN 13](https://www.isbn-international.org/content/what-isbn) (a standard book identifier) 
                          isbn:
                            type: string
                            pattern: (978|979)([0-9]{10})
                            description: Book's [ISBN 13](https://www.isbn-international.org/content/what-isbn) (a standard book identifier) 
                          title:
                            type: string
                            minLength: 1
                            maxLength: 100
                          price:
                            properties:
                              value:
                                type: number
                                minimum: 0
                              currency:
                                type: string
                                description: An ISO 4217 currency code
                                minLength: 3
                                maxLength: 3
                                enum:
                                  - USD
                                  - EUR
                            required:
                              - value
                              - currency
                          authors:
                            type: array
                            minItems: 1
                            items:
                              properties:
                                id:
                                  type: string
                                  description: A unique id identifying an author
                                firstName:
                                  type: string
                                middleName:
                                  type: string
                                lastName:
                                  type: string
                              required:
                                - id
                                - firstName
                                - lastName
                          stars:
                            type: number
                            exclusiveMinimum: true
                            minimum: 0
                            maximum: 5
                        required:
                          - id
                          - isbn
                          - title
                          - price
                          - authors
```
(full description of these portions of the file can be found in [this file](./openapi-books/03-bookshop-search-books-data.openapi.yaml))


### Building a new path to add a book to the database

What kind of data MUST the user send? Which data is optional, if any?

What kind of response do we return? What are the benefits/implications of this choice?

What do we send if a user doesn't give us correct information?

```yaml
    post:
      summary: Add a book
      operationId: addBook
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/BookCreate"
      responses:
        "201":
          description: Book added
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Book"
        "400":
          description: Incorrect data provided
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Errors"
```

### Error Handling

This is a good point to pause and think about our error responses, and how to make "user-friendly" errors. There's a balance here, as well, between making the error message very user-friendly and making it "lightweight" on the implementation for which, if any, information we provide in the error response.

As we start to define our error response, maybe we should consider building this as a reusable "component" in OpenAPI so all of our error messages look the same.

```yaml
paths:
  /books:
    get:
      summary: Search books
      operationId: searchBooks
      responses:
        "200":
          description: Books matching search parameters
          content:
            application/json:
                schema:
                  $ref: "#/components/schemas/Books"

components:
  schemas:
    BookId:
      type: string
      pattern: (978|979)([0-9]{10})
      description: Book's [ISBN 13](https://www.isbn-international.org/content/what-isbn) (a standard book identifier)
    Amount:
      properties:
        value:
          type: number
          minimum: 0
        currency:
          type: string
          description: An ISO 4217 currency code
          minLength: 3
          maxLength: 3
          enum:
            - USD
            - EUR
      required:
        - value
        - currency
    Authors:
      type: array
      minItems: 1
      items:
        properties:
          id:
            type: string
            description: A unique id identifying an author
          firstName:
            type: string
          middleName:
            type: string
          lastName:
            type: string
        required:
          - id
          - firstName
          - lastName
    BookSummary:
      properties:
        id:
          $ref: "#/components/schemas/BookId"
        isbn:
          $ref: "#/components/schemas/BookId"
        title:
          type: string
          minLength: 1
          maxLength: 100
        price:
          $ref: "#/components/schemas/Amount"
        authors:
          $ref: "#/components/schemas/Authors"
        stars:
          type: number
          exclusiveMinimum: true
          minimum: 0
          maximum: 5
      required:
        - id
        - isbn
        - title
        - price
        - authors
    Books:
      type: object  
      properties:
        data:
          type: array
          minItems: 0
          maxItems: 100
          items:
            $ref: "#/components/schemas/BookSummary"
```
(full description of these portions of the file can be found in [this file](./openapi-books/07-bookshop-errors.openapi.yaml))

### Fetching a Single Book

How would you add the path for adding a single book?

Let's discuss how much data we should return in the response. Is it enough to send back a 200-series acknowledgement that we've done some work? What if we're still processing some background data like uploading a photo of the book cover?

```yaml
paths:

  /books/{bookId}:
    parameters:
      - name: bookId
        in: path
        required: true
        schema:
          $ref: "#/components/schemas/BookId"
        description: Book's [ISBN 13](https://www.isbn-international.org/content/what-isbn) (a standard book identifier)
    get:
      summary: Get a book detailed information
      operationId: readBook
      responses:
        "200":
          description: Book matching the provided `bookId`
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Book"
        "401":
          $ref: "#/components/responses/AuthenticationError"
        "404":
          $ref: "#/components/responses/BookNotFoundError"
        "500":
          $ref: "#/components/responses/UnexpectedError"
```
(full description of these portions of the file can be found in [this file](./openapi-books/06-bookshop-add-book.openapi.yaml))

### Adding an endpoint to Delete a book

Typically a very fast operation, with a 204 response which indicates no body data.

But that raises the question of who is allowed to remove books from our database?

```yaml
paths:
...
  /books/{bookId}:
    parameters:
      - name: bookId
        in: path
        required: true
        schema:
          $ref: "#/components/schemas/BookId"
        description: Book's [ISBN 13](https://www.isbn-international.org/content/what-isbn) (a standard book identifier)
    get:
      ...

    delete:
      summary: Delete a book
      operationId: deleteBook
      responses:
        "204":
          description: Book has been deleted
        "401":
          $ref: "#/components/responses/AuthenticationError"
        "404":
          $ref: "#/components/responses/BookNotFoundError"
        "500":
          $ref: "#/components/responses/UnexpectedError"
```
(full description of these portions of the file can be found in [this file](./openapi-books/10-bookshop-delete-a-book.openapi.yaml))


### SECURITY !!

Let's pause to discuss things like OAuth and scopes, and the differences between authentication and authorization.

Authentication: Who are you?

Authorization: What are you allowed to do?

OAuth isn't enough, OAuth is just the specification for the "open authentication" standard. It just tells us that we're allowed to access the API, but not which endpoints we're allowed to use.

Did we miss something in our design??

Good thing we haven't actually built any of the implementation software yet!!

## Other Considerations

In our workshop today we're exploring the idea of building an API from nothing. But what do you do when you already have an API? Or at a minimum, if you've already got a database full of data for which you want to design an API?

In this scenario, we still want to design the contract. The "vocabulary" or terminology that your business already uses can influence the kinds of attributes you will design into your API, or show you where you have an opporunity to introduce additional content to design a better user experience. It may also uncover redundancies in your data as well.

One approach that we've seen is to build an API "in front of" your existing API, that conforms to your new standards. With that in place, you have a facade that can answer queries from users, and offer you the ability to refactor/rebuild the infrastructure behind it.

---

## Next Steps in Postman

Now that we have some working OpenAPI specification work complete, let's take a quick look at what Postman can do with that specification?

#### If you need a working OpenAPI Specification file

If you've fallen behind this morning, have trouble saving your OpenAPI Specification as a plain text file, or run into an error, you can access [working examples](./openapi-books/) for everything we have built this morning for the remainder of this workshop.

## Get Into Postman

If you've been building your OpenAPI Specification in Postman, skip to the [next section](#now-that-we-have-everyone-in-postman)

If you've been editing your OpenAPI Specification file in a different editor, we need to get that into Postman. We'll spend a few moments looking at how to import that into Postman.

### Click that "Import" button

Upload your YAML file. Tell Postman to link the collection as Documentation for now.

## Now that we have everyone in Postman...

In the upper right corner we're going to click on the "Generate Collection" button to tell Postman to build out a collection of requests for each of our defined endpoints.

These will appear as a collection within the API section of Postman, and a mirror copy will appear in the Collections area as well. As you update one area, the other copy of that request will update as well. For example, if you update a request in the Collections area, the same request will be modified in the APIs area of Postman.

## Adding Examples

Our OpenAPI Specification doesn't include any definitions for examples on our endpoints right now, but we can go add some examples of our own.

The existing requests will go do our defined server URL, but we can change it, temporarily, to "postman-echo.com" with a URI path that matches our HTTP method (ie, GET requests will go to `postman-echo.com/get`, POST requests will go to `postman-echo.com/post`, etc)

Now we can send those requests to the Postman Echo server, and we can save the response, and alter it according to our design specification. What did we want our responses to look like?

What should a response look like for a single book?


## Now we can build tests

Once we have an idea of what our responses should look like, we can start to build out some basic testing. We'll go into more detail in the afternoon sessions.

For now, we'll add a simple test to our "get one book" endpoint that ensures we get a successful response.


## Validating Our Schema

With our definition in place, we can use Postman to validate that our responses match our desired schema.  Much of this is handled with Postman without having to write any additional tests.

Our afternoon sessions may cover more extensive software testing and validations.


---

## Our Recommendations

We made several recommendations throughout this session and we want to review them here in the notes.

### Making a Book ID

Do we use our own database-generated ID for a book to be used later in the URI path, or generate something like a UUID, or use the ISBN number? We recommended considerations for using the ISBN as a unique ID for several purposes including your database table unique key.

### Authors are Data, too

When a user enters a book, we recommended inputing an author "ID" value so we don't have users entering the same author data into the system over and over.

### Currencies are not "universal"

We recommend that if you're going to store the price of an item that you should not necessarily assume a floating point number for something like US Dollars and cents, but perhaps using an ISO standard format of some sort, or storing the prices in a separate table.

### Return Objects that you can Extend

For responses that return data, like a list of books, we recommend returning an object, which contains a key, which points to the list of results. This allows the base object to be extended later, if necessary.

Likewise, the "book" object could return its unique ID as a key in the object, and a "data" attribute which you can extend later to contain the attributes of the book.

### Amount of Data returned depends on the endpoint

We recommended that for an endpoint that returns a list of books to send a shorter summary of the object and not every detail. There is a balance here between system processing time and the user having to make another call back to the system to fetch other book data.

However, for a single book, returning more content could be a nice user experience, provided that the system can process the data quickly enough and also return the unique identifier.

### Creating an object should return a reference

While it's much easier to return a "202" status to indicate we received the user's data and we're acting upon that data in the background, a better status would return the unique identifier of the object as a minimum.

