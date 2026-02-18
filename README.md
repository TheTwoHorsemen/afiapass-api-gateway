---

### Repo 3: `afiapass-api-gateway`
**The "Interface" — Spring Boot API for Third-Party Integration**

```markdown
# AfiaPass API Gateway 🌐 🔗

## RESTful Entry Point for the AfiaPass Infrastructure

## 🚀 Overview
This is a Spring Boot 3 service that wraps the **AfiaPass Java SDK**. It provides a clean REST API for mobile apps, web platforms, and government dashboards to interact with the Stellar network without needing to handle blockchain keys directly.

## 👥 Target Users
- **Logistics Platforms**: Integration for delivery apps like Drive-Thru Afia.
- **Government Portals**: Real-time revenue monitoring dashboards.
- **Verification Apps**: Lightweight scanners used by road officials.

## 🔌 API Endpoints
- `POST /v1/permits`: Create a new transit permit.
- `GET /v1/verify/{id}`: Check the on-chain status of a permit.
- `GET /v1/analytics`: Aggregate data on tax collection across routes.

## 🐳 Infrastructure
- **Containerization**: Docker / Kubernetes (EKS)
- **Database**: PostgreSQL (for caching and transaction history)
- **Cache**: Redis (for rapid permit verification)
