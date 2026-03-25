## Practical 7 & 8 - Polytechnic Library API Requirements

**Scenario:**

The Polytechnic Library is developing a new online system to manage book borrowing and user accounts. You and your partner are tasked with building the backend API for this system.

The API needs to be secure and ensure:

- Only authorized users can access the system. This includes librarians and registered library members.
- Library members and Librarians can view books and their availability.
- Librarians have the access to update a book's availability.

**Learning Objectives:**

- Implement data access and manipulation for library resources and library member accounts.
- Develop user authentication mechanisms (registration and login) with password encryption.
- Implement authorization for different user roles (member, librarian) using JWT tokens.

**Technical Requirements:**

- Use Node.js and Express framework to build the API.
- Implement user authentication using username/password with secure hashing (bcryptjs) and JWT-based authentication (jsonwebtoken).
- Implement authorization using roles (member, librarian) to control access to different functionalities (Express middleware).
- Use a database (Microsoft SQL Server) to store user information (Users table) and book data (Books table).
- Design API endpoints for:
  - User registration (POST /register)
  - Login (POST /login)
  - Get all books (GET /books) (accessible by both librarians and members)
  - Update book availability (PUT /books/:bookId/availability) (accessible only by librarians)
- Implement basic error handling and return appropriate status codes (e.g., 200 for success, 401 for unauthorized, 400 for bad request) for successful and failed requests.

**Technologies:**

- Backend: Node.js, Express
- Database: Microsoft SQL Server
- npm packages: mssql, bcryptjs, jsonwebtoken

**Problem:**

Develop a secure API for the Polytechnic library system that allows:

- **Library Members (Students):**
  - Register for an account (POST /register)
  - Login to access the system (POST /login)
  - View a list of books and their availability (Y/N) (GET /books)
- **Librarians:**
  - Register for an account (POST /register)
  - Login to access the system (POST /login)
  - View a list of books and their availability (Y/N) (GET /books)
  - Update the availability of books (Y/N) (PUT /books/:bookId/availability)

**Part 1: Data Access and Manipulation**

1. **Database Schema:** Define database schemas for both library resources and users.

   - **Users Table:**

     - user_id (INT, PRIMARY KEY)
     - username (VARCHAR(255), UNIQUE)
     - passwordHash (VARCHAR(255))
     - role (VARCHAR(20), ('member', 'librarian'))

   - **Books Table:**
     - book_id (INT, PRIMARY KEY)
     - title (VARCHAR(255))
     - author (VARCHAR(255))
     - availability (CHAR(1), ('Y', 'N'))

2. **SQL Scripts:** Create SQL scripts to create the Books and Users tables in Microsoft SQL Server.

3. **Implement Get All Books (GET /books):**

   - This endpoint should retrieve a list of all books from the database.
   - Return a JSON response containing an array of book objects (including title, author, and availability).

4. **Implement Update Book Availability (PUT /books/:bookId/availability):**

   - This endpoint should update the availability of a book (Y/N) based on its ID.
   - It should accept the updated availability value in the request body.
   - Update the book's availability in the database.
   - Upon successful update, return a success message or the updated book object.

**Part 2: Authentication - User Registration**

Implement user registration, hash the user's password before storing it in the database. Implement password hashing using bcryptjs.

1. **User Registration (POST /register):**

   - This endpoint should allow users (members and librarians) to register for the system.
   - It should accept username, password, and role in the request body.
   - Implement user validation (e.g., username uniqueness, password strength).
   - Hash the password before storing it in the database.
   - Upon successful registration, return a success message or create a new user resource.

**Code Example (User Registration and Password Hashing):**

```javascript
const bcrypt = require("bcryptjs");

async function registerUser(req, res) {
  const { username, password, role } = req.body;

  try {
    // Validate user data
    // ... your validation logic here ...

    // Check for existing username
    const existingUser = await getUserByUsername(username);
    if (existingUser) {
      return res.status(400).json({ message: "Username already exists" });
    }

    // Hash password
    const salt = await bcrypt.genSalt(10);
    const hashedPassword = await bcrypt.hash(password, salt);

    // Create user in database
    // ... your database logic here ...

    return res.status(201).json({ message: "User created successfully" });
  } catch (err) {
    console.error(err);
    return res.status(500).json({ message: "Internal server error" });
  }
}
```

**Note:** This code snippet assumes helper functions like `getUserByUsername` to retrieve users from the database.

**Part 3: Authentication - Login and JWT Token Generation**

1. **Login API Endpoint:** Create an API endpoint for user login. This endpoint should:

   - Validate user credentials (username, password).
   - Retrieve the user record from the database.
   - Compare the provided password with the hashed password using bcryptjs.compare().
   - If successful, generate a JWT token containing user information (e.g., user ID, role) and return it to the client.

**Code Example (JWT Token Generation during Login):**

```javascript
const jwt = require("jsonwebtoken");
const bcrypt = require("bcryptjs");

async function login(req, res) {
  const { username, password } = req.body;

  try {
    // Validate user credentials
    const user = await getUserByUsername(username);
    if (!user) {
      return res.status(401).json({ message: "Invalid credentials" });
    }

    // Compare password with hash
    const isMatch = await bcrypt.compare(password, user.passwordHash);
    if (!isMatch) {
      return res.status(401).json({ message: "Invalid credentials" });
    }

    // Generate JWT token
    const payload = {
      id: user.id,
      role: user.role,
    };
    const token = jwt.sign(payload, "your_secret_key", { expiresIn: "3600s" }); // Expires in 1 hour

    return res.status(200).json({ token });
  } catch (err) {
    console.error(err);
    return res.status(500).json({ message: "Internal server error" });
  }
}
```

**Part 4: Authorization using Middleware**

1. **JWT (JSON Web Token):** Implement JWT token verification for authorization. Use a library like jsonwebtoken to verify JWT tokens.

2. **User Roles:** Define user roles (member, librarian) in your application.

3. **Authorization Middleware:** Create an Express middleware function to check for valid JWT tokens and user roles before allowing access to protected API endpoints. This middleware should:

   - Extract the JWT token from the request header.
   - Verify the token's signature and expiration.
   - Decode the token payload to retrieve user information (including role).
   - Check if the user's role has the required permission for the requested endpoint (e.g., member for viewing books, librarian for updating availability).
   - If authorized, proceed with the request. Otherwise, return an error response.

**Code Example (Authorization Middleware):**

```javascript
const jwt = require("jsonwebtoken");

function verifyJWT(req, res, next) {
  const token =
    req.headers.authorization && req.headers.authorization.split(" ")[1];

  if (!token) {
    return res.status(401).json({ message: "Unauthorized" });
  }

  jwt.verify(token, "your_secret_key", (err, decoded) => {
    if (err) {
      return res.status(403).json({ message: "Forbidden" });
    }

    // Check user role for authorization (replace with your logic)
    const authorizedRoles = {
        // Only librarians can create books
        "POST /books": ["librarian"], 
        // Anyone can view books
        "GET /books": ["member", "librarian"], 
        // Only librarians can update availability
        "PUT /books/[0-9]+/availability": ["librarian"], 
    };

    const requestedEndpoint = `${req.method} ${req.url}`; // Include method in endpointl;
    const userRole = decoded.role;

    const authorizedRole = Object.entries(authorizedRoles).find(
      ([endpoint, roles]) => {
        const regex = new RegExp(`^${endpoint}$`); // Create RegExp from endpoint
        return regex.test(requestedEndpoint) && roles.includes(userRole);
      }
    );

    if (!authorizedRole) {
      return res.status(403).json({ message: "Forbidden" });
    }

    req.user = decoded; // Attach decoded user information to the request object
    next();
  });
}
```

**Part 5: Add Authorization to API Endpoints**

1. **Get All Books (GET /books):**

   - Add authorization to this endpoint using the middleware created in Part 4.
   - It should be accessible to both members and librarians.

2. **Update Book Availability (PUT /books/:bookId/availability):**

   - Add authorization using the middleware created in Part 4 to restrict access to librarians only.

**Remember:**

- Replace placeholders like "your_secret_key" with your actual credentials.
- Implement proper error handling throughout the application.
- Secure your database connection details and JWT secret key using environment variables.
