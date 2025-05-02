
# CI/CD Pipeline Automation with Jenkins and Helm

This project demonstrates how to automate application deployment using a CI/CD pipeline powered by Jenkins and Helm. It provides a hands-on guide to setting up Jenkins, creating and deploying Helm charts, and integrating both tools for efficient and secure application delivery.

---

## 📦 Project Components

1. **Jenkins Server Setup**  
2. **Helm Chart Basics**  
3. **Working with Helm Charts**  
4. **Integrating Helm with Jenkins**

---

## 1️⃣ Jenkins Server Setup

### 🎯 Objective
Set up a Jenkins server to automate CI/CD processes with basic security and essential plugin configurations.

### 🛠️ Steps

#### ✅ Jenkins Installation

- Install Jenkins on a dedicated Linux server (e.g., Ubuntu 20.04).
- Recommended installation method:  
  ```bash
  sudo apt update
  sudo apt install openjdk-11-jdk
  wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
  sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
  sudo apt update
  sudo apt install jenkins
  ```

#### 🔌 Plugin Setup

Install the following plugins:
- **Git** – For SCM integration
- **Helm** – For Kubernetes package management
- **Pipeline** – For Jenkinsfile-based CI/CD

Plugin Installation (via Jenkins Dashboard):
- Navigate to: `Manage Jenkins` → `Manage Plugins` → `Available`
- Search and install the required plugins
- Restart Jenkins after plugin installation

#### 🔐 Basic Security Configuration

- Enable security via: `Manage Jenkins` → `Configure Global Security`
- Use Jenkins' own user database
- Set up initial admin user during setup wizard
- Restrict anonymous access
- Limit script execution to trusted sources only

---

## 2️⃣ Helm Chart Basics

### 🎯 Objective
Understand and create basic Helm charts to package Kubernetes applications.

### 📘 What is Helm?

Helm is a package manager for Kubernetes that allows you to define, install, and upgrade even the most complex Kubernetes applications using charts.

### 📦 Creating a Basic Helm Chart

```bash
helm create my-webapp
```

This command scaffolds a Helm chart with directories like:
- `templates/` – Contains Kubernetes manifest templates
- `values.yaml` – Default configuration values

### 🧩 Templating Basics

- Use `{{ .Values.key }}` syntax to inject values
- Customize Kubernetes manifests using Go templating language
- Separate configuration (`values.yaml`) from logic (`templates/*.yaml`)

---

## 3️⃣ Working with Helm Charts

### 🎯 Objective
Deploy and manage applications using Helm charts.

### 🚀 Deploy a Sample Web App

1. Update `values.yaml` with your container image and app name
2. Install the chart:
   ```bash
   helm install my-webapp ./my-webapp
   ```

### ⚙️ Customizing Helm Charts

- Modify `values.yaml` to change app configs like image tag, replicas, environment variables
- Use `--set` flag to override values inline:
  ```bash
  helm install my-webapp ./my-webapp --set image.tag=v2.0.0
  ```

### 🔄 Upgrading and Rolling Back

- Upgrade app:
  ```bash
  helm upgrade my-webapp ./my-webapp --set image.tag=v2.1.0
  ```
- Rollback app:
  ```bash
  helm rollback my-webapp 1
  ```

---

## 4️⃣ Integrating Helm with Jenkins

### 🎯 Objective
Automate application deployments using Jenkins CI/CD pipeline and Helm.

### 🔗 Integration Steps

#### Step 1: Install Helm CLI on Jenkins Server
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

#### Step 2: Configure Jenkins Pipeline

Example `Jenkinsfile`:
```groovy
pipeline {
  agent any
  environment {
    HELM_HOME = '/usr/local/bin'
  }
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/your-org/your-repo.git'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t your-image-name .'
        sh 'docker push your-image-name'
      }
    }
    stage('Helm Deploy') {
      steps {
        sh 'helm upgrade --install your-release ./helm-chart --set image.repository=your-image-name'
      }
    }
  }
}
```

#### 🔐 Simplified Security

- Use credentials plugin to securely store Docker registry and Git credentials
- Restrict script execution with `Approved Scripts` in Jenkins

---

## 🔐 Security Simplified at Every Step

| Component      | Security Measure                                  |
|----------------|---------------------------------------------------|
| Jenkins        | Admin user setup, plugin updates, CSRF protection |
| Helm           | Values.yaml separation, rollback control          |
| CI/CD Pipeline | Secure credentials, limited script execution      |

---

## 🧪 Demonstration Flow

1. Clone the repository
2. Install Jenkins and Helm
3. Create Helm chart for a web application
4. Connect Jenkins to your GitHub repo
5. Trigger a build to deploy the app via Helm

---

## 📄 License

This project is open-sourced under the MIT License.

---

## 👨‍💻 Author

Franklyn Mbelu – Cloud & DevOps Engineer

---

## 📬 Feedback & Contributions

Feel free to fork, raise issues, or submit PRs. Contributions are welcome!

# CI/CD Pipeline Automation with Jenkins and Helm

This project demonstrates how to automate application deployment using a CI/CD pipeline powered by Jenkins and Helm. It provides a hands-on guide to setting up Jenkins, creating and deploying Helm charts, and integrating both tools for efficient and secure application delivery.

---

## 📦 Project Components

1. **Jenkins Server Setup**  
2. **Helm Chart Basics**  
3. **Working with Helm Charts**  
4. **Integrating Helm with Jenkins**

---

## 1️⃣ Jenkins Server Setup

### 🎯 Objective
Set up a Jenkins server to automate CI/CD processes with basic security and essential plugin configurations.

### 🛠️ Steps

#### ✅ Jenkins Installation

- Install Jenkins on a dedicated Linux server (e.g., Ubuntu 20.04).
- Recommended installation method:  
  ```bash
  sudo apt update
  sudo apt install openjdk-11-jdk
  wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
  sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
  sudo apt update
  sudo apt install jenkins
  ```

#### 🔌 Plugin Setup

Install the following plugins:
- **Git** – For SCM integration
- **Helm** – For Kubernetes package management
- **Pipeline** – For Jenkinsfile-based CI/CD

Plugin Installation (via Jenkins Dashboard):
- Navigate to: `Manage Jenkins` → `Manage Plugins` → `Available`
- Search and install the required plugins
- Restart Jenkins after plugin installation

#### 🔐 Basic Security Configuration

- Enable security via: `Manage Jenkins` → `Configure Global Security`
- Use Jenkins' own user database
- Set up initial admin user during setup wizard
- Restrict anonymous access
- Limit script execution to trusted sources only

---

## 2️⃣ Helm Chart Basics

### 🎯 Objective
Understand and create basic Helm charts to package Kubernetes applications.

### 📘 What is Helm?

Helm is a package manager for Kubernetes that allows you to define, install, and upgrade even the most complex Kubernetes applications using charts.

### 📦 Creating a Basic Helm Chart

```bash
helm create my-webapp
```

This command scaffolds a Helm chart with directories like:
- `templates/` – Contains Kubernetes manifest templates
- `values.yaml` – Default configuration values

### 🧩 Templating Basics

- Use `{{ .Values.key }}` syntax to inject values
- Customize Kubernetes manifests using Go templating language
- Separate configuration (`values.yaml`) from logic (`templates/*.yaml`)

---

## 3️⃣ Working with Helm Charts

### 🎯 Objective
Deploy and manage applications using Helm charts.

### 🚀 Deploy a Sample Web App

1. Update `values.yaml` with your container image and app name
2. Install the chart:
   ```bash
   helm install my-webapp ./my-webapp
   ```

### ⚙️ Customizing Helm Charts

- Modify `values.yaml` to change app configs like image tag, replicas, environment variables
- Use `--set` flag to override values inline:
  ```bash
  helm install my-webapp ./my-webapp --set image.tag=v2.0.0
  ```

### 🔄 Upgrading and Rolling Back

- Upgrade app:
  ```bash
  helm upgrade my-webapp ./my-webapp --set image.tag=v2.1.0
  ```
- Rollback app:
  ```bash
  helm rollback my-webapp 1
  ```

---

## 4️⃣ Integrating Helm with Jenkins

### 🎯 Objective
Automate application deployments using Jenkins CI/CD pipeline and Helm.

### 🔗 Integration Steps

#### Step 1: Install Helm CLI on Jenkins Server
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

#### Step 2: Configure Jenkins Pipeline

Example `Jenkinsfile`:
```groovy
pipeline {
  agent any
  environment {
    HELM_HOME = '/usr/local/bin'
  }
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/your-org/your-repo.git'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t your-image-name .'
        sh 'docker push your-image-name'
      }
    }
    stage('Helm Deploy') {
      steps {
        sh 'helm upgrade --install your-release ./helm-chart --set image.repository=your-image-name'
      }
    }
  }
}
```

#### 🔐 Simplified Security

- Use credentials plugin to securely store Docker registry and Git credentials
- Restrict script execution with `Approved Scripts` in Jenkins

---

## 🔐 Security Simplified at Every Step

| Component      | Security Measure                                  |
|----------------|---------------------------------------------------|
| Jenkins        | Admin user setup, plugin updates, CSRF protection |
| Helm           | Values.yaml separation, rollback control          |
| CI/CD Pipeline | Secure credentials, limited script execution      |

---

## 🧪 Demonstration Flow

1. Clone the repository
2. Install Jenkins and Helm
3. Create Helm chart for a web application
4. Connect Jenkins to your GitHub repo
5. Trigger a build to deploy the app via Helm

---

## 📄 License

This project is open-sourced under the MIT License.

---

## 👨‍💻 Author

Franklyn Mbelu – Cloud & DevOps Engineer

---

## 📬 Feedback & Contributions

Feel free to fork, raise issues, or submit PRs. Contributions are welcome!
