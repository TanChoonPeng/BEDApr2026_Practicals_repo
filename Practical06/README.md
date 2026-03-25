# Practical 06: Building User Functionalities and Relationships

## Objective:

This practical session's goal is to expand the existing Books API application by introducing **user management functionalities** and establishing **relationships between users and books**. We will leverage the `mssql` package to interact with the MSSQL database, implement **CRUD operations for users**, and demonstrate how to retrieve **related data through SQL joins**.

## Prerequisites:

- Successful completion of the "Building Maintainable and Secure APIs with MVC" practical (**Practical 04**).
- A working Node.js/Express development environment.
- Microsoft SQL Server Express installed and configured.
- The `bed_db` database and `Books` table created and populated.
- Familiarity with basic Node.js, Express routing, MVC pattern, and database interaction concepts using `mssql`.

## 📂 Structure

This practical consists only of a Lab Activity:

### ✅ Lab Activity (In-Class)

For this Lab Activity, you will find the instructions in the **File:** `PartA_Lab.md`.

**Important Note Regarding Homework:** Practical 06 does not have a Part B homework component. We encourage you to use this dedicated time to begin collaborative work on **Assignment Checkpoint 1** with your team.

## 📤 Submission

- Complete the Lab Activity as specified.
- Submit by pushing all work to your BED Practicals GitHub repository.
- **Deadline:** Before next week’s class

## Success Criteria:

- The `Users` table and `UserBooks` (join) table are successfully created in the `bed_db` database.
- Sample data for `Users` and `UserBooks` is successfully populated.
- User management functionalities (Create, Read, Update, Delete) are implemented in the `userModel.js` and `userController.js` files, adhering to the MVC pattern.
- Database interaction logic for users resides solely within `userModel.js`.
- Request handling logic for user endpoints resides within `userController.js`.
- The API endpoints for users (`/users`, `/users/:id`) are defined in `app.js` and correctly linked to controller functions.
- A user search functionality (e.g., `/users/search?searchTerm=...`) is implemented and correctly returns filtered user data.
- The functionality to retrieve users with their associated books (e.g., `/users/with-books`) is implemented, correctly using SQL JOINs and structuring the response data.
- All new database queries in the `models` files that use variable data utilize `mssql`'s **parameterized query** methods (`.input()` and `@parameterName`).
- All implemented API endpoints for users function correctly when tested with Postman.
- Testing with Postman demonstrates that API responses for user-related operations are as expected.

## Additional Notes:

- Refer back to the code you developed in the previous practical (Practical 04) as your starting point.
- Consult the code examples and concepts covered in the online learning session.
- Work incrementally: Implement one new user functionality or relationship aspect at a time.
- Use Postman frequently to test after implementing each new feature.
- Pay close attention to using `.input()` and `@parameterName` for all variable data in your SQL queries.

## 🤖 Note

- This practical exercise was created with the assistance and usage of **generative AI tools**. ✨

### Happy coding and learning! 🚀
