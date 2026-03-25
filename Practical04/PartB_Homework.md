## Practical 04: Part B - Homework

### Task 1: Reflection on Refactoring

Reflect on the process of refactoring the API code into the MVC structure by answering the following questions:

1.  What were the main changes you made to refactor the code into MVC architecture? Describe how you moved code between files and folders.
2.  What challenges did you face during the refactoring process?
3.  How does the MVC structure change the way you think about adding new features or modifying existing ones compared to the previous non-MVC structure?
4.  In what specific ways do you think the MVC version is more organized or easier to understand and maintain than the previous version where all logic was in `app.js`?
5.  Explain how separating concerns (putting database logic in the **Model** and request handling in the **Controller**) makes the code better from a development perspective.

### Task 2: Reflecting on Robustness & Security

Reflect on the impact of validation, error handling, and parameterized queries by answering the following questions:

1.  How does implementing input validation middleware make the API more reliable and user-friendly? Provide an example of invalid input that your validation would now handle.
2.  Explain in your own words how parameterized queries prevent SQL injection attacks. Why is this approach fundamentally more secure than building SQL query strings by concatenating variable data?
3.  Consider a potential security risk for an API (other than SQL injection, e.g., brute-force attacks, exposing sensitive data in responses). How might robust error handling (like not showing detailed error messages to the client) help mitigate such a risk?

### Task 3: Applying Concepts

- **Apply to Students API:** Take the Students API you built for the homework in the previous practical (Part B, Task 2). Refactor this Students API to follow the MVC architecture, implement input validation for creating/updating students, add general error handling, and ensure all database queries use parameterized queries. Your refactored Students API project should have:

  - A model (`studentModel.js`) for database operations
  - A controller (`studentController.js`) for request handling
  - Validation middleware for student data (e.g., `studentValidation.js`)
  - Main application file (`app.js`) tying everything together

### Submission:

Submit your answers to the reflection and exploration tasks. Ensure your answers are clear and directly address the questions asked.

- Create a text file `Homework_reflection.txt` answering the reflection questions (Tasks 1 & 2).
- Commit and push your refactored code for both Books API and Students API (Task 3) to your BED Practicals **Github repository**.

### Pro-tip:

Always consult your tutor or classmates when in doubt. Pair programming or discussing challenges can be very helpful!

#### End of Homework
