In this section, I'll walk you through the process of deploying the 2048 game application, which was built using React and TypeScript. This guide covers the tools and steps used to efficiently deploy and manage the application through a DevOps pipeline, ensuring robust automation, security scanning, and easy access via a browser interface.

### Tools and Technologies Used:
1. **AWS EC2** – A cloud-based server used to host and run the application and services.
2. **Jenkins** – A widely-used automation server installed on the EC2 instance to manage Continuous Integration and Continuous Deployment (CI/CD) pipelines.
3. **Docker** – Used to containerize the application and its associated services for seamless deployment and scaling.
4. **SonarQube** – A containerized service running in Docker for static code analysis, ensuring code quality and identifying potential issues early.
5. **Trivy** – A vulnerability scanner for Docker containers, helping identify and mitigate security risks.
6. **Minikube and Lens** – Local Kubernetes cluster management with Lens used to port-forward the application to a browser for testing and viewing.

### Step-by-Step Process:

#### 1. **Setting Up the EC2 Instance**  
The first step involved launching an AWS EC2 T2 Medium instance. After the instance was up and running, I performed a system update to ensure all packages and dependencies were up-to-date.

#### 2. **Installing Jenkins and CI/CD Tools**  
Next, I installed **Jenkins** on the EC2 instance to handle the CI/CD pipeline. Along with Jenkins, I set up additional tools like **SonarQube Scanner**, **OWASP Dependency-Check**, **Eclipse Temurin** (for Java support), and **Docker**. These tools collectively form the backbone of the pipeline, automating code quality checks, security scans, and the build/deployment process.

#### 3. **Running SonarQube in Docker**  
Rather than setting up a separate server for **SonarQube**, I opted to run it as a container within Docker on the same EC2 instance. This approach minimized the need for additional infrastructure. I configured Jenkins to connect with SonarQube by generating a token in SonarQube, adding this token to Jenkins credentials, and setting up a webhook in SonarQube to trigger Jenkins jobs based on the results of quality gate checks.

#### 4. **Scanning for Vulnerabilities with Trivy**  
To enhance security, I installed **Trivy**, a vulnerability scanner, on the EC2 instance. Trivy scans Docker images for known security issues, allowing me to address vulnerabilities before deploying the application to production.

#### 5. **Building the Application Pipeline**  
Once the necessary tools were in place, I configured the Jenkins pipeline. The pipeline's purpose was to automate the build process of the React and TypeScript application. When triggered, Jenkins would create a Docker image of the application, run it as a container, and expose it on **port 3000** for access via a web browser.

#### 6. **Testing the Application in the Browser**  
After the pipeline successfully built the Docker image, I was able to easily check the application by accessing it through the browser at the specified port. This provided a quick way to validate the application's functionality before moving on to deployment.

#### 7. **Deploying the Application to Kubernetes**  
Finally, I created a **Kubernetes deployment YAML** file to deploy the application as a containerized service. To view the application in the browser, I used **Lens**—a Kubernetes management tool that allowed me to port-forward the application from the cluster to my local machine. This way, I could access the deployed application directly in a browser for testing and verification.

### Summary  
This deployment pipeline integrates various tools to automate and secure the entire process, from code quality checks with SonarQube, vulnerability scanning with Trivy, to building and deploying the application with Jenkins and Kubernetes. The use of Docker containers ensures that the application runs consistently across different environments, while Kubernetes offers scalability and flexibility for production-ready deployments.

This setup provides a streamlined and repeatable process for deploying not just this game, but any containerized application, ensuring high code quality, security, and easy access during development and production phases.
