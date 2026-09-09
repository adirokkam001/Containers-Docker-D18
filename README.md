# 15. Containers — Docker ⭐⭐⭐⭐⭐

Docker is one of the **most important tools for a DevOps fresher**. It is used to package an application together with everything it needs and run it consistently across different environments.

For beginners, learn Docker in this order:

**Container → Image → Docker Engine → Dockerfile → Docker Hub/Registry → Lifecycle → Ports → Volumes → Networks → Environment Variables → Docker Compose**

---

# 1. What is a Container?

A **container** is a lightweight, isolated environment used to run an application.

For example, suppose you have a Python application:

```text
Python Application
        +
Python
        +
Required Libraries
        +
Configuration
        ↓
     Container
```

Docker packages these things into a container so the application can run consistently.

### Simple Definition

> A container is a lightweight, isolated runtime environment in which an application and its dependencies run together.

### Why Containers?

Without Docker:

```text
Developer Machine
     ↓
"It works on my machine"
     ↓
Server
     ↓
Application fails
```

With Docker:

```text
Application
    +
Dependencies
    ↓
 Docker Image
    ↓
 Container
    ↓
Runs consistently
```

### Containers are useful because they provide:

* Isolation
* Portability
* Fast startup
* Consistent environments
* Easy deployment
* Easy scaling

---

# 2. Container vs Virtual Machine

This is a very common interview question.

## Virtual Machine

```text
Physical Server
      ↓
   Host OS
      ↓
 Hypervisor
   ↓     ↓
 VM1    VM2
 ↓       ↓
Guest OS Guest OS
 ↓       ↓
 App     App
```

Each VM normally has its own complete guest operating system.

## Docker Container

```text
Physical Server
      ↓
   Host OS
      ↓
 Docker Engine
   ↓      ↓
Container Container
   ↓        ↓
  App      App
```

Containers share the host operating system kernel, so they are generally lighter and faster to start than VMs.

### Easy Difference

| VM                        | Container               |
| ------------------------- | ----------------------- |
| Heavy                     | Lightweight             |
| Contains guest OS         | Shares host kernel      |
| Slower startup            | Fast startup            |
| More resources            | Fewer resources         |
| Strong OS-level isolation | Process-level isolation |

---

# 3. What is a Docker Image?

A **Docker image** is a read-only template used to create containers.

Think of it like a **blueprint**.

```text
Docker Image
     ↓
Creates
     ↓
Container
```

For example:

```text
nginx image
    ↓
Nginx container
```

You can create multiple containers from the same image.

```text
             Nginx Image
            /     |     \
           ↓      ↓      ↓
      Container Container Container
```

### Important

**Image ≠ Container**

Image:

> Template used to create a container.

Container:

> Running instance of an image.

---

# 4. What is Docker Engine?

**Docker Engine** is the core technology that allows you to build, run, and manage Docker containers.

It includes the components necessary to:

* Build images
* Run containers
* Manage containers
* Manage networks
* Manage volumes

Simplified:

```text
You
 ↓
Docker CLI
 ↓
Docker Engine
 ↓
Containers
```

When you execute:

```bash
docker run nginx
```

Docker CLI sends the request to the Docker Engine.

---

# 5. Install Docker

For a beginner, first install **Docker Desktop**.

Official Docker Website:

https://www.docker.com/

After installation, verify:

```bash
docker --version
```

Example:

```text
Docker version 27.x.x
```

Then:

```bash
docker info
```

If Docker Engine is running, Docker will display information about your Docker installation.

---

# 6. Your First Docker Container

Let's run your first container.

```bash
docker run hello-world
```

Docker will:

```text
docker run
   ↓
Check local images
   ↓
If image doesn't exist
   ↓
Download hello-world image
   ↓
Create container
   ↓
Run container
```

You should see a message explaining that Docker successfully ran the container.

---

# 7. Docker Hub

**Docker Hub** is a public registry where Docker images can be stored and shared.

Think of Docker Hub like:

```text
GitHub
   ↓
Stores source code

Docker Hub
   ↓
Stores Docker images
```

Docker Hub:

https://hub.docker.com/

Docker Hub contains images such as:

```text
nginx
ubuntu
python
node
mysql
redis
```

---

# 8. What is a Registry?

A **Docker registry** is a location where Docker images are stored.

Examples:

* Docker Hub
* Amazon ECR
* GitHub Container Registry
* Google Artifact Registry
* Azure Container Registry

Conceptually:

```text
Developer
   ↓
Build Image
   ↓
Push Image
   ↓
Docker Registry
   ↓
Pull Image
   ↓
Server
```

### Docker Hub vs Registry

**Registry** = General system for storing images.

**Docker Hub** = A popular public Docker registry.

---

# 9. Docker Image Names

For example:

```bash
docker pull nginx
```

This means:

```text
Registry: Docker Hub
Repository: nginx
Tag: latest
```

The `latest` tag is used by default if you don't specify a tag.

You can also specify a tag:

```bash
docker pull nginx:1.27
```

Another example:

```bash
docker pull python:3.12
```

Tags allow you to select a particular image version.

---

# 10. `docker pull`

Downloads an image from a registry.

```bash
docker pull nginx
```

Example:

```text
Docker Hub
    ↓
nginx image
    ↓
Your computer
```

Check the image:

```bash
docker images
```

---

# 11. `docker images`

Displays images stored locally.

```bash
docker images
```

Example:

```text
REPOSITORY   TAG       IMAGE ID       SIZE
nginx        latest    abc123         190MB
python       3.12      xyz456         1GB
```

Important columns:

* Repository
* Tag
* Image ID
* Created
* Size

---

# 12. `docker run`

Creates and starts a container from an image.

```bash
docker run nginx
```

Conceptually:

```text
Image
 ↓
Create container
 ↓
Start container
```

This is one of the most important Docker commands.

---

# 13. Run a Container in Background

Normally:

```bash
docker run nginx
```

may keep your terminal attached to the container's output.

Use `-d` for detached mode:

```bash
docker run -d nginx
```

`-d` means:

> Run container in the background.

---

# 14. Give a Container a Name

```bash
docker run -d --name my-nginx nginx
```

Now the container is called:

```text
my-nginx
```

You can use the name instead of the container ID:

```bash
docker stop my-nginx
```

---

# 15. `docker ps`

Shows **currently running containers**.

```bash
docker ps
```

Example:

```text
CONTAINER ID   IMAGE   STATUS        PORTS
abc123         nginx   Up 2 minutes
```

---

# 16. `docker ps -a`

Shows **all containers**, including stopped containers.

```bash
docker ps -a
```

Difference:

```text
docker ps
     ↓
Running containers

docker ps -a
     ↓
Running + stopped containers
```

This distinction is important.

---

# 17. Container Lifecycle

A container has different states.

```text
Created
   ↓
Running
   ↓
Stopped
   ↓
Removed
```

For example:

```text
docker run
    ↓
Running
    ↓
docker stop
    ↓
Stopped
    ↓
docker start
    ↓
Running
    ↓
docker rm
    ↓
Removed
```

---

# 18. `docker stop`

Stops a running container.

```bash
docker stop my-nginx
```

Check:

```bash
docker ps
```

It won't appear because it is no longer running.

But:

```bash
docker ps -a
```

will show it.

---

# 19. `docker start`

Starts a previously stopped container.

```bash
docker start my-nginx
```

Important:

```text
docker start
```

starts an **existing stopped container**.

---

# 20. `docker restart`

Stops and starts the container again.

```bash
docker restart my-nginx
```

Useful when you want to restart an application/container.

---

# 21. `docker rm`

Removes a container.

```bash
docker rm my-nginx
```

Usually the container should be stopped first.

Force removal:

```bash
docker rm -f my-nginx
```

### Important

Removing a container does **not necessarily mean removing the image**.

```text
Container → removable separately
Image     → removable separately
```

---

# 22. `docker rmi`

Removes a Docker image.

```bash
docker rmi nginx
```

`rmi` means:

> Remove image.

If a container is using the image, Docker may prevent removal until the relevant container is removed.

---

# 23. `docker logs`

Displays logs produced by a container.

```bash
docker logs my-nginx
```

Follow logs continuously:

```bash
docker logs -f my-nginx
```

`-f` means follow.

This is very useful for troubleshooting.

Example:

```text
Application
    ↓
Error occurs
    ↓
docker logs
    ↓
Find error
```

---

# 24. `docker exec`

Runs a command **inside a running container**.

For example:

```bash
docker exec -it my-nginx bash
```

If the image doesn't have `bash`, try:

```bash
docker exec -it my-nginx sh
```

Meaning:

* `exec` → Execute command
* `-i` → Interactive
* `-t` → Terminal

Once inside:

```bash
ls
```

You are now working inside the container.

Exit:

```bash
exit
```

---

# 25. Port Mapping ⭐⭐⭐⭐⭐

This is extremely important.

Suppose an application inside the container runs on:

```text
Port 80
```

But you want to access it from your computer using:

```text
localhost:8080
```

Use:

```bash
docker run -d -p 8080:80 nginx
```

Format:

```text
-p HOST_PORT:CONTAINER_PORT
```

Therefore:

```text
8080:80
```

means:

```text
Your Computer          Container
localhost:8080  --->   port 80
```

Then open:

```text
http://localhost:8080
```

You should see the Nginx welcome page.

### Remember

```bash
-p 8080:80
```

means:

> Host port 8080 maps to container port 80.

---

# 26. `EXPOSE` vs `-p`

These are often confused.

In Dockerfile:

```dockerfile
EXPOSE 80
```

This documents that the application uses port 80.

But it does **not automatically publish the port to your host**.

To actually publish it:

```bash
docker run -p 8080:80 image-name
```

So:

```text
EXPOSE
   ↓
Documents container port

-p
   ↓
Actually maps host → container port
```

---

# 27. What are Volumes?

Containers are designed to be disposable.

If a container is deleted, data stored only inside its writable container layer can be lost.

For persistent data, use **volumes**.

Example:

```text
Container
   ↓
Application data
   ↓
Docker Volume
   ↓
Persistent storage
```

Volumes are commonly used for:

* Database data
* Application uploads
* Persistent configuration/data

---

# 28. Create a Volume

```bash
docker volume create mydata
```

Check:

```bash
docker volume ls
```

Inspect:

```bash
docker volume inspect mydata
```

---

# 29. Use a Volume

Example:

```bash
docker run -d \
  --name my-nginx \
  -v mydata:/data \
  nginx
```

Format:

```text
-v VOLUME_NAME:CONTAINER_PATH
```

Here:

```text
mydata:/data
```

means:

```text
Docker Volume
     ↓
 /data inside container
```

---

# 30. Why Volumes are Important

Imagine a database container:

```text
MySQL Container
       ↓
Database Data
```

If the container is deleted:

```text
Container deleted
       ↓
Data may be lost
```

With a volume:

```text
MySQL Container
       ↓
Docker Volume
       ↓
Database Data
```

You can remove/recreate the container while keeping the persistent data in the volume.

---

# 31. Docker Networks

Containers often need to communicate with each other.

For example:

```text
Frontend
   ↓
Backend
   ↓
Database
```

Docker networks allow containers to communicate.

Create a network:

```bash
docker network create mynetwork
```

Check:

```bash
docker network ls
```

Run containers on the network:

```bash
docker run -d --name backend --network mynetwork backend-image
```

```bash
docker run -d --name database --network mynetwork mysql
```

Containers on the same Docker network can communicate using container/service names.

For example:

```text
backend → database
```

The backend can connect using:

```text
database
```

instead of needing to know the database container's IP address.

---

# 32. Environment Variables

Environment variables allow you to pass configuration to applications.

Example:

```bash
docker run -e APP_ENV=production myapp
```

Here:

```text
APP_ENV = production
```

Inside the container, the application can read:

```text
APP_ENV
```

Another example:

```bash
docker run \
  -e DB_HOST=database \
  -e DB_USER=admin \
  myapp
```

### Why use environment variables?

They allow you to change configuration without rebuilding the Docker image.

For example:

```text
Development
DB_HOST=dev-db

Production
DB_HOST=prod-db
```

Same image, different configuration.

---

# 33. Dockerfile ⭐⭐⭐⭐⭐

A **Dockerfile** is a text file containing instructions for building a Docker image.

Example:

```dockerfile
FROM python:3.12

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

EXPOSE 8000

CMD ["python", "app.py"]
```

Then:

```text
Dockerfile
    ↓
docker build
    ↓
Docker Image
    ↓
docker run
    ↓
Container
```

---

# 34. Dockerfile — `FROM`

`FROM` specifies the base image.

Example:

```dockerfile
FROM python:3.12
```

Meaning:

> Start with the Python 3.12 image.

Another example:

```dockerfile
FROM nginx
```

---

# 35. Dockerfile — `WORKDIR`

Sets the working directory inside the container.

```dockerfile
WORKDIR /app
```

After this, commands operate from:

```text
/app
```

Instead of repeatedly writing:

```bash
cd /app
```

Docker handles it.

---

# 36. Dockerfile — `COPY`

Copies files from your local machine into the image.

```dockerfile
COPY . .
```

Meaning:

```text
Local project
     ↓
Docker image
     ↓
/app
```

For example:

```dockerfile
COPY app.py /app/
```

---

# 37. Dockerfile — `RUN`

Executes a command while building the image.

Example:

```dockerfile
RUN pip install -r requirements.txt
```

This happens during:

```bash
docker build
```

Important distinction:

```text
RUN
 ↓
Build time
```

---

# 38. Dockerfile — `EXPOSE`

Documents the port used by the application.

```dockerfile
EXPOSE 8000
```

It tells Docker/users:

> This application listens on port 8000 inside the container.

Remember that `EXPOSE` alone doesn't publish the port to the host.

---

# 39. Dockerfile — `ENV`

Defines environment variables in the image.

Example:

```dockerfile
ENV APP_ENV=production
```

Then the application can access:

```text
APP_ENV=production
```

You can override it when running:

```bash
docker run -e APP_ENV=development myapp
```

---

# 40. Dockerfile — `CMD`

Defines the default command to run when the container starts.

Example:

```dockerfile
CMD ["python", "app.py"]
```

When you run:

```bash
docker run myapp
```

Docker executes the CMD.

### Important

`CMD` is generally the **default command** and can be overridden when starting the container.

---

# 41. Dockerfile — `ENTRYPOINT`

Defines the main executable for the container.

Example:

```dockerfile
ENTRYPOINT ["python"]
```

Then:

```bash
docker run myapp app.py
```

effectively runs:

```bash
python app.py
```

### CMD vs ENTRYPOINT

Simple way to remember:

```text
CMD
 ↓
Default command/arguments

ENTRYPOINT
 ↓
Main executable
```

They can also be used together:

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

Result:

```text
python app.py
```

---

# 42. Complete Dockerfile Example

Suppose your project contains:

```text
my-python-app/
│
├── Dockerfile
├── app.py
└── requirements.txt
```

Dockerfile:

```dockerfile
FROM python:3.12

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

EXPOSE 8000

ENV APP_ENV=production

CMD ["python", "app.py"]
```

### Build it

Go into the project:

```bash
cd my-python-app
```

Build:

```bash
docker build -t my-python-app .
```

Here:

```text
docker build
    ↓
Build image

-t my-python-app
    ↓
Give image a name

.
    ↓
Use current directory as build context
```

Check:

```bash
docker images
```

---

# 43. Run Your Own Image

```bash
docker run -d --name python-app my-python-app
```

Check:

```bash
docker ps
```

Check logs:

```bash
docker logs python-app
```

---

# 44. Docker Build Process

Understand this flow carefully:

```text
Dockerfile
    +
Application source code
    ↓
docker build
    ↓
Docker Image
    ↓
docker run
    ↓
Container
```

This is one of the most important Docker concepts.

---

# 45. Docker Build Context

When you run:

```bash
docker build -t myapp .
```

the final `.` means:

> Use the current directory as the build context.

Docker can then access files from that context for instructions such as:

```dockerfile
COPY . .
```

---

# 46. `.dockerignore`

Similar to `.gitignore`.

Create:

```text
.dockerignore
```

Example:

```text
.git
node_modules
__pycache__
*.log
.env
```

This prevents unnecessary files from being sent into the Docker build context.

It helps:

* Reduce build context
* Improve build speed
* Avoid copying unnecessary files
* Prevent accidental inclusion of sensitive files

---

# 47. Docker Image Layers

Docker images are built in layers.

Example:

```dockerfile
FROM python:3.12
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
```

Conceptually:

```text
Layer 1 → Python base image
Layer 2 → Working directory
Layer 3 → requirements.txt
Layer 4 → Installed dependencies
Layer 5 → Application code
```

Docker can reuse unchanged layers, which can make subsequent builds faster.

This is why Dockerfile instruction order matters.

---

# 48. Docker Hub — Push Your Image

First tag your image:

```bash
docker tag my-python-app yourusername/my-python-app:latest
```

Login:

```bash
docker login
```

Push:

```bash
docker push yourusername/my-python-app:latest
```

Flow:

```text
Your Computer
     ↓
Docker Image
     ↓
docker push
     ↓
Docker Hub
```

Someone else can then run:

```bash
docker pull yourusername/my-python-app:latest
```

---

# 49. Docker Compose ⭐⭐⭐⭐⭐

Imagine your application has:

```text
Frontend
   ↓
Backend
   ↓
Database
```

You could manually run three containers:

```bash
docker run ...
docker run ...
docker run ...
```

This becomes difficult to manage.

**Docker Compose** allows you to define multiple services in one YAML file and manage them together.

---

# 50. `docker-compose.yml`

Modern Docker Compose commonly uses:

```text
compose.yaml
```

or:

```text
docker-compose.yml
```

Example:

```yaml
services:

  backend:
    image: my-backend
    ports:
      - "8000:8000"
    environment:
      DB_HOST: database
    depends_on:
      - database

  database:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```

This defines two services:

```text
backend
database
```

---

# 51. What is a Service?

A **service** represents a containerized component in a Compose application.

For example:

```yaml
services:

  frontend:
    ...

  backend:
    ...

  database:
    ...
```

You have three services:

```text
Frontend
Backend
Database
```

Compose creates/manages the corresponding containers.

---

# 52. Compose Networks

Compose automatically creates a network for the application in normal usage.

For example:

```text
backend
   ↓
database
```

The backend can communicate with the database using its service name:

```text
database
```

Example:

```yaml
environment:
  DB_HOST: database
```

This is one of the major benefits of Compose.

---

# 53. Compose Volumes

Example:

```yaml
services:

  database:
    image: mysql
    volumes:
      - db-data:/var/lib/mysql

volumes:
  db-data:
```

Flow:

```text
MySQL Container
      ↓
db-data volume
      ↓
Persistent database data
```

---

# 54. Compose Environment Variables

You can define:

```yaml
environment:
  DB_HOST: database
  DB_USER: admin
  DB_PASSWORD: password
```

Or use an `.env` file.

Example `.env`:

```text
DB_USER=admin
DB_PASSWORD=secret
```

Then Compose can reference variables.

For real projects, avoid committing passwords and other secrets into Git.

---

# 55. Multi-Container Application

Let's understand the complete architecture.

```text
                   Docker Compose
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
      Frontend         Backend       Database
      Container        Container      Container
          │              │              │
          └──────────────┴──────────────┘
                     Network
```

Typical request:

```text
User
 ↓
Frontend
 ↓
Backend
 ↓
Database
```

Each component runs independently.

---

# 56. Docker Compose Commands

Start all services:

```bash
docker compose up
```

Run in background:

```bash
docker compose up -d
```

Stop/remove the application containers and default network:

```bash
docker compose down
```

View running services:

```bash
docker compose ps
```

View logs:

```bash
docker compose logs
```

Follow logs:

```bash
docker compose logs -f
```

Build images defined by the Compose file:

```bash
docker compose build
```

Rebuild and start:

```bash
docker compose up --build
```

---

# 57. Complete Beginner Docker Practice

I recommend doing this practical exercise.

## Step 1 — Check Docker

```bash
docker --version
```

---

## Step 2 — Run Hello World

```bash
docker run hello-world
```

---

## Step 3 — Download Nginx

```bash
docker pull nginx
```

---

## Step 4 — Check Images

```bash
docker images
```

---

## Step 5 — Run Nginx

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

---

## Step 6 — Check Container

```bash
docker ps
```

---

## Step 7 — Open Browser

Go to:

```text
http://localhost:8080
```

You should see the Nginx page.

---

## Step 8 — Check Logs

```bash
docker logs my-nginx
```

---

## Step 9 — Enter Container

```bash
docker exec -it my-nginx bash
```

Then:

```bash
ls
```

Exit:

```bash
exit
```

---

## Step 10 — Stop Container

```bash
docker stop my-nginx
```

---

## Step 11 — Check All Containers

```bash
docker ps -a
```

---

## Step 12 — Start Again

```bash
docker start my-nginx
```

---

## Step 13 — Restart

```bash
docker restart my-nginx
```

---

## Step 14 — Remove Container

```bash
docker stop my-nginx
docker rm my-nginx
```

---

# 58. Commands You Must Remember

| Command          | Purpose                          |
| ---------------- | -------------------------------- |
| `docker pull`    | Download image                   |
| `docker images`  | List images                      |
| `docker ps`      | List running containers          |
| `docker ps -a`   | List all containers              |
| `docker run`     | Create + start container         |
| `docker stop`    | Stop container                   |
| `docker start`   | Start stopped container          |
| `docker restart` | Restart container                |
| `docker rm`      | Remove container                 |
| `docker rmi`     | Remove image                     |
| `docker logs`    | View container logs              |
| `docker exec`    | Execute command inside container |
| `docker build`   | Build image                      |
| `docker push`    | Upload image to registry         |

---

# 59. Most Important Docker Concepts to Remember

Keep this mental model:

```text
              Dockerfile
                  ↓
             docker build
                  ↓
              Docker Image
                  ↓
              docker run
                  ↓
              Container
                  ↓
        ┌─────────┼─────────┐
        ↓         ↓         ↓
      Ports     Volumes   Networks
                            ↓
                     Container ↔ Container
```

And for sharing:

```text
Docker Image
     ↓
docker push
     ↓
Docker Hub / Registry
     ↓
docker pull
     ↓
Another Server
```

---

# 60. Docker vs Docker Compose

## Docker

Used to manage individual containers:

```bash
docker run
docker stop
docker logs
docker exec
```

## Docker Compose

Used to manage **multi-container applications**:

```bash
docker compose up
docker compose down
docker compose logs
```

Think:

```text
Docker
  ↓
Individual containers

Docker Compose
  ↓
Application made of multiple containers
```

---

# 61. What a DevOps Fresher Should Be Able to Explain

Before moving to the next Docker topic, make sure you can answer these:

## Basic

1. What is Docker?
2. What is a container?
3. What is a Docker image?
4. Difference between image and container?
5. What is Docker Engine?
6. What is Docker Hub?
7. What is a Docker registry?

## Commands

8. Difference between `docker ps` and `docker ps -a`?
9. Difference between `docker stop` and `docker rm`?
10. Difference between `docker rmi` and `docker rm`?
11. How do you see container logs?
12. How do you enter a running container?
13. How do you build an image?
14. How do you push an image?

## Networking/Storage

15. What is port mapping?
16. What does `-p 8080:80` mean?
17. What is a Docker volume?
18. Why are volumes needed?
19. What is a Docker network?
20. How do containers communicate?

## Dockerfile

21. What does `FROM` do?
22. What does `WORKDIR` do?
23. Difference between `COPY` and `RUN`?
24. What does `EXPOSE` do?
25. What does `ENV` do?
26. Difference between `CMD` and `ENTRYPOINT`?

## Compose

27. What is Docker Compose?
28. What is a service?
29. How do you start Compose?
30. How do you stop Compose?
31. How do services communicate?
32. How do you configure volumes and environment variables?

---

# 62. Final Docker Learning Path

For your DevOps preparation, learn Docker practically in this exact order:

```text
1. Docker installation
        ↓
2. Images
        ↓
3. Containers
        ↓
4. docker run
        ↓
5. Container lifecycle
        ↓
6. Port mapping
        ↓
7. Logs
        ↓
8. docker exec
        ↓
9. Volumes
        ↓
10. Networks
        ↓
11. Environment variables
        ↓
12. Dockerfile
        ↓
13. Build your own image
        ↓
14. Docker Hub / Registry
        ↓
15. Push & Pull images
        ↓
16. Docker Compose
        ↓
17. Multi-container application
        ↓
18. Frontend → Backend → Database
```

---

# 63. Important Docker Mental Model for DevOps

The complete Docker workflow can be remembered like this:

```text
                 APPLICATION SOURCE CODE
                           │
                           ↓
                      Dockerfile
                           │
                           ↓
                    docker build
                           │
                           ↓
                      DOCKER IMAGE
                           │
              ┌────────────┴────────────┐
              ↓                         ↓
        docker run                 docker push
              ↓                         ↓
         CONTAINER              Docker Registry
              │                         │
       ┌──────┼──────┐                  │
       ↓      ↓      ↓                  │
     Ports  Volume Network              │
       │      │      │                  │
       └──────┴──────┘                  │
              │                         │
              ↓                         ↓
        RUNNING APP              docker pull
                                      ↓
                                Another Server
```

---

# 64. Docker in a Real DevOps Environment

In a real DevOps project, the workflow can look like:

```text
Developer
    ↓
Writes Application
    ↓
Git
    ↓
CI Pipeline
    ↓
Docker Build
    ↓
Docker Image
    ↓
Container Registry
    ↓
Deployment
    ↓
Cloud Server
    ↓
Running Container
```

For example:

```text
Developer
    ↓
GitHub
    ↓
Jenkins / GitHub Actions
    ↓
docker build
    ↓
Docker Image
    ↓
Amazon ECR
    ↓
ECS / EC2 / Kubernetes
    ↓
Application
```

This is why Docker is **mandatory knowledge for modern DevOps roles**.

---

# 65. Key Points to Remember

### Container

> A running isolated environment for an application.

### Image

> A read-only template used to create containers.

### Docker Engine

> The engine that builds and runs containers.

### Dockerfile

> Instructions used to build a Docker image.

### Docker Hub

> A public registry for Docker images.

### Registry

> A storage location for Docker images.

### Port Mapping

```text
-p HOST_PORT:CONTAINER_PORT
```

Example:

```bash
-p 8080:80
```

### Volume

> Persistent storage for container data.

### Network

> Allows containers to communicate with each other.

### Environment Variable

> Configuration passed to an application at runtime.

### Docker Compose

> Tool for defining and managing multi-container applications.

---

# 66. Most Important Commands — Quick Revision

```bash
# Docker version
docker --version

# Docker information
docker info

# Download image
docker pull nginx

# List images
docker images

# Run container
docker run nginx

# Run in background
docker run -d nginx

# Run with name
docker run -d --name my-nginx nginx

# Run with port mapping
docker run -d --name my-nginx -p 8080:80 nginx

# Running containers
docker ps

# All containers
docker ps -a

# Stop container
docker stop my-nginx

# Start container
docker start my-nginx

# Restart container
docker restart my-nginx

# Remove container
docker rm my-nginx

# Force remove container
docker rm -f my-nginx

# Remove image
docker rmi nginx

# View logs
docker logs my-nginx

# Follow logs
docker logs -f my-nginx

# Enter container
docker exec -it my-nginx bash

# Create volume
docker volume create mydata

# List volumes
docker volume ls

# Create network
docker network create mynetwork

# List networks
docker network ls

# Build image
docker build -t myapp .

# Login to Docker Hub
docker login

# Tag image
docker tag myapp yourusername/myapp:latest

# Push image
docker push yourusername/myapp:latest

# Pull image
docker pull yourusername/myapp:latest

# Docker Compose
docker compose up

# Docker Compose in background
docker compose up -d

# Docker Compose down
docker compose down

# Docker Compose status
docker compose ps

# Docker Compose logs
docker compose logs

# Docker Compose follow logs
docker compose logs -f

# Build Compose images
docker compose build

# Build and start
docker compose up --build
```

---

# 67. Final Interview Revision

If an interviewer asks:

### What is Docker?

> Docker is a containerization platform used to package applications and their dependencies into portable containers so they can run consistently across different environments.

### What is a Docker image?

> A Docker image is a read-only template containing the application, dependencies, libraries, and instructions required to create a container.

### What is a container?

> A container is a running instance of a Docker image that provides an isolated environment for an application.

### What is Dockerfile?

> A Dockerfile is a text file containing instructions used by Docker to build a Docker image.

### What is Docker Compose?

> Docker Compose is a tool used to define and manage multi-container applications using a YAML configuration file.

### What is port mapping?

> Port mapping connects a port on the host machine to a port inside a container, for example `-p 8080:80` maps host port 8080 to container port 80.

### What are Docker volumes?

> Docker volumes provide persistent storage for container data so that important data can survive container recreation or removal.

### What are Docker networks?

> Docker networks provide communication between containers and allow applications running in different containers to communicate with each other.

### What is Docker Hub?

> Docker Hub is a public container registry where Docker images can be stored, shared, pulled, and published.

---

# 68. Final Goal

After completing this Docker topic, you should be able to understand and perform:

```text
Docker Installation
       ↓
Pull Image
       ↓
Run Container
       ↓
Check Container
       ↓
Map Ports
       ↓
Read Logs
       ↓
Enter Container
       ↓
Stop / Start / Restart
       ↓
Create Volumes
       ↓
Create Networks
       ↓
Use Environment Variables
       ↓
Write Dockerfile
       ↓
Build Image
       ↓
Run Your Own Image
       ↓
Push Image to Docker Hub
       ↓
Pull Image from Registry
       ↓
Write Docker Compose File
       ↓
Run Multiple Containers
       ↓
Frontend
   ↓
Backend
   ↓
Database
```

**For a DevOps fresher, don't just memorize these commands.** You should actually build at least one Docker image, run it with port mapping, inspect its logs, enter the container, use a volume, create a network, push the image to Docker Hub, and finally run a small **Frontend → Backend → Database** application with Docker Compose.

---

# Docker Learning Status

* [ ] Docker installed
* [ ] Understand containers
* [ ] Understand images
* [ ] Understand Docker Engine
* [ ] Understand Docker Hub
* [x] Understand Docker Registry
* [ ] Practice `docker pull`
* [ ] Practice `docker images`
* [ ] Practice `docker ps`
* [ ] Practice `docker ps -a`
* [ ] Practice `docker run`
* [ ] Practice `docker stop`
* [ ] Practice `docker start`
* [ ] Practice `docker restart`
* [ ] Practice `docker rm`
* [ ] Practice `docker rmi`
* [ ] Practice `docker logs`
* [ ] Practice `docker exec`
* [ ] Understand port mapping
* [ ] Understand volumes
* [ ] Understand networks
* [ ] Understand environment variables
* [ ] Write a Dockerfile
* [ ] Use `FROM`
* [ ] Use `WORKDIR`
* [ ] Use `COPY`
* [ ] Use `RUN`
* [ ] Use `EXPOSE`
* [ ] Use `ENV`
* [ ] Use `CMD`
* [ ] Use `ENTRYPOINT`
* [ ] Build a Docker image
* [ ] Tag an image
* [ ] Push an image to Docker Hub
* [ ] Pull an image from a registry
* [ ] Understand Docker Compose
* [ ] Create `docker-compose.yml`
* [ ] Understand Compose services
* [ ] Understand Compose networks
* [ ] Understand Compose volumes
* [ ] Understand Compose environment variables
* [ ] Run a multi-container application
* [ ] Understand Frontend → Backend → Database architecture

---

# Docker — Complete Beginner Notes

**Topic:** 15. Containers — Docker
**Level:** Beginner → DevOps Fresher
**Importance:** ⭐⭐⭐⭐⭐
**Focus:** Docker Fundamentals + Dockerfile + Docker Compose + Practical Commands

