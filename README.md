"# 2-Tier Backend Application

A robust Node.js Express REST API with PostgreSQL database integration, the **2-Tier Backend Application** provides comprehensive user management functionality with validation, error handling, and database operations.

## 🚀 Features

- **RESTful API** - Complete CRUD operations for user management
- **PostgreSQL Integration** - Robust database connectivity with connection pooling
- **Input Validation** - Comprehensive data validation using Joi
- **Error Handling** - Centralized error handling middleware
- **CORS Support** - Cross-origin resource sharing enabled
- **Environment Configuration** - Secure environment variable management
- **Auto Table Creation** - Automatic database table initialization
- **ES6 Modules** - Modern JavaScript module system

## 📋 Table of Contents

- [Features](#-features)
- [Technologies Used](#-technologies-used)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [Docker Setup](#-docker-setup-recommended)
- [Docker Management Commands](#-docker-management-commands)
- [Environment Variables](#-environment-variables)
- [Database Setup](#-database-setup)
- [Running the Application](#-running-the-application)
- [API Endpoints](#-api-endpoints)
- [Request/Response Examples](#-requestresponse-examples)
- [Error Handling](#-error-handling)
- [Contributing](#-contributing)

## 🛠 Technologies Used

- **Backend Framework**: Express.js 5.1.0
- **Runtime**: Node.js (ES6 Modules)
- **Database**: PostgreSQL
- **Database Driver**: pg (node-postgres) 8.16.3
- **Validation**: Joi 18.0.1
- **Environment Management**: dotenv 17.2.2
- **CORS**: cors 2.8.5
- **Development**: nodemon 3.1.10

## 📁 Project Structure

The **2-Tier Backend Application** follows a modular architecture:

```
src/
├── config/
│   └── db.js                 # Database connection configuration
├── controller/
│   └── userController.js     # User route handlers
├── Data/
│   ├── createTable.js        # Database table creation
│   └── Data.sql             # SQL schema definition
├── Middleware/
│   ├── errorhandler.js       # Global error handling middleware
│   └── inputValidator.js     # Request validation middleware
├── models/
│   └── UsersModel.js        # User data access layer
├── routes/
│   └── userRoute.js         # API route definitions
└── index.js                 # Application entry point
```

## 🔧 Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Tejasvini-malviya/2-Tier-Backend-Application.git
   cd 2-Tier-Backend-Application
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your database credentials
   ```

## 🐳 Docker Setup (Recommended)

This setup provides a complete development environment with PostgreSQL and pgAdmin using Docker containers.

### Option 1: Full Docker Setup

#### Step 1: Create Docker Network
Create a network for containers to communicate:
```bash
docker network create pg-network
```

#### Step 2: Run PostgreSQL Container
Pull the PostgreSQL image:
```bash
docker pull postgres
```

Start PostgreSQL database container:
```bash
docker run --name postgres-db \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=1234 \
  -e POSTGRES_DB=mydb \
  --network pg-network \
  -p 5432:5432 \
  -d postgres
```

**For Windows PowerShell:**
```powershell
docker run --name postgres-db `
  -e POSTGRES_USER=postgres `
  -e POSTGRES_PASSWORD=1234 `
  -e POSTGRES_DB=mydb `
  --network pg-network `
  -p 5432:5432 `
  -d postgres
```

#### Step 3: Run pgAdmin Container
Start pgAdmin for database management:
```bash
docker run --name pgadmin \
  -e PGADMIN_DEFAULT_EMAIL=admin@admin.com \
  -e PGADMIN_DEFAULT_PASSWORD=admin \
  --network pg-network \
  -p 8080:80 \
  -d dpage/pgadmin4
```

**For Windows PowerShell:**
```powershell
docker run --name pgadmin `
  -e PGADMIN_DEFAULT_EMAIL=admin@admin.com `
  -e PGADMIN_DEFAULT_PASSWORD=admin `
  --network pg-network `
  -p 8080:80 `
  -d dpage/pgadmin4
```

#### Step 4: Verify Containers
Check that both containers are running:
```bash
docker ps
```

You should see both `postgres-db` and `pgadmin` containers in the running state.

#### Step 5: Configure pgAdmin
1. **Access pgAdmin**
   - Open your browser and navigate to: http://localhost:8080
   - Login credentials:
     - **Email:** `admin@admin.com`
     - **Password:** `admin`

2. **Add PostgreSQL Server**
   - Click "Add New Server"
   - **General Tab:**
     - **Name:** `Postgres DB`
   - **Connection Tab:**
     - **Host:** `postgres-db`
     - **Port:** `5432`
     - **Username:** `postgres`
     - **Password:** `1234`
     - **Database:** `mydb`
   - Click "Save"

#### Step 6: Build and Run Application Container

**Prerequisites**: Ensure you have a `Dockerfile` in your project root. If you don't have one, create it with appropriate Node.js configuration.

1. **Build the Docker image**
   ```bash
   docker build -t 2-tier-backend-application .
   ```

2. **Run the Node.js application container**
   ```bash
   docker run -p 5000:5000 \
     --network pg-network \
     -e DB_HOST=postgres-db \
     -e DB_USER=postgres \
     -e DB_NAME=mydb \
     -e DB_PASSWORD=1234 \
     -e DB_PORT=5432 \
     -d \
     --name nodeapp \
     2-tier-backend-application
   ```

   **For Windows PowerShell:**
   ```powershell
   docker run -p 5000:5000 `
     --network pg-network `
     -e DB_HOST=postgres-db `
     -e DB_USER=postgres `
     -e DB_NAME=mydb `
     -e DB_PASSWORD=1234 `
     -e DB_PORT=5432 `
     -d `
     --name nodeapp `
     2-tier-backend-application
   ```

#### Step 7: Verify All Containers
Check that all three containers are running:
```bash
docker ps
```

You should see `postgres-db`, `pgadmin`, and `nodeapp` containers in the running state.

### Option 2: Local Development with Docker Database

If you prefer to run the Node.js application locally while using Docker for the database:

1. **Follow Steps 1-5 above** to set up PostgreSQL and pgAdmin containers
2. **Install Node.js dependencies**
   ```bash
   npm install
   ```
3. **Configure environment variables** (see Environment Variables section below)
4. **Start the development server**
   ```bash
   npm run dev
   ```

## 🛠 Docker Management Commands

### Useful Docker Commands

#### Start Existing Containers
```bash
docker start postgres-db
docker start pgadmin
docker start nodeapp
```

#### Stop Containers
```bash
docker stop postgres-db
docker stop pgadmin
docker stop nodeapp
```

#### Remove Containers (⚠️ This will delete all data)
```bash
docker rm postgres-db
docker rm pgadmin
docker rm nodeapp
docker network rm pg-network
```

#### View Container Logs
```bash
docker logs postgres-db
docker logs pgadmin
docker logs nodeapp
```

#### Execute Commands in PostgreSQL Container
```bash
docker exec -it postgres-db psql -U postgres -d mydb
```

### Database Access Points

- **PostgreSQL Database:** `localhost:5432`
- **pgAdmin Web Interface:** `http://localhost:8080`
- **API Server:** `http://localhost:5000`

**Note**: Whether you run the Node.js application locally or in a Docker container, the API will be accessible at the same URL (`http://localhost:5000`) due to port mapping.

## 🌍 Environment Variables

Create a `.env` file in the root directory with the following variables:

### For Local PostgreSQL Setup:
```env
PORT=5000
DB_USER=postgres
DB_PASSWORD=your_password
DB_HOST=localhost
DB_PORT=5432
DB_NAME=your_database_name
```

### For Docker Setup (Application in Container):
```env
PORT=5000
DB_USER=postgres
DB_PASSWORD=1234
DB_HOST=postgres-db
DB_PORT=5432
DB_NAME=mydb
```

### For Local Development with Docker Database:
```env
PORT=5000
DB_USER=postgres
DB_PASSWORD=1234
DB_HOST=localhost
DB_PORT=5432
DB_NAME=mydb
```

## 🗄 Database Setup

The **2-Tier Backend Application** automatically handles database setup:

1. **Install PostgreSQL** on your system
2. **Create a database**:
   ```sql
   CREATE DATABASE your_database_name;
   ```
3. **Update environment variables** with your database credentials
4. **Start the application** - tables will be created automatically

### Database Schema

The application creates a `users` table with the following structure:

```sql
CREATE TABLE IF NOT EXISTS users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    age INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## 🚀 Running the Application

Start the **2-Tier Backend Application** using one of the following methods:

### Development Mode
```bash
npm run dev
```

### Production Mode
```bash
npm start
```

The **2-Tier Backend Application** server will start on `http://localhost:5000` (or your specified PORT).

## 📡 API Endpoints

### Base URL: `http://localhost:5000/api`

| Method | Endpoint | Description | Validation |
|--------|----------|-------------|------------|
| GET | `/users` | Get all users | None |
| GET | `/users/:id` | Get user by ID | ID validation |
| POST | `/users` | Create new user | Full validation |
| PUT | `/users/:id` | Update user | ID + partial validation |
| DELETE | `/users/:id` | Delete user | ID validation |

### Additional Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Health check |
| GET | `/test` | Database connection test |

## 📝 Request/Response Examples

### Create User
**POST** `/api/users`

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "age": 30
}
```

**Response (201):**
```json
{
  "status": 201,
  "message": "User Created",
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com",
    "age": 30,
    "created_at": "2025-09-21T10:30:00.000Z"
  }
}
```

### Get All Users
**GET** `/api/users`

**Response (200):**
```json
{
  "status": 200,
  "message": "Users Retrieved",
  "data": [
    {
      "id": 1,
      "name": "John Doe",
      "email": "john.doe@example.com",
      "age": 30,
      "created_at": "2025-09-21T10:30:00.000Z"
    }
  ]
}
```

### Update User
**PUT** `/api/users/1`

**Request Body:**
```json
{
  "name": "John Smith",
  "age": 31
}
```

**Response (200):**
```json
{
  "status": 200,
  "message": "User Updated",
  "data": {
    "id": 1,
    "name": "John Smith",
    "email": "john.doe@example.com",
    "age": 31,
    "created_at": "2025-09-21T10:30:00.000Z"
  }
}
```

## ⚠️ Error Handling

### Validation Errors (400)
```json
{
  "status": 400,
  "message": "Validation failed",
  "errors": [
    "Name must be at least 2 characters long",
    "Please provide a valid email address"
  ]
}
```

### Server Errors (500)
```json
{
  "status": 500,
  "message": "Something broke!",
  "error": "Detailed error message"
}
```

## 🔍 Validation Rules

### User Creation/Update
- **Name**: 2-100 characters, required for creation
- **Email**: Valid email format, max 100 characters, unique, required for creation
- **Age**: Integer between 1-150, required for creation
- **Update**: At least one field must be provided

### ID Parameter
- Must be a positive integer

## 🧪 Testing the API

Test the **2-Tier Backend Application** API endpoints:

### Using curl

1. **Create a user:**
   ```bash
   curl -X POST http://localhost:5000/api/users \
     -H "Content-Type: application/json" \
     -d '{"name":"John Doe","email":"john@example.com","age":30}'
   ```

2. **Get all users:**
   ```bash
   curl http://localhost:5000/api/users
   ```

3. **Test database connection:**
   ```bash
   curl http://localhost:5000/test
   ```

### Using Postman

Import the following endpoints:
- GET `http://localhost:5000/api/users`
- POST `http://localhost:5000/api/users`
- PUT `http://localhost:5000/api/users/1`
- DELETE `http://localhost:5000/api/users/1`

## 🔧 Code Architecture

The **2-Tier Backend Application** follows the MVC (Model-View-Controller) pattern:

### MVC Pattern
- **Models**: Data access layer (`UsersModel.js`)
- **Views**: JSON responses (handled by controllers)
- **Controllers**: Business logic (`userController.js`)

### Middleware Stack
1. **CORS** - Cross-origin requests
2. **JSON Parser** - Request body parsing
3. **Route Handlers** - API endpoints
4. **Validation** - Input validation
5. **Error Handler** - Global error handling

## 📦 Dependencies

### Production Dependencies
```json
{
  "cors": "^2.8.5",          // Cross-origin resource sharing
  "dotenv": "^17.2.2",       // Environment variable management
  "express": "^5.1.0",       // Web framework
  "joi": "^18.0.1",          // Data validation
  "pg": "^8.16.3"            // PostgreSQL client
}
```

### Development Dependencies
```json
{
  "nodemon": "^3.1.10"       // Auto-restart development server
}
```

## 🤝 Contributing

Contribute to the **2-Tier Backend Application**:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the ISC License.

## 🆘 Troubleshooting

Common issues when running the **2-Tier Backend Application**:

### Common Issues

1. **Database Connection Error**
   - Verify PostgreSQL is running
   - Check environment variables
   - Ensure database exists

2. **Port Already in Use**
   - Change PORT in .env file
   - Kill existing process: `lsof -ti:5000 | xargs kill -9`

3. **Validation Errors**
   - Check request body format
   - Ensure all required fields are provided
   - Verify data types match schema

## 📞 Support

For support with the **2-Tier Backend Application**, email your-email@example.com or create an issue in the repository.

---

**2-Tier Backend Application - Built with ❤️ using Node.js and PostgreSQL**" 
