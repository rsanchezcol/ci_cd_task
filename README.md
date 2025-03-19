# CI/CD Pipeline for Python Application with GitHub Actions

### **Overview**
This repository contains the CI/CD pipeline configuration for a Python application deployed in a Docker container. The pipeline automates the following tasks:
1. Running the pipeline on every commit or pull request.
2. Building and creating a new Docker image for the Python application.
3. Performing vulnerability scanning of the Docker image using **Trivy**.
4. Running performance tests using **Apache JMeter** or any other testing tool.

The pipeline is built using **GitHub Actions**, enabling automated deployment, quality control, and testing for the Python application.

---

## **Key Features**

### 1. **Automatic Run on Each Commit**
The pipeline is triggered automatically for every commit or pull request made to the `main` branch. This ensures all changes are validated before deployment.

### 2. **Docker Image Creation**
The application is containerized using a **Dockerfile**, and the pipeline builds a new image with a unique version identifier (e.g., linked to the Git commit SHA). The newly built Docker image is pushed to a Docker registry (e.g., Docker Hub).

### 3. **Vulnerability Scanning**
Using **Trivy**, the pipeline scans the newly created Docker image for vulnerabilities. This step ensures that the Docker image is secure and free of critical security flaws.

### 4. **Performance Testing**
Performance tests are executed using **Apache JMeter**. The pipeline runs predefined JMeter test plans to evaluate the application’s response times, scalability, and overall reliability.

---

## **Project Files**

### 1. `.github/workflows/ci-cd.yml`
This file defines the GitHub Actions workflow for the CI/CD pipeline. It contains steps for:
- Code checkout
- Building the Docker image
- Vulnerability scanning using Trivy
- Running performance tests using JMeter

### 2. `Dockerfile`
The `Dockerfile` is used to containerize the Python application. It includes instructions for building the application image.

### 3. `performance-tests.jmx`
This file contains a JMeter test plan to evaluate the application's performance. It is executed as part of the pipeline.

### 4. `requirements.txt`
Contains Python dependencies required for the application (e.g., Flask). These dependencies are installed during the Docker build process.

---

## **How It Works**

### **GitHub Actions Workflow**
1. **Triggers**:
   - The pipeline is initiated automatically on a push to the `main` branch.

2. **Steps**:
   - **Checkout Code**: Uses `actions/checkout@v2` to fetch the current repository.
   - **Build Docker Image**: Builds and tags the Docker image using the SHA of the commit.
   - **Push Docker Image**: Pushes the built image to Docker Hub using GitHub Secrets for authentication.
   - **Vulnerability Scan (Trivy)**: Runs Trivy against the newly created Docker image to identify vulnerabilities.
   - **Performance Testing**: Executes JMeter test plans to measure application performance.

```yaml
# Key Structure in ci-cd.yml
jobs:
  build:
    steps:
      - Checkout Code
      - Build Docker Image
      - Push Docker Image
  scan:
    steps:
      - Trivy Vulnerability Scan
  performance:
    steps:
      - Run JMeter Performance Test
```

### **Docker Containerization**
- The Python application is packaged into a Docker container using the `Dockerfile`.
- The container runs the Python application on a specified port (`5000` by default).

### **Vulnerability Scanning**
- **Trivy** is installed during the pipeline execution and scans the Docker image for any known vulnerabilities.
- Results are logged and displayed in the workflow output.

### **Performance Testing**
- **JMeter** runs a performance test plan (`performance-tests.jmx`) against the application. The report file (`results.jtl`) is saved for review.

---

## **Configuration Details**

### **GitHub Secrets**
To automate Docker login and image pushing, the following GitHub Secrets are used:
- `DOCKER_USERNAME`: Docker Hub username.
- `DOCKER_PASSWORD`: Docker Hub password.

### **Environment Requirements**
Ensure the following tools are pre-installed:
- **Docker** on the local machine for development/debugging purposes.
- **JMeter**, for manually running performance tests.

### **Pipeline Output**
The pipeline outputs:
1. Logs for each job step (e.g., Docker build, Trivy scan, JMeter testing).
2. Vulnerability scan results.
3. Performance test results.

---

## **How to Run Locally**

To test the pipeline locally:

1. Clone this repository:
   ```bash
   git clone https://github.com/your-repo/your-project.git
   ```
### **How to Run Locally**

To test the pipeline locally:

2. Build and run the Docker container:
   ```bash
   docker build -t python-app .
   docker run -p 5000:5000 python-app
   ```
3. Run vulnerability scanning:
   ```bash
   trivy image python-app
   ```
4. Execute performance tests using JMeter:
   ```bash
   jmeter -n -t performance-tests.jmx -l results.jtl
   ```

## **Conclusion**
This CI/CD pipeline ensures an automated development-to-deployment process with integrated security and performance testing. Feel free to extend the pipeline to include additional tests or deployments to cloud services such as AWS, Kubernetes, or other environments.
