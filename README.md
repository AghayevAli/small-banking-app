# 🏦 Small Banking Backend Application

This is a simple banking backend REST API

The application allows basic customer balance operations: **top-up**, **purchase**, and **refund**.

```plaintext
       [Postman]
            |
            v
     [API Gateway (8080)]
        /           \
       v             v
[ms-customer (8081)] [ms-transaction (8082)]
       |                     |
 [db_customer]          [db_transaction]
```

---

## ✨ Features

- Create customer (Name, Surname, Birth Date, Phone Number, Balance)
- Initial customer balance is set to **100 AZN**
- Top-up, purchase, and refund operations
- Persist customer and transaction data into **PostgreSQL**
- Microservice-based separation:
  - `ms-customer`
  - `ms-transaction`
  - `api-gateway` for routing
- Docker Compose support for local environment
- Postman collection provided for easy testing

---

## 🔒 Security Notes

In a production-ready architecture, security would typically be designed in the following way:

- Implement a **Centralized Authentication Service (MS-Auth)** that handles login, token generation (JWT), and token verification.
- API Gateway (Spring Cloud Gateway or Kong API Gateway) would **intercept incoming requests** and **validate the JWT token** by calling the MS-Auth service (or by verifying token locally if public keys are available).
- Upon successful validation, required user attributes (such as `customerId`) would be **extracted from the JWT payload** and passed to downstream microservices (e.g., ms-customer, ms-transaction) via request headers.
- This allows all microservices to remain **stateless** and **focus only on business logic**, trusting the Gateway or MS-Auth to handle authentication.

✅ The JWT token would typically contain essential claims like `customerId`, `role`, and `permissions`, making customer identification secure and seamless without additional lookups.

✅ Optionally, a separate lightweight **Auth-Verifier** service could be introduced to verify tokens centrally before routing to business services.

In Kubernetes environments, tools like **Kong API Gateway** or **NGINX with JWT plugins** are often used to offload authentication and route traffic securely to microservices.

---

## 🚀 Optional Enhancements

| Feature                    | Description |
|:----------------------------|:------------|
| **Request/Response Logging** | Implement **Spring AOP** to log all incoming requests and outgoing responses for better observability. |
| **Asynchronous Communication** | Replace direct REST calls between services with asynchronous messaging using **Kafka**. |
| **Idempotency Support** | Introduce an **Idempotency-Key** header, store it in the database with a unique constraint, and prevent duplicate request processing. |
| **Distributed Tracing** | Integrate distributed tracing solutions like **OpenTelemetry** or **Zipkin** to trace and debug across microservices. |
| **Centralized Authentication** | Migrate authentication to a separate service using **OAuth2**, **Keycloak**, or **JWT-based** stateless authentication. |
| **API Documentation** | Add **Swagger/OpenAPI** auto-generation to document and test APIs easily for developers. |

---

## 🛠️ Technology Stack

- **Java 21**
- **Spring Boot 3.2.x**
- **Spring Data JPA**
- **PostgreSQL** (Dockerized)
- **Spring Cloud Gateway** (API Gateway)
- **Docker Compose** (Multi-container orchestration)
- **JUnit 5**, **Mockito** (Unit Testing)

---

## 📋 Notes

- Simple JMeter file added for concurrent request and race condition testing.

## 🐳 Running Locally

> Make sure Docker and Docker Compose are installed!

### 1. Clone the repository with submodules:
```bash
git clone --recurse-submodules https://github.com/AghayevAli/small-banking-app
```
### 2. Start all services:
```bash
docker-compose up --build
```