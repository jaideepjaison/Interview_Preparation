# E-Commerce System Design Cheat Sheet (Interview Friendly)

## 1. Functional Requirements (What the system should do?)

Users should be able to:

* Register/Login
* Search Products
* View Product Details
* Add Products to Cart
* Place Orders
* Make Payments
* Receive Notifications
* View Order History

---

## 2. Non-Functional Requirements (How the system should work?)

### Scalability

System should handle millions of users.

### Availability

Application should work even if some servers fail.

### Performance

Product search should be fast.

### Reliability

Orders and payments should never be lost.

### Security

JWT authentication, encrypted passwords, HTTPS.

---

# 3. High-Level Design (HLD)

Think of each service as a separate team in a company.

```text
                 Client
                    |
              API Gateway
                    |
--------------------------------------------------
|          |          |         |         |
User    Product     Cart      Order    Payment
Svc      Svc        Svc       Svc       Svc
                    |
                  Kafka
                    |
            Notification Svc
```

### Database per Service

```text
User Service      -> MySQL
Product Service   -> MongoDB
Cart Service      -> Redis
Order Service     -> MySQL
Payment Service   -> MySQL
```

### Easy Memory Trick

```text
Users, Orders, Payments -> MySQL

Products -> MongoDB

Cart & Cache -> Redis

Events -> Kafka
```

---

# 4. Order Flow

Imagine buying an iPhone.

```text
1. Search Product
2. Add to Cart
3. Click Place Order
4. Inventory Check
5. Payment Success
6. Order Created
7. Notification Sent
```

Technical Flow:

```text
User
 |
Cart Service
 |
Order Service
 |
Product Service (Inventory Check)
 |
Payment Service
 |
Kafka Event
 |
Notification Service
```

---

# 5. Low-Level Design (LLD)

## Product

```java
class Product {
    Long id;
    String name;
    Double price;
    Integer quantity;
}
```

## Cart

```java
class Cart {
    Long userId;
    List<CartItem> items;
}
```

## Order

```java
class Order {
    Long orderId;
    Long userId;
    Double amount;
    String status;
}
```

## Payment

```java
class Payment {
    Long paymentId;
    Long orderId;
    String status;
}
```

---

# 6. Why Redis?

Redis is stored in RAM.

Very fast.

Example:

```text
iPhone page viewed 1 million times/day
```

Instead of querying database every time:

```text
User
 |
Redis
 |
Database
```

### Use Cases

* Product Cache
* Shopping Cart
* Session Data

---

# 7. Why Kafka?

Kafka is used when services don't need immediate responses.

Example:

Order Created

```text
Order Service
      |
    Kafka
      |
------------------------
|          |           |
Email     SMS      Audit
```

Benefits:

* Asynchronous
* Reliable
* Scalable

Easy Memory:

```text
Kafka = Event Messenger
```

---

# 8. Why Load Balancer?

One server cannot handle all users.

Without Load Balancer:

```text
Users
  |
Server-1
```

Server may crash.

With Load Balancer:

```text
           Load Balancer
                 |
--------------------------------
|              |              |
S1            S2             S3
```

Requests distributed.

Benefits:

* Better Performance
* High Availability
* Scalability

Easy Memory:

```text
Load Balancer = Traffic Police
```

---

# 9. Why Circuit Breaker?

Suppose:

```text
Order Service
      |
Payment Service
```

Payment Service goes down.

Without Circuit Breaker:

```text
Order Service keeps waiting
```

Everything becomes slow.

With Circuit Breaker:

```text
Circuit OPEN
```

No more requests sent.

Fallback response returned.

Easy Memory:

```text
Circuit Breaker = Electric Fuse
```

Too many failures -> Stop sending requests.

---

# 10. Failure Handling

## Scenario 1

Payment Service Down

Solution:

```text
Circuit Breaker
Retry
Fallback
```

---

## Scenario 2

Server Crash

Solution:

```text
Multiple Instances
Load Balancer
```

---

## Scenario 3

Database Slow

Solution:

```text
Redis Cache
```

---

## Scenario 4

Notification Service Down

Solution:

```text
Kafka stores events
```

Notification can be sent later.

---

# 11. Saga Pattern (Most Asked Interview Question)

Problem:

```text
Order Created
Payment Success
Inventory Failed
```

Result:

```text
Money Deducted
No Order
```

Bad State.

### Solution

```text
Create Order
      |
Reserve Inventory
      |
Process Payment
```

If Inventory Fails:

```text
Refund Payment
Cancel Order
```

This is called Compensation.

Easy Memory:

```text
Saga = Undo completed steps when a later step fails.
```

---

# 12. JWT Authentication

Login:

```text
Username + Password
```

Server generates:

```text
JWT Token
```

Every request:

```text
Authorization: Bearer Token
```

Benefits:

* Stateless
* Fast
* Scalable

Easy Memory:

```text
JWT = Digital ID Card
```

---

# 13. Most Common Interview Questions

### Why MongoDB for Products?

Products have different attributes.

Phone:

```json
{
  "ram": "8GB"
}
```

T-Shirt:

```json
{
  "size": "XL"
}
```

MongoDB supports flexible schema.

---

### Why MySQL for Orders?

Orders require:

* Transactions
* Consistency
* ACID Properties

---

### Why Redis?

Fast caching and cart storage.

---

### Why Kafka?

Asynchronous communication between services.

---

### How do you prevent overselling?

Example:

Only 1 iPhone left.

Two users buy simultaneously.

Use:

```text
Optimistic Locking
or
Pessimistic Locking
```

---

### What if Payment succeeds but Order fails?

Use:

```text
Saga Pattern
```

---

### How do services communicate?

```text
REST API
Kafka
```

---

# 14. 30-Second Interview Summary

An e-commerce system consists of User, Product, Cart, Order, Payment, and Notification services. Each service owns its own database. MySQL is used for orders and payments, MongoDB for products, Redis for caching and cart management, and Kafka for asynchronous events. Load Balancers distribute traffic across multiple instances, Circuit Breakers prevent cascading failures, Saga Pattern handles distributed transactions, and JWT provides stateless authentication.
