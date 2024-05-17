Spin up a instance in ec2, since lot of tools jenkins, docker, git are installed better to proceed with large instance to run smoothly

1. install git 
2. configure git 
    - Create ssh keygen new algo - ed25519 (crypotgraphic algorithm faster than rsa)
    - move it to ssh agent as it will take care of auth futher - no need to add ssh key each time for git auth
    - Add the public key to github - ssh keys 
    - check ssh -T git@github.com for auth 
3. Install java 
4. Install jenkins 
    - https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/
5. Setup jenkins with plugins and user
6. Create new pipeline project - provide git as source and also repo name and branch
7. Configure the path for jenkinsfile to take inside repo
8. Pipeline 
    - Use docker as a agent for the jenkins pipeline (it minimises the configuration and also consider if jenkins spins up a new machine to execute this task, once the task completes this instance will be up and running so its better to run as docker agent as container will be destroyed once its process is over so it can free up space)

So inside the docker agent consider using maximum dependencies as docker images - so one is docker and other since i use maven we can add maven and sonarqube cannot be added as i am installing it as a separate thing inside ec2. build a docker image with maven and docker and use that as agent image for pipeline, Similar to tools installed via docker or ec2 add the respecitiuve plkugins in jenkins to manage effectively.(docker pipeline and sonarqube scanner)

Next sonarqube server has to be installed. Install it in same vpc where you are hosting the app (cloud), to avoid security related config.

to install sonarqubve server
apt install unzip
adduser sonarqube - why running as separate user, if compromised only sonarqube will be controlled, not the root systems.
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
unzip *
chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
cd sonarqube-9.4.0.54424/bin/linux-x86-64/
./sonar.sh start

it will run in port 9000, dont forget to add port 9000 allow in ec2 sg 

admin - admin is the username and password, edit and change it. 
So jenkins and sonar is running as separate apps, how jenkins can execute tests in sonarqube, so we have to create token, so go to account and generate token for jenkins in sonarqube 
Add it as credentials in jenkins, we can pass it in pipeline

Install docker in ec2 machine
sudo yum install docker

Add group membership for the default ec2-user so you can run all docker commands without using the sudo command
sudo usermod -a -G docker ec2-user
id ec2-user
newgrp docker

sudo systemctl enable docker.service
sudo systemctl start docker.service

Kubernets and argoCD we can hae it locally as ec2 is already been overloaded
Install docker desktop

Install argocd using operators 
Go to https://operatorhub.io/
Search for argocd get install steps. Install and check whether argocd is up and running
curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.28.0/install.sh | bash -s v0.28.0

This will install the olm operator - which is a kuberneteds component to manage the deployments of operators into k8 cluster and manages its lifecycle upgrades, dependencies, healthchecks

Go to jenkinsfile and read for complete understanding 

What is pom.xml it is the package dependancy for java similar to package.json for react and node app

Dockerfile is nothing but get the JAR anbd execute the JAR file.









