# Mini E-Commerce API

A RESTful backend system that simulates a basic online shopping platform.
The project demonstrates secure authentication, role-based authorization, product management, cart handling, and order processing while maintaining proper business logic and data consistency.

---

## Live Demo



```
https://mini-ecommerce-api-2.onrender.com/
```

---

## Tech Stack

| Layer            | Technology                           |
| ---------------- | ------------------------------------ |
| Language         | Java 17                              |
| Framework        | Spring Boot 4                        |
| Security         | Spring Security + JWT Authentication |
| Database         | PostgreSQL                           |
| ORM              | Spring Data JPA (Hibernate)          |
| Build Tool       | Maven                                |
| Containerization | Docker                               |
| Deployment       | Render                               |
| API Testing      | Postman                              |

---

## Authentication & Authorization

The system uses **JWT (JSON Web Token)** based authentication.

### Roles

* **ADMIN**
* **CUSTOMER**

### Features

* User registration
* User login
* Secure password hashing
* Protected endpoints
* Role-based access control

Only authenticated users can access protected resources.
Admin-only endpoints are restricted using Spring Security authorization.

---

## Core Entities

The system contains the following main entities:

* **User**
* **Product**
* **Cart**
* **Order**
* **OrderItems**

### Relationships

* A User has one Cart
* A Cart contains multiple Products
* An Order belongs to a User
* An Order contains multiple OrderItems
* Each OrderItem references a Product

---

## Features

### Customer Features

* Register account
* Login
* View products
* Add product to cart
* Remove product from cart
* Place order

### Admin Features

* Add product
* Update product
* Delete product
* Update product stock

---

## Business Rules Implemented

* Users cannot order more than available stock
* Product stock is deducted **only after successful order placement**
* Negative inventory is prevented
* Order total is calculated on the backend
* Data consistency is maintained
* Prevents stock misuse due to repeated cancellations

---

## Order Flow

1. Customer adds items to cart
2. Customer places order
3. Backend validates stock availability
4. Order total is calculated
5. Stock is deducted
6. Order is stored in database

---

## Security Implementation

* JWT token authentication
* Stateless session management
* Password encryption using BCrypt
* Role-based endpoint protection
* Unauthorized access returns proper HTTP status codes

---

## API Design

The API follows **RESTful conventions** and uses proper HTTP methods:

| Method | Usage            |
| ------ | ---------------- |
| GET    | Retrieve data    |
| POST   | Create resources |
| PUT    | Update resources |
| DELETE | Remove resources |

### Example Status Codes

* `200 OK`
* `201 Created`
* `400 Bad Request`
* `401 Unauthorized`
* `403 Forbidden`
* `404 Not Found`

---

## ⚙️ Setup Instructions (Local)

### 1. Clone Repository

```bash
git clone https://github.com/<your-username>/mini-ecommerce-api.git
cd mini-ecommerce-api
```

### 2. Configure Database

Create a PostgreSQL database and update environment variables:

```
SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/ecommerce_db
SPRING_DATASOURCE_USERNAME=postgres
SPRING_DATASOURCE_PASSWORD=yourpassword
```

### 3. Run Application

```bash
mvn clean package
java -jar target/api-0.0.1-SNAPSHOT.jar
```

Application will start on:

```
http://localhost:8080
```

---

## 🐳 Running with Docker

Build image:

```bash
docker build -t mini-ecommerce-api .
```

Run container:

```bash
docker run -p 8080:8080 mini-ecommerce-api
```

---

## Database Behavior

Hibernate configuration:

* Auto table creation (`ddl-auto=update`)
* Relationships handled with JPA mappings
* Transactions ensure data integrity

---

## Key Architectural Decisions

* JWT used instead of sessions for stateless authentication
* Business logic handled in Service layer
* Controller layer only handles HTTP requests/responses
* Repository layer handles database operations
* Transactions used during order placement to maintain consistency

---

## Assumptions

* Each user owns a single cart
* Orders cannot be modified after placement
* Stock is reserved only during successful order creation
* Payments are simulated (no real payment gateway)

---

## API Documentation

You can test endpoints using **Postman**.

## 🧪 Testing the Live API (Postman)

Base URL:

```
https://mini-ecommerce-api-2.onrender.com
```

---

### 🔐 Test Authentication

#### 1. Register Admin

```
POST https://mini-ecommerce-api-2.onrender.com/api/auth/register-admin
Content-Type: application/json

{
  "username": "admin",
  "email": "admin@example.com",
  "password": "admin123"
}
```

**Expected Response**

```json
{
  "message": "Admin user registered successfully!"
}
```

---

#### 2. Register Customer

```
POST https://mini-ecommerce-api-2.onrender.com/api/auth/register
Content-Type: application/json

{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "password123"
}
```

---

#### 3. Login (Admin or Customer)

```
POST https://mini-ecommerce-api-2.onrender.com/api/auth/login
Content-Type: application/json

{
  "username": "admin",
  "password": "admin123"
}
```

**Expected Response**

```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "type": "Bearer",
  "id": 1,
  "username": "admin",
  "email": "admin@example.com",
  "role": "ADMIN"
}
```

⚠️ **IMPORTANT:** Copy the `token` value.
You must use it in the `Authorization` header for protected requests.

Header format:

```
Authorization: Bearer <YOUR_TOKEN>
```

---

## 📦 Product Management (Admin Only)

### Create Product

```
POST https://mini-ecommerce-api-2.onrender.com/api/products/admin
Authorization: Bearer <PASTE_ADMIN_TOKEN_HERE>
Content-Type: application/json

{
  "name": "Laptop",
  "description": "High-performance laptop",
  "price": 999.99,
  "stockQuantity": 50
}
```

### Get All Products

```
GET https://mini-ecommerce-api-2.onrender.com/api/products
Authorization: Bearer <PASTE_ADMIN_TOKEN_HERE>
```

---

## 🛍️ Cart Operations (Customer)

First login as customer and copy the token.

```
POST https://mini-ecommerce-api-2.onrender.com/api/auth/login
Content-Type: application/json

{
  "username": "john_doe",
  "password": "password123"
}
```

### Add to Cart

```
POST https://mini-ecommerce-api-2.onrender.com/api/cart/items
Authorization: Bearer <PASTE_CUSTOMER_TOKEN_HERE>
Content-Type: application/json

{
  "productId": 1,
  "quantity": 2
}
```

### View Cart

```
GET https://mini-ecommerce-api-2.onrender.com/api/cart
Authorization: Bearer <PASTE_CUSTOMER_TOKEN_HERE>
```

---

## 📦 Order Placement

### Place Order

```
POST https://mini-ecommerce-api-2.onrender.com/api/orders
Authorization: Bearer <PASTE_CUSTOMER_TOKEN_HERE>
```

### View Orders

```
GET https://mini-ecommerce-api-2.onrender.com/api/orders
Authorization: Bearer <PASTE_CUSTOMER_TOKEN_HERE>
```


---

## What This Project Demonstrates

* Secure backend authentication
* Role-based authorization
* Transaction handling
* Database relationships
* Clean layered architecture
* REST API best practices

---

## Author

Jerry Jeriomio Joydhor

GitHub: https://github.com/Jerry1921

---

## Acknowledgement

This project was built as a backend engineering practice project to demonstrate real-world API design and business logic implementation.
