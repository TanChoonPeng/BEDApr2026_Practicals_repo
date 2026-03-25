## Part B: Homework (45 min)

### **Task 1: Write a Short Report (Max 500 Words)**

For this task,

1. Create a new file and save it as `homework_task_b2_report.txt`.

2. Write a short report in the file that addresses the following points:

- **In your `foods-api` API, describe the role of the following elements**:
  - `app.use(express.json())`
  - `req.body`
  - `req.query`
  - `req.params.id`
- **Describe the significance of using different HTTP methods** (e.g., GET, POST, PUT, DELETE) in your API. How do they relate to CRUD operations?
- **Explain the meaning of these HTTP status codes**:
  - `200 OK`
  - `201 Created`
  - `400 Bad Request`
  - `404 Not Found`
  - `500 Internal Server Error`

---

### **Task 2: Capture Screenshots of Postman Tests**

For this task, you need to capture and submit screenshots of your Postman tests for the following CRUD operations:

1. `POST /foods` - Create a new food.
2. `GET /foods` - Retrieve all foods.
3. `PUT /foods/:id` - Update an existing food.
4. `DELETE /foods/:id` - Delete a food by its ID.

### **Instructions to Create a Screenshot Folder:**

1. **Create a New Folder for Screenshots:**

   - On your computer, navigate to your `foods-api` project folder.
   - Inside the `foods-api` folder, create a new folder named `screenshots`.

   ```bash
   mkdir screenshots
   ```

2. **Take Screenshots of Postman Requests and Responses:**
   - Open Postman and run each of the following API requests:
     - `POST /foods`
     - `GET /foods`
     - `PUT /foods/:id`
     - `DELETE /foods/:id`
   - For each request, take a screenshot of both the **request** and the **response** section in Postman.
     - **Request**: Include the method, URL, headers, and body (if applicable).
     - **Response**: Include the status code, headers, and body.
3. **Save the Screenshots:**
   - Save each screenshot with a clear, descriptive filename. For example:
     - `POST_foods_request.png`
     - `POST_foods_response.png`
     - `GET_foods_request.png`
     - `GET_foods_response.png`
     - `PUT_foods_1_request.png`
     - `PUT_foods_1_response.png`
     - `DELETE_foods_1_request.png`
     - `DELETE_foods_1_response.png`
4. **Submit the Screenshots:**
   - Once you have captured all the required screenshots, place them inside the `screenshots` folder.
   - Make sure the `screenshots` folder contains all the necessary screenshots and is named clearly.

---

### **Submission before next class**

- Commit and push the following into your BED Practicals Github repo:
  1. Your extended `app.js` file with the new functionality.
  2. The written report (max 300 words).
  3. Screenshots of Postman tests.

---

### **Pro-tip:**

- Always test your endpoints using Postman to ensure that they work as expected.
