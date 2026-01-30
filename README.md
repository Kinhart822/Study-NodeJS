# NodeJS Fundamentals Project

Welcome to the **NodeJS Fundamentals** learning project! This repository serves as a practical guide to building web applications and APIs using Node.js and the Express framework, integrated with a PostgreSQL database.

This document details all the technical knowledge, libraries, and concepts implemented in this project.

---

## 📚 Technical Stack & Libraries

This project uses a robust stack to demonstrate modern Node.js development practices:

- **Runtime Environment**: [Node.js](https://nodejs.org/)
- **Framework**: [Express.js](https://expressjs.com/) (v5.1.0) - Fast, unopinionated, minimalist web framework.
- **Templating Engine**: [EJS (Embedded JavaScript)](https://ejs.co/) - Generates HTML markup with plain JavaScript.
- **Database**: [PostgreSQL](https://www.postgresql.org/) - Advanced open-source relational database.
- **Database Client**: [pg (node-postgres)](https://node-postgres.com/) - Non-blocking PostgreSQL client for Node.js.
- **File Uploads**: [Multer](https://github.com/expressjs/multer) - Middleware for handling `multipart/form-data`.
- **Transpiler**: [Babel](https://babeljs.io/) - Used to write modern JavaScript (ES6+ imports/exports) that compiles to Node.js compatible code.
- **Environment Variables**: [dotenv](https://www.npmjs.com/package/dotenv) - Manages configuration (ports, keys) outside the code.
- **Logging**: [Morgan](https://www.npmjs.com/package/morgan) - HTTP request logger middleware.
- **Development Tool**: [Nodemon](https://nodemon.io/) - Automatically restarts the server on file changes.

---

## 📂 Project Structure

Understanding the `src` folder organization:

```text
src/
├── configs/
│   ├── connectDB.js     # Database connection pool setup
│   └── viewEngine.js    # Configures EJS and static file folders
├── controller/
│   ├── homeController.js # Logic for standard web pages (Server-Side Rendering)
│   └── APIController.js  # Logic for API endpoints (JSON responses)
├── modules/             # (Optional) Domain-specific business logic
├── public/              # Static assets (CSS, Images, JS)
├── route/
│   ├── web.js           # Routes for browser based navigation
│   └── api.js           # Routes for RESTful API endpoints
├── views/               # EJS template files (.ejs)
└── server.js            # Entry point of the application
```

---

## 🧠 Knowledge & Concepts Detailed

### 1. Server Setup (`server.js`)

- **Express Instantiation**: Creating the app instance `const app = express()`.
- **Middleware Configuration**:
  - `express.urlencoded({ extended: true })`: Parses incoming request bodies (form submissions).
  - `express.json()`: Parses incoming JSON payloads (API requests).
  - `morgan('combined')`: Logs requests for debugging.
- **Port Configuration**: Using `process.env.PORT` to allow dynamic port assignment via `.env` file.

### 2. View Engine Configuration (`configs/viewEngine.js`)

- **Static Files**: `app.use(express.static('./src/public'))` serves files like images and CSS directly.
- **EJS Setup**: Configures standard View Engine lookup directories so we can just return `res.render('index.ejs')`.

### 3. Database Integration (`configs/connectDB.js`)

- **Connection Pooling**: Uses `Pool` from `pg` library. This is efficient as it reuses database connections instead of opening/closing one for every single request.
- **Async/Await**: All database operations use `await pool.query(...)` to handle asynchronous DB responses cleanly without "callback hell".

### 4. Routing System

The project separates routes into two distinct types:

- **Web Routes (`route/web.js`)**:
  - Designed for human interaction via browser.
  - Returns HTML Pages (Server-Side Rendering).
  - Includes routes for Home, Detail, Edit, and Upload pages.
- **API Routes (`route/api.js`)**:
  - Designed for client-side apps (React/Vue/Mobile) or external consumers.
  - Returns pure JSON data.
  - Follows RESTful conventions (`GET`, `POST`, `PUT`, `DELETE`).
  - Prefixed with `/api/v1` for versioning.

### 5. Controllers Basics

Logic is separated from routes. Controllers handle the "How" while Routes handle the "Where".

- **Receiving Data**:
  - `req.body`: For POST/PUT data (form fields).
  - `req.params`: For URL parameters (e.g., `/user/:id`).
  - `req.query`: For query strings (e.g., `?page=1`).
- **Sending Responses**:
  - `res.render()`: Compile an EJS template and send HTML.
  - `res.json()` or `res.send()`: Send raw data or JSON.

### 6. CRUD Operations Implemented

We built a full "Create, Read, Update, Delete" flow for Users:

- **C (Create)**: `INSERT INTO users...` - Form submission to create a new row.
- **R (Read)**: `SELECT * FROM users...` - Display list of users or details of one user.
- **U (Update)**: `UPDATE users SET...` - Edit existing user data.
- **D (Delete)**: `DELETE FROM users...` - Remove a user.

### 7. File Uploads (Advanced Feature)

Implemented in `route/web.js` using **Multer**.

- **Disk Storage**: Configured to save files to `src/public/image/`.
- **File Renaming**: Appends a timestamp to filenames to imply uniqueness.
- **Validation**: `imageFilter` regex ensures only image formats (jpg, png, etc.) are accepted.
- **Error Handling**: Special middleware `multerErrorHandler` catches specific errors like "File too large" or "Too many files".
- **Modes**:
  - `uploadSingle`: Handles one file (`req.file`).
  - `uploadMultiple`: Handles an array of files (`req.files`).

---

## 🚀 How to Run

1.  **Install Dependencies**:

    ```bash
    npm install
    ```

2.  **Database Setup**:
    - Ensure PostgreSQL is running.
    - Create a local database.
    - Update `.env` file with your credentials (`DB_HOST`, `DB_USER`, `DB_PASSWORD`, `DB_NAME`).

3.  **Start the Server**:

    ```bash
    npm start
    ```

    (This runs `nodemon --exec babel-node src/server.js`)

4.  **Access the App**:
    - Web: [http://localhost:3000](http://localhost:3000)
    - API: [http://localhost:3000/api/v1/users](http://localhost:3000/api/v1/users)

---

_Created by Pham Hoang Anh for Self-Learning._
