# 🐳 Phase 1: Beginner — Basic Dockerized FastAPI App

![Docker](https://img.shields.io/badge/Docker-Containerization-2496ED?logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-API_Development-009688?logo=fastapi&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04-E95420?logo=ubuntu&logoColor=white)
![Level](https://img.shields.io/badge/Level-Beginner-green)

A beginner-friendly project to understand Docker fundamentals by containerizing a simple FastAPI application using a full Ubuntu base image.

This project demonstrates how to build a Docker image, run a container, expose application ports, inspect logs, and manage containers using essential Docker CLI commands.

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Project Structure](#-project-structure)
- [Technologies Used](#-technologies-used)
- [Application Endpoints](#-application-endpoints)
- [Understanding the Dockerfile](#-understanding-the-dockerfile)
- [Run Locally Without Docker](#-run-locally-without-docker)
- [Build the Docker Image](#-build-the-docker-image)
- [Run the Container](#-run-the-container)
- [Access the Application](#-access-the-application)
- [Essential Docker Commands](#-essential-docker-commands)
- [Inspect Image Size](#-inspect-image-size)
- [Core Docker Concepts](#-core-docker-concepts)
- [What I Learned](#-what-i-learned)
- [What's Next?](#-whats-next)

---

## 📖 Project Overview

The goal of Phase 1 is to understand the fundamentals of Docker by packaging a Python-based FastAPI application into a container.

Instead of using a preconfigured Python image, this project starts with `ubuntu:22.04` and manually installs Python, pip, and the required application dependencies.

### 🎯 Learning Objectives

- Understand the difference between Docker images and containers.
- Learn how Dockerfiles define application environments.
- Build a Docker image using a custom Dockerfile.
- Run and manage containers.
- Understand Docker layers and build caching.
- Map container ports to host machine ports.
- Inspect container logs and access an interactive shell.
- Understand why full operating system images can increase image size.

---

## 📁 Project Structure

```text
phase1-beginner/
│
├── app/
│   └── main.py              # FastAPI application with three routes
│
├── requirements.txt         # Python dependencies
│
├── Dockerfile               # Docker image configuration
│
└── README.md                # Project documentation
```

---

## 🛠 Technologies Used

| Technology | Purpose |
|---|---|
| Docker | Containerization |
| Ubuntu 22.04 | Base operating system |
| Python | Application programming |
| FastAPI | Building REST APIs |
| Uvicorn | Running the FastAPI application |
| Linux CLI | Installing packages and managing the environment |

---

## 🚀 Application Endpoints

The FastAPI application provides three endpoints.

| Endpoint | Method | Description |
|---|---|---|
| `/` | GET | Returns the application's welcome response |
| `/health` | GET | Provides a basic health-check response |
| `/info` | GET | Returns application information |

### API Documentation

After starting the application, open:

- **Application:** http://localhost:8000
- **Interactive API Documentation:** http://localhost:8000/docs
- **Alternative API Documentation:** http://localhost:8000/redoc
- **Health Check:** http://localhost:8000/health
- **Application Information:** http://localhost:8000/info

---

## 🐳 Understanding the Dockerfile

This project uses a single-stage Docker build based on Ubuntu 22.04.

### Dockerfile Instructions

| Instruction | Purpose |
|---|---|
| `FROM ubuntu:22.04` | Sets Ubuntu as the base image |
| `ENV DEBIAN_FRONTEND=noninteractive` | Prevents interactive prompts during package installation |
| `RUN apt-get update && apt-get install ...` | Installs Python, pip, and other required system packages |
| `WORKDIR /app` | Sets the working directory inside the container |
| `COPY requirements.txt .` | Copies the dependency file into the image |
| `RUN pip3 install ...` | Installs the Python dependencies |
| `COPY app/ app/` | Copies the application source code |
| `EXPOSE 8000` | Documents the application's listening port |
| `CMD [...]` | Defines the default command used to start the application |

> **Note:** `EXPOSE 8000` does not publish the port to the host. Port mapping is configured when running the container with `-p 8000:8000`.

### Why Copy `requirements.txt` Separately?

Docker caches build layers. By copying the dependency file and installing packages before copying application code, Docker can reuse the dependency installation layer when only the application source code changes.

This can make subsequent image builds faster.

---

## 💻 Run Locally Without Docker

You can run the application directly on your machine to verify that it works before containerizing it.

### Step 1: Navigate to the Project

```bash
cd phase1-beginner
```

### Step 2: Create a Virtual Environment

```bash
python3 -m venv venv
```

### Step 3: Activate the Virtual Environment

```bash
source venv/bin/activate
```

### Step 4: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 5: Start the Application

```bash
uvicorn app.main:app --reload
```

Open http://localhost:8000/docs to access the interactive API documentation.

To stop the local development server, press `Ctrl + C`.

---

## 🏗️ Build the Docker Image

Make sure Docker is installed and running.

From the `phase1-beginner` directory, execute:

```bash
docker build -t docker-mastery:phase1 .
```

### Command Breakdown

| Command | Meaning |
|---|---|
| `docker build` | Builds an image from a Dockerfile |
| `-t` | Assigns a name and tag to the image |
| `docker-mastery` | Image name |
| `phase1` | Image tag |
| `.` | Uses the current directory as the build context |

The build context contains the files Docker can access during the build.

---

## ▶️ Run the Container

Start a container from the newly created image:

```bash
docker run -d --name phase1-app -p 8000:8000 docker-mastery:phase1
```

### Command Breakdown

| Option | Meaning |
|---|---|
| `-d` | Runs the container in the background |
| `--name phase1-app` | Assigns a name to the container |
| `-p 8000:8000` | Maps host port 8000 to container port 8000 |
| `docker-mastery:phase1` | Specifies the image to run |

The application should now be accessible through your browser.

---

## 🌐 Access the Application

Once the container is running, visit the following URLs:

| Resource | URL |
|---|---|
| Home | http://localhost:8000 |
| Swagger UI | http://localhost:8000/docs |
| ReDoc | http://localhost:8000/redoc |
| Health Check | http://localhost:8000/health |
| Application Info | http://localhost:8000/info |

**Expected result:** The browser should display the API response or the interactive API documentation, depending on the endpoint you open.

---

## 🔧 Essential Docker Commands

### 1. List Running Containers

```bash
docker ps
```

### 2. List All Containers

```bash
docker ps -a
```

### 3. View Container Logs

```bash
docker logs phase1-app
```

### 4. Follow Logs in Real Time

```bash
docker logs -f phase1-app
```

Press `Ctrl + C` to stop following the logs.

### 5. Open a Shell Inside the Container

```bash
docker exec -it phase1-app bash
```

Useful commands inside the container:

```bash
pwd
ls -la
python3 --version
pip3 --version
exit
```

### 6. Stop the Container

```bash
docker stop phase1-app
```

### 7. Remove the Container

```bash
docker rm phase1-app
```

Remove the container only after stopping it.

### 8. List Docker Images

```bash
docker images
```

### 9. Remove the Docker Image

```bash
docker rmi docker-mastery:phase1
```

The image cannot be removed normally while it is still referenced by a container.

> **Tip:** If port 8000 is already in use, change the host port:
>
> ```bash
> docker run -d --name phase1-app -p 8001:8000 docker-mastery:phase1
> ```
>
> Then open http://localhost:8001.

---

## 📦 Inspect Image Size

Check the size of the Docker image:

```bash
docker images docker-mastery:phase1
```

Because this image uses a full Ubuntu operating system and installs Python and application dependencies, it will generally be larger than a purpose-built Python slim image.

The actual size depends on the installed packages, image version, and build configuration.

### Why Does Image Size Matter?

- Larger images take longer to download and distribute.
- They consume more disk space.
- They can increase image transfer time during deployment.
- They provide an opportunity to learn image optimization techniques.

This limitation motivates the next phase of the project.

---

## 🧠 Core Docker Concepts

### 1. Image vs Container

**Docker Image:** A packaged template containing the application, dependencies, and required filesystem.

**Docker Container:** A running or stopped instance created from an image.

### 2. Docker Layers

Docker builds an image through a sequence of instructions. Many filesystem-changing instructions produce reusable image layers, while instructions such as `CMD` configure image metadata.

Docker can reuse cached build results when the relevant inputs and instructions have not changed.

### 3. Port Mapping

```bash
-p 8000:8000
```

The format is:

```text
-p HOST_PORT:CONTAINER_PORT
```

It connects a port on the host to the application's port inside the container.

### 4. Docker Build Context

The final `.` in this command:

```bash
docker build -t docker-mastery:phase1 .
```

specifies the current directory as the build context.

Files outside the build context cannot normally be copied into the image using `COPY`.

### 5. Why Use a Smaller Base Image?

A full Ubuntu image includes many operating system components that a simple Python application may not need.

A smaller, purpose-built Python image can reduce the final image size and improve distribution efficiency.

---

## ✅ What I Learned

After completing this phase, I can:

- [x] Understand Docker images and containers.
- [x] Write a basic Dockerfile.
- [x] Install Python dependencies inside a Docker image.
- [x] Build and run a Dockerized FastAPI application.
- [x] Map container ports to host ports.
- [x] Inspect container logs and execute commands inside containers.
- [x] Manage containers and images using Docker CLI commands.
- [x] Understand basic layer caching and image-size trade-offs.

---

## 🔜 What's Next?

### Phase 2 — Intermediate: Optimized Docker Image

The next phase will focus on improving the containerization workflow.

Planned improvements:

- Replace the full Ubuntu base image with a slim Python image.
- Explore multi-stage Docker builds.
- Reduce the final image size.
- Improve Docker build efficiency.
- Introduce an automated CI pipeline.
- Learn additional container image optimization techniques.

---

## 👨‍💻 Author

**Sagar Savita**

B.Tech Computer Science Engineering | AI DevOps Engineer

Interested in DevOps, Cloud Computing, Linux, Docker, Kubernetes, Automation, and CI/CD.

- GitHub: [@sagarsavitahamirpur713-sketch](https://github.com/sagarsavitahamirpur713-sketch)

---

⭐ If you find this project useful for learning Docker fundamentals, consider giving the repository a star!
