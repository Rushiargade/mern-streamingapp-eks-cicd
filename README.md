
# MERN Streaming Application – DevOps Orchestration Project

## Project Overview

This project demonstrates a **DevOps workflow for deploying a MERN microservices application** using containerization, CI/CD, and Kubernetes orchestration.

The system consists of multiple backend microservices, a React frontend, and a MongoDB database. Each component runs inside its own Docker container and is orchestrated locally using Docker Compose.

The project will later extend to:

* Jenkins CI/CD pipelines
* AWS ECR image registry
* Kubernetes deployment (EKS)
* Helm charts for application packaging
* Monitoring and logging

---

# Application Architecture

The application follows a **microservices architecture**.

| Service           | Port  | Description                           |
| ----------------- | ----- | ------------------------------------- |
| Auth Service      | 3001  | User authentication and JWT handling  |
| Streaming Service | 3002  | Video catalogue and playback APIs     |
| Admin Service     | 3003  | Admin operations and asset management |
| Chat Service      | 3004  | Real-time chat for watch parties      |
| Frontend          | 3000  | React single page application         |
| MongoDB           | 27017 | Shared database                       |

Each backend service connects to the same MongoDB instance.

---

# Technology Stack

### Backend

* Node.js
* Express.js
* MongoDB
* Mongoose

### Frontend

* React.js

### DevOps Tools

* Docker
* Docker Compose
* Git / GitHub

Future integration:

* Jenkins
* AWS ECR
* Kubernetes (EKS)
* Helm
* CloudWatch Monitoring

---

# Repository Structure

```
mern-streamingapp-eks-cicd

backend/
│
├── adminService
├── authService
├── chatService
└── streamingService

frontend/
│
├── src
├── public
└── Dockerfile

docker-compose.yml
README.md
```

---

# Step 1 – Clone the Repository

```
git clone https://github.com/Rushiargade/mern-streamingapp-eks-cicd.git
cd mern-streamingapp-eks-cicd
```

---

# Step 2 – Build Backend Docker Images

Navigate to the backend directory and build each microservice image.

```
cd backend

docker build -t admin-service -f adminService/Dockerfile .
docker build -t auth-service -f authService/Dockerfile .
docker build -t streaming-service -f streamingService/Dockerfile .
docker build -t chat-service -f chatService/Dockerfile .
```

Verify images:

```
docker images
```

---

# Step 3 – Build Frontend Image

Navigate to the frontend folder.

```
cd ../frontend
```

Build the frontend container.

```
docker build -t frontend-service .
```

---

# Step 4 – Start the Application Using Docker Compose

From the project root directory:

```
docker-compose up -d
```

This command starts:

* MongoDB container
* Auth Service
* Streaming Service
* Admin Service
* Chat Service
* Frontend container

---

# Step 5 – Verify Running Containers

```
docker ps
```

Expected running containers:

```
mongo
auth-service
streaming-service
admin-service
chat-service
frontend
```

---

# Step 6 – Access the Application

Frontend UI:

```
http://localhost:3000
```

Backend APIs:

```
http://localhost:3001
http://localhost:3002
http://localhost:3003
http://localhost:3004
```

---

# Docker Networking

Docker Compose automatically creates a shared network where containers communicate using service names.

Example Mongo connection used in services:

```
mongodb://mongo:27017/streamingapp
```

---

# DevOps Implementation Progress

Completed:

* Repository fork and setup
* Microservices architecture analysis
* Dockerization of backend services
* Frontend containerization
* Docker Compose orchestration
* Local testing of all services

Pending DevOps stages:

1. Jenkins CI pipeline
2. Docker image push to AWS ECR
3. Kubernetes deployment (EKS)
4. Helm chart packaging
5. Monitoring and logging setup
6. ChatOps integration

---

# Future Architecture

Final pipeline will follow this flow:

```
Developer → GitHub → Jenkins CI → Build Docker Images
→ Push to AWS ECR → Deploy to Kubernetes (EKS)
→ Monitor using CloudWatch
```

---

# Author

Rushikesh Argade
Cloud / DevOps Engineer
