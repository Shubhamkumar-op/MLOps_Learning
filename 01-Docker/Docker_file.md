# Docker — Dockerfile

## 1. What is a Dockerfile?

A **Dockerfile** is a text file containing instructions that Docker uses to build a Docker image.

It defines how the image should be created, including:

* Base environment
* Dependencies
* Working directory
* Application files
* Default command

### Basic workflow

```text
Dockerfile
    ↓
docker build
    ↓
Docker Image
    ↓
docker run
    ↓
Docker Container
```

### Golden Rule

```text
Dockerfile → Image → Container
```

---

# 2. Why do we need a Dockerfile?

Without a Dockerfile, we may have to manually configure an environment every time.

For example, an ML application may require:

```text
Python
numpy
pandas
scikit-learn
FastAPI
uvicorn
```

Different machines may have different versions or configurations.

A Dockerfile lets us define the environment as instructions so the image can be built consistently.

This is especially useful in MLOps because reproducibility and consistent environments are important.

---

# 3. Dockerfile Instructions Learned

We learned these five core instructions:

```text
FROM
RUN
WORKDIR
COPY
CMD
```

---

# 4. `FROM`

## Meaning

`FROM` specifies the **base image** used to build the new Docker image.

### Example

```dockerfile
FROM ubuntu
```

This means:

> Start building my image using the Ubuntu image as the base.

### Another example

For a Python application:

```dockerfile
FROM python:3.12
```

### Think

```text
Base Image
    ↓
FROM
    ↓
Our Docker Image
```

---

# 5. `RUN`

## Meaning

`RUN` executes a command **during the Docker image build process**.

### Example

```dockerfile
RUN apt update
```

When Docker builds the image, it executes this command.

### Important

```text
RUN
 ↓
BUILD TIME
```

It is used for things such as:

* Installing packages
* Updating packages
* Creating directories
* Running setup commands

Example:

```dockerfile
RUN apt update
```

---

# 6. `WORKDIR`

## Meaning

`WORKDIR` sets the working directory inside the image/container.

### Example

```dockerfile
WORKDIR /app
```

After this instruction, `/app` becomes the working directory.

### Why use it?

It keeps application files organized.

For example:

```text
/app
├── main.py
├── requirements.txt
└── model.pkl
```

### Think

```text
Container
    ↓
WORKDIR /app
    ↓
Current working directory = /app
```

---

# 7. `COPY`

## Meaning

`COPY` copies files or directories from the **build context on the local machine** into the Docker image.

### Syntax

```dockerfile
COPY <source> <destination>
```

### Example

```dockerfile
COPY hello.txt /app/
```

This means:

```text
Local project
    │
    │ hello.txt
    ↓
COPY
    ↓
Docker image
    │
    ↓
/app/hello.txt
```

### Important

`COPY` happens during:

```text
docker build
```

---

# 8. `CMD`

## Meaning

`CMD` specifies the default command that should run when a container starts from the image.

### Example

```dockerfile
CMD ["cat", "/app/hello.txt"]
```

This runs:

```bash
cat /app/hello.txt
```

when the container starts.

### Important Difference

```text
RUN
 ↓
During image BUILD

CMD
 ↓
When container STARTS
```

---

# 9. Our First Dockerfile

Our complete Dockerfile was:

```dockerfile
FROM ubuntu

RUN apt update

WORKDIR /app

COPY hello.txt /app/

CMD ["cat", "/app/hello.txt"]
```

### What each line does

```text
FROM ubuntu
→ Use Ubuntu as the base image

RUN apt update
→ Update Ubuntu package information during build

WORKDIR /app
→ Set /app as working directory

COPY hello.txt /app/
→ Copy hello.txt into /app

CMD ["cat", "/app/hello.txt"]
→ Display hello.txt when container starts
```

---

# 10. Building the Docker Image

We built the image using:

```bash
docker build -t my-first-docker-image .\01-Docker
```

## Breakdown

### `docker build`

Tells Docker to build an image.

### `-t`

Used to assign a name/tag to the image.

```text
-t my-first-docker-image
```

### `.\01-Docker`

Specifies the build context directory containing the Dockerfile and files needed for the build.

### Result

```text
Dockerfile
    ↓
docker build
    ↓
my-first-docker-image:latest
```

---

# 11. Verify the Image

We used:

```bash
docker images
```

and saw:

```text
my-first-docker-image:latest
```

This confirmed that our custom image was successfully created.

---

# 12. Running Our Custom Image

We ran:

```bash
docker run --name my-first-container my-first-docker-image
```

This created a container from:

```text
my-first-docker-image
```

with the name:

```text
my-first-container
```

### Workflow

```text
my-first-docker-image
          ↓
      docker run
          ↓
my-first-container
```

---

# 13. How `CMD` Worked

Our Dockerfile contained:

```dockerfile
CMD ["cat", "/app/hello.txt"]
```

When we ran:

```bash
docker run --name my-first-container my-first-docker-image
```

Docker executed:

```bash
cat /app/hello.txt
```

The file contents were printed.

Then the command finished.

Therefore the container exited.

---

# 14. Why Did the Container Say `Exited (0)`?

We checked:

```bash
docker ps -a
```

and saw:

```text
my-first-container
Exited (0)
```

This is **not an error**.

`0` means the command completed successfully.

The sequence was:

```text
Container starts
      ↓
CMD executes
      ↓
cat /app/hello.txt
      ↓
File contents printed
      ↓
cat finishes
      ↓
No main process remains
      ↓
Container exits
```

### Important Concept

A container normally stays running only while its main process is running.

Our main process was:

```bash
cat /app/hello.txt
```

It finishes immediately, so the container exits.

---

# 15. `RUN` vs `CMD`

This is an important interview question.

| `RUN`                         | `CMD`                           |
| ----------------------------- | ------------------------------- |
| Executes during image build   | Executes when container starts  |
| Used to build/configure image | Defines default runtime command |
| Build time                    | Container runtime               |

### Example

```dockerfile
RUN apt update
```

Happens while:

```text
docker build
```

is running.

Whereas:

```dockerfile
CMD ["cat", "/app/hello.txt"]
```

runs when:

```text
docker run
```

starts the container.

### Remember

```text
RUN → BUILD
CMD → RUN
```

---

# 16. Dockerfile vs Image vs Container

These three must not be confused.

### Dockerfile

Instructions for building an image.

```text
Dockerfile
```

### Image

The built package/template.

```text
my-first-docker-image
```

### Container

A running or stopped instance created from the image.

```text
my-first-container
```

### Relationship

```text
Dockerfile
    ↓ docker build
Image
    ↓ docker run
Container
```

---

# 17. Practical Commands Used

### Create Dockerfile

```powershell
New-Item -Path .\01-Docker\Dockerfile -ItemType File
```

### Open Dockerfile in VS Code

```powershell
code .\01-Docker\Dockerfile
```

### Create practice file

```powershell
New-Item -Path .\01-Docker\hello.txt -ItemType File
```

### Pull base image

```powershell
docker pull ubuntu
```

### List images

```powershell
docker images
```

### Build image

```powershell
docker build -t my-first-docker-image .\01-Docker
```

### Run custom image

```powershell
docker run --name my-first-container my-first-docker-image
```

### List containers

```powershell
docker ps
```

```powershell
docker ps -a
```

---

# 18. Common Mistakes

## Mistake 1 — Confusing `RUN` and `CMD`

Wrong understanding:

```text
RUN = container startup
```

Correct:

```text
RUN → image build time
CMD → container startup
```

---

## Mistake 2 — Thinking Dockerfile is the image

They are different.

```text
Dockerfile → instructions

Image → result of building those instructions
```

---

## Mistake 3 — Thinking `docker run` modifies the image

`docker run` creates a container from an image.

```text
Image
 ↓
docker run
 ↓
Container
```

---

## Mistake 4 — Thinking `Exited (0)` means failure

`Exited (0)` means the process completed successfully.

Non-zero exit codes generally indicate failure.

---

# 19. Placement Interview Questions

### Q1. What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker image.

### Q2. What does `FROM` do?

`FROM` specifies the base image for the Docker image.

### Q3. What does `RUN` do?

`RUN` executes commands during image building.

### Q4. What does `WORKDIR` do?

`WORKDIR` sets the working directory inside the image/container.

### Q5. What does `COPY` do?

`COPY` copies files or directories from the build context into the Docker image.

### Q6. What does `CMD` do?

`CMD` defines the default command executed when a container starts.

### Q7. Difference between `RUN` and `CMD`?

```text
RUN → build time
CMD → container runtime
```

### Q8. What is the relationship between Dockerfile, image, and container?

```text
Dockerfile
    ↓ docker build
Image
    ↓ docker run
Container
```

### Q9. Why did our container exit?

Because its main command:

```bash
cat /app/hello.txt
```

finished successfully. There was no long-running process left.

---

# 20. Quick Revision

```text
FROM
→ Base image

RUN
→ Execute command during build

WORKDIR
→ Set working directory

COPY
→ Copy files into image

CMD
→ Default command when container starts
```

### Most Important Flow

```text
Dockerfile
     ↓
docker build
     ↓
Docker Image
     ↓
docker run
     ↓
Docker Container
```

### Remember

```text
RUN → BUILD TIME

CMD → CONTAINER START TIME
```

**Dockerfile → Image → Container**
