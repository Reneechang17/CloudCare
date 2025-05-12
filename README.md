
# CloudCare☁️🏥 Microservices Project

A fully containerized microservices-based healthcare platform with modular services for patient management, billing, analytics, and authentication. Built with Spring Boot, gRPC, Kafka, Docker, and deployed using AWS CloudFormation via LocalStack.

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture Overview](#architecture-overview)
- [Key Concepts](#key-concepts)
- [Project Structure](#project-structure)
- [How to Run Locally](#how-to-run-locally)
- [Testing](#testing)
- [Infrastructure as Code & Cloud Deployment](#infrastructure-as-code--cloud-deployment)

---

## Features

- Full-stack microservice communication using REST, gRPC, and Kafka  
- Secure JWT-based authentication and role-based access  
- Kafka-based event-driven architecture for data streaming  
- Dockerized deployment with complete cloud simulation via LocalStack  
- Infrastructure as Code using AWS CloudFormation  
- Comprehensive integration tests  

---

## Tech Stack

- **Java / Spring Boot** – Service development  
- **Spring Security / JWT** – Authentication  
- **Apache Kafka** – Asynchronous communication  
- **gRPC / Protocol Buffers** – Synchronous communication  
- **Docker / Docker Compose** – Containerization  
- **AWS CloudFormation / LocalStack** – IaC and deployment  
- **JUnit / Postman HTTP Client** – Testing  

---

## 🧱 Architecture Overview

### 📌 Local Deployment Architecture  
![Local Architecture](./assets/local-architecture.png)

### ☁️ Cloud Deployment on AWS  
![Cloud Architecture](./assets/cloud-architecture.png)

### 🔀 Internal Spring Boot Architecture  
![Spring Boot](./assets/spring-boot-arch.png)

### 🔗 gRPC Flow  
![gRPC Architecture](./assets/grpc-arch.png)

### 🔊 Kafka Streaming Overview  
![Kafka Intro](./assets/kafka-intro.png)  
![Kafka Flow](./assets/kafka-arch.png)

### 🔐 Authentication Flow  
![Auth Architecture](./assets/auth-arch.png)

### 🧵 API Gateway Routing  
![API Gateway](./assets/api-gateway-arch.png)

---

## 💡 Key Concepts

### Why Use DTOs?  
![DTOs](./assets/why-use-dtos.png)

### What Is an API Gateway?  
![API Gateway Intro](./assets/intro-api-gateway.png)

### Types of Testing  
![Testing Types](./assets/testing-overview.png)

---

## 🧱 Project Structure

```
patient-management/
├── api-gateway/            # Gateway with routing & auth filter
├── auth-service/           # Login, JWT generation, validation
├── patient-service/        # REST + gRPC + Kafka producer
├── billing-service/        # gRPC server for billing logic
├── analytics-service/      # Kafka consumer for analytics
├── infrastructure/         # CloudFormation scripts and LocalStack deploy
├── api-requests/           # IDE-based .http test files
└── docker-compose.yml      # Local docker orchestration
```

---

## 🚀 How to Run Locally

### Prerequisites

- Docker  
- Java 17+  
- AWS CLI (for LocalStack)  

### Quickstart

```bash
# Step 1: Start local infra & services
$ docker-compose up --build

# Step 2: Deploy infrastructure to LocalStack
$ cd infrastructure
$ ./localstack-deploy.sh
```

✅ Deployment confirmation:  
![Stack Deploy](./assets/test-stack-created.png)

---

## 🔐 Auth & API Testing

### 1. Login Request

```http
POST http://<GATEWAY_URL>/auth/login
Content-Type: application/json

{
  "email": "testuser@test.com",
  "password": "password123"
}
```

✅ Response:  
![JWT Token](./assets/test-jwt-login.png)

---

### 2. Get Patients (Authorized)

```http
GET http://<GATEWAY_URL>/api/patients
Authorization: Bearer <token>
```

✅ Response:  
![Get Patients](./assets/test-get-patient.png)

---

### 3. Create Patient → Triggers Kafka Event

```http
POST http://<GATEWAY_URL>/api/patients
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "John Doe Kafka Test",
  "email": "kafka_test123@example.com",
  "address": "123 main street22",
  "dateOfBirth": "1995-09-09",
  "registeredDate": "2024-11-28"
}
```

✅ Kafka test verified:  
![Kafka Test](./assets/test-kafka-result.png)

---

## 🏗️ Infrastructure as Code & Cloud Deployment

Our infrastructure is defined and deployed using AWS CloudFormation templates, written via the AWS CDK (Cloud Development Kit) in Java. To avoid unnecessary costs during development, we use **LocalStack** to simulate the AWS environment locally.

### 🧱 What’s Included in the Stack

- **VPC** – A private, isolated network for the services  
- **RDS** – PostgreSQL databases for Auth and Patient services  
- **MSK** – Managed Kafka cluster for event streaming  
- **ECS Cluster** – Hosts all containerized microservices  
- **Application Load Balancer (ALB)** – For routing traffic from frontend to the gateway  

### 🚀 Deployment Steps with LocalStack

1. Ensure Docker and LocalStack are running  
2. From the `infrastructure/` directory, run:

```bash
./localstack-deploy.sh
```

This will:

- Synthesize the CloudFormation templates from Java code  
- Deploy resources like ECS services, RDS, MSK, and ALB  
- Output an internal test domain like:

```
lb-XXXX.elb.localhost.localstack.cloud
```

✅ Successful output looks like this:  
![Stack Deploy](./assets/test-stack-created.png)

### 🌐 Visual Confirmation in LocalStack Console  
![LocalStack UI](./assets/test-stack-console.png)

---