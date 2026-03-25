### Ensuring API Quality with Unit Testing (Step 3)

#### Testing API Models with Jest

In this step, we'll shift our focus from testing controllers to testing models. Models are the heart of your application's data logic – they interact with your database, represent your data structures, and often contain business logic related to your data. Thoroughly testing your models is essential for maintaining data integrity, ensuring the correctness of calculations and transformations, and preventing regressions when your database schema or application logic changes.

**Writing Tests for the `Book` Model**

The `Book` model contains functions to fetch books and update their availability. We'll write unit tests for these functions to ensure they operate as expected. As always, we'll mock the database connection to isolate the model's behavior from the database itself.

1.  **Create a Test File:** In your `tests` directory, create a new file named `book.test.js`. This file will house your unit tests for the `Book` model.

2.  **Write Tests for `getAllBooks`:** Add the following code to `book.test.js`:

    ```javascript
    // book.test.js
    const Book = require("../models/bookModel");
    const sql = require("mssql");

    jest.mock("mssql"); // Mock the mssql library

    describe("Book.getAllBooks", () => {
      beforeEach(() => {
        jest.clearAllMocks();
      });

      it("should retrieve all books from the database", async () => {
        const mockBooks = [
          {
            id: 1,
            title: "The Lord of the Rings",
            author: "J.R.R. Tolkien",
            availability: "Y",
          },
          {
            id: 2,
            title: "The Hitchhiker's Guide to the Galaxy",
            author: "Douglas Adams",
            availability: "N",
          },
        ];

        const mockRequest = {
          query: jest.fn().mockResolvedValue({ recordset: mockBooks }),
        };
        const mockConnection = {
          request: jest.fn().mockReturnValue(mockRequest),
          close: jest.fn().mockResolvedValue(undefined),
        };

        sql.connect.mockResolvedValue(mockConnection); // Return the mock connection

        const books = await Book.getAllBooks();

        expect(sql.connect).toHaveBeenCalledWith(expect.any(Object));
        expect(mockConnection.close).toHaveBeenCalledTimes(1);
        expect(books).toHaveLength(2);
        expect(books[0].id).toBe(1);
        expect(books[0].title).toBe("The Lord of the Rings");
        expect(books[0].author).toBe("J.R.R. Tolkien");
        expect(books[0].availability).toBe("Y");
        // ... Add assertions for the second book
      });

      it("should handle errors when retrieving books", async () => {
        const errorMessage = "Database Error";
        sql.connect.mockRejectedValue(new Error(errorMessage));
        await expect(Book.getAllBooks()).rejects.toThrow(errorMessage);
      });
    });
    ```

**Explanation of `getAllBooks` Tests:**

- **Mocking:** The `mssql` library is mocked to simulate database interactions without actually connecting to a database.
- **Test Case 1: Success:**
  - Sets up mock data for books.
  - Mocks the database connection, request, and query to return the mock books.
  - Calls `getAllBooks` and checks if the returned data matches the mock data.
- **Test Case 2: Error Handling:**
  - Simulates a database error by making `sql.connect` reject with an error.
  - Verifies that `getAllBooks` throws the expected error when a database issue occurs.

3.  **Run the tests:** To run your unit tests, open the terminal in your project directory and run:

    ```bash
    npm test
    ```

    If everything is set up correctly, you should see output indicating that your tests have passed. If any tests fail, Jest will provide error messages to help you debug.

**Exercise: Test `updateBookAvailability` Function**

Now, let's test the `updateBookAvailability` function, following a similar pattern to the test for `getAllBooks`:

1.  **Add the Test Suite:** Inside the `book.test.js` file, create another `describe` block for the `updateBookAvailability` function:

    ```javascript
    // book.test.js (continue in the same file)
    describe("Book.updateBookAvailability", () => {
      // ... mock mssql and other necessary components

      it("should update the availability of a book", async () => {
        // ... arrange: set up mock book data and mock database interaction
        // ... act: call updateBookAvailability with the test data
        // ... assert: check if the database was updated correctly and the updated book is returned
      });

      it("should return null if book with the given id does not exist", async () => {
        // ... arrange: set up mocks for a non-existent book id
        // ... act: call updateBookAvailability
        // ... assert: expect the function to return null
      });

      // Add more tests for error scenarios (e.g., database error)
    });
    ```

2.  **Write Test Cases:** Implement the following test cases within the `describe` block:

    - **Test Case 1: Successful Update:**
      - Set up mock data for the book to update and the updated availability status.
      - Mock the database connection, request, and query to simulate a successful update.
      - Call `updateBookAvailability` and verify that:
        - The correct SQL query and parameters were passed.
        - The updated book object is returned.
    - **Test Case 2: Book Not Found:**
      - Mock the database interactions to simulate a scenario where the book with the given ID doesn't exist.
      - Call `updateBookAvailability` and verify that it returns `null`.
    - **Test Case 3: Database Error:**
      - Mock the database interactions to throw an error.
      - Call `updateBookAvailability` and verify that it throws the appropriate error.

**Remember:**

- Use `beforeEach` to reset your mocks before each test case to keep your tests independent.
- Leverage Jest's assertion methods (e.g., `expect`, `toBe`, `toHaveBeenCalledWith`, etc.) to verify the expected behavior.
- Aim for comprehensive test coverage to build confidence in the reliability of your model's functionality.

Feel free to ask for help if you get stuck. This exercise is a great opportunity to solidify your understanding of testing models in isolation!
