# 🌐 FlyHorizons - API Gateway

This is the **API Gateway** for **FlyHorizons**, an enterprise-grade airline booking system. Powered by **Kong Gateway**, it acts as the unified entry point for all internal microservices—handling routing, CORS policies, rate limiting, and service-level access control.

---

## 🚀 Overview

The API Gateway simplifies client interaction with the FlyHorizons microservices. It routes requests to services like **User**, **Flight**, **Airport**, **Booking**, and **Payment**, applying essential gateway-level policies such as:

- ✅ **CORS configuration**
- 📉 **Rate limiting**
- 🔒 (Optional) Authentication and authorization
- 🔁 Load balancing (if deployed with multiple replicas)

Configured using Kong’s declarative YAML syntax, the gateway enables quick and maintainable updates across services.

---

## ⚙️ Gateway Setup

FlyHorizons uses **Kong in DB-less mode**, defined through a declarative configuration file.

### Example: `kong.yaml`

```yaml
_format_version: "3.0"
_transform: true

services:
  - name: flight-service
    url: http://flight-service:8080
    routes:
      - name: flight-route
        paths: [ /flights ]
        strip_path: false
    plugins:
      - name: cors
        config:
          origins: [ "http://localhost:5173" ]
          methods: [ GET, POST, PUT, DELETE, OPTIONS ]
          headers: [ Accept, Authorization, Content-Type ]
          exposed_headers: [ Authorization ]
          credentials: true
          max_age: 3600
      - name: rate-limiting
        config:
          minute: 60
          hour: 1000
          policy: local
          fault_tolerant: true

  # Additional services: user-service, airport-service, booking-service, payment-service
  # (see full configuration in this repo)
```

---

## 🧩 Registered Services

| **Service**         | **Path Prefix**         | **Description**                             | **Rate Limit**           |
|---------------------|--------------------------|---------------------------------------------|--------------------------|
| **User Service**    | `/users`, `/login`       | Handles user registration & login           | 20 req/min, 500/hr       |
| **Flight Service**  | `/flights`               | Provides flight schedules & data            | 60 req/min, 1000/hr      |
| **Airport Service** | `/airports`              | Serves airport, terminal, and gate info     | 60 req/min, 1000/hr      |
| **Booking Service** | `/bookings`              | Manages seat booking & reservations         | 60 req/min, 1000/hr      |
| **Payment Service** | `/payments`              | Processes mock payment transactions         | 60 req/min, 1000/hr      |

> All services include **CORS support** for local development (e.g., `http://localhost:5173`).

---

## 🛡 Plugins Enabled

### ✅ CORS  
Configured for frontend development environments to allow browser access.

### 📉 Rate Limiting  
Applied per route with fault tolerance enabled. Limits are customizable per service.

## 📄 License
This project is shared for educational and portfolio purposes only. Commercial use, redistribution, or modification is not allowed without explicit written permission. All rights reserved © 2025 Beatrice Marro.

## 👤 Author
Beatrice Marro GitHub: https://github.com/beamarro
