
# ⚙️ Jenkins Notes for DevOps Engineers

---

## 🚀 1. What is Jenkins?

- Jenkins is an **open-source automation server** used to build, test, and deploy software automatically.
- Plays a major role in **Continuous Integration (CI)** and **Continuous Delivery (CD)**.
- Supports multiple languages and tools, highly extensible with plugins.

### 📌 Use Case:
> Automate the pipeline from code commit to production deployment.

---

## 🔄 2. CI/CD Pipeline Overview

- **Continuous Integration (CI):** Frequently integrating code into a shared repository.
- **Continuous Delivery (CD):** Automatically preparing code changes for release.

### 🔗 Typical Stages:
1. Code Checkout
2. Compile/Build
3. Test
4. Package/Artifact Creation
5. Deploy to Environment

---

## 🖥 3. Installing Jenkins (Ubuntu/Debian)

### ✅ Step-by-step:

```bash
# Install Java (required)
sudo apt update
sudo apt install openjdk-17-jre
java -version

# Add Jenkins repo & key
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

# Install Jenkins
sudo apt-get update
sudo apt-get install jenkins
```

---

## 📜 4. Jenkins Declarative Pipeline Basics

Declarative syntax offers a **readable and structured way** to define CI/CD pipelines.

### 🧱 Pipeline Structure:

```groovy
pipeline {
  agent any

  environment {
    BUILD_ENV = "staging"
  }

  parameters {
    string(name: 'VERSION', defaultValue: '1.0.0')
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/user/repo.git'
      }
    }

    stage('Build') {
      steps {
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test'
      }
    }

    stage('Deploy') {
      steps {
        echo "Deploying version ${params.VERSION} to ${env.BUILD_ENV}"
      }
    }
  }

  post {
    success {
      echo "Build and Deploy successful!"
    }
    failure {
      echo "Pipeline failed!"
    }
  }
}
```

---

## 🛠 5. Key Pipeline Concepts

| Concept         | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `agent`         | Defines where pipeline/stages run (e.g., any, docker).                      |
| `stages`        | Logical divisions (e.g., build, test, deploy).                              |
| `steps`         | Commands/scripts inside a stage.                                            |
| `environment`   | Set env variables accessible across pipeline.                               |
| `parameters`    | Accept input like string, boolean, choice at runtime.                       |
| `post`          | Define actions like email/slack notifications based on success/failure.     |

---

## 🧪 6. Useful Built-in Steps

- `git`: clone repo
- `sh`: execute shell commands
- `junit`: publish test results
- `archiveArtifacts`: archive build files
- `input`: prompt user
- `docker`: run inside container
- `withCredentials`: access secrets

---

## 🔐 7. Managing Secrets

Use Jenkins Credentials Plugin with:

```groovy
withCredentials([string(credentialsId: 'my-secret', variable: 'API_KEY')]) {
  sh 'curl -H "Authorization: Bearer $API_KEY" https://api.example.com'
}
```

---

## 🎨 8. Blue Ocean

- Plugin for **graphical visualization** of pipelines.
- Helps monitor pipeline stages and debug issues easily.

---

## 🧰 9. Best Practices

✅ Keep pipelines modular with shared libraries  
✅ Use version control (Git) for Jenkinsfiles  
✅ Add retries and error handling  
✅ Use descriptive stage/step names  
✅ Test pipelines in isolated environments  

---

## ❓ 10. Interview-style Questions

- How do you structure pipelines for modularity?
- How do you manage credentials securely in Jenkins?
- How do you test Jenkins pipeline changes before production?
- What's the difference between `scripted` and `declarative` pipelines?

---

Would you like this converted to PDF or included alongside your Docker notes?
