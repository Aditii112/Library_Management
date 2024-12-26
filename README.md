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
## How to Run
1. Clone the repository:
