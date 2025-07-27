# Jenkins Pipeline for Java based application using Maven, SonarQube, Argo CD, and Kubernetes.

![Screenshot 2023-03-28 at 9 38 09 PM](https://user-images.githubusercontent.com/43399466/228301952-abc02ca2-9942-4a67-8293-f76647b6f9d8.png)

✅ Summary:
In this project, CI part is handled by Jenkins, CD part is handled by Argo CD. Once the development team develops a java application, we will go head and trigger the jenkins pipeline, which will checkout the code from configured git repo. After that build and test of the application is done using Maven (For build and test purpose used a separate docker image which is build using JRE 17 and maven), Static code Analysis is done by SonarQube (configured locally on EC2 instance). Docker image is stored in personal Docker Hub registry, which will used as docker agent in pipeline. Once the static code analysis is done, application image will be built and uploaded to Docker hub. This ends our CI part.

CD part is handled by Argo CD. I have configured a minikube cluster for this demo. Installed Argo CD in the kubernetes cluster. Instead of Argo Image updater, used a shell script to update the Application manifest repo once the image is build and pushed to Dockerhub. The deployment manifest is updated with latest build nunmber. As Argo CD is a kubernetes controller, it will always try to maintain same state between application manifest and K8S cluster. 

🚀 Workflow:

- Checkout – Pulls source from Git repository.
- Build & Test – Compiles and packages the application via Maven.
- Static Code Analysis – Runs SonarQube scan to catch code smells, vulnerabilities, and enforce quality gates.
- Docker Build & Push – Dockerizes the app, builds an image tagged with the Jenkins BUILD_NUMBER, and pushes it to Docker Hub.
- Deployment Manifest Update – Updates the Kubernetes deployment YAML to use the new image tag, and pushes changes back to Git (for GitOps via Argo CD).


Below are the step-by-step details to set up an end-to-end Jenkins pipeline for a Java application using SonarQube, Argo CD, and Kubernetes:

Prerequisites:

   -  Java application code hosted on a Git repository
   -  Jenkins server
   -  Kubernetes cluster
   -  Docker Hub Registry Account
   -  Argo CD

Steps:

    1. Install the necessary Jenkins plugins:
       1.1 Git plugin
       1.2 Maven Integration plugin
       1.3 Pipeline plugin
       1.4 Kubernetes Continuous Deploy plugin
       1.5 SonarQube Code Scanner.

    2. Create a new Jenkins pipeline:
       2.1 In Jenkins, create a new pipeline job and configure it with the Git repository URL for the Java application.
       2.2 Add a Jenkinsfile to the Git repository to define the pipeline stages.

    3. Define the pipeline stages:
        Stage 1: Checkout the source code from Git.
        Stage 2: Build the Java application using Maven.
        Stage 3: Run unit tests using JUnit and Mockito.
        Stage 4: Run SonarQube analysis to check the code quality.
        Stage 5: Package the application into a JAR file.
        Stage 6: Deploy the application to a test environment using Helm.
        Stage 7: Run user acceptance tests on the deployed application.
        Stage 8: Promote the application to a production environment using Argo CD.

    4. Configure Jenkins pipeline stages:
        Stage 1: Use the Git plugin to check out the source code from the Git repository.
        Stage 2: Use the Maven Integration plugin to build the Java application.
        Stage 3: Use the JUnit and Mockito plugins to run unit tests.
        Stage 4: Use the SonarQube plugin to analyze the code quality of the Java application.
        Stage 5: Use the Maven Integration plugin to package the application into a JAR file.
        Stage 6: Use the Kubernetes Continuous Deploy plugin to deploy the application to a test environment using Helm.
        Stage 7: Use a testing framework like Selenium to run user acceptance tests on the deployed application.
        Stage 8: Use Argo CD to promote the application to a production environment.

    5. Set up Argo CD:
        Install Argo CD on the Kubernetes cluster.
        Set up a Git repository for Argo CD to track the changes in the Helm charts and Kubernetes manifests.
        Create a Helm chart for the Java application that includes the Kubernetes manifests and Helm values.
        Add the Helm chart to the Git repository that Argo CD is tracking.

    6. Configure Jenkins pipeline to integrate with Argo CD:
       6.1 Add the Argo CD API token to Jenkins credentials.
       6.2 Update the Jenkins pipeline to include the Argo CD deployment stage.

    7. Run the Jenkins pipeline:
       7.1 Trigger the Jenkins pipeline to start the CI/CD process for the Java application.
       7.2 Monitor the pipeline stages and fix any issues that arise.

This end-to-end Jenkins pipeline will automate the entire CI/CD process for a Java application, from code checkout to production deployment, using popular tools like SonarQube, Argo CD, and Kubernetes.
