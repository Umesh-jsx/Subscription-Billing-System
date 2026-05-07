# Subscription Billing System
### Enterprise Java Spring Boot Application

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Tech Stack](#tech-stack)
3. [Project Structure](#project-structure)
4. [Prerequisites](#prerequisites)
5. [Database Setup](#database-setup)
6. [How to Run in Eclipse](#how-to-run-in-eclipse)
7. [API Endpoints](#api-endpoints)
8. [Swagger Documentation](#swagger-documentation)
9. [Sample Request Bodies](#sample-request-bodies)
10. [Default Seed Data](#default-seed-data)
11. [Architecture Overview](#architecture-overview)

---

## Project Overview

A production-ready **Subscription Billing System** built with Java Spring Boot following enterprise-level
layered architecture. Supports user management, subscription plans, billing/invoicing, audit logging,
and a reporting dashboard — all exposed via documented REST APIs.

---

## Tech Stack

| Layer          | Technology                        |
|----------------|-----------------------------------|
| Language        | Java 17                          |
| Framework       | Spring Boot 3.2.0                |
| Persistence     | Spring Data JPA / Hibernate      |
| Database        | MySQL 8.x (H2 for tests)         |
| Security        | Spring Security 6                |
| Documentation   | SpringDoc OpenAPI 2 (Swagger UI) |
| Build Tool      | Maven                            |
| Utilities       | Lombok, MapStruct                |
| Testing         | JUnit 5, Mockito                 |

---

## Project Structure

```
subscription-billing-system/
├── src/
│   ├── main/
│   │   ├── java/com/billing/subscription/
│   │   │   ├── SubscriptionBillingApplication.java   ← Main class
│   │   │   ├── config/
│   │   │   │   ├── SecurityConfig.java               ← Spring Security
│   │   │   │   ├── SwaggerConfig.java                ← OpenAPI config
│   │   │   │   ├── JpaAuditConfig.java               ← JPA auditing
│   │   │   │   └── DataInitializer.java              ← Sample seed data
│   │   │   ├── controller/
│   │   │   │   ├── UserController.java
│   │   │   │   ├── PlanController.java
│   │   │   │   ├── SubscriptionController.java
│   │   │   │   ├── InvoiceController.java
│   │   │   │   ├── DashboardController.java
│   │   │   │   └── AuditLogController.java
│   │   │   ├── service/                              ← Interfaces
│   │   │   │   ├── UserService.java
│   │   │   │   ├── PlanService.java
│   │   │   │   ├── SubscriptionService.java
│   │   │   │   ├── InvoiceService.java
│   │   │   │   └── DashboardService.java
│   │   │   ├── serviceImpl/                          ← Implementations
│   │   │   │   ├── UserServiceImpl.java
│   │   │   │   ├── PlanServiceImpl.java
│   │   │   │   ├── SubscriptionServiceImpl.java
│   │   │   │   ├── InvoiceServiceImpl.java
│   │   │   │   └── DashboardServiceImpl.java
│   │   │   ├── repository/
│   │   │   │   ├── UserRepository.java
│   │   │   │   ├── PlanRepository.java
│   │   │   │   ├── SubscriptionRepository.java
│   │   │   │   ├── InvoiceRepository.java
│   │   │   │   └── AuditLogRepository.java
│   │   │   ├── entity/
│   │   │   │   ├── BaseEntity.java                   ← Audit base
│   │   │   │   ├── User.java
│   │   │   │   ├── Plan.java
│   │   │   │   ├── Subscription.java
│   │   │   │   ├── Invoice.java
│   │   │   │   └── AuditLog.java
│   │   │   ├── dto/
│   │   │   │   ├── ApiResponse.java                  ← Generic wrapper
│   │   │   │   ├── UserDTO.java
│   │   │   │   ├── PlanDTO.java
│   │   │   │   ├── SubscriptionDTO.java
│   │   │   │   ├── InvoiceDTO.java
│   │   │   │   └── DashboardDTO.java
│   │   │   ├── enums/
│   │   │   │   ├── UserRole.java
│   │   │   │   ├── PlanType.java
│   │   │   │   ├── SubscriptionStatus.java
│   │   │   │   └── BillingStatus.java
│   │   │   ├── exception/
│   │   │   │   ├── GlobalExceptionHandler.java
│   │   │   │   ├── ResourceNotFoundException.java
│   │   │   │   ├── DuplicateResourceException.java
│   │   │   │   └── BusinessException.java
│   │   │   └── audit/
│   │   │       └── AuditService.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       ├── java/com/billing/subscription/
│       │   ├── SubscriptionBillingApplicationTests.java
│       │   └── serviceImpl/
│       │       ├── UserServiceImplTest.java
│       │       └── PlanServiceImplTest.java
│       └── resources/
│           └── application-test.properties           ← H2 config for tests
└── pom.xml
```

---

## Prerequisites

Ensure the following are installed before running the project:

| Requirement      | Version     | Download                                 |
|------------------|-------------|------------------------------------------|
| Java (JDK)       | 17 or above | https://adoptium.net                     |
| Maven            | 3.8+        | https://maven.apache.org/download.cgi    |
| MySQL            | 8.0+        | https://dev.mysql.com/downloads/         |
| Eclipse IDE      | 2023-06+    | https://www.eclipse.org/downloads/       |

---

## Database Setup

### Step 1 – Start MySQL and create the database

```sql
-- Login to MySQL
mysql -u root -p

-- Create database (or it auto-creates on first run)
CREATE DATABASE IF NOT EXISTS subscription_billing;

-- Verify
SHOW DATABASES;
```

### Step 2 – Update credentials in `application.properties`

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/subscription_billing?createDatabaseIfNotExist=true&useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
spring.datasource.username=root
spring.datasource.password=your_password_here
```

> Tables are auto-created by Hibernate (`ddl-auto=update`). No SQL scripts required.

---

## How to Run in Eclipse

### Option A — Import as Maven Project (Recommended)

1. Open **Eclipse IDE**
2. Go to **File → Import → Maven → Existing Maven Projects**
3. Browse to the `subscription-billing-system` folder
4. Click **Finish** — Eclipse will auto-download all Maven dependencies
5. Wait for the build to complete (watch the Progress bar at the bottom)
6. Right-click on `SubscriptionBillingApplication.java`
7. Select **Run As → Java Application**
8. Watch the console — you should see:

```
Started SubscriptionBillingApplication in X.XXX seconds
```

### Option B — Run via Maven in Terminal

```bash
cd subscription-billing-system
mvn clean install -DskipTests
mvn spring-boot:run
```

### Option C — Build JAR and Run

```bash
mvn clean package -DskipTests
java -jar target/subscription-billing-system-1.0.0.jar
```

---

## Swagger Documentation

Once the application is running, open your browser:

```
http://localhost:8080/swagger-ui.html
```

All API endpoints are documented with request/response models and can be tested directly from the browser.

---

## API Endpoints

### Users — `/api/v1/users`

| Method | URL                         | Description              |
|--------|-----------------------------|--------------------------|
| POST   | `/api/v1/users`             | Create new user          |
| GET    | `/api/v1/users`             | Get all users (paginated)|
| GET    | `/api/v1/users/{id}`        | Get user by ID           |
| GET    | `/api/v1/users/email/{email}` | Get user by email      |
| GET    | `/api/v1/users/search?search=` | Search users          |
| GET    | `/api/v1/users/role/{role}` | Filter users by role     |
| PUT    | `/api/v1/users/{id}`        | Update user              |
| DELETE | `/api/v1/users/{id}`        | Delete user              |
| PATCH  | `/api/v1/users/{id}/activate`   | Activate user        |
| PATCH  | `/api/v1/users/{id}/deactivate` | Deactivate user      |

### Plans — `/api/v1/plans`

| Method | URL                                  | Description               |
|--------|--------------------------------------|---------------------------|
| POST   | `/api/v1/plans`                      | Create plan               |
| GET    | `/api/v1/plans`                      | Get all plans (paginated) |
| GET    | `/api/v1/plans/{id}`                 | Get plan by ID            |
| GET    | `/api/v1/plans/active`               | Get active plans          |
| GET    | `/api/v1/plans/search?search=`       | Search plans              |
| GET    | `/api/v1/plans/type/{planType}`      | Filter by plan type       |
| GET    | `/api/v1/plans/price-range?minPrice=&maxPrice=` | Filter by price |
| PUT    | `/api/v1/plans/{id}`                 | Update plan               |
| DELETE | `/api/v1/plans/{id}`                 | Delete plan               |
| PATCH  | `/api/v1/plans/{id}/activate`        | Activate plan             |
| PATCH  | `/api/v1/plans/{id}/deactivate`      | Deactivate plan           |

### Subscriptions — `/api/v1/subscriptions`

| Method | URL                                         | Description              |
|--------|---------------------------------------------|--------------------------|
| POST   | `/api/v1/subscriptions`                     | Create subscription      |
| GET    | `/api/v1/subscriptions`                     | Get all (paginated)      |
| GET    | `/api/v1/subscriptions/{id}`                | Get by ID                |
| GET    | `/api/v1/subscriptions/user/{userId}`       | Get by user              |
| GET    | `/api/v1/subscriptions/plan/{planId}`       | Get by plan              |
| GET    | `/api/v1/subscriptions/status/{status}`     | Filter by status         |
| GET    | `/api/v1/subscriptions/search`              | Search subscriptions     |
| PUT    | `/api/v1/subscriptions/{id}`                | Update subscription      |
| PATCH  | `/api/v1/subscriptions/{id}/cancel`         | Cancel subscription      |
| PATCH  | `/api/v1/subscriptions/{id}/activate`       | Activate subscription    |
| PATCH  | `/api/v1/subscriptions/{id}/renew`          | Renew subscription       |

### Invoices — `/api/v1/invoices`

| Method | URL                                          | Description              |
|--------|----------------------------------------------|--------------------------|
| POST   | `/api/v1/invoices`                           | Create invoice           |
| GET    | `/api/v1/invoices`                           | Get all (paginated)      |
| GET    | `/api/v1/invoices/{id}`                      | Get by ID                |
| GET    | `/api/v1/invoices/number/{invoiceNumber}`    | Get by invoice number    |
| GET    | `/api/v1/invoices/subscription/{id}`         | Get by subscription      |
| GET    | `/api/v1/invoices/status/{status}`           | Filter by status         |
| GET    | `/api/v1/invoices/search`                    | Search invoices          |
| PUT    | `/api/v1/invoices/{id}`                      | Update invoice           |
| PATCH  | `/api/v1/invoices/{id}/pay`                  | Mark as paid             |
| PATCH  | `/api/v1/invoices/{id}/overdue`              | Mark as overdue          |
| POST   | `/api/v1/invoices/process-overdue`           | Auto-process overdue     |

### Dashboard — `/api/v1/dashboard`

| Method | URL                       | Description              |
|--------|---------------------------|--------------------------|
| GET    | `/api/v1/dashboard/stats` | Get full dashboard stats |

### Audit Logs — `/api/v1/audit-logs`

| Method | URL                                          | Description                  |
|--------|----------------------------------------------|------------------------------|
| GET    | `/api/v1/audit-logs`                         | Get all audit logs           |
| GET    | `/api/v1/audit-logs/entity/{entityName}`     | Logs by entity type          |
| GET    | `/api/v1/audit-logs/entity/{name}/{id}`      | Logs by entity name + ID     |

---

## Sample Request Bodies

### Create User
```json
POST /api/v1/users
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "password": "securePass123",
  "phone": "9876543210",
  "role": "ROLE_USER"
}
```

### Create Plan
```json
POST /api/v1/plans
{
  "name": "Starter Monthly",
  "description": "Perfect for individuals",
  "price": 9.99,
  "planType": "MONTHLY",
  "durationInDays": 30,
  "maxUsers": 1,
  "features": "Email Support, 5GB Storage",
  "active": true
}
```

### Create Subscription
```json
POST /api/v1/subscriptions
{
  "userId": 1,
  "planId": 1,
  "startDate": "2026-05-07",
  "autoRenew": true,
  "notes": "First subscription"
}
```

### Create Invoice
```json
POST /api/v1/invoices
{
  "subscriptionId": 1,
  "amount": 9.99,
  "taxAmount": 1.80,
  "dueDate": "2026-06-07",
  "notes": "Monthly billing cycle"
}
```

### Pagination & Sorting (query params)
```
GET /api/v1/users?page=0&size=10&sort=id,asc
GET /api/v1/subscriptions?page=0&size=5&sort=createdAt,desc
GET /api/v1/invoices/search?status=PENDING&page=0&size=10
```

---

## Default Seed Data

On first startup, the application seeds sample data automatically:

### Users
| Name          | Email                    | Password    | Role           |
|---------------|--------------------------|-------------|----------------|
| Admin User    | admin@billing.com        | admin123    | ROLE_ADMIN     |
| Manager One   | manager@billing.com      | manager123  | ROLE_MANAGER   |
| John Doe      | john.doe@example.com     | user123     | ROLE_USER      |
| Jane Smith    | jane.smith@example.com   | user123     | ROLE_USER      |

### Plans
| Name             | Type     | Price    | Duration |
|------------------|----------|----------|----------|
| Basic Monthly    | MONTHLY  | $9.99    | 30 days  |
| Pro Quarterly    | QUARTERLY| $49.99   | 90 days  |
| Business Annual  | ANNUAL   | $299.99  | 365 days |
| Enterprise Annual| ANNUAL   | $999.99  | 365 days |

---

## Architecture Overview

```
HTTP Request
     │
     ▼
┌─────────────────────┐
│   Controller Layer   │  ← @RestController — handles HTTP, delegates to service
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│    Service Layer     │  ← @Service interface + @Transactional implementation
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Repository Layer    │  ← Spring Data JPA repositories
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   Database (MySQL)   │
└─────────────────────┘

Cross-cutting concerns:
  DTO Layer       → Request/Response objects, validation
  Entity Layer    → JPA-mapped domain objects with BaseEntity audit fields
  Exception Layer → GlobalExceptionHandler with @RestControllerAdvice
  Audit Layer     → AuditService logs all CREATE/UPDATE/DELETE operations
  Config Layer    → Security, Swagger, JPA Auditing, Data Seeding
```

---

## Common Issues & Fixes

| Problem | Solution |
|--------|----------|
| `Access denied for user 'root'@'localhost'` | Check MySQL username/password in `application.properties` |
| `Communications link failure` | Ensure MySQL service is running: `sudo service mysql start` |
| `Port 8080 already in use` | Change: `server.port=8081` in `application.properties` |
| `Unknown database 'subscription_billing'` | Add `createDatabaseIfNotExist=true` to JDBC URL (already included) |
| Lombok not working in Eclipse | Install Lombok: run `java -jar lombok.jar` and point to Eclipse installation |
| Build errors after import | Right-click project → Maven → Update Project → check Force Update |

---

## Running Tests

```bash
# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=UserServiceImplTest

# Skip tests during build
mvn clean install -DskipTests
```

Tests use an **H2 in-memory database** — no MySQL required for testing.

---

*Subscription Billing System v1.0.0 — Built with Spring Boot 3.2.0 & Java 17*
