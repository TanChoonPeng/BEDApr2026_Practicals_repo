## Practical 04: Part A - Lab Activity

### Lab Activity

In this part, you will be refactoring code based on your existing Books API code to adopt the **MVC architecture** and implement key robustness and security features.

### Task 1: Project Setup & Review

1.  **Create a New Project Directory:**

    - Navigate to your BED Practicals repository
    - Create a new folder: `Practical04`
    - Inside this folder, create another folder: `books-api-mvc-db`

2.  **Initialize the Project:**

    - Open the terminal in the `books-api-mvc-db` directory
    - Run `npm init -y` to initialize a new Node.js project
    - Install the required packages:
      ```bash
      npm install express mssql joi dotenv
      ```

3.  **Copy Configuration Files:**

    - Copy your `dbConfig.js` file from the previous practical
    - Create a new `.env` file to store environment variables:
      ```
      PORT=3000
      DB_USER=your_username
      DB_PASSWORD=your_password
      DB_SERVER=localhost
      DB_DATABASE=bed_db
      DB_PORT=1433
      ```
    - Update your `.gitignore` file and add `.env` to it

    **Explanation: Environment Variables and Security**

    - **`dotenv` Package:**

      - Loads variables from `.env` into `process.env`.
      - Keeps configuration separate from code (security, flexibility).
      - Required/configured early in your app (e.g., `require('dotenv').config();`).

    - **`.env` File:**

      - Root-level file (`.env`).
      - Stores `KEY=VALUE` config (ports, credentials, etc.).
      - Values accessible via `process.env.KEY` in your code.

    - **`.gitignore` File:**
      - Tells Git which files/folders to ignore.
      - Include `node_modules/` (large dependencies).
      - **Crucially, include `.env`**: Prevents accidental commit of sensitive credentials to version control.

4.  **Update dbConfig.js file:**

    - Update your `dbConfig.js` file with the environment variables:

    ```javascript
    module.exports = {
      user: process.env.DB_USER,
      password: process.env.DB_PASSWORD,
      server: process.env.DB_SERVER,
      database: process.env.DB_DATABASE,
      trustServerCertificate: true,
      options: {
        port: parseInt(process.env.DB_PORT), // Default SQL Server port
        connectionTimeout: 60000, // Connection timeout in milliseconds
      },
    };
    ```

5.  **Review the Previous Implementation:**
    - Examine your previous `app.js` file
    - Notice how all logic is contained within route handlers
    - Identify areas that would benefit from separation of concerns

### Task 2: Implement MVC Architecture

1.  **Create Folder Structure:**

    - Create the following folders within your project:
      - `models` (for database interaction logic)
      - `controllers` (for request handling logic)
      - `middlewares` (for validation middleware)

2.  **Set Up the Model:**

    - Create a file `models/bookModel.js` with the following content:

      ```javascript
      const sql = require("mssql");
      const dbConfig = require("../dbConfig");

      // Get all books
      async function getAllBooks() {
        let connection;
        try {
          connection = await sql.connect(dbConfig);
          const query = "SELECT id, title, author FROM Books";
          const result = await connection.request().query(query);
          return result.recordset;
        } catch (error) {
          console.error("Database error:", error);
          throw error;
        } finally {
          if (connection) {
            try {
              await connection.close();
            } catch (err) {
              console.error("Error closing connection:", err);
            }
          }
        }
      }

      // Get book by ID
      async function getBookById(id) {
        let connection;
        try {
          connection = await sql.connect(dbConfig);
          const query = "SELECT id, title, author FROM Books WHERE id = @id";
          const request = connection.request();
          request.input("id", id);
          const result = await request.query(query);

          if (result.recordset.length === 0) {
            return null; // Book not found
          }

          return result.recordset[0];
        } catch (error) {
          console.error("Database error:", error);
          throw error;
        } finally {
          if (connection) {
            try {
              await connection.close();
            } catch (err) {
              console.error("Error closing connection:", err);
            }
          }
        }
      }

      // Create new book
      async function createBook(bookData) {
        let connection;
        try {
          connection = await sql.connect(dbConfig);
          const query =
            "INSERT INTO Books (title, author) VALUES (@title, @author); SELECT SCOPE_IDENTITY() AS id;";
          const request = connection.request();
          request.input("title", bookData.title);
          request.input("author", bookData.author);
          const result = await request.query(query);

          const newBookId = result.recordset[0].id;
          return await getBookById(newBookId);
        } catch (error) {
          console.error("Database error:", error);
          throw error;
        } finally {
          if (connection) {
            try {
              await connection.close();
            } catch (err) {
              console.error("Error closing connection:", err);
            }
          }
        }
      }

      module.exports = {
        getAllBooks,
        getBookById,
        createBook,
      };
      ```

    - **Explanation of `models/bookModel.js`:**

      - This file represents the **Model** layer in the MVC pattern, dedicated solely to database interactions for the `Books` data.
      - It imports the necessary `mssql` library and your database configuration (`dbConfig.js`).
      - It defines **core `async` functions** (`getAllBooks`, `getBookById`, `createBook`) corresponding to specific data operations on the `Books` table.

      - **Function Breakdown:**

        - **`getAllBooks()`:**
          - Connects to the database.
          - Executes a simple `SELECT *` query to retrieve all book records.
          - Returns the result as an array of book objects (`result.recordset`).
        - **`getBookById(id)`:**
          - Connects to the database.
          - Retrieves a single book by its ID using a **parameterized query**: `SELECT id, title, author FROM Books WHERE id = @id`.
          - The `@id` parameter is safely added using `request.input("id", id)`, preventing SQL injection.
          - Returns the book object if found (`result.recordset[0]`), or `null` if no record matches the ID.
        - **`createBook(bookData)`:**
          - Connects to the database.
          - Inserts a new book using a **parameterized query** with `@title` and `@author`: `INSERT INTO Books (title, author) VALUES (@title, @author);`.
          - Includes `SELECT SCOPE_IDENTITY() AS id;` in the query to get the ID of the newly inserted row.
          - Calls `getBookById()` to retrieve and return the full, newly created book record.

      - **🔐 Security Note: Parameterized Queries**

        - All database interactions use **parameterized queries** (`request.input(...)` and `@parameterName`).
        - This is a fundamental security practice that prevents **SQL injection attacks** by keeping user-provided data strictly separate from the SQL command logic.

      - **🔁 Connection Management**

        - Each function is designed to handle its own database connection lifecycle.
        - A connection is established using `sql.connect(dbConfig)` at the start of the `try` block.
        - The `finally` block is used to ensure the connection is always closed (`connection.close()`), even if an error occurs during the query.
        - This pattern helps prevent connection leaks and ensures efficient resource management.

      - `module.exports` is used to make these database interaction functions available for the **Controller** file to import and use.

3.  **Set Up the Controller:**

    - Create a file `controllers/bookController.js` with the following content:

      ```javascript
      const bookModel = require("../models/bookModel");

      // Get all books
      async function getAllBooks(req, res) {
        try {
          const books = await bookModel.getAllBooks();
          res.json(books);
        } catch (error) {
          console.error("Controller error:", error);
          res.status(500).json({ error: "Error retrieving books" });
        }
      }

      // Get book by ID
      async function getBookById(req, res) {
        try {
          const id = parseInt(req.params.id);
          const book = await bookModel.getBookById(id);
          if (!book) {
            return res.status(404).json({ error: "Book not found" });
          }

          res.json(book);
        } catch (error) {
          console.error("Controller error:", error);
          res.status(500).json({ error: "Error retrieving book" });
        }
      }

      // Create new book
      async function createBook(req, res) {
        try {
          const newBook = await bookModel.createBook(req.body);
          res.status(201).json(newBook);
        } catch (error) {
          console.error("Controller error:", error);
          res.status(500).json({ error: "Error creating book" });
        }
      }

      module.exports = {
        getAllBooks,
        getBookById,
        createBook,
      };
      ```

    - **Explanation of `controllers/bookController.js`:**

      - This file defines the **endpoint handler functions** that act as the **Controller** layer in the MVC pattern.
      - It imports the `bookModel` to delegate database operations to the Model layer.
      - Each function (like `getAllBooks`, `getBookById`, `createBook`) is an `async` function designed to handle incoming Express requests, receiving the `req` (request) and `res` (response) objects.
      - These functions are responsible for:

        - Processing incoming HTTP requests (e.g., extracting data from `req.params` or `req.body`).
        - Calling the appropriate function in the `bookModel` to perform the required data operation.
        - Handling the results returned by the Model or catching any errors thrown.
        - Formatting the data and sending the final HTTP response back to the client using methods like `res.status().json()`.

      - **Function Breakdown:**

        - **`getAllBooks(req, res)`:**
          - Calls `bookModel.getAllBooks()` to fetch all book records from the database.
          - Sends the retrieved array of books as a JSON response with a default `200 OK` status.
          - Includes basic `try...catch` error handling to log database errors (propagated from the Model) and send a generic `500 Internal Server Error` response.
        - **`getBookById(req, res)`:**
          - Extracts the book ID from `req.params.id` and attempts to parse it as an integer.
          - Includes a basic check (`isNaN`) to return a `400 Bad Request` if the ID is invalid _before_ calling the Model. (Note: Comprehensive ID validation is ideally handled by middleware).
          - Calls `bookModel.getBookById(id)` to fetch the specific book.
          - If the Model returns `null` (book not found), sends a `404 Not Found` response.
          - If a book is found, sends the book object as a JSON response.
          - Catches other errors and sends a `500 Internal Server Error`.
        - **`createBook(req, res)`:**
          - Passes the request body (`req.body`) directly to `bookModel.createBook()` for insertion.
          - On successful creation (indicated by the Model returning the new book object), sends a `201 Created` status code along with the new book data as a JSON response.
          - Catches errors from the Model and sends a `500 Internal Server Error` response.

      - **🔗 Key Points:**

        - **Separation of Concerns:** Controllers manage the flow of requests and responses but delegate the data persistence logic entirely to the Model.
        - **Error Handling:** Each controller method uses a `try...catch` block to handle potential errors originating from the Model, logging them server-side and sending a consistent error response to the client. (A global error handler middleware is typically added in `app.js` for a more robust solution).
        - **Input Processing & Response Formatting:** Controllers handle parsing incoming data (`req.params`, `req.body`) and formatting the outgoing response (`res.json`, `res.status`).

      - `module.exports` makes these controller functions available for use in your main application file (`app.js`) when defining routes.

4.  **Create the Main Application File:**

    - Create a file `app.js` with the following content:

      ```javascript
      const express = require("express");
      const sql = require("mssql");
      const dotenv = require("dotenv");
      // Load environment variables
      dotenv.config();

      const bookController = require("./controllers/bookController");

      // Create Express app
      const app = express();
      const port = process.env.PORT || 3000;

      // Middleware
      app.use(express.json()); // Parse JSON request bodies
      app.use(express.urlencoded({ extended: true })); // Parse URL-encoded request bodies

      // Routes for books
      // Link specific URL paths to the corresponding controller functions
      app.get("/books", bookController.getAllBooks);
      app.get("/books/:id", bookController.getBookById);
      app.post("/books", bookController.createBook);

      // Start server
      app.listen(port, () => {
        console.log(`Server running on port ${port}`);
      });

      // Graceful shutdown
      // Listen for termination signals (like Ctrl+C)
      process.on("SIGINT", async () => {
        console.log("Server is gracefully shutting down");
        // Close any open connections
        await sql.close();
        console.log("Database connections closed");
        process.exit(0); // Exit the process
      });
      ```

    - **Explanation of `app.js`:**
      - This is the **entry point** of your Express application, responsible for setting up the server and routing.
      - It imports necessary packages (`express`, `mssql`, `dotenv`).
      - `dotenv.config()` loads environment variables from your `.env` file, making sensitive configuration available via `process.env`.
      - Middleware is applied using `app.use()` to handle incoming request bodies (`express.json`, `express.urlencoded`).
      - Routes are defined using `app.get`, `app.post`, etc., mapping specific URL paths to the corresponding functions in the `bookController`.
      - `app.listen()` starts the web server on the configured port.
      - The `process.on('SIGINT', ...)` block implements a **graceful shutdown**, ensuring database connections are closed cleanly when the server receives a termination signal (like when you press Ctrl+C in the terminal).

### Task 3: Implement Input Validation Middleware

1.  **Create the Validation Middleware:**

    - Create a file `middlewares/bookValidation.js` with the following content:

      ```javascript
      const Joi = require("joi"); // Import Joi for validation

      // Validation schema for books (used for POST/PUT)
      const bookSchema = Joi.object({
        title: Joi.string().min(1).max(50).required().messages({
          "string.base": "Title must be a string",
          "string.empty": "Title cannot be empty",
          "string.min": "Title must be at least 1 character long",
          "string.max": "Title cannot exceed 50 characters",
          "any.required": "Title is required",
        }),
        author: Joi.string().min(1).max(50).required().messages({
          "string.base": "Author must be a string",
          "string.empty": "Author cannot be empty",
          "string.min": "Author must be at least 1 character long",
          "string.max": "Author cannot exceed 50 characters",
          "any.required": "Author is required",
        }),
        // Add validation for other fields if necessary (e.g., year, genre)
      });

      // Middleware to validate book data (for POST/PUT)
      function validateBook(req, res, next) {
        // Validate the request body against the bookSchema
        const { error } = bookSchema.validate(req.body, { abortEarly: false }); // abortEarly: false collects all errors

        if (error) {
          // If validation fails, format the error messages and send a 400 response
          const errorMessage = error.details
            .map((detail) => detail.message)
            .join(", ");
          return res.status(400).json({ error: errorMessage });
        }

        // If validation succeeds, pass control to the next middleware/route handler
        next();
      }

      // Middleware to validate book ID from URL parameters (for GET by ID, PUT, DELETE)
      function validateBookId(req, res, next) {
        // Parse the ID from request parameters
        const id = parseInt(req.params.id);

        // Check if the parsed ID is a valid positive number
        if (isNaN(id) || id <= 0) {
          // If not valid, send a 400 response
          return res
            .status(400)
            .json({ error: "Invalid book ID. ID must be a positive number" });
        }

        // If validation succeeds, pass control to the next middleware/route handler
        next();
      }

      module.exports = {
        validateBook,
        validateBookId,
      };
      ```

    - **Explanation of `middlewares/bookValidation.js`:**
      - This file defines **reusable middleware functions** for input validation, using the `joi` library.
      - `bookSchema`: Defines the expected structure and constraints for book data (`title`, `author`). Custom `.messages()` provide user-friendly error feedback.
      - `validateBook(req, res, next)`:
        - Takes `req.body` and validates it against `bookSchema` using `Joi.validate()`.
        - `abortEarly: false` ensures _all_ validation errors are captured, not just the first one.
        - If errors occur, it formats them into a single string and sends a `400 Bad Request` response, stopping the request cycle.
        - If valid, it calls `next()` to pass the request to the next handler (the controller function).
      - `validateBookId(req, res, next)`:
        - Parses the `id` from `req.params`.
        - Checks if the ID is a valid positive integer.
        - Sends a `400 Bad Request` if invalid.
        - Calls `next()` if valid.
      - `module.exports` makes these middleware functions available to `app.js` for use in routes.

2.  **Update the routes in app.js file:**

    - Ensure your routes in `app.js` file are updated to include the validation middleware as shown:

    ```javascript
    const express = require("express");
    const sql = require("mssql");
    const dotenv = require("dotenv");
    // Load environment variables
    dotenv.config();

    const bookController = require("./controllers/bookController");
    const {
      validateBook,
      validateBookId,
    } = require("./middlewares/bookValidation"); // import Book Validation Middleware

    // Create Express app
    const app = express();
    const port = process.env.PORT || 3000;

    // Middleware (Parsing request bodies)
    app.use(express.json()); // Parse JSON request bodies
    app.use(express.urlencoded({ extended: true })); // Parse URL-encoded request bodies
    // --- Add other general middleware here (e.g., logging, security headers) ---

    // Routes for books
    // Apply middleware *before* the controller function for routes that need it
    app.get("/books", bookController.getAllBooks);
    app.get("/books/:id", validateBookId, bookController.getBookById); // Use validateBookId middleware
    app.post("/books", validateBook, bookController.createBook); // Use validateBook middleware
    // Add routes for PUT/DELETE if implemented, applying appropriate middleware

    // Start server
    app.listen(port, () => {
      console.log(`Server running on port ${port}`);
    });

    // Graceful shutdown
    process.on("SIGINT", async () => {
      console.log("Server is gracefully shutting down");
      await sql.close();
      console.log("Database connections closed");
      process.exit(0);
    });
    ```

### Task 4: Ensure Parameterized Queries

Verify that all your database interactions use **Parameterized Queries** to prevent **SQL injection**.

- **Review Model Functions:** Go back to your `bookModel.js` file.
- **Check All Queries with Variables:** Examine every SQL query that includes data from variables (especially data coming from user input via the controller, like `bookId`, `bookData.title`, `bookData.author`).
- **Verify Parameterization:** Ensure that for every such variable, you are using the `request.input('parameterName', variable)` method and referencing the parameter in the query string with `@parameterName`.

### Task 5: Testing with Postman

Thoroughly test your refactored and enhanced API using **Postman**.

- Run your Node.js application (`node app.js`).
- Test all implemented **CRUD endpoints** (`GET /books`, `GET /books/:id`, `POST /books`) to ensure they still work correctly after refactoring.
- **Test Validation:** Send `POST /books` requests with invalid or missing data to verify that your validation middleware intercepts them and returns a **400 Bad Request** response with the expected error message.
- Verify **graceful shutdown** (`Ctrl+C`) still works.

### Task 6: Implement Update and Delete Books

Do it yourself! Implement Update and Delete Books endpoints including the validation middleware based on the refactored code.

- Test the implemented endpoints (`PUT /books/:id`, `DELETE /books/:id`) to ensure they work correctly.
- **Test Validation:** Send `PUT /books` requests with invalid or missing data to verify that your validation middleware intercepts them and returns a **400 Bad Request** response with the expected error message.

#### End of Lab Activity
