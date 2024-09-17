# ReUseFlow-

```markdown
# Login API Documentation

## Introduction

This API allows user registration, login, and user management with JWT authentication. It supports two user roles: `ADMIN` and `CLIENTE`. The API uses Spring Security for authentication and authorization, and JWT tokens to manage sessions.

## Endpoints

### 1. Register a New User

**URL:** `/api/auth/register`  
**Method:** `POST`  
**Description:** Register a new user in the system.

#### Request Body:
```json
{
  "username": "exampleUser",
  "password": "examplePassword",
  "role": "CLIENTE"  // Optional: default is CLIENTE if not provided
}
```

#### Response:
- `200 OK`: User registered successfully.
- `400 Bad Request`: If the request is invalid.

#### Example Response:
```text
User registered successfully
```

---

### 2. Login User

**URL:** `/api/auth/login`  
**Method:** `POST`  
**Description:** Authenticates a user and returns a JWT token.

#### Request Body:
```json
{
  "username": "exampleUser",
  "password": "examplePassword"
}
```

#### Response:
- `200 OK`: Returns the JWT token along with the username and user role.
- `401 Unauthorized`: If authentication fails.

#### Example Response:
```json
{
  "token": "jwt-token-here",
  "username": "exampleUser",
  "role": "CLIENTE"
}
```

---

### 3. Get All Users

**URL:** `/api/auth/users`  
**Method:** `GET`  
**Description:** Retrieves a list of all registered users. Only accessible if the user is authenticated.

#### Response:
- `200 OK`: Returns a list of users.

#### Example Response:
```json
[
  {
    "id": 1,
    "username": "exampleUser",
    "role": "CLIENTE"
  },
  {
    "id": 2,
    "username": "adminUser",
    "role": "ADMIN"
  }
]
```

---

## Security

### JWT Token

All authenticated routes require a valid JWT token. Include the token in the `Authorization` header as follows:

```text
Authorization: Bearer <jwt-token-here>
```

The JWT contains the user's `username` and `role`, which are used for role-based authorization.

---

## User Model

### Fields:

- `id`: Auto-generated user ID.
- `username`: Username for the user.
- `password`: Encrypted password stored in the database.
- `role`: User role (either `ADMIN` or `CLIENTE`).

---

## Roles

### Role Enum:

- `ADMIN`: Has administrative privileges.
- `CLIENTE`: Default user role.

---

## Security Configuration

The API allows all requests to `/api/auth/**` without authentication, while all other routes require authentication. Passwords are encrypted using BCrypt.

```java
http
    .csrf(csrf -> csrf.disable())
    .authorizeHttpRequests(auth -> auth
        .requestMatchers("/api/auth/**").permitAll()
        .anyRequest().authenticated()
    )
    .httpBasic();
```

---

## JWT Util

The `JwtUtil` class generates tokens with a 10-hour expiration time and includes the user's role in the token payload. 

### Token Structure:
- `sub`: Username
- `role`: User role
- `iat`: Issued at time
- `exp`: Expiration time
```

### Example JWT Payload:
```json
{
  "sub": "exampleUser",
  "role": "CLIENTE",
  "iat": 1631585400,
  "exp": 1631628600
}
```

---

## How to Run

1. Clone the repository.
2. Install dependencies.
3. Set up your database and update the application properties.
4. Run the application.

---

## Dependencies

- Spring Boot
- Spring Security
- Spring Data JPA
- JWT
- Lombok
- H2 (or other database)
```

