# Practical 01 - Part B: Homework Assignment

---

## 📚 Part B: Homework (To be submitted)

### Task: Build a Simple Express API for yourself!

#### Requirements:

- Inside the `Practical01` folder, create a new folder named `PartB_Homework`.
- Change to the `PartB_Homework` directory:
  ```bash
   cd Practical01/PartB_Homework
  ```
- Inside this folder, initialize the project:
  ```bash
  npm init -y
  ```
- Install Express

```bash
   npm install express
```

- Create a server with the following routes:

| Method | Route    | Description                                 |
| ------ | -------- | ------------------------------------------- |
| GET    | /        | Returns "Welcome to Homework API"           |
| GET    | /intro   | Returns a short introduction about yourself |
| GET    | /name    | Returns your name                           |
| GET    | /hobbies | Returns your hobbies                        |
| GET    | /food    | Returns your favourite foods                |

---

### 🔥 Bonus (Optional)

- Figure out how to return a **JSON list** for the `/hobbies` route:

  ```json
  ["coding", "reading", "cycling"],
  ```

- Additionally, figure out how to return an individual **JSON object** for a new `/student` route:

```json
{
  "name": "Alex",
  "hobbies": ["coding", "reading", "cycling"],
  "intro": "Hi, I'm Alex, a Year 2 student passionate about building APIs!"
}
```

---

### 📤 Submission Instructions:

- Complete the homework as specified in Part B
- Submit by commit and push all work to your GitHub repository
- **Deadline:** Before next week’s class

---

### 💡 Tips:

- Use `console.log()` for debugging
- Test routes with Postman
- Comment your code for clarity
