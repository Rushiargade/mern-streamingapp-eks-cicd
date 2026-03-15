## DevOps Implementation

This project demonstrates the end-to-end DevOps implementation of a microservices-based MERN application using containerization, CI automation, and Kubernetes orchestration.

The implementation currently covers the following stages:

* Source code management
* Containerization
* Continuous Integration
* Kubernetes orchestration

---

# 1. Source Code Management (GitHub)

The original project repository was forked and maintained in a personal GitHub repository for development and CI integration.

Repository:

```
https://github.com/Rushiargade/mern-streamingapp-eks-cicd
```

The repository structure organizes backend microservices, frontend application code, CI configuration, and Kubernetes manifests.

Project structure:

```id="repo_structure"
mern-streamingapp-eks-cicd
│
├── backend
│   ├── adminService
│   ├── authService
│   ├── chatService
│   └── streamingService
│
├── frontend
│
├── k8s
│   ├── mongo.yaml
│   ├── auth.yaml
│   ├── streaming.yaml
│   ├── admin.yaml
│   ├── chat.yaml
│   └── frontend.yaml
│
├── Jenkinsfile
└── README.md
```

Each backend service operates independently and communicates through REST APIs.

---

# 2. Containerization Using Docker

Each backend microservice and the frontend application were containerized using Docker.

The backend services were built using Node.js base images and expose their respective application ports.

Example backend build:

```id="backend_build"
cd backend

docker build -t auth-service -f authService/Dockerfile .
docker build -t streaming-service -f streamingService/Dockerfile .
docker build -t admin-service -f adminService/Dockerfile .
docker build -t chat-service -f chatService/Dockerfile .
```

The React frontend application was also containerized and served using an Nginx container.

Frontend build:

```id="frontend_build"
cd frontend
docker build -t frontend-service .
```

After building the containers, the images were verified using:

```id="verify_images"
docker images
```

This ensured all application services were available as Docker images.

---

# 3. Local Orchestration Using Docker Compose

Before moving to Kubernetes, the application was tested locally using Docker Compose to verify service communication and networking.

Docker Compose was used to run:

* MongoDB container
* Backend microservices
* Frontend application

Start services:

```id="compose_start"
docker-compose up -d
```

Verify running containers:

```id="compose_check"
docker ps
```

The frontend application was accessible locally, confirming that all services were functioning correctly.

---

# 4. Continuous Integration Using Jenkins

A Jenkins CI pipeline was implemented to automate Docker image builds whenever code changes are pushed to the repository.

Jenkins was deployed using Docker:

```id="jenkins_start"
docker run -d -p 8080:8080 -p 50000:50000 \
--name jenkins \
-v jenkins_home:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock \
jenkins/jenkins:lts
```

Docker CLI was installed inside the Jenkins container to allow Jenkins pipelines to build Docker images.

---

## Jenkins Pipeline

A Jenkins pipeline was created using a `Jenkinsfile` stored in the repository.

The pipeline performs the following stages:

1. Checkout source code from GitHub
2. Build Docker images for backend services
3. Build Docker image for frontend application
4. Verify Docker images

Example Jenkins pipeline stages:

```id="jenkins_pipeline"
pipeline {
    agent any

    stages {

        stage('Build Backend Images') {
            steps {
                sh '''
                cd backend
                docker build -t auth-service -f authService/Dockerfile .
                docker build -t streaming-service -f streamingService/Dockerfile .
                docker build -t admin-service -f adminService/Dockerfile .
                docker build -t chat-service -f chatService/Dockerfile .
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh '''
                cd frontend
                docker build -t frontend-service .
                '''
            }
        }

        stage('Verify Images') {
            steps {
                sh 'docker images'
            }
        }
    }
}
```

This pipeline ensures that container images are automatically rebuilt whenever code changes occur.

---

# 5. Kubernetes Orchestration (Minikube)

After verifying the application using Docker Compose, the deployment was migrated to Kubernetes to demonstrate container orchestration.

A local Kubernetes cluster was created using Minikube.

Start cluster:

```id="minikube_start"
minikube start
```

Verify cluster:

```id="cluster_check"
kubectl get nodes
```

---

# 6. Building Images for Minikube

To allow Kubernetes to use locally built images, Docker was switched to the Minikube Docker environment.

```id="minikube_docker"
minikube -p minikube docker-env
```

Then the application images were rebuilt so they are available inside the Kubernetes cluster.

---

# 7. Kubernetes Deployment

Application services were deployed using Kubernetes manifests located in the `k8s` directory.

Deploy all services:

```id="deploy_k8s"
kubectl apply -f k8s/
```

Verify pods:

```id="pods_check"
kubectl get pods
```

Example running pods:

```
mongo
auth-service
streaming-service
admin-service
chat-service
frontend
```

Each microservice runs in its own Kubernetes pod.

---

# 8. Service Communication

Kubernetes services were used to enable communication between microservices.

Example MongoDB connection string used by backend services:

```
mongodb://mongo:27017/streamingapp
```

Kubernetes automatically resolves service names through internal DNS.

---

# 9. Exposing the Frontend Application

The frontend service was exposed using a NodePort service.

Access the application:

```id="frontend_access"
minikube service frontend
```

This command opens the application in a browser using the Minikube cluster IP and NodePort.

---

# 10. Kubernetes Architecture

The current deployment architecture is shown below:

```
Developer
   │
   ▼
GitHub Repository
   │
   ▼
Jenkins CI Pipeline
   │
   ▼
Docker Image Build
   │
   ▼
Kubernetes Cluster (Minikube)
   │
   ├── MongoDB Pod
   ├── Auth Service Pod
   ├── Streaming Service Pod
   ├── Admin Service Pod
   ├── Chat Service Pod
   └── Frontend Pod
```

This architecture enables containerized microservices to run in a scalable Kubernetes environment.

