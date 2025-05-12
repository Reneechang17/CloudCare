
# CloudCare☁️🏥 Microservices Project

A fully containerized microservices-based healthcare platform with modular services for patient management, billing, analytics, and authentication. Built with Spring Boot, gRPC, Kafka, Docker, and deployed using AWS CloudFormation via LocalStack.

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
![Local Architecture](https://github.com/Reneechang17/CloudCare/blob/main/statics/Final%20Archi.jpg)

### ☁️ Cloud Deployment on AWS  
![Cloud Architecture](https://github.com/Reneechang17/CloudCare/blob/main/statics/Final%20Archi-Cloud.jpg)

### 🔀 Internal Spring Boot Architecture  
![Spring Boot](https://github.com/Reneechang17/CloudCare/blob/main/statics/SpringBoot%20Archi.jpg)

### 🔗 gRPC Flow  
![gRPC Architecture](https://github.com/Reneechang17/CloudCare/blob/main/statics/gRPC%20Archi.jpg)

### 🔊 Kafka Streaming Overview  
![Kafka Intro](https://github.com/Reneechang17/CloudCare/blob/main/statics/Kafka%20Intro.jpg)  
![Kafka Flow](https://github.com/Reneechang17/CloudCare/blob/main/statics/Kafka%20Archi.jpg)

### 🔐 Authentication Flow  
![Auth Architecture](https://github.com/Reneechang17/CloudCare/blob/main/statics/Auth%20Archi.jpg)

### 🧵 API Gateway Routing  
![API Gateway](https://github.com/Reneechang17/CloudCare/blob/main/statics/API%20Gateway%20Archi.jpg)

---

## Project Structure

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

## How to Run Locally

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

---

## Auth & API Testing

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
![Login](https://github.com/Reneechang17/CloudCare/blob/main/statics/login%20test.jpg)

---

### 2. Get Patients (Authorized)

```http
GET http://<GATEWAY_URL>/api/patients
Authorization: Bearer <token>
```

✅ Response:  
![Get Patients](https://github.com/Reneechang17/CloudCare/blob/main/statics/get-patients.jpg)

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

✅ Verified:  
![Create Patient](https://github.com/Reneechang17/CloudCare/blob/main/statics/create-patient.jpg)

---

## Infrastructure as Code & Cloud Deployment

Our infrastructure is defined and deployed using AWS CloudFormation templates, written via the AWS CDK (Cloud Development Kit) in Java. To avoid unnecessary costs during development, we use **LocalStack** to simulate the AWS environment locally.

### What’s Included in the Stack

- **VPC** – A private, isolated network for the services  
- **RDS** – PostgreSQL databases for Auth and Patient services  
- **MSK** – Managed Kafka cluster for event streaming  
- **ECS Cluster** – Hosts all containerized microservices  
- **Application Load Balancer (ALB)** – For routing traffic from frontend to the gateway  

### Deployment Steps with LocalStack

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
![Stack Deploy](https://github.com/Reneechang17/CloudCare/blob/main/statics/localstack%20snapshot.jpg)
