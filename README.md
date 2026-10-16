# CampusFlow-
Student Management Portal
# CampusFlow — Student Management Portal

## Project Overview

CampusFlow is a student management website built using HTML, CSS, JavaScript, Node.js, Express, and MySQL.

The project demonstrates four backend development tasks:

1. REST API development using Node.js and Express.
2. MySQL database integration.
3. Authentication and authorization using bcryptjs and JSON Web Tokens (JWT).
4. Form handling and data validation.

The website provides a user-friendly interface for managing student records through the existing backend API.

## Features

- **Dashboard:** Displays student statistics, server status, and project milestones.
- **User registration and login:** Allows users to create accounts and sign in.
- **Student management:** View, search, add, and edit student records.
- **Admin authorization:** Restricts student deletion to administrator accounts.
- **Form validation:** Validates student and account details.
- **Database integration:** Saves and retrieves student information using MySQL.
- **Responsive design:** Provides a clean interface for different screen sizes.
- **Login prompt:** When logged out, clicking Add Student displays a message asking the user to log in.

## Technologies Used

- HTML5
- CSS3
- JavaScript
- Node.js
- Express.js
- MySQL
- mysql2
- bcryptjs
- jsonwebtoken
- dotenv

## Project Structure

```text
student-api/
├── public/
│   ├── index.html
│   ├── styles.css
│   ├── app.js
│   └── admin-login.css
├── server.js
├── .env
├── .gitignore
├── package.json
└── README.md
```

The `admin-login.css` file is needed if your separate administrator login form uses it.

## Prerequisites

Before running the application, install:

- Node.js and npm
- MySQL Server
- Visual Studio Code

You must also have the MySQL database and required tables configured.

## Installation and Setup

### 1. Open the project

Open the `student-api` folder in Visual Studio Code.

### 2. Install dependencies

Open the VS Code terminal and run:

```bash
npm install
```

If the required packages are missing, install them using:

```bash
npm install express mysql2 bcryptjs jsonwebtoken dotenv
```

### 3. Configure environment variables

Create or update the `.env` file in the root folder:

```env
DB_HOST=localhost
DB_USER=your_mysql_username
DB_PASSWORD=your_mysql_password
DB_NAME=student_db
JWT_SECRET=your_long_random_secret
PORT=3000
```

Replace the example values with your actual MySQL credentials and a secure JWT secret.

**Important:** Never publish your `.env` file or share your database password.

### 4. Start MySQL

Make sure MySQL Server is running and the database specified by `DB_NAME` exists.

### 5. Start the server

Run:

```bash
node server.js
```

When the connection succeeds, the terminal should display messages similar to:

```text
MySQL database connected successfully!
Server running at http://localhost:3000
```

### 6. Open the website

Open this address in your browser:

http://localhost:3000

Keep the terminal running while using the application.

## How to Use the Website

### User registration and login

1. Open CampusFlow.
2. Select the user login option.
3. Create an account with your name, email, and password, or sign in with an existing account.
4. After logging in, open the Students section.

### Add a student

1. Log in to your account.
2. Click Add Student.
3. Enter the student's name and course.
4. Submit the form.
5. The backend validates the data and saves it to MySQL.

If you are logged out, clicking Add Student should display a login message and open the login form.

### Edit a student

1. Open Students.
2. Find the required student.
3. Click the edit button.
4. Update the student's details and save the changes.

### Delete a student

Only an account with the `admin` role can delete student records.

1. Sign in with an administrator account.
2. Open Students.
3. Click the delete button beside the selected student.
4. Confirm the deletion.

Deletion permanently removes the record from the database in the current implementation.

## Administrator Account Setup

Newly registered accounts receive the `user` role by default.

To assign administrator access to an existing account, connect to MySQL and execute:

```sql
USE student_db;

UPDATE users
SET role = 'admin'
WHERE email = 'your-registered-email@example.com';

SELECT name, email, role
FROM users
WHERE email = 'your-registered-email@example.com';
```

Replace the example email with the actual registered email. If your database has a different name, use the value configured in `DB_NAME`.

After changing the role, log out and log in again so the new JWT contains the updated role.

Do not display or publish administrator passwords.

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/register` | Register a new user |
| POST | `/api/auth/login` | Authenticate a user and return a JWT |
| GET | `/api/students` | Retrieve all students |
| GET | `/api/students/:id` | Retrieve a student by ID |
| POST | `/api/students` | Add a student |
| PUT | `/api/students/:id` | Update a student |
| DELETE | `/api/students/:id` | Delete a student; admin only |

Student endpoints require a valid JWT in the request header:

```http
Authorization: Bearer YOUR_JWT_TOKEN
```

The Express backend enforces authentication and authorization independently of the frontend.

## Database Tables

The backend expects a `users` table containing fields for:

- `id`
- `name`
- `email`
- `password`
- `role`

The password field stores the bcrypt password hash.

The `students` table contains:

- `id`
- `name`
- `course`

The tables must exist in the configured MySQL database before the application can use them.

## Troubleshooting

### Website does not open

- Confirm that `node server.js` is running.
- Check that `app.use(express.static("public"));` is present in `server.js`.
- Confirm that `index.html`, `styles.css`, and `app.js` are inside `public`.
- Refresh the browser using Ctrl + F5.

### Database connection fails

- Confirm that MySQL Server is running.
- Check the values in `.env`.
- Verify that the database and required tables exist.

### Student records do not load

- Log in before accessing protected student endpoints.
- Check the browser console and VS Code terminal for errors.
- Verify that the backend returns student records in the `data` property.

### Delete button is missing

- Confirm that the account's role is exactly `admin`.
- Log out and log in again.
- Verify that the login response includes the correct user role.

### Add Student does nothing when logged out

- Ensure the Add Student buttons are not disabled in `updateSessionUI()`.
- Confirm that the buttons call `openStudentForm()`.
- Verify that `openStudentForm()` displays the login message and opens the login form when the user is logged out.

## Security Notes

- Do not commit `.env` or publish administrator credentials.
- Never store plain-text passwords; use password hashing.
- The current frontend stores the JWT in `localStorage`, which has security trade-offs if the site is vulnerable to cross-site scripting.
- Keep authorization and validation checks on the server.
- Use HTTPS and appropriate secret management before deploying to production.

## Project Objective

CampusFlow demonstrates how a frontend website communicates with a REST API, authenticates users, validates submitted data, and performs database operations using MySQL.

The project is intended for learning and demonstration purposes and should receive additional security hardening before production deployment.
