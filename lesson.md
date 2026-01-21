# Lesson : Cloud Native Application - Remote Containerization


## Lesson Objectives

By the end of this lesson, learners will be able to:

1. **Explain** the principles of Cloud Native Applications and their relevance in modern DevOps practices.
2. **Identify** the role of container image registries in container-based deployment workflows.
3. **Apply** semantic versioning concepts when tagging Docker images.
4. **Push and pull** Docker images to and from Docker Hub using a Spring Boot DevOps demo project.

---

## Brief

### Preparation

A Docker Hub account is needed and integrated with Docker Desktop. It is preferred to use email credentials to sign up for DockerHub.

---

## Lesson Overview

In the previous lesson, learners started learning about containerization and hosting a docker container locally. Learners have also experienced what it meant by building images and deploying them locally using `docker run`.

In this lesson, learners will go deeper into understanding **Cloud Native Applications** and **Container Registries**. By the end of this lesson, learners would have attempted pushing and pulling images to and fro the container registry.

This lesson is essential when we move towards learning CI/CD Pipeline using containerization.

---

## Self Studies Check

**Q1: Which of the following describes the Cloud Native App?**

A - Cloud native applications are independent services, packaged as self-contained, lightweight containers that are portable and can be scaled (in or out) rapidly based on the demand.

B - They are microservices and serverless functions.

C - They are delivered with CI/CD toolchains.

D - All of the above


**Q2: Which of the following is NOT a benefit of Cloud Native App?**

A - It makes event driven architecture possible.

B - It maximizes the benefits that cloud brings.

C - It is typically smaller than traditional app and it makes deployment easier.

D - It allows software update with zero downtime.

---

## Part 1 - Principles of Cloud Native App

The quick answer to *what makes an app cloud native* are simply `use of container technology` or `serverless`. This is half true because there are qualities in how "cloud native" your application is. To migrate a traditional software into cloud native applications is not easy feat. Some migrations can take months and years to complete.

Instead of nailing the *practical outcomes* such as containerization or serverless, let us focus on understanding the *principles* of cloud native app. As a Cloud or DevOps Engineer, the details of how the software is written may not be your top priority. However, it is important for you to learn to discern how ready a piece of software is to be cloud native.

### Cloud Native Principles

1. **Single Concern**  
   Every container should address a single concern and do it well.

2. **High Observability**  
   Containerized applications must provide APIs for system health checks and logging.

3. **Lifecycle Conformance**  
   Containers should be able to read events and react to them. Sample events include `PreStart`, `PostStop`, `SIGTERM` (Terminate Signal), and `SIGKILL` (Kill Signal).

4. **Image Immutability**  
   Containers should be immutable and should not change between different environments.

5. **Process Disposability**  
   Containers should be disposable or recyclable, meaning they can be replaced by another container instance at any point in time.

6. **Self Containment**  
   Containers should contain everything they need at build time. They should rely only on the presence of the Linux kernel and any other libraries or dependencies at the time they are built.

7. **Runtime Confinement**  
   Every container should declare its resource requirements and pass that information to the platform, and adhere to those requirements.

---

## Part 2 - What is a Container Image Registry

A container registry acts as a place to store container images and share them via a process of uploading (**pushing**) to the registry and downloading (**pulling**) into another system. Once you pull the image, the application within it can be run on that system.

Consider this logical deployment flow:

<img src="./assets/images/container-registry-deployment.png" />

### Typical Workflow

1. Developers push code to a version control system such as GitHub.
2. When code changes are detected, the system will:
   - Clone the repository  
   - Build a container image  
   - Publish the image to an image registry  
3. When deployment is triggered, the system pulls the image from the registry and deploys it.

---

## Part 3 - Semantic Versioning

In automated deployment processes such as Continuous Deployment, systems need to identify which image to pull from a registry. Images are uniquely identified using **tags**, which are typically version numbers.

The most common versioning approach is **Semantic Versioning** (SemVer).  
Reference: https://semver.org/

### Version Format

```
MAJOR.MINOR.PATCH
```

Example: `2.1.5`

- **MAJOR** – Incompatible API changes  
- **MINOR** – Backwards-compatible feature additions  
- **PATCH** – Backwards-compatible bug fixes  

### Release Cycle Terms

- **Alpha** – Features are incomplete and core elements are still under heavy testing  
- **Beta** – Software is tested by a larger group of users, often external  
- **RC (Release Candidate)** – All features are complete with no known critical bugs  

---

### Activity – Discuss

Based on the given scenarios, discuss what should be the next version of the software.

**Scenario 1**  
Current version: `2.1.3`  
A new feature and two patches are added. No breaking changes.

**Scenario 2**  
Current version: `2.1.3`  
A breaking change with three new features and more than ten patches.

**Scenario 3**  
Current version: `2.1.3`  
Six new features added.

---

## Part 4 - Push and Pull Images to and from the Registry

In this section, **Docker Hub** will be used as the container image registry. Docker Hub is a service provided by Docker for finding and sharing container images.

You will use the `docker push` and `docker pull` commands with your **Spring Boot DevOps demo project**.

---

### Docker Hub Account Creation

1. Go to https://hub.docker.com and register a new account or sign in to an existing account.
2. Ensure Docker Desktop is signed in to Docker Hub  
   - Docker Desktop → Settings / Dashboard → Sign in

---

### Create Docker Hub Repository

1. In Docker Hub, click **Create Repository**
2. Name the repository: `docker-devops-demo`
3. Add an optional description and select **Public**
4. Note the required naming convention:

```
your_dockerhub_username/docker-devops-demo:tag
```

<img src="./assets/images/docker-command.png"   />

---

### Tagging Images and Pushing to the Registry

Before pushing, make a small visible change in your **Spring Boot application** (for example, modify the response message of the `/hello` endpoint) and rebuild the JAR and Docker image.

#### Retag an Existing Image

```bash
docker image tag mysampleapp:latest <your_dockerhub_username>/docker-devops-demo:latest
```

Push the image:

```bash
docker push <your_dockerhub_username>/docker-devops-demo:latest
```

<img src="./assets/images/repo.png" width="50%"/>

---

#### Build and Tag in One Step (Alternative)

```bash
docker build -t <your_dockerhub_username>/docker-devops-demo:latest .
```

> **Note:** In production, tagging everything as `latest` is discouraged.  
> Use semantic versions such as `1.0.0`.

> **Note:** In real-world scenarios, one Docker Hub repository typically stores all images built from a single code base.

---

### Pulling Images from the Registry

```bash
docker pull <your_dockerhub_username>/docker-devops-demo:latest
```

Run the image:

```bash
docker run <your_dockerhub_username>/docker-devops-demo:latest
```

Pulling images from registries allows developers and systems to share runnable artifacts, eliminating the “it works on my machine” problem.

---

### Activity – Pulling Images

Share your Docker Hub repository with a groupmate.  
Pull their image and run it on your local machine.

---

## Part 5 - Docker Hub Images

Docker Hub is not only a storage for application images; it also hosts **base images** used to create Dockerfiles.

<img src="./assets/images/dockerhub-registry.PNG" width="50%"/>

These base images are referenced using the `FROM` keyword.

<img src="./assets/images/from-dockerfile.PNG" />

---

### Activity

Use Docker Hub to find suitable base images for:

1. ExpressJS application  
2. Java Spring Boot application  
3. Python Flask application
