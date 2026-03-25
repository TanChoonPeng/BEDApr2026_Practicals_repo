# Practical 05: Part A - Lab Activity

## Lab Activity

In this part, you will add a simple HTML/JavaScript frontend to your Books API project, allowing users to interact with the API through a web page.

### Task 1: Project Setup and Directory Structure

1. Navigate to your `books-api-mvc-db` project folder from Practical 04. This is where you will continue working.
2. Create a new folder in the root of your project named `public`. This folder will hold all your static frontend files (HTML, CSS, client-side JavaScript).

### Task 2: Serve Static Files with `express.static`

Configure your Express application to serve the static files from the `public` folder.

1. Open your `app.js` file.

2. Require the built-in `path` module at the top of the file:

   ```javascript
   const path = require("path");
   ```

3. Add the `express.static` middleware. Place this middleware _before_ your API routes, so Express checks for static files first. A good place is after your body-parsing middleware (`express.json()`, `express.urlencoded()`).

   ```javascript
   // ... other requires and app setup ...

   app.use(express.json());
   app.use(express.urlencoded({ extended: true })); // Ensure extended is true for urlencoded

   // --- Serve static files from the 'public' directory ---
   // When a request comes in for a static file (like /index.html, /styles.css, /script.js),
   // Express will look for it in the 'public' folder relative to the project root.
   app.use(express.static(path.join(__dirname, "public")));

   // ... your API routes (e.g.,
   ```

**Explanation:**

- `path.join(__dirname, 'public')` creates the absolute path to your `public` folder. `__dirname` is a Node.js global variable that gives you the directory path of the currently executing script (`app.js`).

- `express.static()` is a built-in Express middleware that serves static files. It looks for files in the specified directory relative to the URL path requested by the client. For example, a request to `/index.html` will look for `index.html` inside the directory provided to `express.static`.

### Task 3: Create the Index View (`public/index.html`)

Create the main HTML file that will display the list of books and serve as the starting point for your frontend.

1.  Inside the `public` folder, create a new file named `index.html`.

2.  Create a client-side JavaScript file: Inside the `public` folder, create a new file named `script.js`.

3.  Add the basic HTML structure in `index.html`:

    ```html
    <!DOCTYPE html>

    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Books List</title>
        <style>
          body {
            font-family: sans-serif;
            margin: 20px;
          }
          #booksList {
            margin-top: 20px;
          }
          .book-item {
            border: 1px solid #ccc;
            padding: 10px;
            margin-bottom: 10px;
            border-radius: 5px;
          }
          .book-item h3 {
            margin-top: 0;
          }
          .book-item button {
            margin-right: 5px;
          }
          #message {
            margin-top: 15px;
            font-weight: bold;
          } /* Style for the message div */
        </style>
      </head>
      <body>
        <h1>All Books</h1>
        <p><a href="create.html">Add New Book</a></p>
        <button id="fetchBooksBtn">Refresh Books List</button>
        <div id="message"></div>
        <div id="booksList">Loading books...</div>

        <script src="script.js"></script>
      </body>
    </html>
    ```

### Task 4: Implement GET (Read All) in `public/script.js`

Write the JavaScript code to fetch the list of books from your API and display them in `index.html`.

1.  Open `public/script.js`.

2.  Write the asynchronous function to fetch and display books:

    ```javascript
    // Get references to the HTML elements you'll interact with:
    const booksListDiv = document.getElementById("booksList");
    const fetchBooksBtn = document.getElementById("fetchBooksBtn");
    const messageDiv = document.getElementById("message"); // Get reference to the message div
    const apiBaseUrl = "http://localhost:3000";

    // Function to fetch books from the API and display them
    async function fetchBooks() {
      try {
        booksListDiv.innerHTML = "Loading books..."; // Show loading state
        messageDiv.textContent = ""; // Clear any previous messages (assuming a message div exists or add one)

        // Make a GET request to your API endpoint
        const response = await fetch(`${apiBaseUrl}/books`);

        if (!response.ok) {
          // Handle HTTP errors (e.g., 404, 500)
          // Attempt to read error body if available, otherwise use status text
          const errorBody = response.headers
            .get("content-type")
            ?.includes("application/json")
            ? await response.json()
            : { message: response.statusText };
          throw new Error(
            `HTTP error! status: ${response.status}, message: ${errorBody.message}`
          );
        }

        // Parse the JSON response
        const books = await response.json();

        // Clear previous content and display books
        booksListDiv.innerHTML = ""; // Clear loading message
        if (books.length === 0) {
          booksListDiv.innerHTML = "<p>No books found.</p>";
        } else {
          books.forEach((book) => {
            const bookElement = document.createElement("div");
            bookElement.classList.add("book-item");
            // Use data attributes or similar to store ID on the element if needed later
            bookElement.setAttribute("data-book-id", book.id);
            bookElement.innerHTML = `
                        <h3>${book.title}</h3>
                        <p>Author: ${book.author}</p>
                        <p>ID: ${book.id}</p>
                        <button onclick="viewBookDetails(${book.id})">View Details</button>
                        <button onclick="editBook(${book.id})">Edit</button>
                        <button class="delete-btn" data-id="${book.id}">Delete</button>
                    `;
            booksListDiv.appendChild(bookElement);
          });
          // Add event listeners for delete buttons after they are added to the DOM
          document.querySelectorAll(".delete-btn").forEach((button) => {
            button.addEventListener("click", handleDeleteClick);
          });
        }
      } catch (error) {
        console.error("Error fetching books:", error);
        booksListDiv.innerHTML = `<p style="color: red;">Failed to load books: ${error.message}</p>`;
      }
    }

    // Placeholder functions for other actions (to be implemented later or in other files)
    function viewBookDetails(bookId) {
      console.log("View details for book ID:", bookId);
      // In a real app, redirect to view.html or show a modal
      // window.location.href = `view.html?id=${bookId}`; // Assuming you create view.html
      alert(`View details for book ID: ${bookId} (Not implemented yet)`);
    }

    function editBook(bookId) {
      console.log("Edit book with ID:", bookId);
      // In a real app, redirect to edit.html with the book ID
      window.location.href = `edit.html?id=${bookId}`; // Assuming you create edit.html
    }

    // Placeholder/Partial implementation for Delete (will be completed by learners)
    function handleDeleteClick(event) {
      const bookId = event.target.getAttribute("data-id");
      console.log("Attempting to delete book with ID:", bookId);
      // --- Start of code for learners to complete ---
      alert(
        `Attempting to delete book with ID: ${bookId} (Not implemented yet)`
      );
      // TODO: Implement the fetch DELETE request here
      // TODO: Handle success (204) and error responses (404, 500)
      // TODO: On successful deletion, remove the book element from the DOM
      // --- End of code for learners to complete ---
    }

    // Fetch books when the button is clicked
    fetchBooksBtn.addEventListener("click", fetchBooks);

    // Optionally, fetch books when the page loads
    // window.addEventListener('load', fetchBooks); // Or call fetchBooks() directly
    ```

3.  **Test:**

    - Run your Node.js application (`node app.js`).

    - Open your browser and navigate to `http://localhost:3000/index.html`.

    - Click the "Refresh Books List" button. You should see the list of books fetched from your API displayed on the page.

    - Click the "View Details" and "Edit" buttons – they should log messages to the browser console and trigger alerts (View Details) or attempt to navigate (Edit).

    - Click the "Delete" button – it should trigger the `handleDeleteClick` function and show the alert message.

### Task 5: Create the Create View (`public/create.html`)

Create an HTML file with a form to input new book data.

1.  Inside the `public` folder, create a new file named `create.html`.

2.  Create a client-side JavaScript file for this page: Inside the `public` folder, create `create-script.js`.

3.  Add the HTML form structure in `create.html`:

    ```html
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Create New Book</title>
        <style>
          body {
            font-family: sans-serif;
            margin: 20px;
          }
          form div {
            margin-bottom: 15px;
          }
          label {
            display: block;
            margin-bottom: 5px;
          }
          input[type="text"] {
            width: 300px;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 4px;
          }
          button {
            padding: 10px 15px;
            background-color: #5cb85c;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
          }
          button:hover {
            background-color: #4cae4c;
          }
          #message {
            margin-top: 15px;
            font-weight: bold;
          }
        </style>
      </head>
      <body>
        <h1>Create New Book</h1>
        <p><a href="index.html">Back to Books List</a></p>

        <form id="createBookForm">
          <div>
            <label for="title">Title:</label>
            <input type="text" id="title" name="title" required />
          </div>
          <div>
            <label for="author">Author:</label>
            <input type="text" id="author" name="author" required />
          </div>
          <button type="submit">Create Book</button>
        </form>

        <div id="message"></div>

        <script src="create-script.js"></script>
      </body>
    </html>
    ```

### Task 6: Implement POST (Create) in `public/create-script.js`

Write the JavaScript code to handle the form submission and send a POST request to create a new book via your API.

1.  Open `public/create-script.js`.

2.  Add an event listener to the form's submit event:

    ```javascript
    // Get references to the form and message elements:
    const createBookForm = document.getElementById("createBookForm");
    const messageDiv = document.getElementById("message");
    const apiBaseUrl = "http://localhost:3000";

    createBookForm.addEventListener("submit", async (event) => {
      event.preventDefault(); // Prevent the default browser form submission

      messageDiv.textContent = ""; // Clear previous messages

      // Collect data from the form inputs
      const titleInput = document.getElementById("title");
      const authorInput = document.getElementById("author");

      const newBookData = {
        title: titleInput.value,
        author: authorInput.value,
      };

      try {
        // Make a POST request to your API endpoint
        const response = await fetch(`${apiBaseUrl}/books`, {
          method: "POST", // Specify the HTTP method
          headers: {
            "Content-Type": "application/json", // Tell the API we are sending JSON
          },
          body: JSON.stringify(newBookData), // Send the data as a JSON string in the request body
        });

        // Check for API response status (e.g., 201 Created, 400 Bad Request, 500 Internal Server Error)
        const responseBody = response.headers
          .get("content-type")
          ?.includes("application/json")
          ? await response.json()
          : { message: response.statusText };

        if (response.status === 201) {
          messageDiv.textContent = `Book created successfully! ID: ${responseBody.id}`;
          messageDiv.style.color = "green";
          createBookForm.reset(); // Clear the form after success
          console.log("Created Book:", responseBody);
        } else if (response.status === 400) {
          // Handle validation errors from the API (from Practical 04 validation middleware)
          messageDiv.textContent = `Validation Error: ${responseBody.message}`;
          messageDiv.style.color = "red";
          console.error("Validation Error:", responseBody);
        } else {
          // Handle other potential API errors (e.g., 500 from error handling middleware)
          throw new Error(
            `API error! status: ${response.status}, message: ${responseBody.message}`
          );
        }
      } catch (error) {
        console.error("Error creating book:", error);
        messageDiv.textContent = `Failed to create book: ${error.message}`;
        messageDiv.style.color = "red";
      }
    });
    ```

3.  **Test:**

    - Run your Node.js application (`node app.js`).

    - Open your browser and navigate to `http://localhost:3000/create.html`.

    - Fill out the form and click "Create Book".

    - Check the message area on the page and your browser's developer console for the result.

    - Try submitting with missing data to test your backend validation (should return 400).

    - Go back to `index.html` and refresh the list to see the newly created book.

**Explanation:**

The `create-script.js` JavaScript file is responsible for handling the user interaction on the `create.html` page and communicating with your backend API to create a new book.

- **Element References:** It starts by getting references to the HTML form (`createBookForm`) and the `div` where messages will be displayed (`messageDiv`) using their IDs.
- **Event Listener:** An event listener is attached to the form's `submit` event. When the user clicks the submit button, the provided asynchronous function is executed.
- **`event.preventDefault()`:** This is crucial\! By default, a form submission causes the browser to reload the page. `event.preventDefault()` stops this default behavior, allowing our JavaScript to handle the submission via an AJAX call (using `fetch`).
- **Collecting Form Data:** The code retrieves the values entered by the user in the "Title" and "Author" input fields.
- **Creating Data Object:** It then creates a simple JavaScript object (`newBookData`) containing the collected title and author. This object represents the data we want to send to the API.
- **Using `fetch` (POST Request):**
  - The `fetch()` function is used to send an HTTP request to the API endpoint (`${apiBaseUrl}/books`).
  - `method: 'POST'` specifies that this is a POST request (used for creating resources).
  - `headers: { 'Content-Type': 'application/json' }` is important. It tells the API server that the data we are sending in the body is in JSON format.
  - `body: JSON.stringify(newBookData)` converts our JavaScript `newBookData` object into a JSON string, which is the format expected by the API's body-parsing middleware. This string is then sent as the request body.
- **Handling Response:**
  - `await fetch(...)` waits for the API's response.
  - `response.ok` checks if the HTTP status code is in the 2xx range (indicating success).
  - `response.status` allows checking specific status codes (e.g., 201, 400).
  - `await response.json()` attempts to parse the response body as JSON.
  - **Conditional Logic:** The code checks the `response.status`:
    - If `201 Created`, it shows a success message to the user and clears the form (`createBookForm.reset()`).
    - If `400 Bad Request`, it indicates a validation error (caught by your backend validation middleware) and displays the error message received from the API.
    - For any other non-`ok` status (like 500 Internal Server Error), it throws an error to be caught by the `catch` block.
- **Error Handling (`try...catch`):** The `try...catch` block wraps the `fetch` call and response processing. If any error occurs (network issues, API returns non-2xx status, JSON parsing fails), the `catch` block catches it, logs the error to the browser console, and displays a generic failure message to the user on the page.

### Task 7: Create the Update View (`public/edit.html`) - Partial Implementation

Create the HTML file and JavaScript to fetch and display the data for a specific book, leaving the update logic for homework.

1.  Inside the `public` folder, create a new file named `edit.html`.

2.  Create a client-side JavaScript file for this page: Inside the `public` folder, create `edit-script.js`.

3.  Add the HTML structure for the edit form:

    ```html
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Edit Book</title>
        <style>
          body {
            font-family: sans-serif;
            margin: 20px;
          }
          form div {
            margin-bottom: 15px;
          }
          label {
            display: block;
            margin-bottom: 5px;
          }
          input[type="text"] {
            width: 300px;
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 4px;
          }
          button {
            padding: 10px 15px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
          }
          button:hover {
            background-color: #0056b3;
          }
          #message {
            margin-top: 15px;
            font-weight: bold;
          }
        </style>
      </head>
      <body>
        <h1>Edit Book</h1>
        <p><a href="index.html">Back to Books List</a></p>

        <div id="loadingMessage">Loading book data...</div>
        <form id="editBookForm" style="display: none;">
          <input type="hidden" id="bookId" />
          <div>
            <label for="editTitle">Title:</label>
            <input type="text" id="editTitle" name="title" required />
          </div>
          <div>
            <label for="editAuthor">Author:</label>
            <input type="text" id="editAuthor" name="author" required />
          </div>
          <button type="submit">Update Book</button>
        </form>

        <div id="message"></div>

        <script src="edit-script.js"></script>
      </body>
    </html>
    ```

### Task 8: Partial Implementation for Update Form in `public/edit-script.js`

Write the JavaScript to get the book ID from the URL, fetch the existing book data from your API, and populate the form.

1.  Open `public/edit-script.js`.

2.  Write the following javascript in `edit-script.js`:

    ```javascript
    // Get references to the elements
    const editBookForm = document.getElementById("editBookForm");
    const loadingMessageDiv = document.getElementById("loadingMessage"); // Element to show loading state
    const messageDiv = document.getElementById("message"); // Element to display messages (success/error)
    const bookIdInput = document.getElementById("bookId"); // Hidden input to store the book ID
    const editTitleInput = document.getElementById("editTitle"); // Input for the book title
    const editAuthorInput = document.getElementById("editAuthor"); // Input for the book author

    // Base URL for the API.
    const apiBaseUrl = "http://localhost:3000";

    // Function to get book ID from URL query parameter (e.g., edit.html?id=1)
    function getBookIdFromUrl() {
      const params = new URLSearchParams(window.location.search); // Get URL query parameters
      return params.get("id"); // Return the value of the 'id' parameter
    }

    // Function to fetch existing book data from the API based on ID
    async function fetchBookData(bookId) {
      try {
        // Make a GET request to the API endpoint for a specific book
        const response = await fetch(`${apiBaseUrl}/books/${bookId}`);

        // Check if the HTTP response status is not OK (e.g., 404, 500)
        if (!response.ok) {
          // Attempt to read error body if available (assuming JSON), otherwise use status text
          const errorBody = response.headers
            .get("content-type")
            ?.includes("application/json")
            ? await response.json()
            : { message: response.statusText };
          // Throw an error with status and message
          throw new Error(
            `HTTP error! status: ${response.status}, message: ${errorBody.message}`
          );
        }

        // Parse the JSON response body into a JavaScript object
        const book = await response.json();
        return book; // Return the fetched book object
      } catch (error) {
        // Catch any errors during the fetch or processing
        console.error("Error fetching book data:", error);
        // Display an error message to the user
        messageDiv.textContent = `Failed to load book data: ${error.message}`;
        messageDiv.style.color = "red";
        loadingMessageDiv.textContent = ""; // Hide loading message if it was shown
        return null; // Indicate that fetching failed
      }
    }

    // Function to populate the form fields with the fetched book data
    function populateForm(book) {
      bookIdInput.value = book.id; // Store the book ID in the hidden input
      editTitleInput.value = book.title; // Set the title input value
      editAuthorInput.value = book.author; // Set the author input value
      loadingMessageDiv.style.display = "none"; // Hide the loading message
      editBookForm.style.display = "block"; // Show the edit form
    }

    // --- Code to run when the page loads ---

    // Get the book ID from the URL when the page loads
    const bookIdToEdit = getBookIdFromUrl();

    // Check if a book ID was found in the URL
    if (bookIdToEdit) {
      // If an ID exists, fetch the book data and then populate the form
      fetchBookData(bookIdToEdit).then((book) => {
        if (book) {
          // If book data was successfully fetched, populate the form
          populateForm(book);
        } else {
          // Handle the case where fetchBookData returned null (book not found or error)
          loadingMessageDiv.textContent = "Book not found or failed to load.";
          messageDiv.textContent = "Could not find the book to edit.";
          messageDiv.style.color = "red";
        }
      });
    } else {
      // Handle the case where no book ID was provided in the URL
      loadingMessageDiv.textContent = "No book ID specified for editing.";
      messageDiv.textContent =
        "Please provide a book ID in the URL (e.g., edit.html?id=1).";
      messageDiv.style.color = "orange";
    }

    // --- Start of code for learners to complete (Form Submission / PUT Request) ---

    // Add an event listener for the form submission (for the Update operation)
    editBookForm.addEventListener("submit", async (event) => {
      event.preventDefault(); // Prevent the default browser form submission

      console.log("Edit form submitted (PUT logic to be implemented)");
      alert("Update logic needs to be implemented!"); // Placeholder alert

      // TODO: Collect updated data from form fields (editTitleInput.value, editAuthorInput.value)
      // TODO: Get the book ID from the hidden input (bookIdInput.value)
      // TODO: Implement the fetch PUT request to the API endpoint /books/:id
      // TODO: Include the updated data in the request body (as JSON string)
      // TODO: Set the 'Content-Type': 'application/json' header
      // TODO: Handle the API response (check status 200 for success, 400 for validation, 404 if book not found, 500 for server error)
      // TODO: Provide feedback to the user using the messageDiv (success or error messages)
      // TODO: Optionally, redirect back to the index page on successful update
    });

    // --- End of code for learners to complete ---
    ```

    **Explanation of the Code:**

This JavaScript code (`public/edit-script.js`) is responsible for the initial loading of data on the "Edit Book" page (`edit.html`).

- **Element References:** The code first gets references to the important HTML elements on the page, such as the form itself, elements to display loading and general messages, and the input fields for the book's ID, title, and author.
- **`getBookIdFromUrl()` Function:** This function is a utility to extract the `id` value from the URL's query string (e.g., if the URL is `edit.html?id=5`, this function will return `5`). This is how the page knows _which_ book to load for editing.
- **`fetchBookData(bookId)` Function:** This asynchronous function performs a `GET` request to your backend API's `/books/:id` endpoint, using the `fetch` API. It takes the `bookId` as an argument and includes it in the URL to request data for a specific book. It includes error handling to check the HTTP response status and parse the JSON data received from the API.
- **`populateForm(book)` Function:** This function takes the book data (a JavaScript object) fetched by `fetchBookData` and fills the corresponding input fields in the HTML form. It also hides the "Loading..." message and makes the form visible.
- **Code on Page Load:** The code outside of the functions runs automatically when `edit-script.js` is loaded by the browser.
  - It calls `getBookIdFromUrl()` to get the ID.
  - If an ID is found, it calls `fetchBookData()` to get the book details from the API.
  - Once the data is fetched (using `.then()`), it calls `populateForm()` to display the data in the form.
  - It includes basic handling for cases where no ID is in the URL or if the API fails to find/fetch the book.

3.  **Test:**

    - Ensure your Node.js application is running.
    - Go to `index.html`, click the "Edit" button for an existing book. This should navigate you to `edit.html?id=<bookId>`.
    - Verify that the form loads with the correct book data pre-filled.
    - Try changing the data and clicking "Update Book". You should see the console log and the alert message, indicating the submission was intercepted but the update logic is pending.
    - Try navigating to `edit.html` without an ID, or with an invalid ID (e.g., `edit.html?id=abc` or `edit.html?id=99999`) and observe the messages.

4.  **To Complete:** The code includes an event listener for the form's `submit` event. This listener is set up to prevent the default form submission and log a message/alert. The **`TODO` comments** clearly indicate the section where you need to add the JavaScript code to:

    - Collect the updated data from the form.
    - Perform a `fetch` `PUT` request to the API's `/books/:id` endpoint.
    - Include the updated data in the request body.
    - Handle the API's response (success or failure).
    - Provide feedback to the user.

## Summary and Remaining Task

Based on the tasks completed so far, you have successfully implemented the GET All, POST Create in the View layer.

**Fully Implemented Functionalities:**

- **GET (Read All):** Fetching and displaying all books from the API on `index.html`.
- **POST (Create):** Submitting a form to create a new book via the API on `create.html`.

However, the GET (View Details) functionality is partially complete. The DELETE functionality is partially complete. The PUT (Update) functionality is also partially implemented with the form submission listener in place.

### Your remaining task is to complete the partially implemented functionalities:

**Partially Implemented Functionalities:**

- **GET (View Details):** Fetching the data for a single book based on its ID.
- **DELETE (Delete):** Sending a DELETE request to the API and updating the list on `index.html`.
- **PUT (Update):** Performing a `fetch` `PUT` request to the API's `/books/:id` endpoint

## End of Lab Activity
