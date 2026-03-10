# 🚀 EShop Microservices

<div align="center">

```
███████╗███████╗██╗  ██╗ ██████╗ ██████╗
██╔════╝██╔════╝██║  ██║██╔═══██╗██╔══██╗
█████╗  ███████╗███████║██║   ██║██████╔╝
██╔══╝  ╚════██║██╔══██║██║   ██║██╔═══╝
███████╗███████║██║  ██║╚██████╔╝██║
╚══════╝╚══════╝╚═╝  ╚═╝ ╚═════╝ ╚═╝

M I C R O S E R V I C E S  A R C H I T E C T U R E
```

**Production-grade Microservices built with .NET 8, CQRS, DDD, and Cloud-Native patterns**

[![.NET](https://img.shields.io/badge/.NET-8.0-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)](https://dotnet.microsoft.com)
[![C#](https://img.shields.io/badge/C%23-12.0-239120?style=for-the-badge&logo=csharp&logoColor=white)](https://docs.microsoft.com/en-us/dotnet/csharp/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)](https://www.rabbitmq.com/)
[![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)](https://redis.io/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)

</div>

---

## 📐 Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                        CLIENT APPLICATIONS                          │
│                    ASP.NET Core Web (Razor + Bootstrap 4)           │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────────┐
│                      YARP API GATEWAY                               │
│            Rate Limiting · Routing · Reverse Proxy                  │
└──────┬───────────────────┬───────────────────┬──────────────────────┘
       │                   │                   │
       ▼                   ▼                   ▼
┌──────────────┐  ┌──────────────────┐  ┌─────────────────┐
│   CATALOG    │  │     BASKET       │  │    ORDERING     │
│  Microservice│  │   Microservice   │  │  Microservice   │
│              │  │                  │  │                 │
│ PostgreSQL   │  │  Redis Cache     │  │  SQL Server     │
│ (Marten)     │  │  + PostgreSQL    │  │  (EF Core)      │
└──────────────┘  └────────┬─────────┘  └────────┬────────┘
                           │  gRPC                │
                           ▼                      │
                  ┌─────────────────┐             │
                  │    DISCOUNT     │             │
                  │  Microservice   │             │
                  │  (gRPC Server)  │             │
                  └─────────────────┘             │
                                                  │
                  ┌───────────────────────────────┘
                  │      RabbitMQ Message Broker
                  │   (BasketCheckout → Ordering)
                  └───────────────────────────────
```

---

## 🧩 Microservices Breakdown

### 🛍️ Catalog Microservice
> *Vertical Slice Architecture · CQRS · Marten · PostgreSQL*

| Feature | Technology |
|---|---|
| API Style | ASP.NET Core 8 Minimal APIs |
| Architecture | Vertical Slice with Feature Folders |
| CQRS | MediatR + FluentValidation Pipeline |
| Database | PostgreSQL via **Marten** (Transactional Document DB) |
| Endpoint Definition | **Carter** Library |
| Validation | FluentValidation Behaviours |

- ✅ Vertical Slice Architecture with **Feature Folders**
- ✅ CQRS Validation **Pipeline Behaviours** with MediatR
- ✅ **Marten** for .NET Transactional Document DB on PostgreSQL
- ✅ **Carter** Library for clean Minimal API endpoint definition
- ✅ Latest **.NET 8 & C# 12** features

---

### 🧺 Basket Microservice
> *Redis Caching · gRPC · MassTransit · RabbitMQ*

| Feature | Technology |
|---|---|
| Cache Layer | **Redis** Distributed Cache |
| Sync Communication | **gRPC** with Discount Service |
| Async Messaging | **MassTransit** + RabbitMQ |
| Design Patterns | Proxy · Decorator · Cache-Aside |

- ✅ **Redis** as Distributed Cache over BasketDB
- ✅ **Proxy**, **Decorator** and **Cache-Aside** Design Patterns
- ✅ Highly Performant **inter-service gRPC** Communication
- ✅ Consume Discount gRPC Service to calculate final product price
- ✅ Publish **BasketCheckout** event queue via MassTransit + RabbitMQ

---

### 💸 Discount Microservice
> *gRPC Server · Clean Architecture · DDD*

- ✅ High-performance **gRPC Server** for synchronous inter-service communication
- ✅ Exposes discount calculation as a strongly-typed **Protobuf contract**

---

### 📦 Ordering Microservice
> *DDD · Clean Architecture · EF Core · RabbitMQ Consumer*

| Feature | Technology |
|---|---|
| Architecture | **DDD + Clean Architecture** |
| CQRS | MediatR · FluentValidation · **Mapster** |
| ORM | Entity Framework Core (Code-First) |
| Database | SQL Server (auto-migrate on startup) |
| Messaging | MassTransit RabbitMQ Consumer |

- ✅ **Tactical DDD** — Entities, Value Objects, Aggregates, Aggregate Roots
- ✅ CQRS with **MediatR**, **FluentValidation** and **Mapster**
- ✅ **EF Core Code-First** Approach with Migrations
- ✅ DDD Entity Configurations in Clean Architecture
- ✅ Auto-migrate to **SQL Server** on application startup
- ✅ Consume **BasketCheckout** event queue via MassTransit-RabbitMQ

---

### 🌐 API Gateway — YARP
> *Reverse Proxy · Rate Limiting · Gateway Routing Pattern*

```yaml
Gateway Features:
  ├── Yarp Reverse Proxy
  ├── Route / Cluster / Path / Transform / Destinations config
  ├── Rate Limiting: FixedWindowLimiter
  └── Gateway Routing Pattern
```

- ✅ **YARP Reverse Proxy** applying Gateway Routing Pattern
- ✅ Full YARP configuration: Routes, Clusters, Paths, Transforms
- ✅ **Rate Limiting** with `FixedWindowLimiter`

---

### 🖥️ Web Client
> *ASP.NET Core Razor · Refit · Bootstrap 4*

- ✅ ASP.NET Core Web Application with **Bootstrap 4** and Razor Templates
- ✅ Consume YARP API Gateway using **Refit** Library
- ✅ Generated **HttpClientFactory** via Refit

---

## 🔄 Async Communication Flow

```
Basket Service                   RabbitMQ                 Ordering Service
     │                              │                            │
     │──── Publish ──────────────►  │                            │
     │   BasketCheckout Event       │                            │
     │   (MassTransit)              │ ◄──── Subscribe ───────────│
     │                              │    (MassTransit Consumer)  │
     │                              │                            │
     │                              │ ──── Deliver Event ───────►│
     │                              │                            │
     │                              │                  Create Order in DB
```

---

## 🛠️ Technology Stack

| Category | Technologies |
|---|---|
| **Runtime** | .NET 8 · C# 12 |
| **API** | ASP.NET Core 8 · Minimal APIs · Carter |
| **Architecture** | Vertical Slice · Clean Architecture · DDD |
| **CQRS / Mediator** | MediatR · FluentValidation · Mapster |
| **Databases** | PostgreSQL · SQL Server · Redis |
| **Document DB** | Marten (PostgreSQL) |
| **ORM** | Entity Framework Core (Code-First) |
| **Messaging** | RabbitMQ · MassTransit |
| **gRPC** | Protobuf · gRPC (inter-service) |
| **API Gateway** | YARP Reverse Proxy |
| **Caching** | Redis (Distributed Cache) |
| **Design Patterns** | Proxy · Decorator · Cache-Aside · CQRS · DDD |
| **HTTP Client** | Refit · HttpClientFactory |
| **Containerization** | Docker · Docker Compose |
| **Cross-Cutting** | Logging · Global Exception Handling · Health Checks |

---

## 🐳 Running with Docker

```bash
# Clone the repository
git clone https://github.com/shoyo2312/Ecommerce-Microservices-Dotnet.git
cd eshop-microservices

# Start all services with Docker Compose
docker-compose up -d

# Services will be available at:
# 🛍️  Catalog API        → http://localhost:6000
# 🧺  Basket API         → http://localhost:6001
# 💸  Discount gRPC      → http://localhost:7070
# 📦  Ordering API       → http://localhost:6003
# 🌐  API Gateway        → http://localhost:6004
# 🖥️  Web Client         → http://localhost:6005
# 🐇  RabbitMQ Dashboard → http://localhost:15672
```

---

## 📁 Project Structure

```
📦 EShopMicroservices
├── 📂 src
│   ├── 📂 Services
│   │   ├── 📂 Catalog
│   │   │   ├── Catalog.API          ← Vertical Slice + CQRS
│   │   ├── 📂 Basket
│   │   │   ├── Basket.API           ← Redis + gRPC + MassTransit
│   │   ├── 📂 Discount
│   │   │   ├── Discount.Grpc        ← gRPC Server
│   │   ├── 📂 Ordering
│   │   │   ├── Ordering.Domain      ← DDD Entities & Value Objects
│   │   │   ├── Ordering.Application ← CQRS Handlers
│   │   │   ├── Ordering.Infrastructure ← EF Core, Repos
│   │   │   └── Ordering.API         ← Minimal API Endpoints
│   ├── 📂 ApiGateways
│   │   └── YarpApiGateway           ← YARP Reverse Proxy
│   └── 📂 WebApps
│       └── Shopping.Web             ← Razor + Refit Client
├── 📄 docker-compose.yml
└── 📄 docker-compose.override.yml
```

---

## 🏗️ Key Design Patterns

```
┌─────────────────────────────────────────────┐
│              DESIGN PATTERNS APPLIED        │
├──────────────────┬──────────────────────────┤
│  Proxy           │ Basket → Discount gRPC   │
│  Decorator       │ Cache Behaviour Layer    │
│  Cache-Aside     │ Redis Distributed Cache  │
│  CQRS            │ Command/Query Separation │
│  Mediator        │ MediatR Pipeline         │
│  Repository      │ EF Core + Marten         │
│  Gateway Routing │ YARP Reverse Proxy       │
│  Pub/Sub         │ RabbitMQ Topic Exchange  │
└──────────────────┴──────────────────────────┘
```

---

## 📡 Cross-Cutting Concerns

- 🪵 **Structured Logging** — consistent across all microservices
- 🛡️ **Global Exception Handling** — unified error responses
- 💓 **Health Checks** — per-service readiness & liveness probes
- 🔄 **Auto DB Migration** — EF Core migrates SQL Server on startup
- 📊 **Validation Pipelines** — FluentValidation behaviours via MediatR

---

<div align="center">

**Built with ❤️ using .NET 8 Microservices Best Practices**

*Clean Architecture · Domain-Driven Design · Cloud-Native Patterns*

</div>
