# 🚀 DevOps Project: SonarQube - Code Quality Analysis
This project is a One-Stop Real-Time Solution for performing Code Quality Analysis using SonarQube, seamlessly integrated with Jenkins, Maven, and Docker. It demonstrates a CI/CD pipeline where source code is built, analyzed, and deployed automatically. The final application runs inside a Docker container and is accessible via a web browser.
# 🧩 Tech Stack
- Jenkins – CI/CD automation server
- Maven – Project build and dependency management
- SonarQube – Static code analysis tool
- Docker – Containerization platform
- GitHub – Source code repository
# 📦 Features
- Automated build and code quality checks via Jenkins
- Code analysis using SonarQube
- Dockerized application deployment
- Real-time integration of all tools in a single pipeline
- Clean, maintainable, and production-ready setup
# 🛠️ Prerequisites
- Java (JDK 17)
- Maven
- Docker
- Jenkins (with required plugins)
- SonarQube (running locally or remotely)
- Git
# Steps 
1. Launch Ubuntu VM with instance type as t2.large, AMI: 24.04, Storage: 25 GB, No. of instances: 1
2. Connect to the VM
3. sudo apt update
4. Lets setup Jenkins
   - sudo apt install openjdk-17-jre-headless -y
   - sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
   - https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
   - echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
   - https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
   - /etc/apt/sources.list.d/jenkins.list > /dev/null
   - sudo apt-get update
   - sudo apt-get install jenkins -y
   - vi j.sh <Paste the above commands>
     - Lets give the permission to j.sh file to execute;
       - sudo chmod +x j.sh
       - ls
5. SonarQube Setup
   - Lets setup sonar server using Docker (this is the simplest way)
     - sudo apt install docker.io
   - Lets give permissions to run docker
     - sudo chmod 666 /var/run/docker.sock
   - Lets run docker commands to install SonarQube
     - docker run -d --name sonarqube -p 9000:9000 sonarqube:lts-community
     - docker ps
   - You can see SonarQube container status
     - Open Port No. 9000 in the SG of VM
   - Access SonarQube;
     - <publicip>:9000 --- You can see the SonarQube console
     - Click on 'Home'
     - Default login name and password is 'admin'
     - Set new password
     - You can see the homepage of SonarQube 
     - With this Jenkins and SonarQube setup is ready
6. Jenkins Pipeline Job for SonarQube Analysis
   - Jenkins Dashboard ---> Manage Jenkins ---> Plugins ---> Available Plugins ---> Search for 'SonarQube Scanner' and select it. Also search for 'Eclipse Temurin installer' and 'Pipeline Stage View" and select them ---> Install ---> Click on 'Go back to the top of page'

   - Jenkins Dashboard ---> Manage Jenkins ---> Tools ---> Add JDK ---> Name: jdk17, 'Check' install automatically, Select 'Install from adoptium.net' from dropdown, Version: select jdk-17.0.11+9 ---> Click on 'Add SonarQube Scanner', Name: sonar-scanner, 'Check' install automatically, Select 'Install from Maven Central', Version: SonarQube Scanner 5.0.1.3006 ---> Click on 'Add Maven' ---> Name:  maven3,  'Check' install automatically, Select 'Install from Apache' from dropdown, Version: 3.9.7 ---> Apply ---> Save

   Lets configure the SonarQube server;
   - Goto SonarQube console --- Click on 'Administration' --- Click on 'Security' --- Select 'Users' --- You can see 'Tokens' --- Click on 3 dashes icon --- A New dialogue --- Name: token, Expires in: 90days --- Generate --- You can see token in green colour. Copy it. 

   - Manage Jenkins --- Security --- Credentials --- Click on 'global' --- Click on 'Add Credentials' --- Kind: Secret text, Scope: Global, Secret: <Paste the token copied from SonarQube console>, ID: sonar-token, Description: sonar-token --- Create

   Similarly configure dockerhub credentials with id as "dockerhub-credentials" with kind as "username with password"

   - Manage Jenkins --- System --- Scroll down to 'SonarQube servers' --- Click on 'Add SonarQube' --- Name: sonar-server --- ServerURL: <PublicIPofSQinstalledVM>:9000 --- Server Authentication Token: select 'sonar-token' --- Apply --- Save
7. Create a new Pipeline Job and use the Jenkinsfile
8. Run the Pipeline
   - Trigger the Jenkins pipeline manually
   - After completion:
     - check code quality in SonarQube Dashboard
     - Access your application at: http://localhost:8080
# 📊 SonarQube Dashboard
- SonarQube will display metrics like:
- Code Smells
- Bugs
- Vulnerabilities
- Code Coverage
- Technical Debt



