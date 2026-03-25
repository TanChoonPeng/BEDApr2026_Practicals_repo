# Practical 01 - Part A: Introduction to Node.js and Express Framework (Lab Activity)

---

### 🎯 Objective:

By the end of this practical, you will:

- Understand the basics of Express.js
- Set up a simple backend server using Node.js and Express
- Create basic routes and handle HTTP methods
- Prepare for CRUD implementation in upcoming sessions

---

### 🧰 Pre-requisites:

- Node.js and npm installed
- Basic JavaScript knowledge
- A code editor (e.g., VS Code)
- Postman or similar API testing tool

---

## 🧪 Part A: Lab Activity (To be done in class)

### Step 1: Project Setup

1. Create a new folder named `Practical01` to store your practical files
   ```bash
   mkdir Practical01
   ```
2. Inside the `Practical01` folder, create another folder named `PartA_Lab` for your Part A Lab activity.
   ```bash
   mkdir Practical01/PartA_Lab
   ```
3. Change to the `PartA_Lab` directory:
   ```bash
   cd Practical01/PartA_Lab
   ```
4. Initialize the Node.js project:
   ```bash
   npm init -y
   ```
5. Install Express:
   ```bash
   npm install express
   ```

#### 🔍 What's been done so far?

We've successfully created the **Node.js project** and installed the **Express framework** using the **Node Package Manager (npm)**.

If you'd like, you can open and inspect the `package.json` file in your project folder. You'll find details about the project dependencies, including Express.

---

### Step 2: Create the Server

1. Create a file named `index.js`.
2. Add the following code:

   ```js
   const express = require("express");
   const app = express();
   const PORT = 3000;

   app.get("/", (req, res) => {
     res.send("Hello from Express!");
   });

   app.listen(PORT, () => {
     console.log(`Server is running on http://localhost:${PORT}`);
   });
   ```

3. Run the server:

   ```bash
   node index.js
   ```

4. Open the browser or Postman and visit:  
   `http://localhost:3000`

#### 🔍 Expected Output:

When you visit the URL, you should see the message "Hello from Express!" displayed. This indicates that the server is running successfully.

---

### Step 3: Add More Routes

In Express, **routes** are used to define how the application will respond to a specific HTTP request. Routes are paired with HTTP methods (like GET, POST, PUT, DELETE) to specify what action to take when a particular URL path is accessed.

- **What is a Route?**
  A route consists of a URL pattern and an HTTP method. When a client sends a request (e.g., a browser or Postman), Express checks if the URL matches any of the defined routes, and if it does, it executes the associated function (route handler) to return a response.

1. Create the following routes:

- `GET /about` → Returns "About Page"
- `GET /contact` → Returns "Contact Page"

Example code:

```js
// Define route for About Page
app.get("/about", (req, res) => {
  res.send("About Page");
});

// Define route for Contact Page
app.get("/contact", (req, res) => {
  res.send("Contact Page");
});

// Listen on the port after defining routes
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

When you add or modify routes in your Express app, you'll need to restart the server for changes to take effect. Follow these steps to restart your Node.js server:

2. Stop the current server:
   In the terminal where the server is running, press `Ctrl + C` to stop the server.

3. Restart the server by running the following command:

   ```bash
   node index.js
   ```

Now, you can test the following routes in your browser or Postman:

- **About Page:**  
  Visit [http://localhost:3000/about](http://localhost:3000/about) to see the "About Page".

- **Contact Page:**  
  Visit [http://localhost:3000/contact](http://localhost:3000/contact) to see the "Contact Page".

---

#### 🔍 Important Concepts:

- **HTTP Methods:**
  Routes can be defined for different HTTP methods such as `GET`, `POST`, `PUT`, and `DELETE`.

  - `GET` is used to retrieve data.
  - `POST` is used to send data.
  - `PUT` is used to update data.
  - `DELETE` is used to remove data.

- **Route Handler:**
  A route handler is the function that runs when a route is matched. It takes at least two arguments: `req` (request) and `res` (response).

📌 **Important:**
Make sure **all route definitions are placed before** the `app.listen()` method in your `index.js`. This ensures that the server knows about the routes before it starts listening for requests.
