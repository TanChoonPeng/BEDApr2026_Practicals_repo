# Part B: Homework

## Tasks:

- **Task 1: Extending the Books API**
- **Task 2: Creating a Students API**

---

### Task 1: Extending the Books API

Now that you have completed the practical implementing the Read (Get All and Get By ID) and Create functionalities of the Books API, your homework is to add the remaining CRUD operations: **Update** and **Delete**.

You should continue to work on the `app.js` you completed in the practical:

1.  **Implement the Update Route:**

    - Add a `PUT` route handler for the path `/books/:id`.
    - Inside this handler, embed the full logic required to update a book in the database based on the `id` from the URL parameters and the updated book data from the request body (`req.body`).
    - This will involve:
      - Acquiring a connection from the `mssql` connection pool.
      - Executing a parameterized `UPDATE` SQL query to modify the book data where the `id` matches.
      - Handling the case where the book with the given ID is not found (return a `404 Not Found` response). You can check `result.rowsAffected` after the update query.
      - If the update is successful, it's good practice to fetch the updated book data from the database and return it in the response (with a `200 OK` status).
      - Handling potential errors during the database operation (return a `500 Internal Server Error`).
      - Ensuring the database connection is closed in a `finally` block.

2.  **Implement the Delete Route:**

    - Add a `DELETE` route handler for the path `/books/:id`.
    - Inside this handler, embed the full logic required to delete a book from the database based on the `id` from the URL parameters.
    - This will involve:
      - Acquiring a connection from the `mssql` connection pool.
      - Executing a parameterized `DELETE` SQL query to remove the book where the `id` matches.
      - Handling the case where the book with the given ID is not found (return a `404 Not Found` response). Check `result.rowsAffected`.
      - If the deletion is successful, return a `204 No Content` status code.
      - Handling potential errors during the database operation (return a `500 Internal Server Error`).
      - Ensuring the database connection is closed in a `finally` block.

3.  **Test Your Implementation:**
    - Run your updated `app.js` file.
    - Use Postman to test your new `PUT /books/:id` and `DELETE /books/:id` endpoints.
    - Verify that you can update existing books and delete books successfully.
    - Test the edge cases, such as trying to update or delete a book that does not exist (you should get a 404).

**Deliverables:**

- Your updated `app.js` file containing the implemented PUT and DELETE route handlers.

This homework will reinforce your understanding of embedding database logic directly within Express route handlers and completing the basic CRUD functionalities for the API.

---

### Task 2: Creating a Students API

Based on your experience building the Books API in this practical, your homework is to create a completely new API from scratch: a **Students API**.

Your task is to implement **full CRUD (Create, Read, Update, Delete)** operations for managing student records.

Here are the steps you should follow:

1.  **Database Setup:**

    - Connect to your Microsoft SQL Server Express instance using SSMS.
    - You can either create a new database (e.g., `students_db`) or add a new table to your existing `bed_db`. Let's assume you'll add a table to `bed_db` for simplicity.
    - Create a table named `Students` with the following columns (adjust data types as appropriate for SQL Server, e.g., `INT`, `VARCHAR`):
      - `student_id` (e.g., `INT`, Primary Key, Auto-incrementing)
      - `name` (e.g., `VARCHAR(100)`, required)
      - `address` (e.g., `VARCHAR(255)`, optional)
      - You can add other relevant student fields if you like.

2.  **Node.js Project Setup:**

    - Create a new folder for your Students API project (e.g., `students-api`).
    - Navigate into the folder in your terminal.
    - Initialize a new npm project: `npm init -y`
    - Install the necessary packages: `npm install express mssql`
    - Create the main application file: `touch app.js`

3.  **Code the `app.js` File:**

    - Open the `app.js` file in your code editor.
    - Copy the basic structure from the Books API `app.js`:
      - Require `express` and `mssql`.
      - Initialize the Express app and define the port.
      - Use `express.json()` and `express.urlencoded()` middleware.
      - Define your `dbConfig` object (make sure the `database` name and potentially user/password are correct for your SQL Server setup).
      - Set up the database connection pool startup and graceful shutdown logic.
    - Implement the **full CRUD** operations for Students, embedding the database interaction logic directly within the route handlers, following the pattern of the Books API `GET /books` and `POST /books` handlers:
      - **GET /students:** Retrieve all students.
      - **GET /students/:id:** Retrieve a student by their `student_id`.
      - **POST /students:** Create a new student. Get the student data (`name`, `address`, etc.) from `req.body`. Remember to fetch the newly created student (including their generated `student_id`) after the insert.
      - **PUT /students/:id:** Update an existing student by their `student_id`. Get the updated data from `req.body`. Handle the case where the student is not found (404). Fetch the updated student to return.
      - **DELETE /students/:id:** Delete a student by their `student_id`. Handle the case where the student is not found (404). Return 204 on success.
    - For each route handler, include a `try...catch...finally` block to manage database operations, basic error handling (sending 500 on database errors, 404 for not found), and ensure the database connection is closed.

4.  **Test Your Students API:**
    - Run your `app.js` file (`node app.js`).
    - Use Postman to test all your new Students API endpoints (`GET /students`, `GET /students/:id`, `POST /students`, `PUT /students/:id`, `DELETE /students/:id`).
    - Verify that all CRUD operations work as expected.
    - Test edge cases like getting, updating, or deleting a non-existent student ID (should return 404).
    - Test sending invalid data (e.g., missing required fields in POST/PUT) and observe the resulting `400 Bad Request Error` from the database.

This assignment will give you hands-on practice in applying the consolidated coding style to build a new API with full CRUD functionality.

---

### **Submission before next class**

- Commit and push the following into your BED Practicals Github repo:
  1. The extended Books API containing the implemented PUT and DELETE route handlers.
  2. The newly created Students containing the implemented GET, POST, PUT and DELETE route handlers.

---

### **Pro-tip:**

- Always consult your tutor or classmates when in doubt.
