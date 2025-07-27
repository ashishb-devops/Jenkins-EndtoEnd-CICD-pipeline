# Jenkins Pipeline for Java based application using Maven, SonarQube, Argo CD, and Kubernetes.

![Screenshot 2023-03-28 at 9 38 09 PM](https://user-images.githubusercontent.com/43399466/228301952-abc02ca2-9942-4a67-8293-f76647b6f9d8.png)

## ✅ Summary:

In this project, CI part is handled by Jenkins, CD part is handled by Argo CD. Once the development team develops a java application, we will go head and trigger the jenkins pipeline, which will checkout the code from configured git repo. After that build and test of the application is done using Maven (For build and test purpose used a separate docker image which is build using JRE 17 and maven), Static code Analysis is done by SonarQube (configured locally on EC2 instance). Docker image is stored in personal Docker Hub registry, which will used as docker agent in pipeline. Once the static code analysis is done, application image will be built and uploaded to Docker hub. This ends our CI part.

CD part is handled by Argo CD. I have configured a minikube cluster for this demo. Installed Argo CD in the kubernetes cluster. Instead of Argo Image updater, used a shell script to update the Application manifest repo once the image is build and pushed to Dockerhub. The deployment manifest is updated with latest build nunmber. As Argo CD is a kubernetes controller, it will always try to maintain same state between application manifest and K8S cluster. 

## 🚀 Workflow:

- Checkout – Pulls source from Git repository.
- Build & Test – Compiles and packages the application via Maven.
- Static Code Analysis – Runs SonarQube scan to catch code smells, vulnerabilities, and enforce quality gates.
- Docker Build & Push – Dockerizes the app, builds an image tagged with the Jenkins BUILD_NUMBER, and pushes it to Docker Hub.
- Deployment Manifest Update – Updates the Kubernetes deployment YAML to use the new image tag, and pushes changes back to Git (for GitOps via Argo CD).


Below are the step-by-step details to set up an end-to-end Jenkins pipeline for a Java application using SonarQube, Argo CD, and Kubernetes:

## Prerequisites:

   -  Java application code hosted on a Git repository
   -  Jenkins server
   -  Kubernetes cluster
   -  Docker Hub Registry Account
   -  Argo CD installed on kubernetes

Steps:

    1. Install the necessary Jenkins plugins:
       1.1 Git plugin
       1.2 Maven Integration plugin
       1.3 Docker plugin
       1.4 SonarQube Code Scanner.

    2. Create a new Jenkins pipeline:
       2.1 In Jenkins, create a new pipeline job and configure it with the Git repository URL for the Java application.
       2.2 Add a Jenkinsfile to the Git repository to define the pipeline stages.

    3. Define the pipeline stages:
       - Agent: Uses a Docker image with Maven and Docker CLI pre-installed, with Docker socket mounted for host-level Docker access.
       - Build & Test: Executes mvn clean package within project folder.
       - Static Analysis: Runs Sonar Maven plugin pointed at your SonarQube server using Jenkins-stored token.
       - Docker Build & Push: Tags image as yourusername/ultimate-cicd:${BUILD_NUMBER} and pushes to registry.
       - Deployment Update: Clones the same Git repo, modifies deployment.yml to use new tag, and pushes commit to main branch for Argo CD sync.

    4. Required Configuration:
         4.1: Use the Git plugin to check out the source code from the Git repository.
         4.2: Use the Maven-Docker image as docker agent to build the Java application.
         4.3: Use the SonarQube plugin to analyze the code quality of the Java application.
         4.4: Use the Maven to package the application into a JAR file.
         4.5: Install Docker on the Jenkins host.
         4.6: Add credentials: sonarqube, docker-cred, and github.
         4.7: Set SONAR_URL in the pipeline environment to your SonarQube server endpoint
         4.8: Argo CD should track the repo path spring-boot-app-manifests. Once the image tag is updated and committed, Argo CD will automatically deploy the latest version..

    5. Set up Argo CD:
        5.1: Install Argo CD Operator on the minikube Kubernetes cluster.
        5.2: Set up a Git repository for Argo CD to track the changes in application manifests.
        5.3: Set up Argo CD yaml file.

    6. Configure Jenkins pipeline to integrate with Argo CD:
       6.1 Add the Argo CD API token to Jenkins credentials.
       6.2 Update the Jenkins pipeline to include the Argo CD deployment stage.

    7. Run the Jenkins pipeline:
       7.1 Trigger the Jenkins pipeline to start the CI/CD process for the Java application.
       7.2 Monitor the pipeline stages and fix any issues that arise.

This end-to-end Jenkins pipeline will automate the entire CI/CD process for a Java application, from code checkout to production deployment, using popular tools like SonarQube, Argo CD, and Kubernetes.
