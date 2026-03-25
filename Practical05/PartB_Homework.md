# Practical 05: Part B - Homework

## Homework

This part encourages you to reflect on the concepts learned throughout the last few practicals and apply your new knowledge. Answer the reflection questions and complete the coding tasks.

### Task 1: Applying the View Layer to the Students API

Take the Students API project you worked on for the homework in Practical 04. Your task is to add a simple HTML/JavaScript View layer to this API, following the patterns you implemented for the Books API in Part A.

1. In your Students API project folder, create a `public` directory.

2. Configure `express.static` in your Students API's `app.js` to serve files from the `public` folder.

3. Create the following HTML files in the `public` folder:

   - `students.html`: To list all students (GET All).

   - `create-student.html`: To create a new student (POST).

   - `edit-student.html`: To update an existing student (GET One for Update + PUT).

4. Write the necessary client-side JavaScript (in separate `.js` files linked to your HTML) to implement **full CRUD** functionality for the Students API:

   - In `students.html`, fetch and display all students. Include buttons/links for View (optional), Edit, and Delete.

   - In `create-student.html`, implement the form submission to create a new student via a POST request.

   - In `edit-student.html`, fetch the existing student data based on the URL ID, populate the form, and implement the form submission to update the student via a PUT request.

   - In `students.html` (or a separate delete confirmation page/modal if you prefer), implement the delete functionality via a DELETE request. After successful deletion, update the UI (e.g., remove the student from the list).

5. Test your Students API View layer thoroughly using your browser.

### Task 1.1: Refactor Students API Styling with External CSS

To improve code organization and maintainability, refactor the styling used in your **Students API** HTML pages (`students.html`, `create-student.html`, `edit-student.html`) from Task 1 by moving the inline `<style>` tags into a separate CSS file.

1. Inside your `public` folder (or a dedicated `public/css` subfolder) in your **Students API** project, create a new file named `main.css`.

2. Copy the CSS rules from the `<style>` tags in your **Students API** `students.html`, `create-student.html`, and `edit-student.html` files and paste them into `main.css`. Combine and organize the rules as needed.

3. In each of your **Students API** HTML files (`students.html`, `create-student.html`, `edit-student.html`), remove the `<style>` tag and replace it with a `<link>` tag in the `<head>` section to link to your `main.css` file. Ensure the path is correct (e.g., `<link rel="stylesheet" href="main.css">` or `<link rel="stylesheet" href="css/main.css">`).

4. Test your Students API View layer again in the browser to ensure the styling is still applied correctly.

### Task 2: Reflection and Review

Reflect on your learning journey from Practical 03 to Practical 05, focusing on the evolution of your API project structure, robustness, and the separation of concerns.

1.  **Separation of Concerns:**

    - In your own words, explain the distinct responsibilities of the Model, View (the external frontend), and Controller in your final project structure.

    - How does having a separate frontend View (Practical 05) simplify the responsibilities of your backend API?

2.  **Robustness and Security:**

    - Consider the journey from a simple API (Practical 03) to a more robust one (Practical 04) and a full-stack application (Practical 05). At which stage do you think it became easier to identify and fix bugs related to data handling or API responses? Why?

3.  **Challenges and Problem Solving:**

    - What was the most challenging aspect for you across Practical 03, 04, and 05? Describe the problem and how you approached solving it.

    - Thinking critically, if you had to add a new feature (e.g., adding a "genre" field to books, or implementing user authentication), how would the current MVC structure with a separate View layer help you approach this task in a more organized and efficient way compared to the initial Practical 03 structure?

4.  **Experiential Learning:** How did the hands-on coding and refactoring in these practicals help you understand the concepts of MVC, validation, error handling, and parameterized queries compared to just reading about them?

### Format:

Submit your answers to the reflection and exploration tasks as a short written report or document (e.g., `Practical05_Homework_Reflection.md`). Ensure your answers are clear, well-explained, and directly address the questions asked.

## Submission:

Commit and push your updated `books-api-mvc-db` project code, updated **Students API** and your reflection document to your BED Practicals Github repository by the deadline specified by your instructor.

## Pro-tip:

- Use the browser's developer tools (especially the "Network" tab) to inspect the HTTP requests your frontend is sending and the responses it receives from your API. This is invaluable for debugging frontend-backend communication.

## End of Homework
