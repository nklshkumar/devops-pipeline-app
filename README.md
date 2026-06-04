# DevOps Pipeline App

A simple CI/CD project demonstrating:

- GitHub
- Jenkins
- Docker
- Node.js
- Automated Testing
- Automated Deployment

---

# Architecture

GitHub
↓
Jenkins Pipeline
↓
Install Dependencies
↓
Run Tests
↓
Build Docker Image
↓
Deploy Container
↓
Node.js Application

---

# Repository Structure

```
devops-pipeline-app/
│
├── app/
│   ├── app.js
│   ├── package.json
│   └── test.js
│
├── Dockerfile
├── Jenkinsfile
├── docker-compose.yml
└── docker-compose.jenkins.yml
```

---

# Initial Setup

## Clone Repository

```bash
git clone https://github.com/nklshkumar/devops-pipeline-app.git
cd devops-pipeline-app
```

---

# Start Jenkins

```bash
docker compose -f docker-compose.jenkins.yml up -d
```

Verify:

```bash
docker ps
```

Expected:

```text
jenkins
```

---

# Access Jenkins

Open:

```text
http://localhost:8080
```

Get initial admin password:

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

---

# Required Jenkins Plugins

Install:

- Git Plugin
- GitHub Plugin
- Docker Pipeline Plugin
- Pipeline Plugin

---

# Create Jenkins Pipeline

Create New Item:

```text
devops-pipeline
```

Select:

```text
Pipeline
```

---

# Configure SCM

Pipeline Definition:

```text
Pipeline script from SCM
```

SCM:

```text
Git
```

Repository:

```text
https://github.com/nklshkumar/devops-pipeline-app.git
```

Script Path:

```text
Jenkinsfile
```

---

# Enable Automatic Trigger

Configure Job

Build Triggers

Enable:

```text
GitHub hook trigger for GITScm polling
```

---

# Pipeline Flow

## Stage 1 - Install Dependencies

Runs:

```bash
npm install
```

Purpose:

- Read package.json
- Download dependencies

---

## Stage 2 - Run Tests

Runs:

```bash
npm test
```

Purpose:

- Validate application before deployment

---

## Stage 3 - Build Docker Image

Runs:

```bash
docker build -t devops-app .
```

Purpose:

- Build application image

---

## Stage 4 - Deploy Container

Runs:

```bash
docker rm -f devops-app || true
docker run -d -p 3000:3000 --name devops-app devops-app
```

Purpose:

- Remove old container
- Deploy new version

---

# Verify Deployment

Check container:

```bash
docker ps
```

Expected:

```text
devops-app
```

Test application:

```bash
curl localhost:3000
```

Expected Output:

```text
🚀 CI/CD Pipeline Running Successfully!
```

Health Endpoint:

```bash
curl localhost:3000/health
```

Expected:

```json
{
  "status": "UP"
}
```

---

# Useful Commands

## View Running Containers

```bash
docker ps
```

## View Images

```bash
docker images
```

## Jenkins Logs

```bash
docker logs -f jenkins
```

## Application Logs

```bash
docker logs -f devops-app
```

## Stop Application

```bash
docker stop devops-app
```

## Remove Application

```bash
docker rm -f devops-app
```

## Restart Jenkins

```bash
docker compose -f docker-compose.jenkins.yml restart
```

---

# Triggering CI/CD

Push any change:

```bash
git add .
git commit -m "test change"
git push origin main
```

Flow:

```text
Git Push
↓
GitHub Webhook
↓
Jenkins Triggered
↓
Install Dependencies
↓
Run Tests
↓
Build Image
↓
Deploy Container
```

---

# Troubleshooting

## package.json not found

Check Jenkins workspace:

```bash
docker exec -it jenkins bash
```

```bash
ls -R /var/jenkins_home/workspace
```

Verify Jenkins workspace path matches Jenkinsfile.

---

## Docker command not found

Verify Docker socket mounted:

```bash
docker inspect jenkins
```

Look for:

```text
/var/run/docker.sock
```

---

## Verify Application

```bash
docker ps
```

```bash
curl localhost:3000
```

---

# Future Improvements

- SonarQube Integration
- Docker Hub Push
- AWS ECR Push
- Kubernetes Deployment
- ArgoCD
- Prometheus
- Grafana
- Security Scanning
- Terraform Infrastructure