# Part A: Lab Activity

## Quick Overview of REST API:

REST (Representational State Transfer) is an architectural style for designing networked applications. RESTful APIs use HTTP requests to perform CRUD operations.

**Key Concepts:**

- **Client-Server Model**: Separation of concerns between the client (frontend/Postman) and server (Node.js/Express)
- **Stateless Communication**: Each request contains all information needed to process it
- **HTTP Methods**: GET, POST, PUT, DELETE correspond to Read, Create, Update, Delete
- **Resources**: Entities (like "foods") identified by URLs

**HTTP Request Structure:**

```
METHOD /resource HTTP/1.1
Host: example.com
Content-Type: application/json

{request body if applicable}
```

**HTTP Response Structure:**

```
HTTP/1.1 STATUS_CODE STATUS_MESSAGE
Content-Type: application/json

{response body}
```

---

## Lab Activity Instructions

### **Steps:**

1. **Setup Project Folder and Minimal Express App**

   - Create a project folder and initialize it.
   - Build a simple Express app with a basic endpoint (e.g., GET `/hello`).

2. **Guided Coding to Implement Foods CRUD Functionality**

   - Implement CRUD operations (Create, Read, Update, Delete) for the "foods" resource.

3. **Hands-on Testing Using Postman**

   - Use Postman to test all CRUD endpoints by sending HTTP requests and inspecting responses.

4. **Reflection & Q&A**
   - Reflect on what you've learned and answer the provided questions to reinforce your understanding.

---

### ⏰ Step 1: Setup Project folder and Minimal Express App

#### **Mini-steps:**

1. **Setup project folder:**

   First, create a folder for the entire practical:

   ```bash
   mkdir Practical02
   cd Practical02
   ```

   Then, create the **foods-api** folder:

   ```bash
   mkdir foods-api
   cd foods-api
   npm init -y
   npm install express
   ```

2. **Create** `app.js` with:

   ```javascript
   const express = require("express");
   const app = express();
   const PORT = 3000;

   app.use(express.json());

   app.get("/hello", (req, res) => {
     res.send("Hello, world!");
   });

   app.listen(PORT, () => {
     console.log(`Server running at http://localhost:${PORT}`);
   });
   ```

3. **Run** the server in the terminal: `node app.js`
4. **Test** in Postman:
   - Method: GET
   - URL: `http://localhost:3000/hello`
   - Expect response “Hello, world!”

> **Pro-tip:** Restart the server after code changes (`Ctrl+C`, then `node app.js`).

---

### ⏰ Step 2: Guided coding to implement Foods CRUD functionality

> **Remember:** At top of `app.js` include:
>
> ```javascript
> const express = require("express");
> const app = express();
> const PORT = 3000;
> app.use(express.json());
> ```
>
> In the middle, **add the guided codes in the below mini-steps**:
>
> And at bottom:
>
> ```javascript
> app.listen(PORT, () => {
>   console.log(`Server running at http://localhost:${PORT}`);
> });
> ```

> **Tip:** Restart server after each change.

#### **Mini-steps:**

#### 1. **Initialise Food Array:**

Create an **in-memory “database”** to store the food data:

```javascript
let foods = [];
```

Each food object will have:

- `id` (unique number)
- `name` (string)
- `calories` (number)

#### 2. **Create Food (POST /foods)**

```javascript
app.post("/foods", (req, res) => {
  const { name, calories } = req.body;
  if (!name || calories == null) {
    return res
      .status(400)
      .json({ message: "Cannot create food: name and calories are required." });
  }
  const newFood = { id: Date.now(), name, calories };
  foods.push(newFood);
  res
    .status(201)
    .json({ message: "Food created successfully.", food: newFood });
});
```

- The POST method creates a new food resource.

- It checks that both name and calories are provided in the request body. If either is missing, it responds with a 400 status code.

- If valid, it generates a new food object with a unique id (using Date.now()), adds it to the foods array, and returns a 201 status code.

#### 3. **Read All Foods (GET /foods)**

```javascript
app.get("/foods", (req, res) => {
  const { name } = req.query;
  let results = foods;
  if (name) {
    results = foods.filter((f) => f.name.includes(name));
    return res.json({
      message: `Found ${results.length} food(s) matching name filter.`,
      foods: results,
    });
  }
  res.json({
    message: `Retrieved all foods (${results.length}).`,
    foods: results,
  });
});
```

- The GET method retrieves all food items in the foods array.

- It allows an optional query parameter name to filter foods by their name.

- If a name is provided, it filters the foods array and returns a filtered list.

- If no query is provided, it returns all foods in the array.

#### 4. **Update Food (PUT /foods/:id)**

```javascript
app.put("/foods/:id", (req, res) => {
  const foodId = Number(req.params.id);
  const { name, calories } = req.body;
  if (!name || calories == null) {
    return res
      .status(400)
      .json({ message: "Cannot update: name and calories are required." });
  }
  const idx = foods.findIndex((f) => f.id === foodId);
  if (idx === -1) {
    return res
      .status(404)
      .json({ message: `No food found with id ${foodId}.` });
  }
  foods[idx] = { id: foodId, name, calories };
  res.json({
    message: `Food with id ${foodId} updated successfully.`,
    food: foods[idx],
  });
});
```

- The PUT method updates an existing food item identified by id.

- It checks that name and calories are provided in the request body. If not, it responds with a 400 status code.

- It searches for the food item by id. If the food item is not found, it responds with a 404 status code.

- If the food is found, it updates the corresponding food object and responds with a 200 status code.

#### 5. **Delete Food (DELETE /foods/:id)**

```javascript
app.delete("/foods/:id", (req, res) => {
  const foodId = Number(req.params.id);
  const exists = foods.some((f) => f.id === foodId);
  if (!exists) {
    return res
      .status(404)
      .json({ message: `No food found with id ${foodId}.` });
  }
  foods = foods.filter((f) => f.id !== foodId);
  res.json({ message: `Food with id ${foodId} deleted successfully.` });
});
```

- The DELETE method deletes a food item identified by id.

- It checks if the food item exists by searching for the id in the foods array. If not found, it responds with a 404 status code.

- If the food item exists, it filters it out of the array and responds with a success message.

---

### ⏰ Step 3: Hands-on Testing using Postman

| Task           | Method | URL                                    | Body (raw JSON)                         |
| -------------- | ------ | -------------------------------------- | --------------------------------------- |
| Create food    | POST   | `http://localhost:3000/foods`          | `{ "name": "Apple", "calories": 95 }`   |
| Read all foods | GET    | `http://localhost:3000/foods`          | —                                       |
| Filter by name | GET    | `http://localhost:3000/foods?name=App` | —                                       |
| Update food    | PUT    | `http://localhost:3000/foods/{id}`     | `{ "name": "Banana", "calories": 105 }` |
| Delete food    | DELETE | `http://localhost:3000/foods/{id}`     | —                                       |

> **Pro-tip:** Inspect both in Postman’s **Headers** and **Body** panels to see how client & server communicate.|

---

### ⏰ Step 4: Reflection & Q&A

1. **Create a new text file:**

   - Create a new file and save it with the name `Reflection_QA_PartA.txt`.

2. **Reflection:**

   - Write a **brief reflection** (2-3 sentences) on the concepts you learned during Part A of the practical. For example:
     - What did you learn about REST APIs?
     - How did you understand the use of HTTP methods (GET, POST, PUT, DELETE)?
     - Did you face any challenges while building the CRUD operations?

3. **Q&A:**

   - Answer the following **questions** based on your understanding:

     1. **Why use POST for creating resources and PUT for updating?**
     2. **What status code should be returned when attempting to update a non-existent resource?**

4. **Save the file** along with your code.

---
