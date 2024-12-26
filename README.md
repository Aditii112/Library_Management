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
    # Create a book
    @app.route('/books', methods=['POST'])
    def create_book():
        new_book={'id':len(books)+1, 'title':request.json['title'], 'author':request.json['author']}
    books.append(new_book)
    return new_book

    Create a New Book (POST /books)
    Allows users/members to add a new book to the library.
    Accepts JSON input with book details (title, author, published_date, genre, status).
    Generates a new ID for the book, adds it to the books dictionary, and returns the newly created book.

2. Retrieve All Books (GET /books)
   # Get all books (retrieve the book)
   @app.route('/books', methods=['GET'])
   def get_books():
      return books

   Fetches all books, with optional pagination support.
   Retrieve a Book by ID. 
   Fetches details of a specific book using its ID.
   Searches for the book by its ID.
   Returns the book if found, or an error message if not.

3. Update a Book (PUT /books/<int:book_id>)
    Update a book
    @app.route('/books/<int:book_id>', methods=['PUT'])
    def update_book(book_id):
        for book in books:
            if book['id']==book_id:
                book['title']=request.json['title']
                book['author']=request.json['author']
                return book 
        return {'error':'Book not found'}

   Allows users to update details of an existing book by its ID.
   Takes a JSON payload with updated book information.
   Updates the book details if the book exists, otherwise returns an error.

4. Delete a Book (DELETE /books/<int:book_id>)
   # Delete a book
    @app.route('/books/<int:book_id>', methods=['DELETE'])
    def delete_book(book_id):
        for book in books:
            if book['id']==book_id:
                books.remove(book)
                return {"data":"Book Deleted Successfully"}
    
        return {'error':'Book not found'}

   Deletes a book from the books dictionary using its ID.
   Returns a success message if deleted, or an error message if the book does not exist.

5. Search by Title or Author.
   # Get or Search a specific book by title 
   @app.route('/books/<int:book_title>', methods=['GET'])
   def get_book_by_title(book_title):
       for book in books:
           if book['title']==book_title:
              return book
    return {'error':'Book not found'}
   
    Search by Title (GET /books/title/<string:book_title>)
    Searches for books based on the title.

   # Get a specific book by Author's name
   @app.route('/books/<int:book_author>', methods=['GET'])
   def get_book_by_author(author_name):
       for book in books:
           if book['author']==author_name:
              return book
    return {'error':'Book not found'}
   
   Search by Author (GET /books/author/<string:author_name>)
   Searches for books based on the author’s name.

## Pagination
Pagination Helper Function:
# Pagination helper function
def paginate_data(data, page, limit):
    start = (page - 1) * limit
    end = start + limit
    return data[start:end]

Used to manage large datasets by limiting the number of books displayed per page.
GET Books with Pagination:
# GET books with pagination
@app.route('/books', methods=['GET'])
def get_books_by_id():
    page = int(request.args.get('page', 1))
    limit = int(request.args.get('limit', 10))
    paginated_books = paginate_data(books, page, limit)
    return jsonify({
        "page": page,
        "limit": limit,
        "total_books": len(books),
        "books": paginated_books
    }), 200

Handles requests for books with pagination using page and limit query parameters.

## Token-based Authentication
Generate Token:
# In-memory token storage
tokens = {}
# Generate a simple token
def generate_token(username):
    token = hashlib.sha256(f"{username}{time.time()}".encode()).hexdigest()
    tokens[token] = username
    return token
    
Creates a secure token for users who successfully authenticate.
Token Authentication Decorator:
# Authentication decorator
def token_required(func):
    def wrapper(*args, **kwargs):
        token = request.headers.get('Authorization')
        if not token or token not in tokens:
            return jsonify({"message": "Token is invalid or missing"}), 401
        return func(*args, **kwargs)
    wrapper.__name__ = func.__name__  # Preserve function name
    return wrapper
    
Protects API endpoints by requiring a valid token.
Login Endpoint:
Provides the functionality to generate a token upon successful login.
# Login endpoint to generate token
@app.route('/auth/login', methods=['POST'])
def login():
    data = request.get_json()
    if data['username'] == 'admin' and data['password'] == 'password123':
        token = generate_token(data['username'])
        return jsonify({"token": token}), 200
    return jsonify({"message": "Invalid credentials"}), 401

File Upload Functionality
Allowed File Types:
Checks if the uploaded file is among the allowed types (png, jpg, jpeg).
def allowed_file(filename):
    ALLOWED_EXTS = ['png', 'jpg', 'jpeg']
    return '.' in filename and filename.rsplit('.', 1)[1].lower() in ALLOWED_EXTS


# Run the flask App
if __name__ == '__main__':
    app.run(debug=True)
