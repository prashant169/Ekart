# Spring Boot Shopping Cart Web App

## About

This is a demo project for practicing Spring + Thymeleaf. The idea was to build some basic shopping cart web app.

It was made using **Spring Boot**, **Spring Security**, **Thymeleaf**, **Spring Data JPA**, **Spring Data REST and Docker**. 
Database is in memory **H2**.

There is a login and registration functionality included.

Users can shop for products. Each user has his own shopping cart (session functionality).
Checkout is transactional.

## Configuration

### Configuration Files

Folder **src/resources/** contains config files for **shopping-cart** Spring Boot application.

* **src/resources/application.properties** - main configuration file. Here it is possible to change admin username/password,
as well as change the port number.

## How to run

There are several ways to run the application. You can run it from the command line with included Maven Wrapper, Maven or Docker. 

Once the app starts, go to the web browser and visit `http://localhost:8070/home`

Admin username: **admin**

Admin password: **admin**

User username: **user**

User password: **password**

### Maven Wrapper

#### Using the Maven Plugin

Go to the root folder of the application and type:
```bash
$ chmod +x scripts/mvnw
$ scripts/mvnw spring-boot:run
```

#### Using Executable Jar

Or you can build the JAR file with 
```bash
$ scripts/mvnw clean package
``` 

Then you can run the JAR file:
```bash
$ java -jar target/shopping-cart-0.0.1-SNAPSHOT.jar
```

### Maven

Open a terminal and run the following commands to ensure that you have valid versions of Java and Maven installed:

```bash
$ java -version
java version "1.8.0_102"
Java(TM) SE Runtime Environment (build 1.8.0_102-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.102-b14, mixed mode)
```

```bash
$ mvn -v
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-10T16:41:47+00:00)
Maven home: /usr/local/Cellar/maven/3.3.9/libexec
Java version: 1.8.0_102, vendor: Oracle Corporation
```

#### Using the Maven Plugin

The Spring Boot Maven plugin includes a run goal that can be used to quickly compile and run your application. 
Applications run in an exploded form, as they do in your IDE. 
The following example shows a typical Maven command to run a Spring Boot application:
 
```bash
$ mvn spring-boot:run
``` 

#### Using Executable Jar

To create an executable jar run:

```bash
$ mvn clean package
``` 

To run that application, use the java -jar command, as follows:

```bash
$ java -jar target/shopping-cart-0.0.1-SNAPSHOT.jar
```

To exit the application, press **ctrl-c**.

### Docker

It is possible to run **shopping-cart** using Docker:

Build Docker image:
```bash
$ mvn clean package
$ docker build -t shopping-cart:dev -f docker/Dockerfile .
```

Run Docker container:
```bash
$ docker run --rm -i -p 8070:8070 \
      --name shopping-cart \
      shopping-cart:dev
```

##### Helper script

It is possible to run all of the above with helper script:

```bash
$ chmod +x scripts/run_docker.sh
$ scripts/run_docker.sh
```

## Docker 

Folder **docker** contains:

* **docker/shopping-cart/Dockerfile** - Docker build file for executing shopping-cart Docker image. 
Instructions to build artifacts, copy build artifacts to docker image and then run app on proper port with proper configuration file.

## Util Scripts

* **scripts/run_docker.sh.sh** - util script for running shopping-cart Docker container using **docker/Dockerfile**

## Tests

Tests can be run by executing following command from the root of the project:

```bash
$ mvn test
```

## Helper Tools

### HAL REST Browser

Go to the web browser and visit `http://localhost:8070/`

You will need to be authenticated to be able to see this page.

### H2 Database web interface

Go to the web browser and visit `http://localhost:8070/h2-console`

In field **JDBC URL** put 
```
jdbc:h2:mem:shopping_cart_db
```

In `/src/main/resources/application.properties` file it is possible to change both
web interface url path, as well as the datasource url.
______________________________________________________________________________________________________________
# 🚀 Ekart Java App – Local CI/CD with GitHub Actions & Argo CD

A complete **CI/CD pipeline** for the Java-based **Ekart** application using **GitHub Actions** for building and pushing Docker images, and **Argo CD** for automated deployment via GitOps—all running **locally** using Docker Desktop or Minikube.

---

## 📌 Problem Statement

> Set up a complete CI/CD pipeline for the Java-based Ekart application using GitHub Actions for building and pushing Docker images to Docker Hub, and Argo CD for automated deployment via GitOps. All components—including Kubernetes, Argo CD, and the application—must be installed and configured locally, ensuring the entire pipeline works end-to-end without relying on cloud services.

---

## 🧰 Tech Stack

| Component      | Tool/Service              |
|----------------|---------------------------|
| CI             | GitHub Actions            |
| CD             | Argo CD                   |
| Container Reg. | Docker Hub                |
| App            | Java (Ekart)              |
| Cluster        | Minikube or Docker Desktop |
| Git Repo       | https://github.com/prashant169/Ekart.git |

---

## 🗂️ Repository Structure

```
Ekart/
├── .github/workflows/docker-build.yml   # GitHub Actions CI pipeline
├── k8s/                                 # Kubernetes YAMLs for Ekart
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ekart-app.yaml                   # Argo CD Application
├── Dockerfile
├── README.md
└── src/                                 # Java application code
```

---

## 🔧 Prerequisites

Install the following locally:

- Docker Desktop (with Kubernetes enabled) OR Minikube
- kubectl
- Argo CD CLI (optional)
- Git
- Java + Maven
- GitHub + Docker Hub accounts

---

## 🚀 Step-by-Step Setup

### 1️⃣ Install Argo CD Locally

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 2️⃣ Access Argo CD UI

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Visit: https://localhost:8080  
Login: `admin`  
Password:

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d && echo
```

---

### 3️⃣ Create Ekart Kubernetes Manifests

Make sure you have:
- `deployment.yaml`
- `service.yaml`
- `ekart-app.yaml`

in the `k8s/` directory of your repo.

---

### 4️⃣ Configure GitHub Actions CI

Create `.github/workflows/docker-build.yml` in your Ekart repo.

Also add these GitHub Secrets:
- `DOCKER_USERNAME`
- `DOCKER_PASSWORD`

---

### 5️⃣ Sync App in Argo CD UI

- Open Argo CD UI
- Select `ekart-app`
- Click **Sync**
- Enable:
  - Prune ✅
  - Apply out-of-sync only ✅
  - Auto-create namespace ✅

---

### 6️⃣ Verify Deployment

```bash
kubectl get pods -n ekart
kubectl get svc -n ekart
minikube service ekart-ssvc -n ekart
```

---

## 🔄 Optional: Auto Image Tag Sync with Argo CD Image Updater

Use annotations like:

```yaml
metadata:
  annotations:
    argocd-image-updater.argoproj.io/image.update-strategy: latest
    argocd-image-updater.argoproj.io/write-back-method: git
    argocd-image-updater.argoproj.io/image-list: ekart=prashantphad45/ekart
```

---

## ✅ Final Result

🎯 GitHub Actions → Docker Hub → Argo CD → Local Kubernetes

Fully automated and cloud-free CI/CD pipeline.

---

## 👨‍💻 Author

**Prashant**  
DevOps Engineer | GitOps Enthusiast | Java CI/CD Specialist

![image](https://github.com/user-attachments/assets/8c12f338-4c80-4d01-b25d-3e68ee8b486b)


![image](https://github.com/user-attachments/assets/5b63327f-0d58-4b45-a186-29a93313e9e8)  




