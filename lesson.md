# Lesson 4.5: Cloud Native Application - Remote Containerization

## Learning Objectives

By the end of this lesson, you will be able to:

1. **Explain** the principles of Cloud Native Applications and their relevance in modern DevOps practices
2. **Identify** the role of container image registries in container-based deployment workflows
3. **Apply** semantic versioning concepts when tagging Docker images
4. **Push and pull** Docker images to and from Docker Hub using a Spring Boot DevOps demo project

---

## Prerequisites

Before starting this lesson, ensure you have:

- Completed Lesson 4.4 (Docker - Local Containerization)
- Docker Desktop installed and running
- Your **devops-demo** Spring Boot project
- Internet connection for Docker Hub access

---

## Preparation

A Docker Hub account is needed and integrated with Docker Desktop. It is preferred to use email credentials to sign up for Docker Hub.

---

## Introduction

In the previous lesson, you learned about containerization and hosting a Docker container locally. You experienced building images and deploying them locally using `docker run`.

In this lesson, you will go deeper into understanding **Cloud Native Applications** and **Container Registries**. By the end of this lesson, you will have pushed and pulled images to and from a container registry.

This lesson is essential when we move towards learning CI/CD Pipeline using containerization.

---

## Self Studies Check (10 minutes)

**Q1: Which of the following describes the Cloud Native App?**

A - Cloud native applications are independent services, packaged as self-contained, lightweight containers that are portable and can be scaled (in or out) rapidly based on the demand

B - They are microservices and serverless functions

C - They are delivered with CI/CD toolchains

D - All of the above

---

**Q2: Which of the following is NOT a benefit of Cloud Native App?**

A - It makes event driven architecture possible

B - It maximizes the benefits that cloud brings

C - It is typically smaller than traditional app and it makes deployment easier

D - It allows software update with zero downtime

**Answers will be discussed during the lesson.**

---

## Part 1 - Principles of Cloud Native App (20 minutes)

The quick answer to *what makes an app cloud native* is simply `use of container technology` or `serverless`. This is half true because there are qualities in how "cloud native" your application is. To migrate a traditional software into cloud native applications is not an easy feat. Some migrations can take months and years to complete.

Instead of nailing the *practical outcomes* such as containerization or serverless, let us focus on understanding the *principles* of cloud native app. As a Cloud or DevOps Engineer, the details of how the software is written may not be your top priority. However, it is important for you to learn to discern how ready a piece of software is to be cloud native.

### Cloud Native Principles

**1. Single Concern**  
Every container should address a single concern and do it well.

**Example:** A web server container should only serve web requests, not also handle database operations.

**2. High Observability**  
Containerized applications must provide APIs for system health checks and logging.

**Example:** Your Spring Boot app exposes `/actuator/health` endpoint for monitoring.

**3. Lifecycle Conformance**  
Containers should be able to read events and react to them. Sample events include `PreStart`, `PostStop`, `SIGTERM` (Terminate Signal), and `SIGKILL` (Kill Signal).

**Example:** When a container receives a shutdown signal, it should gracefully close connections before stopping.

**4. Image Immutability**  
Containers should be immutable and should not change between different environments.

**Example:** The same Docker image runs in development, staging, and production without modifications.

**5. Process Disposability**  
Containers should be disposable or recyclable, meaning they can be replaced by another container instance at any point in time.

**Example:** If a container crashes, the orchestrator (like Kubernetes) can quickly replace it with a new instance.

**6. Self Containment**  
Containers should contain everything they need at build time. They should rely only on the presence of the Linux kernel and any other libraries or dependencies at the time they are built.

**Example:** Your Docker image includes Java, your JAR file, and all dependencies - nothing else needs to be installed.

**7. Runtime Confinement**  
Every container should declare its resource requirements and pass that information to the platform, and adhere to those requirements.

**Example:** Container declares it needs 512MB RAM and 0.5 CPU cores.

---

## Part 2 - What is a Container Image Registry (10 minutes)

A container registry acts as a place to store container images and share them via a process of uploading (**pushing**) to the registry and downloading (**pulling**) into another system. Once you pull the image, the application within it can be run on that system.

### Container Registry Deployment Flow

```
┌──────────────┐
│  Developers  │
│  Push Code   │
└──────┬───────┘
       │
       ▼
┌──────────────────┐
│  Version Control │
│   (GitHub)       │
└──────┬───────────┘
       │
       ▼
┌──────────────────┐      ┌──────────────────┐
│   CI/CD System   │─────▶│  Container       │
│   - Clone repo   │      │  Registry        │
│   - Build image  │      │  (Docker Hub)    │
│   - Push image   │      │                  │
└──────────────────┘      └────────┬─────────┘
                                   │
                                   │ Pull
                                   ▼
                          ┌──────────────────┐
                          │  Production      │
                          │  Environment     │
                          └──────────────────┘
```

### Typical Workflow

1. Developers push code to a version control system such as GitHub
2. When code changes are detected, the CI/CD system will:
   - Clone the repository  
   - Build a container image  
   - Publish the image to an image registry  
3. When deployment is triggered, the system pulls the image from the registry and deploys it

**Benefits of Container Registries:**
- **Centralized storage** - All team members access the same images
- **Version control** - Track different versions of your application
- **Distribution** - Easy to share images across environments
- **Security** - Private registries keep proprietary images secure

---

## Part 3 - Semantic Versioning (25 minutes)

In automated deployment processes such as Continuous Deployment, systems need to identify which image to pull from a registry. Images are uniquely identified using **tags**, which are typically version numbers.

The most common versioning approach is **Semantic Versioning** (SemVer).  

**Reference:** https://semver.org/

### Version Format

```
MAJOR.MINOR.PATCH
```

**Example:** `2.1.5`

- **MAJOR** – Incompatible API changes (breaking changes)
- **MINOR** – Backwards-compatible feature additions  
- **PATCH** – Backwards-compatible bug fixes  

### Examples

**Starting version:** `1.0.0`

| Change | New Version | Reason |
|--------|-------------|--------|
| Fix bug in login | `1.0.1` | Patch - bug fix |
| Add new "export" feature | `1.1.0` | Minor - new feature |
| Change API response format | `2.0.0` | Major - breaking change |

### Release Cycle Terms

- **Alpha** – Features are incomplete and core elements are still under heavy testing  
- **Beta** – Software is tested by a larger group of users, often external  
- **RC (Release Candidate)** – All features are complete with no known critical bugs  

**Examples:**
- `1.0.0-alpha.1` - First alpha release
- `1.0.0-beta.2` - Second beta release
- `1.0.0-rc.1` - First release candidate
- `1.0.0` - Final stable release

---

### 👨‍💻 Activity – Semantic Versioning Discussion (10 minutes)

Based on the given scenarios, discuss what should be the next version of the software.

**Scenario 1**  
Current version: `2.1.3`  
A new feature and two patches are added. No breaking changes.

**Answer:** `2.2.0` (Minor version bump for new feature, patch count resets)

---

**Scenario 2**  
Current version: `2.1.3`  
A breaking change with three new features and more than ten patches.

**Answer:** `3.0.0` (Major version bump for breaking change, minor and patch reset)

---

**Scenario 3**  
Current version: `2.1.3`  
Six new features added.

**Answer:** `2.2.0` or `2.7.0` depending on strategy:
- `2.2.0` if you bump minor once for "this release has features"
- `2.7.0` if you bump minor for each feature (6 features = +6 to minor)

Most teams use the first approach.

---

## Part 4 - Push and Pull Images to and from Docker Hub (70 minutes)

In this section, **Docker Hub** will be used as the container image registry. Docker Hub is a service provided by Docker for finding and sharing container images.

You will use the `docker push` and `docker pull` commands with your **Spring Boot DevOps demo project**.

---

### Step 1: Docker Hub Account Creation (10 minutes)

**If you don't have a Docker Hub account:**

1. Go to https://hub.docker.com
2. Click **Sign Up**
3. Fill in:
   - Docker ID (username) - choose carefully, you'll use this in image names
   - Email address
   - Password
4. Verify your email
5. Sign in to Docker Hub

**If you already have an account:**
1. Go to https://hub.docker.com
2. Sign in

---

### Step 2: Sign in to Docker Desktop (5 minutes)

**Option 1: Via Docker Desktop UI**
1. Open Docker Desktop
2. Click **Sign in** (top-right)
3. Enter your Docker Hub credentials
4. You should see your username displayed

**Option 2: Via Command Line**

```bash
docker login
```

**You'll be prompted:**
```
Username: your_dockerhub_username
Password: [password won't show as you type]
```

**Expected output:**
```
Login Succeeded
```

**Verify you're logged in:**
```bash
docker info | grep Username
```

**Expected output:**
```
Username: your_dockerhub_username
```

---

### Step 3: Create Docker Hub Repository (10 minutes)

1. Go to https://hub.docker.com
2. Click **Repositories** (top menu)
3. Click **Create Repository**
4. Fill in:
   - **Repository name:** `devops-demo`
   - **Description:** "DevOps demo Spring Boot application"
   - **Visibility:** Public (Free tier requirement)
5. Click **Create**

**Your repository URL will be:**
```
https://hub.docker.com/r/YOUR_USERNAME/devops-demo
```

**Image naming convention:**
```
YOUR_USERNAME/devops-demo:TAG
```

**Example:**
```
john123/devops-demo:latest
john123/devops-demo:1.0.0
john123/devops-demo:1.0.1
```

---

### Step 4: Modify Your Application (5 minutes)

Make a small visible change so you can verify the new image works.

**Edit `DemoController.java`:**

```java
@GetMapping("/hello")
public String hello() {
    return "DevOps demo - now on Docker Hub!";
}
```

---

### Step 5: Rebuild JAR and Docker Image (10 minutes)

**Navigate to your project:**
```bash
cd path/to/devops-demo
```

**Rebuild the JAR:**
```bash
mvn clean package -DskipTests
```

**Expected output:**
```
[INFO] BUILD SUCCESS
[INFO] Total time: X seconds
```

**Verify JAR exists:**
```bash
ls target/*.jar
```

**You should see:**
```
target/devops-demo-0.0.1-SNAPSHOT.jar
```

---

### Step 6: Build and Tag Docker Image (10 minutes)

You have two options for building and tagging:

**Option 1: Build with final tag directly (Recommended)**

Replace `YOUR_USERNAME` with your actual Docker Hub username:

```bash
docker build -t YOUR_USERNAME/devops-demo:latest .
```

**Example:**
```bash
docker build -t john123/devops-demo:latest .
```

**Expected output:**
```
[+] Building 3.2s (8/8) FINISHED
=> [1/3] FROM docker.io/library/eclipse-temurin:21-jdk-alpine
=> [2/3] COPY target/*.jar app.jar
=> [3/3] CMD ["java", "-jar", "app.jar"]
=> => naming to docker.io/john123/devops-demo:latest
```

---

**Option 2: Build then retag (Alternative)**

If you already have an image tagged as `mysampleapp`:

```bash
# First, check what images you have
docker image ls

# If you see mysampleapp, retag it:
docker image tag mysampleapp:latest YOUR_USERNAME/devops-demo:latest
```

**If you don't see `mysampleapp`, use Option 1 above.**

---

**Verify your image:**
```bash
docker image ls
```

**You should see:**
```
REPOSITORY                    TAG       IMAGE ID       CREATED          SIZE
john123/devops-demo          latest    abc123def456   10 seconds ago   350MB
```

---

### Step 7: Push Image to Docker Hub (10 minutes)

**Make sure you're logged in:**
```bash
docker login
```

**Push the image:**

Replace `YOUR_USERNAME` with your actual Docker Hub username:

```bash
docker push YOUR_USERNAME/devops-demo:latest
```

**Example:**
```bash
docker push john123/devops-demo:latest
```

**Expected output:**
```
The push refers to repository [docker.io/john123/devops-demo]
5f70bf18a086: Pushed 
a5b456789def: Pushed 
latest: digest: sha256:abc123...xyz789 size: 1234
```

**This may take 1-3 minutes depending on your internet speed.**

**Verify on Docker Hub:**
1. Go to https://hub.docker.com/r/YOUR_USERNAME/devops-demo
2. You should see your image with tag `latest`
3. Check the "Last pushed" timestamp

**Success!** Your image is now on Docker Hub! 🎉

---

### Step 8: Pull Image from Docker Hub (5 minutes)

Now let's simulate pulling the image on a different machine.

**First, remove your local image (optional but demonstrates the pull):**

```bash
docker image rm YOUR_USERNAME/devops-demo:latest
```

**Pull from Docker Hub:**

```bash
docker pull YOUR_USERNAME/devops-demo:latest
```

**Example:**
```bash
docker pull john123/devops-demo:latest
```

**Expected output:**
```
latest: Pulling from john123/devops-demo
a1b2c3d4e5f6: Pull complete
Digest: sha256:abc123...xyz789
Status: Downloaded newer image for john123/devops-demo:latest
docker.io/john123/devops-demo:latest
```

---

### Step 9: Run the Pulled Image (5 minutes)

```bash
docker run -d -p 8080:8080 YOUR_USERNAME/devops-demo:latest
```

**Example:**
```bash
docker run -d -p 8080:8080 john123/devops-demo:latest
```

**Expected output:**
```
def789ghi012jkl345mno678pqr901stu234vwx567yza890bcd123
```

**Test your application:**

Open browser: `http://localhost:8080/hello`

**Expected response:**
```
DevOps demo - now on Docker Hub!
```

**Success!** You've completed the full push/pull cycle! 🎉

---

### Understanding What Happened

**The Docker Hub Workflow:**

```
1. Local Machine
   ├─ Build image: john123/devops-demo:latest
   └─ Push to Docker Hub
   
2. Docker Hub
   ├─ Store image: john123/devops-demo:latest
   └─ Available for anyone to pull
   
3. Any Machine (yours or teammate's)
   ├─ Pull from Docker Hub
   └─ Run the image
```

**Benefits:**
- ✅ No need to share JAR files
- ✅ No "works on my machine" problems
- ✅ Same image runs everywhere
- ✅ Easy collaboration with team

---

### Versioning Your Images (Bonus)

Instead of always using `latest`, use semantic versions:

```bash
# Build with version 1.0.0
docker build -t john123/devops-demo:1.0.0 .

# Push version 1.0.0
docker push john123/devops-demo:1.0.0

# Also tag and push as latest
docker tag john123/devops-demo:1.0.0 john123/devops-demo:latest
docker push john123/devops-demo:latest
```

**Now you have two tags pointing to the same image:**
- `john123/devops-demo:1.0.0` - Specific version
- `john123/devops-demo:latest` - Latest version

**In production:** Always use specific version tags, not `latest`!

---

### 👨‍💻 Activity – Partner Exercise: Pull and Run (15 minutes)

Work with a partner to practice pulling and running each other's images.

**Instructions:**

**Step 1: Share your repository**
- Tell your partner your Docker Hub username
- Your partner's repository will be: `PARTNER_USERNAME/devops-demo:latest`

**Step 2: Pull your partner's image**
```bash
docker pull PARTNER_USERNAME/devops-demo:latest
```

**Step 3: Run your partner's image**

Use port 8081 to avoid conflicts with your own image:
```bash
docker run -d -p 8081:8080 PARTNER_USERNAME/devops-demo:latest
```

**Step 4: Test in browser**

Open: `http://localhost:8081/hello`

**Step 5: Discuss with your partner**
- What message appears?
- Is it different from your version?
- Did it work on the first try?

**Step 6: Clean up**
```bash
docker ps  # Find container ID
docker stop CONTAINER_ID
docker rm CONTAINER_ID
```

---

## Part 5 - Docker Hub Base Images (15 minutes)

Docker Hub is not only a storage for application images; it also hosts **base images** used to create Dockerfiles.

### Official Base Images

Docker Hub contains official images maintained by Docker and the open-source community:

**Common base images:**
- `eclipse-temurin:21-jdk-alpine` - Java 21 (what we're using)
- `node:20-alpine` - Node.js 20
- `python:3.11-slim` - Python 3.11
- `nginx:alpine` - Nginx web server
- `postgres:16` - PostgreSQL database
- `redis:alpine` - Redis cache

### How Dockerfiles Use Base Images

Your Dockerfile references these using the `FROM` keyword:

```dockerfile
FROM eclipse-temurin:21-jdk-alpine
WORKDIR /app
COPY target/*.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
```

**The `FROM` line** means:
- Download the `eclipse-temurin:21-jdk-alpine` image from Docker Hub
- Use it as the foundation for your image
- Your image = base image + your application files

---

### 👨‍💻 Activity – Find Base Images (15 minutes)

Use Docker Hub (https://hub.docker.com) to find suitable base images for:

**1. ExpressJS application**
- Search for "node" on Docker Hub
- Find official Node.js image
- Which tag would you use for Node.js 20?
- **Answer:** `node:20` or `node:20-alpine`

**2. Java Spring Boot application**  
- Search for "java" or "eclipse-temurin"
- Find official Eclipse Temurin image
- Which tag would you use for Java 21?
- **Answer:** `eclipse-temurin:21` or `eclipse-temurin:21-jdk-alpine`

**3. Python Flask application**
- Search for "python" on Docker Hub
- Find official Python image
- Which tag would you use for Python 3.11?
- **Answer:** `python:3.11` or `python:3.11-slim`

**Discussion:**
- Why use `-alpine` or `-slim` tags?
  - **Answer:** Smaller image size, faster downloads, less storage
- When to use full images vs slim/alpine?
  - **Answer:** Use slim/alpine for production, full images when you need extra tools for debugging

---

## Summary

### What You Accomplished Today

1. ✅ Understood Cloud Native Application principles
2. ✅ Learned about container registries and their role in DevOps
3. ✅ Applied semantic versioning to Docker images
4. ✅ Created a Docker Hub account and repository
5. ✅ Built, tagged, and pushed images to Docker Hub
6. ✅ Pulled and ran images from Docker Hub
7. ✅ Explored base images available on Docker Hub

### Key Takeaways

**Cloud Native Principles:**
- Applications should be immutable, disposable, and self-contained
- High observability enables effective monitoring and debugging
- Single concern principle keeps containers simple and manageable

**Container Registries:**
- Central storage for sharing Docker images across teams and environments
- Enable consistent deployments - same image runs everywhere
- Support version control through image tags

**Semantic Versioning:**
- MAJOR.MINOR.PATCH format (e.g., 2.1.5)
- Major = breaking changes, Minor = features, Patch = bug fixes
- Use specific versions in production, not `latest`

**Docker Hub Workflow:**
- Build → Tag → Push → Pull → Run
- Images are portable - share easily with team members
- Base images provide foundation for custom images

---

## Troubleshooting Guide

### Issue 1: "denied: requested access to the resource is denied"

**Cause:** Not logged in to Docker Hub

**Solution:**
```bash
docker login
# Enter username and password
```

---

### Issue 2: "repository does not exist"

**Cause:** Repository not created on Docker Hub or wrong naming

**Solution:**
1. Verify repository exists at https://hub.docker.com
2. Check naming: must be `YOUR_USERNAME/repository-name:tag`
3. Repository name must match exactly (case-sensitive)

---

### Issue 3: "no basic auth credentials"

**Cause:** Docker Desktop not signed in

**Solution:**
1. Open Docker Desktop
2. Click Sign in (top-right)
3. Enter Docker Hub credentials
4. Try push command again

---

### Issue 4: Push is very slow

**Cause:** Large image or slow internet

**Solution:**
- Be patient - first push takes longest
- Subsequent pushes only upload changed layers
- Consider using `-alpine` base images for smaller sizes

---

### Issue 5: Can't access application after docker run

**Cause:** Forgot port mapping

**Solution:**
Add `-p 8080:8080` to docker run:
```bash
docker run -d -p 8080:8080 USERNAME/devops-demo:latest
```

---

## Additional Resources

### Docker Hub Documentation
- [Docker Hub Quickstart](https://docs.docker.com/docker-hub/)
- [Docker Hub Repositories](https://docs.docker.com/docker-hub/repos/)
- [Official Images](https://docs.docker.com/docker-hub/official_images/)

### Docker Commands Reference
- [docker push](https://docs.docker.com/engine/reference/commandline/push/)
- [docker pull](https://docs.docker.com/engine/reference/commandline/pull/)
- [docker tag](https://docs.docker.com/engine/reference/commandline/tag/)

### Semantic Versioning
- [Semantic Versioning Specification](https://semver.org/)
- [Versioning Best Practices](https://semver.org/#faq)

### Video Tutorials
- [Docker Hub Tutorial](https://www.youtube.com/results?search_query=docker+hub+tutorial)
- [Container Registry Explained](https://www.youtube.com/results?search_query=container+registry+explained)

---

**End of Lesson 4.5**

**Congratulations!** You've successfully pushed your first Docker image to Docker Hub and learned how to share containerized applications. This is a crucial skill for modern DevOps practices! 🎉

**Next Lesson:** Lesson 4.6 - Docker Compose (Multi-container Applications)