# Java Registration CI/CD Pipeline

This project demonstrates a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Java Spring Boot application.

## 🛠️ Tech Stack
*   **Language:** Java 17 (Spring Boot)
*   **Build Tool:** Maven
*   **CI/CD Server:** Jenkins
*   **Code Quality:** SonarQube
*   **Artifact Repository:** JFrog Artifactory
*   **Containerization:** Docker
*   **Security Scanning:** Trivy
*   **Orchestration:** Kubernetes (Manifests included)
*   **Cloud Provider:** AWS EC2

## 🚀 Pipeline Stages
1.  **Checkout:** Pulls code from Bitbucket/GitHub.
2.  **Build:** Compiles Java code and packages it into a JAR file using Maven.
3.  **SonarQube Analysis:** Scans code for bugs and vulnerabilities.
4.  **Quality Gate:** Ensures code meets quality standards before proceeding.
5.  **JFrog Publish:** Uploads the artifact to JFrog Artifactory.
6.  **Trivy Scan (FS):** Scans the source code for security vulnerabilities.
7.  **Docker Build & Push:** Builds a Docker image and pushes it to Docker Hub.
8.  **Trivy Scan (Image):** Scans the Docker image for vulnerabilities.
9.  **Kubernetes Deploy:** Deploys the application to a K8s cluster.
10. **Notification:** Sends an email report upon success or failure.

## 📂 Project Structure
*   `src/main/java`: Java source code.
*   `src/main/resources/templates`: HTML templates (Thymeleaf).
*   `kubernetes/`: YAML files for deployment and service.
*   `Jenkinsfile`: The pipeline definition.
*   `Dockerfile`: Instructions for building the container.

## 🏃 How to Run Locally
1.  Clone the repository.
2.  Run `mvn clean package`.
3.  Run `java -jar target/java-registration-app-1.0.0.jar`.
4.  Open browser to `http://localhost:8080/register`.
