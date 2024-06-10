# EC2 Instance Setup

Spin up a instance in EC2. Since a lot of tools like Jenkins, Docker, Git are installed, it's better to proceed with a large instance to run smoothly.

## Git Installation and Configuration

1. Install Git.
2. Configure Git:
    - Create SSH keygen new algorithm - ed25519 (cryptographic algorithm faster than RSA).
    - Move it to SSH agent as it will take care of auth further - no need to add SSH key each time for Git auth.
    - Add the public key to GitHub - SSH keys.
    - Check SSH -T git@github.com for auth.

## Java Installation

Install Java.

## Jenkins Installation and Configuration

1. Install Jenkins. Follow the instructions on [this page](https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/).
2. Setup Jenkins with plugins and user.
3. Create a new pipeline project - provide Git as source and also repo name and branch.
4. Configure the path for Jenkinsfile to take inside repo.

## Pipeline Configuration

Use Docker as an agent for the Jenkins pipeline. This minimizes the configuration and is efficient as the Docker container will be destroyed once its process is over, freeing up space.

## SonarQube Server Installation

SonarQube server has to be installed in the same VPC where you are hosting the app (cloud), to avoid security-related config.

```bash
apt install unzip
adduser sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start
```

Don't forget to add port 9000 allow in EC2 security group.

# Docker Installation
Install Docker in EC2 machine.

```bash
sudo yum install docker
sudo usermod -a -G docker ec2-user
id ec2-user
newgrp docker
sudo systemctl enable docker.service
sudo systemctl start docker.service
```

# Kubernetes and ArgoCD Installation
Kubernetes and ArgoCD can be had locally as EC2 is already overloaded. Install Docker Desktop.

# Continuous Delivery with ArgoCD
Add the ArgoCD operator which will create all ArgoCD architecture components (repo server, app controller, etc.).

```bash
curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.28.0/install.sh | bash -s v0.28.0
```
This will install the OLM operator - which is a Kubernetes component to manage the deployments of operators into K8 cluster and manages its lifecycle upgrades, dependencies, health checks.

# Jenkinsfile
Go to Jenkinsfile and read for complete understanding.

## pom.xml
pom.xml is the package dependency for Java, similar to package.json for React and Node app.

# Dockerfile
Dockerfile is nothing but get the JAR and execute the JAR file.

# Static Code Analysis
Tell the URL at which Sonar server is running. Execute mvn target sonar:sonar with auth token created in Jenkins and URL of server.

# Docker Image Creation and Deployment
Get Docker creds, build image and push to registry using creds. Finally, update the deployment file with image tag.

# Continuous Delivery
Add the ArgoCD operator which will create all ArgoCD architecture components (repo server, app controller, etc.). Get the secrets from ArgoCD-cluster and echo secret | base64 -d to decode and get the initial admin password.

Port forward the service - and run it locally. Provide admin, updated password, create a test app- app name test, project (default) if you give own values you should have auth configured. Provide Git repo URL and path for the manifests.

Provide cluster URL on the other side and namespace which ArgoCD runs and sync automatically and create the project.

That's it all done! Entire pipeline executed.


