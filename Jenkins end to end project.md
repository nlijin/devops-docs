# 🔐 Jenkins End-to-End CI/CD with Plugins (SonarQube, OWASP Dependency Check, Docker)

This example demonstrates how to integrate essential DevSecOps tools into a Jenkins pipeline for a Node.js or Java application.

---

## 🧰 Tools Used

| Tool/Plugin             | Purpose                                |
|-------------------------|----------------------------------------|
| **SonarQube**           | Code quality and static analysis       |
| **OWASP Dependency-Check** | Security vulnerability scanning       |
| **Docker**              | Build and deploy app                   |
| **JUnit**               | Run and report test results            |
| **Slack / Email**       | Notification on pipeline completion    |

---

## 🏗️ Jenkins Setup Requirements

### ✅ Plugins to Install:
- **SonarQube Scanner**
- **OWASP Dependency-Check Plugin**
- **JUnit Plugin**
- **Docker Pipeline**
- **Slack Notification Plugin** (Optional)

---

## 🛠️ Global Configuration (Jenkins → Manage Jenkins)

- **SonarQube**: Add a new server in **Configure System**.
- **OWASP Dependency Check**: Plugin auto-installs its CLI.
- **Docker Credentials**: Create DockerHub credentials (username/password).
- **Slack**: Configure Slack workspace if needed.

---

## 📁 Sample Project Folder Structure
my-app/
├── src/
├── test/
├── Dockerfile
├── package.json / pom.xml
├── sonar-project.properties


---

## 📜 Jenkinsfile (Declarative)

```groovy
pipeline {
  agent any

  environment {
    DOCKERHUB_USER = credentials('dockerhub-user')
    DOCKERHUB_PASS = credentials('dockerhub-pass')
    SONARQUBE_ENV = 'SonarQube-Server'
  }

  stages {

    stage('Checkout') {
      steps {
        git 'https://github.com/user/my-app.git'
      }
    }

    stage('Install & Build') {
      steps {
        sh 'npm ci' // or mvn clean install
      }
    }

    stage('Static Code Analysis - SonarQube') {
      steps {
        withSonarQubeEnv("${SONARQUBE_ENV}") {
          sh 'sonar-scanner'
        }
      }
    }

    stage('Dependency Vulnerability Check') {
      steps {
        dependencyCheck additionalArguments: '--format HTML --out dependency-check-report'
      }
    }

    stage('Unit Tests') {
      steps {
        sh 'npm test' // or mvn test
        junit '**/test-results/*.xml'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t my-app:latest .'
      }
    }

    stage('Push to Docker Hub') {
      steps {
        sh '''
          echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin
          docker tag my-app:latest $DOCKERHUB_USER/my-app:latest
          docker push $DOCKERHUB_USER/my-app:latest
        '''
      }
    }

    stage('Deploy (Optional)') {
      steps {
        echo "Deploying to staging server..."
        // Add SSH or K8s deployment logic here
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: '**/dependency-check-report/**'
    }
    success {
      echo '✅ Build Passed!'
    }
    failure {
      echo '❌ Build Failed!'
    }
  }
}

