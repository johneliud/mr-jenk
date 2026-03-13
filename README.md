# mr-jenk

> **Automated CI/CD pipeline for e-commerce microservices platform**

> mr-jenk is a dedicated Jenkins-based automation suite designed to manage the full lifecycle of an e-commerce microservices platform. It handles everything from source code integration and automated testing to secure deployment and rollback strategies.

---

## Project Overview

The goal of this project is to implement a robust, scalable, and automated CI/CD pipeline that reduces manual intervention and ensures high-quality software delivery through continuous integration and deployment.

---

## Key Features

### Core Automation
* Automated builds triggered by GitHub webhooks.
* Multi-stage pipeline definitions using Jenkinsfile.
* Parallel execution for backend and frontend services.

### Testing & Quality
* JUnit integration for backend unit and integration tests.
* Jasmine/Karma integration for Angular frontend testing.
* Automated test report generation and status tracking.

### Deployment & Reliability
* Secure credential management for target environments.
* Automated rollback mechanisms for failed deployments.
* Parameterized builds for environment-specific configurations.

---

## Architecture

### Technology Stack
* **CI/CD Engine:** Jenkins
* **Containerization:** Docker
* **Version Control:** Git / GitHub
* **Build Tools:** Maven (Backend), Angular CLI (Frontend)
* **Testing:** JUnit (backend), Vitest/Jasmine (frontend)

### Jenkins Configuration (Issue 1)
The initial setup (Issue 1) established the core automation server with the following specifications:

* **Default Port:** 18080
* **Base URL:** http://localhost:18080
* **Security:** Admin credentials initialized and security realm configured.
* **Automation:** Configured to trigger automatically via GitHub Webhooks on every commit to the `main` branch.

### Agent Configuration (Issue 2)
To support distributed builds and parallel execution, two dedicated build agents have been configured:

* **Backend Agent:** Labeled as `backend`, responsible for Java/Maven microservices and integration tests.
* **Frontend Agent:** Labeled as `frontend`, responsible for Angular/Node.js builds and UI testing.
* **Remote Root Directory:** `/home/john/jenkins-agent` (configured on both nodes).

### Backend Testing (Issue 5)
Each of the four microservices has a dedicated Jenkinsfile defining a Maven-based pipeline:

* **Stages:** Initialize → Checkout → Build → Unit Test → Package
* **Test Runner:** Maven Surefire (`mvn test`), with JUnit XML results published to the Jenkins UI.
* **Artifact Archiving:** The packaged JAR is archived on successful builds for download from Jenkins.

| Service | Jenkins Job |
|---|---|
| user-service | `user-service` |
| product-service | `product-service` |
| media-service | `media-service` |
| api-gateway | `api-gateway` |

### Frontend Testing (Issue 6)
The Angular frontend has a dedicated Jenkinsfile (`Jenkinsfile.frontend`) running on the `frontend` agent:

* **Stages:** Initialize → Checkout → Install Dependencies → Test → Build
* **Test Runner:** Vitest via `ng test --watch=false`, using the `@angular/build:unit-test` builder (Angular 21).
* **Test Reports:** JUnit XML output via Vitest's built-in `--reporter=junit`; results published to the Jenkins UI via the JUnit plugin.
* **Production Build:** `ng build --configuration production` runs after tests pass.

### Orchestration
The root `Jenkinsfile` triggers all five jobs (four backend services + frontend) in parallel. A failure in any job fails the orchestrator and prevents the Deploy stage from running.

---

## Plugin Configuration

The following core plugins have been installed and configured to support the pipeline:

* **Git:** Integration with GitHub repositories for source code management.
* **Pipeline:** Support for orchestrating complex workflows via Groovy scripts.
* **JUnit:** Processing and visualization of XML test results.
* **Email Extension:** Advanced notification system for build status reporting.

---

## Setup Instructions

### Prerequisites
* Docker installed on the host machine.
* Java Development Kit (JDK) installed (if running natively).

### 1. Clone Repository
```bash
git clone https://github.com/johneliud/mr-jenk.git
cd mr-jenk
```

### 2. Initial Jenkins Setup
Jenkins is configured to run on port 18080. If running via Docker, use:
```bash
docker run -p 18080:18080 -p 50000:50000 jenkins/jenkins:lts
```

### 3. Verify Installation
Access the dashboard at `http://localhost:18080` and ensure the required plugins (Git, Pipeline, JUnit, Email Extension) are active in **Manage Jenkins > Manage Plugins**.

### 4. GitHub Webhook Configuration (Issue 4)
To enable automatic build triggers on every commit to the `main` branch:

#### On GitHub:
1. Navigate to your repository **Settings > Webhooks > Add webhook**.
2. **Payload URL:** `https://bentlee-flawed-sunshine.ngrok-free.dev/github-webhook/`
3. **Content type:** `application/json`
4. **Which events:** Select "Just the push event."
5. Click **Add webhook**.

#### On Jenkins:
1. Open your Pipeline job configuration.
2. Under **Build Triggers**, check the box for **GitHub hook trigger for GITScm polling**.
3. Save the configuration.

---

## Project Structure

```text
mr-jenk/
├── Jenkinsfile                    # Orchestrator pipeline — triggers all service jobs in parallel
├── README.md                      # Project documentation
└── .gitignore                     # Git ignore patterns
```

---

## Roadmap

* [x] Install and Configure Jenkins (Issue 1)
* [x] Configure Build Agents (Issue 2)
* [x] Create Jenkins Job with GitHub Integration (Issue 3)
* [x] Set Up Automatic Build Triggers (Issue 4)
* [x] Integrate Backend Automated Testing (Issue 5)
* [x] Integrate Frontend Automated Testing (Issue 6)
* [ ] Implement Automated Deployment Stage (Issue 7)
* [ ] Implement Rollback Strategy (Issue 8)
* [ ] Set Up Build Notifications (Issue 9)

---

## License

This project is licensed under the MIT License.

**Built for microservices automation**
