# 🐳 Dockerfiles Collection

A practical collection of **reusable Dockerfiles** for different programming languages, frameworks, and application types.

This repository is designed as a **DevOps reference library** for quickly containerizing applications, practicing Docker, preparing for interviews, and understanding production-oriented containerization.

The collection includes both **simple Dockerfiles** and **multi-stage Dockerfiles**.

---

## 🚀 What's Inside?

This repository covers commonly used application stacks:

| Technology | Simple | Multi-Stage | Typical Port |
|---|:---:|:---:|---:|
| 🟢 Node.js | ✅ | ✅ | `3000` |
| 🐍 Python | ✅ | ✅ | `8000` |
| 🎸 Django | ✅ | ✅ | `8000` |
| ☕ Java / Spring Boot | ✅ | ✅ | `8080` |
| 🐹 Go | ✅ | ✅ | `8080` |
| 🔵 .NET | ✅ | ✅ | `8080` |
| ⚛️ React | ✅ | ✅ | `5173 / 80` |
| 🅰️ Angular | ✅ | ✅ | `4200 / 80` |
| ▲ Next.js | ✅ | ✅ | `3000` |
| 🐘 PHP | ✅ | ✅ | `80` |
| 💎 Ruby / Rails | ✅ | ✅ | `3000` |
| 🦀 Rust | ✅ | ✅ | `8080` |
| 🌐 Static Website | ✅ | — | `80` |

> **Note:** Ports, build commands, and runtime commands may vary depending on the application.

---

# 📁 Repository Structure

```text
dockerfiles/
│
├── README.md
│
├── nodejs/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
├── python/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
├── django/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
├── java-springboot/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
├── golang/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
├── dotnet/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
├── react/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
├── angular/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
├── nextjs/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
├── php/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
├── ruby/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
├── rust/
│   ├── Dockerfile
│   ├── Dockerfile.multistage
│   └── .dockerignore
│
└── static-nginx/
    ├── Dockerfile
    └── .dockerignore
```

---

# 🐳 What is a Dockerfile?

A Dockerfile is a text file containing instructions that Docker uses to build a container image.

A typical Dockerfile follows this flow:

```text
Application Source Code
        │
        ▼
   Base Image
        │
        ▼
   Working Directory
        │
        ▼
   Dependencies
        │
        ▼
   Application Code
        │
        ▼
      Build
        │
        ▼
   Docker Image
        │
        ▼
    Container
```

A basic Dockerfile:

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

---

# 🏗️ Simple Dockerfile

A simple Dockerfile uses one image for building and running the application.

### Example

```dockerfile
FROM golang:1.24-alpine

WORKDIR /app

COPY . .

RUN go build -o app .

EXPOSE 8080

CMD ["./app"]
```

### Advantages

- Simple and easy to understand
- Quick to create
- Good for learning
- Suitable for simple applications

### Limitations

- Can produce larger images
- Build tools remain in the final image
- May contain unnecessary files and dependencies

---

# 🏗️ Multi-Stage Dockerfile

A multi-stage Dockerfile separates the **build environment** from the **runtime environment**.

### Build Stage

Contains:

- Source code
- Compilers
- Build tools
- Development dependencies

### Runtime Stage

Contains only what is required to run the application.

```text
                Source Code
                    │
                    ▼
        ┌─────────────────────┐
        │     BUILD STAGE     │
        │                     │
        │ Compiler            │
        │ Build Tools         │
        │ Dependencies        │
        └──────────┬──────────┘
                   │
                   │ Build Artifact
                   ▼
        ┌─────────────────────┐
        │    RUNTIME STAGE    │
        │                     │
        │ Runtime Only        │
        │ Application         │
        └──────────┬──────────┘
                   │
                   ▼
             Final Image
```

### Example

```dockerfile
FROM golang:1.24-alpine AS build

WORKDIR /app

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN go build -o app .


FROM alpine:latest

WORKDIR /app

COPY --from=build /app/app .

EXPOSE 8080

CMD ["./app"]
```

### Advantages

- Smaller final images
- Better separation of build and runtime
- Fewer unnecessary packages
- Faster image transfer
- Better security posture
- Suitable for production workloads

---

# 🔍 How to Choose the Right Dockerfile

When you receive an application, first inspect the project.

Don't blindly create a Dockerfile.

### Node.js

Look for:

```text
package.json
package-lock.json
```

Use:

```text
nodejs/
```

---

### Python

Look for:

```text
requirements.txt
pyproject.toml
```

Use:

```text
python/
```

---

### Django

Look for:

```text
manage.py
requirements.txt
```

Use:

```text
django/
```

---

### Java / Spring Boot

Look for:

```text
pom.xml
```

or:

```text
build.gradle
```

Use:

```text
java-springboot/
```

---

### Go

Look for:

```text
go.mod
go.sum
```

Use:

```text
golang/
```

---

### .NET

Look for:

```text
*.csproj
*.sln
```

Use:

```text
dotnet/
```

---

### React

Look for:

```text
package.json
src/
```

and React dependencies such as:

```text
react
react-dom
```

Use:

```text
react/
```

---

# 🧠 Dockerization Workflow

A practical Dockerization process:

```text
                 Application
                      │
                      ▼
              Identify Technology
                      │
                      ▼
             Check Dependency Files
                      │
                      ▼
             Identify Build Command
                      │
                      ▼
             Identify Start Command
                      │
                      ▼
               Identify Port
                      │
                      ▼
              Choose Base Image
                      │
                      ▼
               Create Dockerfile
                      │
                      ▼
                 Build Image
                      │
                      ▼
                Run Container
                      │
                      ▼
                  Test App
                      │
                      ▼
              Optimize Image
                      │
                      ▼
              Push / Deploy
```

---

# 🔧 Common Docker Commands

### Build an Image

```bash
docker build -t myapp .
```

### Build Using Another Dockerfile

```bash
docker build -f Dockerfile.multistage -t myapp .
```

### Run a Container

```bash
docker run -d \
  --name myapp \
  -p 8080:8080 \
  myapp
```

### List Running Containers

```bash
docker ps
```

### List All Containers

```bash
docker ps -a
```

### View Logs

```bash
docker logs myapp
```

### Follow Logs

```bash
docker logs -f myapp
```

### Stop Container

```bash
docker stop myapp
```

### Start Container

```bash
docker start myapp
```

### Remove Container

```bash
docker rm myapp
```

### List Images

```bash
docker images
```

### Remove Image

```bash
docker rmi myapp
```

---

# 📦 Common Dockerfile Instructions

| Instruction | Purpose |
|---|---|
| `FROM` | Defines the base image |
| `WORKDIR` | Sets the working directory |
| `COPY` | Copies files into the image |
| `ADD` | Copies files with additional capabilities |
| `RUN` | Executes commands during image build |
| `ENV` | Defines environment variables |
| `ARG` | Defines build-time variables |
| `EXPOSE` | Documents the application port |
| `CMD` | Defines the default container command |
| `ENTRYPOINT` | Defines the main executable |
| `USER` | Specifies the container user |
| `VOLUME` | Defines a mount point |
| `HEALTHCHECK` | Defines a container health check |

---

# 🚫 .dockerignore

A `.dockerignore` file prevents unnecessary files from being sent to the Docker build context.

Example:

```text
.git
.env
Dockerfile
.dockerignore
README.md
*.log
__pycache__
node_modules
```

The contents should be customized according to the application.

---

# 🔐 Dockerfile Best Practices

## 1. Use Minimal Base Images

Prefer smaller images where appropriate:

```dockerfile
FROM node:22-alpine
```

instead of unnecessarily large images.

---

## 2. Pin Versions

Avoid relying on:

```dockerfile
FROM node:latest
```

Prefer:

```dockerfile
FROM node:22-alpine
```

This makes builds more predictable.

---

## 3. Use Multi-Stage Builds

Multi-stage builds are especially useful for compiled applications such as:

- Java
- Go
- .NET
- Rust

They are also useful for frontend applications such as:

- React
- Angular

---

## 4. Never Hardcode Secrets

❌ Avoid:

```dockerfile
ENV DATABASE_PASSWORD=mysecretpassword
```

Use environment variables or a proper secrets-management solution.

---

## 5. Use `.dockerignore`

Avoid sending unnecessary files to the Docker daemon.

---

## 6. Avoid Running as Root

For production workloads, use a non-root user whenever possible.

---

## 7. Keep Images Small

Remove unnecessary packages, build dependencies, caches, and development tools from the final image.

---

## 8. Order Dockerfile Instructions Carefully

Copy dependency files before application source code when possible.

Example:

```dockerfile
COPY package*.json ./

RUN npm ci

COPY . .
```

This improves Docker layer caching.

---

## 9. Use Exec Form for CMD

Prefer:

```dockerfile
CMD ["node", "server.js"]
```

instead of:

```dockerfile
CMD node server.js
```

---

# 🧪 Example: Dockerizing a Spring Boot Application

Suppose a project contains:

```text
pom.xml
src/
```

### 1. Build the application

```bash
mvn clean package
```

### 2. Build the Docker image

```bash
docker build -t spring-app .
```

### 3. Run the container

```bash
docker run -d \
  --name spring-app \
  -p 8080:8080 \
  spring-app
```

### 4. Check the container

```bash
docker ps
```

### 5. Check logs

```bash
docker logs spring-app
```

---

# 🔄 Docker in a DevOps Pipeline

Dockerfiles are commonly used as part of a CI/CD pipeline:

```text
Developer
    │
    ▼
Git Push
    │
    ▼
CI Pipeline
    │
    ├── Test
    ├── Security Scan
    ├── Docker Build
    ├── Image Scan
    └── Push Image
             │
             ▼
       Container Registry
             │
             ▼
       Deployment
             │
             ▼
       Kubernetes / Cloud
```

Typical tools that can be integrated with these Dockerfiles:

```text
GitHub Actions
Jenkins
GitLab CI/CD
AWS ECR
Azure Container Registry
Docker Hub
Kubernetes
Terraform
Trivy
```

---

# 🎯 Production Dockerization Checklist

Before using an image in production:

- [ ] Choose an appropriate base image
- [ ] Pin important image versions
- [ ] Add `.dockerignore`
- [ ] Use multi-stage builds where appropriate
- [ ] Keep the final image small
- [ ] Remove unnecessary dependencies
- [ ] Don't hardcode secrets
- [ ] Use a non-root user when possible
- [ ] Define the correct application port
- [ ] Use a proper `CMD` or `ENTRYPOINT`
- [ ] Test the container locally
- [ ] Check application logs
- [ ] Add health checks where appropriate
- [ ] Scan the image for vulnerabilities
- [ ] Tag images properly
- [ ] Push the image to a container registry

---

# 🧩 Example Image Tagging

Instead of using only:

```bash
docker build -t myapp .
```

use meaningful tags:

```bash
docker build -t myapp:v1.0.0 .
```

or:

```bash
docker build -t myapp:latest .
```

For CI/CD, a commit SHA or release version is often preferable:

```text
myapp:1.0.0
myapp:1.1.0
myapp:<commit-sha>
```

---

# 🎓 Learning Objectives

This repository is designed to help practice:

- 🐳 Docker fundamentals
- 📦 Containerization
- 🏗️ Multi-stage builds
- 🔧 Dockerfile optimization
- 🔐 Container security
- 📉 Image size optimization
- 🚀 Application deployment
- 🔄 CI/CD integration
- ☸️ Kubernetes readiness
- ☁️ Cloud container deployments

The goal is **not to memorize Dockerfiles**.

The goal is to understand how to:

```text
Inspect Application
        ↓
Identify Technology
        ↓
Understand Dependencies
        ↓
Understand Build Process
        ↓
Understand Runtime
        ↓
Choose Base Image
        ↓
Write Dockerfile
        ↓
Build Image
        ↓
Run Container
        ↓
Test
        ↓
Optimize
        ↓
Deploy
```

---

# 🚀 Future Improvements

Planned additions:

- [ ] Docker Compose examples
- [ ] Production Nginx configurations
- [ ] Docker networking examples
- [ ] Docker volumes examples
- [ ] Docker health checks
- [ ] Non-root container examples
- [ ] Docker security examples
- [ ] Image optimization examples
- [ ] Docker Hub publishing examples
- [ ] GitHub Actions CI/CD examples
- [ ] Jenkins CI/CD examples
- [ ] Trivy security scanning
- [ ] Kubernetes deployment manifests
- [ ] Kubernetes Services
- [ ] ConfigMaps and Secrets
- [ ] Helm charts
- [ ] AWS ECR deployment
- [ ] Azure Container Registry deployment

---

# 🛠️ Technologies

```text
Docker
Dockerfiles
Linux
Nginx
Node.js
Python
Django
Java
Spring Boot
Go
.NET
React
Angular
Next.js
PHP
Ruby
Rust
Git
GitHub
CI/CD
Kubernetes
AWS
Azure
Terraform
```

---

# 👨‍💻 Author

## Himanshu Gohil

**DevOps & Cloud Enthusiast**

Interested in:

```text
Linux
Docker
Kubernetes
Jenkins
GitHub Actions
AWS
Azure
Terraform
CI/CD
Cloud Infrastructure
DevOps Automation
```

---

## ⭐ Support

If you find this repository useful for learning Docker and DevOps, consider giving it a ⭐ on GitHub.

**Happy Containerizing! 🐳🚀**