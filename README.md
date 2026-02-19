# 🌐 AfiaPass API Gateway

The **AfiaPass API Gateway** is a high-performance Spring Boot 3 microservice that orchestrates the **AfiaPass Java SDK**. It acts as the "Interface" layer, providing a secure, RESTful entry point for third-party platforms, government dashboards, and mobile applications to interact with the Stellar Soroban network without needing to handle blockchain keys directly.

---

## ⚙️ Key Features

### 🔌 RESTful Blockchain Orchestration
* **Seamless Abstraction**: Wraps complex Soroban contract invocations, SEP-10 offline JWT generation, and SDK key management into simple REST endpoints.
* **Target Ecosystem**: Designed for immediate integration by logistics platforms (Drive-Thru Afia), government monitoring portals, and lightweight verification scanners.

### 🚀 Enterprise-Grade Scalability
* **Project Loom (Virtual Threads)**: Leverages Java 21's lightweight virtual threads to handle thousands of concurrent blockchain requests without thread exhaustion or blocking.
* **Stateless Architecture**: Fully containerized and ready for horizontal scaling across Kubernetes (EKS) clusters.

### ⚡ High-Speed Caching & Persistence
* **Redis Integration**: Caches verified permits to allow sub-millisecond response times for road officials scanning QR codes in the field, reducing RPC node loads.
* **PostgreSQL Database**: Maintains an off-chain ledger of transaction histories, API rate limits, and analytics data for government reporting.

---

## 📁 Project Structure

```text
afiapass-api-gateway/
├── README.md
├── Dockerfile
├── docker-compose.yml
├── pom.xml
├── .env.example
│
├── src/main/java/org/afiapass/gateway/
│   ├── api/                   # REST Controllers & OpenAPI Definitions
│   │   ├── PermitController.java
│   │   └── AnalyticsController.java
│   │
│   ├── service/               # SDK Wrapper & Orchestration Logic
│   │   ├── GatewayPermitService.java
│   │   └── CacheService.java
│   │
│   ├── repository/            # PostgreSQL JPA Repositories
│   │   └── PermitRecordRepository.java
│   │
│   ├── model/                 # Database Entities & REST DTOs
│   │   ├── dto/
│   │   └── entity/
│   │
│   └── config/                # SDK Beans, Security, & Redis Configs
│       ├── AfiaPassAutoConfiguration.java
│       └── SecurityConfig.java
│
└── src/main/resources/
    ├── application.yml        # Core Spring Boot properties
    └── db/migration/          # Flyway database migrations
```

🧭 How AfiaPass Gateway Works
🎫 Permit Issuance Workflow

    Client Request: A logistics vendor (e.g., Drive-Thru Afia) sends a POST /v1/permits request containing rider and route details.

    Validation: The Gateway validates the incoming JSON payload and checks the vendor's API key.

    SDK Invocation: The Gateway triggers the AfiaPass Java SDK via a Virtual Thread to execute the Soroban tax-split contract.

    Data Persistence: Upon blockchain success, the transaction hash and generated SEP-10 JWT are saved to PostgreSQL.

    Response: The final Permit payload is returned to the client to be rendered as a QR code.

🔍 Verification & Analytics Workflow

    GET /v1/verify/{id}: A road official scans a QR code. The Gateway checks Redis first for instant validation. If missing, it uses the SDK's TokenVerifier to cryptographically prove the permit offline, then caches the result.

    GET /v1/analytics: Government portals can query aggregate data directly from PostgreSQL without needing to scrape the blockchain.

⚡ Getting Started
Prerequisites

    Java 21+ (GraalVM or Temurin recommended)

    Maven 3.9+

    Docker & Docker Compose

    Stellar RPC Access (Testnet or Mainnet URL)

Installation

1. Clone Repository
   <prev>
   ```
        Bash
        
        git clone [https://github.com/TheTwoHorsemen/afiapass-api-gateway.git](https://github.com/TheTwoHorsemen/afiapass-api-gateway.git)
        cd afiapass-api-gateway
   ```
   </prev>
2. Configure Environment
Copy the example environment file and add your database credentials and Stellar configuration.
   ```text
        Bash
        
        cp .env.example .env
   ```

4. Start Infrastructure (Postgres & Redis)
Use Docker Compose to spin up the required caching and database layers locally.
    ```text
        Bash
        
        docker-compose up -d
    ```

4. Run the Application
Start the Spring Boot server using the Maven wrapper.
    ```text
        Bash
        
        ./mvnw spring-boot:run
    ```

🛡️ Security & Architecture Constraints

    Key Isolation: The Gateway never exposes the Platform's Stellar Secret Key via the API. All signing happens securely and internally within the encapsulated SDK layer.

    Rate Limiting: Built-in Redis-backed API rate limiting to prevent DDoS attacks and accidental spam from third-party vendor integrations.
