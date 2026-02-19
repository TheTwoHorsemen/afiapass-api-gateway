# 🌐 AfiaPass API Gateway — The Interface Layer 🔗

**AfiaPass API Gateway** is a high-performance Spring Boot 3 microservice that orchestrates the **AfiaPass Java SDK**. It provides a secure, RESTful entry point for third-party platforms, government dashboards, and mobile applications to interact with the Stellar network.

By wrapping the complexity of blockchain key management, transaction signing, and Soroban contract invocation, the Gateway allows developers to issue and verify transit permits using standard JSON/REST patterns.

### 🔑 Quick Summary

| Property | Value |
| :--- | :--- |
| **Framework** | **Spring Boot 3.x (Java 21)** |
| **SDK Integration**| **AfiaPass Java SDK (Loom-Enabled)** |
| **Database** | **PostgreSQL** (Transaction Persistence) |
| **Cache** | **Redis** (High-Speed Verification) |
| **Deployment** | **Docker / Kubernetes (AWS EKS)** |

---

### 👥 Target Ecosystem

* **Logistics Platforms**: Rapid integration for delivery apps like **Drive-Thru Afia**.
* **Government Portals**: Real-time revenue monitoring and fiscal transparency dashboards.
* **Verification Apps**: Lightweight, offline-first scanners used by road officials.

---

### 🔌 Primary API Endpoints

The Gateway exposes a versioned REST API (`/v1`) with full OpenAPI/Swagger documentation.

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `POST` | `/v1/permits` | Issues a new transit permit via Soroban contract split. |
| `GET` | `/v1/verify/{id}` | Checks the on-chain status and JWT validity of a permit. |
| `GET` | `/v1/analytics` | Returns aggregated data on tax collection across routes. |
| `GET` | `/v1/health` | Service health check and Stellar RPC connectivity status. |

---

### 📁 Project Structure

The Gateway follows a standard Spring Boot hexagonal structure, keeping the SDK orchestration separate from the REST controllers.

```text
afiapass-api-gateway/
├── src/main/java/org/afiapass/gateway/
│   ├── api/                # REST Controllers & OpenApi Definitions
│   ├── service/            # SDK Wrapper & Orchestration Logic
│   ├── repository/         # PostgreSQL JPA Repositories
│   ├── model/              # Database Entities & REST DTOs
│   └── config/             # SDK Beans & Security Configurations
├── src/main/resources/
│   └── application.yml     # Environment-specific properties
├── Dockerfile              # Multi-stage build for EKS
└── docker-compose.yml      # Local Dev Environment (Postgres + Redis)

🛠️ Development Setup

Prerequisites

    Java 21+ (GraalVM recommended)

    Docker & Docker Compose

    Access to a Stellar RPC Node (Testnet or Mainnet)

1. Clone and Initialize Environment:
Bash

git clone [https://github.com/TheTwoHorsemen/afiapass-api-gateway.git](https://github.com/TheTwoHorsemen/afiapass-api-gateway.git)
cp .env.example .env

2. Start Infrastructure (Postgres & Redis):
Bash

docker-compose up -d

3. Run the Application:
Bash

./mvnw spring-boot:run

🐳 Deployment & Scalability

The Gateway is designed to be stateless, allowing for horizontal scaling across Kubernetes clusters.

    Containerization: Optimized Docker images using distroless bases for security.

    Observability: Prometheus/Grafana metrics exposed via spring-boot-starter-actuator.

    Resilience: Integrated with the Java SDK's Virtual Thread pool to handle thousands of concurrent blockchain requests without thread exhaustion.

🛡️ Security

    Key Isolation: The Gateway never exposes the Platform's Stellar Secret Key. All signing happens internally within the SDK layer.

    Rate Limiting: Redis-backed rate limiting to prevent DDoS attacks on the Stellar RPC nodes.

    JWT Validation: Built-in verification for incoming third-party requests using OAuth2/OIDC standards.

🗺️ Roadmap

    Phase 1: MVP REST wrapper for issuePermit and verifyPermit.

    Phase 2: Government Dashboard API with route-based analytics.

    Phase 3: Webhook support for real-time payment notifications.
