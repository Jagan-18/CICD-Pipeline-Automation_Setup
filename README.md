# CICD-Pipeline-Automation_Setup
Sure! Below is your complete README content formatted in markdown. You can paste this directly into your GitHub repository's README.md file:

```markdown
# Setup Instructions

## 1. Update Package List
```bash
sudo apt update
```

## 2. Install OpenJDK 17
```bash
sudo apt install openjdk-17-jdk -y
```
```bash
java -version
```

## 3. Install Maven
```bash
sudo apt install maven -y
```
```bash
mvn -v
```

## 4. Add Jenkins Repository and Key
```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian/jenkins.io.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable/ binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
```

## 5. Install Jenkins
```bash
sudo apt-get install jenkins -y
```

## 6. Enable Jenkins at Boot
```bash
sudo systemctl enable jenkins
```

## 7. Start Jenkins Service
```bash
sudo systemctl start jenkins
```

## 8. Check Jenkins Service Status
```bash
sudo systemctl status jenkins
```

## 9. Get Initial Admin Password for Jenkins
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

## 10. Restart Jenkins (After Plugin Installations)
```bash
sudo systemctl restart jenkins
```

## 11. Access Jenkins Web Interface
Open the following URL in your browser:
```
http://localhost:8080
```

## 12. Retrieve the Unlock Key for Jenkins
When you first access Jenkins, you’ll be asked for the unlock key:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

## 13. Install Required Plugins Manually

1. **SonarQube Scanner:** This plugin integrates Jenkins with SonarQube for code analysis.
   - Go to `Manage Jenkins` > `Manage Plugins` > `Available` tab.
   - Search for "SonarQube Scanner" and install it.
   
2. **JDK (Eclipse Temurin Installer):** Manage Java versions.
   - Search for "JDK" and install it.
   
3. **Pipeline: Stage View:** Visualize Jenkins pipelines.
   - Search for "Pipeline: Stage View" and install it.

4. **OWASP Dependency-Check:** Integrates OWASP Dependency-Check for checking vulnerable dependencies.
   - Search for "OWASP Dependency-Check Plugin" and install it.

5. **Blue Ocean Personalization:** Modern UI for Jenkins pipelines.
   - Search for "Blue Ocean" and install it.

6. **Docker Plugin:** Manage Docker containers and images.
7. **Docker Pipeline Plugin:** Adds Docker steps to Jenkins Pipeline DSL.
8. **Docker Build Step Plugin:** Adds a build step for Docker images.

## 14. Restart Jenkins After Installing Plugins
```bash
sudo systemctl restart jenkins
```

## 15. Run Application Web Interface
You can use the following command to run your application:
```bash
java -jar -Dserver.port=8081 target/*.jar
```

---

## Docker Installation

1. **Update package list:**
```bash
sudo apt update
```

2. **Install required dependencies:**
```bash
sudo apt install ca-certificates curl gnupg
```

3. **Install Docker's official GPG key:**
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

4. **Add Docker repository:**
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

5. **Update package list:**
```bash
sudo apt update
```

6. **Install Docker:**
```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

7. **Install Docker Compose:**
```bash
sudo apt install docker-compose -y
```

8. **Restart Docker Service:**
```bash
sudo service docker restart
```

9. **Set Docker socket permissions:**
```bash
sudo chmod 666 /var/run/docker.sock
```

10. **Restart Docker:**
```bash
sudo systemctl restart docker
```

## 16. Verify Docker Installation
Run the following command to check if Docker is running:
```bash
docker pull hello-world
```

## 17. Run SonarQube Docker Container
To create a SonarQube container:
```bash
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```

## 18. Verify Running Docker Containers
To check if the container is running:
```bash
docker ps
```

## 19. Access SonarQube Web Interface
Open the following URL in your browser:
```
https://<IP address>:9000
```

### Default Credentials:
- **Username:** admin
- **Password:** admin
- **New Password:** admin123

---

## Running SonarQube Scan in Jenkins

```bash
withSonarQubeEnv('sonar') {
    sh '''
        $SCANNER_HOME/bin/sonar-scanner \
        -Dsonar.projectKey=santa \
        -Dsonar.projectName=santa \
        -Dsonar.java.binaries=target/classes
    '''
}
```

### Key Points:
1. Ensure that `SCANNER_HOME` is set correctly in Jenkins or replace it with the actual path to your SonarQube scanner.
2. Confirm the location of your compiled `.class` files and adjust `sonar.java.binaries` accordingly.
3. Ensure that your SonarQube server is configured with the correct `sonar` ID in Jenkins.

---

## OWASP Dependency-Check Script in Jenkins

Run the OWASP Dependency-Check scan:
```bash
// Run OWASP Dependency-Check scan
dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DC'
```

Publish the Dependency-Check report:
```bash
// Publish the Dependency-Check report
dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
```

### Key Points:
1. `--scan ./`: Scans the current directory. Adjust this if you want to scan a different directory.
2. `odcInstallation: 'DC'`: Ensure `DC` matches the name of the Dependency-Check installation in Jenkins.
3. `dependencyCheckPublisher pattern: '**/dependency-check-report.xml'`: Adjust the pattern if your report is in a different location.

---

## Trivy Installation

1. **Install dependencies:**
```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release
```

2. **Add Trivy repository GPG key:**
```bash
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
```

3. **Add Trivy repository:**
```bash
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
```

4. **Update package list:**
```bash
sudo apt-get update
```

5. **Install Trivy:**
```bash
sudo apt-get install trivy -y
```
```

---

Feel free to edit the sections as needed and add any extra setup or instructions. This README should provide a thorough guide to setting up Jenkins, SonarQube, Docker, OWASP Dependency-Check, and Trivy on a Linux system.
