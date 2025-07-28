# ⚙️ Jenkins Pipeline Examples (Easy ➝ Advanced)

---

## 🟢 1. Hello World Bash Job (Beginner)

### 🧪 Goal:
Run a basic shell command in Jenkins.

### 📜 Pipeline:
```groovy
pipeline {
  agent any
  stages {
    stage('Hello') {
      steps {
        sh 'echo "Hello, Jenkins!"'
      }
    }
  }
}
```

---

## 🟡 2. Git Pull + Node.js Build (Basic CI)

### 🧪 Goal:
Clone a repo and install Node dependencies.

### 📜 Pipeline:
```groovy
pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/user/sample-node-app.git'
      }
    }
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
  }
}
```

---

## 🟠 3. CI with Unit Testing and Archiving Artifacts

### 🧪 Goal:
Run unit tests and archive the build output.

### 📜 Pipeline:
```groovy
pipeline {
  agent any
  stages {
    stage('Clone') {
      steps {
        git 'https://github.com/user/sample-node-app.git'
      }
    }
    stage('Install') {
      steps {
        sh 'npm ci'
      }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Build') {
      steps {
        sh 'npm run build'
        archiveArtifacts artifacts: 'build/**', fingerprint: true
      }
    }
  }
}
```

---

## 🔵 4. Build + Deploy to EC2 (Dev Environment)

### 🧪 Goal:
Deploy a Node or React app to an EC2 instance.

### 📜 Pipeline:
```groovy
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'npm run build'
      }
    }
    stage('Deploy to EC2') {
      steps {
        sshagent(['EC2-SSH-CREDENTIALS']) {
          sh 'scp -r ./build ubuntu@<EC2_IP>:/var/www/html'
        }
      }
    }
  }
}
```

---

## 🔴 5. Docker Build + Push to Docker Hub

### 🧪 Goal:
Build an image and push it to Docker Hub.

### 📜 Pipeline:
```groovy
pipeline {
  agent any
  environment {
    DOCKERHUB_USER = credentials('dockerhub-user')
    DOCKERHUB_PASS = credentials('dockerhub-pass')
  }
  stages {
    stage('Build Image') {
      steps {
        sh 'docker build -t myapp:latest .'
      }
    }
    stage('Login & Push') {
      steps {
        sh '''
          echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin
          docker tag myapp:latest $DOCKERHUB_USER/myapp:latest
          docker push $DOCKERHUB_USER/myapp:latest
        '''
      }
    }
  }
}
```

---

## ⚫ 6. Full CI/CD with Approval (Manual Gate)

### 🧪 Goal:
Build → Test → Approve → Deploy.

### 📜 Pipeline:
```groovy
pipeline {
  agent any
  parameters {
    string(name: 'DEPLOY_ENV', defaultValue: 'staging')
  }
  stages {
    stage('Build') {
      steps {
        sh 'npm run build'
      }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Manual Approval') {
      steps {
        input message: "Approve deployment to ${params.DEPLOY_ENV}?"
      }
    }
    stage('Deploy') {
      steps {
        echo "Deploying to ${params.DEPLOY_ENV}..."
      }
    }
  }
}
```

---

## 🧠 Bonus: Shared Library Usage (Advanced Reusability)

### 📁 Structure:
```
vars/
└── deployApp.groovy
```

### 📜 `vars/deployApp.groovy`:
```groovy
def call(env) {
  echo "Deploying app to ${env}..."
  // ssh or kubectl commands here
}
```

### 📜 Jenkinsfile:
```groovy
@Library('my-shared-library') _
pipeline {
  agent any
  stages {
    stage('Deploy') {
      steps {
        deployApp('production')
      }
    }
  }
}
```

---
