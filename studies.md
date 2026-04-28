# 4.5 Self Studies

**Estimated Preparation Time:** 65 minutes

---

## Task 1 — Push and Pull Docker Images with Docker Hub (25 minutes)

Watch the following video on working with Docker Hub:

- 📹 [Docker Hub — Push and Pull Images — https://youtu.be/oGxkLH_OAlc?si=3DcO5Elz9alMyD_g]

While watching, refer to **lesson.md Part 4** and pay attention to:
- How to create a Docker Hub account and repository
- How to log in to Docker Hub from the command line using `docker login`
- How to tag an image correctly using your Docker Hub username
- The difference between `docker push` and `docker pull` and when each is used

**Guiding Questions:**
1. Why must your image be tagged with your Docker Hub username before pushing?
2. What does `docker pull` do when the image already exists locally?
3. Why is it recommended to use specific version tags (e.g. `1.0.0`) instead of `latest` in production?

---

## Task 2 — Cloud Native Principles (20 minutes)

No video for this task. Refer to **lesson.md Part 1** and read through the seven Cloud Native principles carefully, then answer the following:

1. For each principle, think of a real-world consequence of **not** following it. For example — if a container is not self-contained, what could go wrong?
2. Which principle do you think is most important for a DevOps engineer to enforce? Write a short explanation of your reasoning.
3. Look at your devops-demo project — which Cloud Native principles does it currently satisfy? Which ones does it not yet satisfy?

**Guiding Questions:**
1. What is the difference between Image Immutability and Process Disposability?
2. Why does Single Concern make containers easier to manage and scale?
3. How does High Observability support monitoring in a production environment?

---

## Task 3 — Semantic Versioning Practice (20 minutes)

No video for this task. Refer to **lesson.md Part 3** and the Semantic Versioning specification at https://semver.org, then complete the following:

Given a starting version of `1.0.0`, determine the correct next version for each scenario:

1. A critical bug fix is applied to the login feature
2. A new user profile page is added with no breaking changes
3. The entire authentication API is redesigned and is no longer backwards compatible
4. Three bug fixes and one new feature are released together
5. The team releases a version for external testers before the final release

Write your answers down and check them against the lesson activity answers during class.

**Guiding Questions:**
1. When does the PATCH number reset to zero?
2. What does a MAJOR version bump signal to other developers using your API?
3. What is the difference between a Beta release and a Release Candidate?

---

## Active Engagement Strategies

- Pause the video during Task 1 whenever a command is shown and run it yourself in your terminal
- After watching, close the video and try to complete the full push/pull workflow from memory
- For Tasks 2 and 3, write your answers before class — do not just read through the content

---

## Additional Reading Material

- [Cloud Native Principles — CNCF](https://www.cncf.io/about/who-we-are/)
- [Docker Hub Documentation](https://docs.docker.com/docker-hub/)
- [Semantic Versioning Specification](https://semver.org/)
- [Container Registries Explained — Red Hat](https://www.redhat.com/en/topics/cloud-native-apps/what-is-a-container-registry)