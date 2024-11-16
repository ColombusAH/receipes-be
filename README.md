

make sure you have nest cli installed 
## Description
create with nest cli :
```bash
$ nest new receipes-be
```
## Project setup

```bash
$ npm install
```

## Compile and run the project

```bash
# development
$ npm run start

# watch mode
$ npm run start:dev

# production mode
$ npm run start:prod
```

## Run tests

```bash
# unit tests
$ npm run test

# e2e tests
$ npm run test:e2e

# test coverage
$ npm run test:cov
```

# **Recipes App**

A full-stack Recipes App built with **NestJS**. The app provides user authentication and allows users to manage and interact with recipes.

---

## **Features**

### **Authentication**
1. **Register**:
   - Users can register with an email and password.
   - Passwords are hashed for security.

2. **Login**:
   - Users can log in with their credentials.
   - A JWT is generated upon successful login.

3. **Logout**:
   - Users can log out to invalidate their session.
   - JWT is removed from cookies.

---

### **Recipes**
1. **Add Recipes**:
   - Authenticated users can create new recipes by providing a name, description, and list of ingredients.

2. **View All Recipes**:
   - All users (authenticated or not) can view a list of recipes.

3. **Edit Own Recipes**:
   - Authenticated users can edit recipes they have created.

4. **Mark as Favorite**:
   - Authenticated users can mark recipes as their favorites.

5. **User Recipes**:
   - Authenticated users can view a list of recipes they have created.

---

## **Endpoints**

### **Authentication**
- **POST** `/auth/register`: Register a new user.
- **POST** `/auth/login`: Login with email and password.
- **POST** `/auth/logout`: Logout and clear the session.

### **Recipes**
- **POST** `/recipes`: Add a new recipe (Authenticated).
- **GET** `/recipes`: Get all recipes.
- **PUT** `/recipes/:id`: Edit a recipe (Authenticated, owner-only).
- **POST** `/recipes/:id/favorite`: Mark a recipe as favorite (Authenticated).
- **GET** `/users/me/recipes`: Get recipes created by the current user (Authenticated).

---

## **Technologies Used**

### **Backend**
- **NestJS**
  - Authentication with JWT and cookies.
  - Modular architecture for scalability.
- **TypeORM**
  - Database interactions.
  - Recipe and User entity relationships.
- **PostgreSQL**
  - Database for development.

---


## Proposed solution using nestjs feature 
### **1. Modules**

#### **AuthModule**
- **Purpose**: Handles all authentication-related functionality.
- **Features**:
  - User registration with hashed passwords.
  - Login with JWT generation and validation.
  - Logout by clearing the JWT token.
- **Why**: Encapsulates all authentication logic in a single module for modularity and scalability.

#### **UsersModule**
- **Purpose**: Manages user-related operations and data.
- **Features**:
  - Retrieve authenticated user's details.
  - Manage user profiles (optional).
  - Provide a user’s associated recipes.
- **Why**: Separates user management from authentication for clarity and maintainability.

#### **RecipesModule**
- **Purpose**: Manages recipes and related features.
- **Features**:
  - Create, edit, delete recipes.
  - Fetch all recipes or user-specific recipes.
  - Mark recipes as favorites.
- **Why**: Encapsulates recipe logic to ensure separation of concerns.

---

### **2. Entities**

#### **User Entity**
- **Purpose**: Represents a user in the system, focused on authentication and identity.
- **Key Fields**:
  - `id`: Unique identifier.
  - `email`: User's email (unique).
  - `passwordHash`: Hashed password for security.
- **Why**: Keeps authentication and identity data separate from profile or recipe data.

#### **UserProfile Entity** (optional)
- **Purpose**: Stores additional profile details about a user.
- **Key Fields**:
  - `id`: Unique identifier.
  - `name`: User's display name.
  - `avatar`: Profile picture URL (optional).
- **Why**: Separates optional user details to avoid bloating the `User` entity.

#### **Recipe Entity**
- **Purpose**: Represents recipes created by users.
- **Key Fields**:
  - `id`: Unique identifier.
  - `name`: Name of the recipe.
  - `description`: Recipe description.
  - `ingredients`: List of ingredients.
  - `createdBy`: Relationship to the `User` entity.
- **Why**: Clearly models recipe data and establishes relationships with the user.

#### **Favorite Entity**
- **Purpose**: Tracks recipes marked as favorites by users.
- **Key Fields**:
  - `id`: Unique identifier.
  - `userId`: Relationship to the `User` entity.
  - `recipeId`: Relationship to the `Recipe` entity.
- **Why**: Separates favorite functionality from the `Recipe` entity for better scalability.

---

### **3. Controllers**

#### **AuthController**
- **Purpose**: Exposes endpoints for authentication-related operations.
- **Endpoints**:
  - `POST /auth/register`: Register a new user.
  - `POST /auth/login`: Login and generate a JWT.
  - `POST /auth/logout`: Logout and clear the JWT token.
- **Why**: Centralizes authentication routes for better organization.

#### **UsersController**
- **Purpose**: Exposes endpoints for user-related data.
- **Endpoints**:
  - `GET /users/me`: Retrieve authenticated user’s details.
  - `GET /users/me/recipes`: Fetch recipes created by the authenticated user.
- **Why**: Separates user-related logic from recipes or authentication.

#### **RecipesController**
- **Purpose**: Exposes endpoints for recipe-related operations.
- **Endpoints**:
  - `POST /recipes`: Create a new recipe (Authenticated).
  - `GET /recipes`: Fetch all recipes.
  - `PUT /recipes/:id`: Edit a recipe (Authenticated, owner-only).
  - `POST /recipes/:id/favorite`: Mark a recipe as favorite (Authenticated).
  - `GET /recipes/user/:userId`: Fetch recipes by a specific user.
- **Why**: Manages all recipe interactions to ensure modularity.

---

### **4. Services**

#### **AuthService**
- **Purpose**: Implements authentication logic.
- **Features**:
  - Hash passwords for secure storage.
  - Validate credentials during login.
  - Generate and verify JWT tokens.
- **Why**: Keeps authentication logic reusable and encapsulated.

#### **UsersService**
- **Purpose**: Manages user-specific logic.
- **Features**:
  - Retrieve authenticated user details.
  - Fetch recipes created by a user.
- **Why**: Allows clear separation of user-related business logic.

#### **RecipesService**
- **Purpose**: Implements recipe-related logic.
- **Features**:
  - Create, update, and delete recipes.
  - Fetch recipes by user or globally.
  - Mark recipes as favorites.
- **Why**: Centralizes recipe logic for better maintainability.

---

### **5. Guards**

#### **JWT Auth Guard**
- **Purpose**: Protects endpoints by validating JWT tokens.
- **Why**: Ensures that only authenticated users can access protected routes.

#### **Owner Guard**
- **Purpose**: Ensures that only the owner of a recipe can edit or delete it.
- **Why**: Adds an additional layer of authorization for sensitive actions.

---

### **6. Interceptors**

#### **Response Transformation Interceptor**
- **Purpose**: Standardizes API responses for consistency.
- **Why**: Provides a uniform response structure across the application.

#### **Logging Interceptor**
- **Purpose**: Logs incoming requests and outgoing responses.
- **Why**: Helps with debugging and monitoring.

---

### **7. Middleware**

#### **Cookie Parser Middleware**
- **Purpose**: Parses cookies to retrieve JWT tokens.
- **Why**: Simplifies JWT management by storing tokens in cookies.

---

### **8. Database**

- **Database**: PostgreSQL
  - **Why**: A relational database is well-suited for modeling the relationships between users, recipes, and favorites.
- **ORM**: TypeORM
  - **Why**: Simplifies database interactions and provides decorators for defining entities and relationships.

---

### **9. Configuration Management**

#### **ConfigModule**
- **Purpose**: Centralizes application configuration (e.g., database credentials, JWT secrets).
- **Why**: Simplifies environment-specific setups.

