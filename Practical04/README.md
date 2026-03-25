## Practical 04: Building Maintainable and Secure APIs with MVC

## Objective:

This practical session's goal is to refactor and enhance the Books API built in the previous practical. We will apply the **Model-View-Controller (MVC) architectural pattern** to improve code organization, implement **input validation using middleware** for data integrity, integrate **general error handling** for better API reliability, and ensure robust protection against **SQL injection vulnerabilities** using **parameterized queries**.

## Prerequisites:

- Successful completion of the "Building REST API with MSSQL Integration" practical (**Practical 03**).
- A working Node.js/Express development environment.
- Microsoft SQL Server Express installed and configured (from Practical 03).
- The `bed_db` database and `Books` table created and populated (from Practical 03).
- Familiarity with basic Node.js, Express routing, and fundamental database interaction concepts using `mssql`.
- Completion of the online learning session.

## 📂 Structure

This practical is divided into two parts:

### ✅ Part A: Lab Activity (In-Class)

For this Lab Activity part, you will find the instructions in the **File:** `PartA_Lab.md`

### 📝 Part B: Homework (To be submitted)

For this Homework part, you will find the instructions in the **File:** `PartB_Homework.md`

### ✨ Optional Topic: Nodemon

For those interested in improving their **Node.js development workflow**, information on **nodemon** (a tool to automatically restart your Node.js server) is available in the optional extra **File**: `Extra.md`.

## 📤 Submission

- Complete the Lab Activity and Homework as specified in Part A and B
- Submit by pushing all work to your BED Practicals GitHub repository
- **Deadline:** Before next week’s class

## Success Criteria:

- The Books API project code is clearly organized into `controllers` and `models` directories following the MVC pattern.
- Database interaction logic resides solely within the `models` files.
- Request handling logic (processing input, calling models, preparing responses) resides within the `controllers` files.
- API endpoints are defined in `app.js` and correctly linked to controller functions.
- Input validation middleware is applied to the `POST /books` route (and potentially other routes) and correctly returns a **400 status** with an informative error body for invalid input.
- All database queries in the `models` files that use variable data (like book ID, title, author) utilize `mssql`'s **parameterized query** methods (`.input()` and `@parameterName`).
- The API's **CRUD operations** (Create, Read, Update, Delete - if implemented) function correctly when tested with Postman.
- Testing with Postman demonstrates that validation errors (**400**) and server errors (**500/404**) are handled gracefully with appropriate responses.
- Part B reflection questions are answered thoughtfully, demonstrating understanding of the concepts.

## Additional Notes:

- Refer back to the code you developed in the previous practical (Practical 03) as your starting point.
- Consult the code examples and concepts covered in the online learning session.
- Work incrementally: Refactor one part at a time (e.g., move Model logic first, then Controller logic, then add validation, then error handling).
- Use Postman frequently to test after implementing each new feature or refactoring step.
- Remember the importance of `next()` in middleware to pass control, and `next(err)` to pass errors to the error handler.
- Pay close attention to using `.input()` and `@parameterName` for all variable data in your SQL queries.

## 🤖 Note

- This practical exercise was created with the assistance and usage of **generative AI tools**. ✨

### Happy coding and learning! 🚀
