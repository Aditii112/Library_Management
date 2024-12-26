# Library Management System API

A Flask-based API for managing books in a library.

## Features
- CRUD operations for books
- Search by title or author
- Pagination support
- Token-based authentication
- File upload functionality

## API Endpoints
- `GET /books` - Retrieve all books (supports pagination).
- `POST /books` - Add a new book.
- `GET /books/<id>` - Retrieve a book by ID.
- `GET /books/title/<title>` - Search a book by title.
- `GET /books/author/<author>` - Search a book by author.

## Authentication
- Login: `POST /auth/login`

## DESCRIPTION ABOUT EACH CRUD OPERATIONS
1. CREATE Description:
    Create a New Book (POST /books)
    Allows users/members to add a new book to the library.
    Accepts JSON input with book details (title, author, published_date, genre, status).
    Generates a new ID for the book, adds it to the books dictionary, and returns the newly created book.

2. Retrieve All Books (GET /books)
   Fetches all books, with optional pagination support.
   Retrieve a Book by ID. 
   Fetches details of a specific book using its ID.
   Searches for the book by its ID.
   Returns the book if found, or an error message if not.

3. Update a Book (PUT /books/<int:book_id>)
   Allows users to update details of an existing book by its ID.
   Takes a JSON payload with updated book information.
   Updates the book details if the book exists, otherwise returns an error.

4. Delete a Book (DELETE /books/<int:book_id>)
   Deletes a book from the books dictionary using its ID.
   Returns a success message if deleted, or an error message if the book does not exist.

5. Search by Title or Author.
   Search by Title (GET /books/title/<string:book_title>)
   Searches for books based on the title.
   Search by Author (GET /books/author/<string:author_name>)
   Searches for books based on the author’s name.

## Pagination
Pagination Helper Function:
Used to manage large datasets by limiting the number of books displayed per page.
GET Books with Pagination:
Handles requests for books with pagination using page and limit query parameters.

## Token-based Authentication
Generate Token:
Creates a secure token for users who successfully authenticate.
Token Authentication Decorator:
Protects API endpoints by requiring a valid token.
File Upload Functionality
Allowed File Types:
Checks if the uploaded file is among the allowed types (png, jpg, jpeg).


