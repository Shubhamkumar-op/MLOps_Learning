# Day 4 — Dockerfile Advanced

## Topics Covered

1. ENV
2. EXPOSE
3. ENTRYPOINT
4. CMD vs ENTRYPOINT
5. COPY vs ADD
6. RUN vs CMD vs ENTRYPOINT
7. Docker Build Cache
8. Dockerfile Best Practices

---

# 1. ENV

## Definition

`ENV` is used to create an **environment variable** inside a Docker image/container.

### Simple Meaning

It is used to store configuration values.

### Example

```dockerfile
ENV APP_NAME="MLOps-Docker"
```

Now inside the container:

```text
APP_NAME = MLOps-Docker
```

### Test

```bash
docker run --rm my-first-docker-image:v2 printenv APP_NAME
```

Output:

```text
MLOps-Docker
```

### Common Uses

```text
PORT=8000
MODEL_NAME=model.pkl
ENVIRONMENT=production
```

### Remember

```text
ENV → Store configuration/value
```

---

# 2. EXPOSE

## Definition

`EXPOSE` tells Docker which port the application inside the container is expected to use.

### Example

```dockerfile
EXPOSE 8000
```

This means:

> The application inside the container is expected to listen on port 8000.

### Important

`EXPOSE` **does not publish the port** to your computer.

To publish a port:

```bash
docker run -p 8000:8000 my-image
```

Here:

```text
8000 → Computer/Host port
8000 → Container port
```

### Remember

```text
EXPOSE → Tells/document the container port

-p → Actually maps/publishes the port
```

---

# 3. ENTRYPOINT

## Definition

`ENTRYPOINT` defines the **main program** that the container is supposed to run.

### Example

```dockerfile
ENTRYPOINT ["echo"]
```

If we run:

```bash
docker run my-image Hello
```

Docker effectively runs:

```bash
echo Hello
```

Output:

```text
Hello
```

If we run:

```bash
docker run my-image Shubham
```

Docker effectively runs:

```bash
echo Shubham
```

### Remember

```text
ENTRYPOINT → Main program
```

---

# 4. CMD

## Definition

`CMD` defines the **default command or default arguments** for a container.

### Example

```dockerfile
CMD ["echo", "Hello"]
```

Running:

```bash
docker run my-image
```

gives:

```text
Hello
```

The default CMD can be replaced when running the container.

### Remember

```text
CMD → Default behavior
```

---

# 5. CMD + ENTRYPOINT Together

They are often used together.

Example:

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

When we run:

```bash
docker run my-image
```

Docker effectively runs:

```bash
python app.py
```

Here:

```text
ENTRYPOINT → python
CMD → app.py
```

If we run:

```bash
docker run my-image test.py
```

Docker effectively runs:

```bash
python test.py
```

### Easy Way to Remember

```text
ENTRYPOINT → Main program
CMD → Default argument
```

---

# 6. COPY vs ADD

Both can copy files into a Docker image.

## COPY

Example:

```dockerfile
COPY hello.txt /app/
```

Meaning:

```text
Local file
    ↓
COPY
    ↓
Docker image
    ↓
/app/hello.txt
```

## ADD

`ADD` can also copy files but has some additional features, such as automatic extraction of certain local compressed archives.

### Which should we normally use?

For normal file copying:

```text
COPY → Preferred
```

### Remember

```text
COPY → Simple copying

ADD → Copying + additional features
```

---

# 7. RUN vs CMD vs ENTRYPOINT

This is an important interview topic.

## RUN

```dockerfile
RUN apt update
```

Runs **during image building**.

```text
docker build
    ↓
RUN
```

## CMD

```dockerfile
CMD ["python", "app.py"]
```

Runs when the container starts and provides the default command/arguments.

## ENTRYPOINT

```dockerfile
ENTRYPOINT ["python"]
```

Runs when the container starts and defines the main executable.

### Quick Table

| Instruction  | Simple Meaning            | When?           |
| ------------ | ------------------------- | --------------- |
| `RUN`        | Execute command           | Build time      |
| `CMD`        | Default command/arguments | Container start |
| `ENTRYPOINT` | Main executable           | Container start |

### Easy Memory

```text
RUN → BUILD

CMD → DEFAULT

ENTRYPOINT → MAIN PROGRAM
```

---

# 8. Docker Build Cache

During our Docker build we saw:

```text
CACHED
```

### What does it mean?

Docker remembers the result of previous build steps.

If nothing relevant has changed, Docker can reuse the previous result instead of doing the work again.

### First Build

```text
RUN apt update
    ↓
Command actually runs
```

### Second Build

```text
RUN apt update
    ↓
CACHED
    ↓
Previous result reused
```

### Why is this useful?

It makes Docker builds faster.

For example, installing many Python/ML packages can take a long time.

If Docker can reuse that layer:

```text
First build → Takes time

Next build → Reuses cache
```

### Remember

```text
Docker Cache → Reuse previous build results
```

---

# 9. Dockerfile Best Practices

## 1. Use a suitable base image

For Python applications:

```dockerfile
FROM python:3.12
```

instead of always starting with:

```dockerfile
FROM ubuntu
```

A Python image already contains Python.

---

## 2. Use WORKDIR

Prefer:

```dockerfile
WORKDIR /app
```

instead of:

```dockerfile
RUN cd /app
```

`WORKDIR` is designed specifically for setting the working directory.

---

## 3. Prefer COPY

For normal file copying:

```dockerfile
COPY . /app/
```

is generally preferred over:

```dockerfile
ADD . /app/
```

---

## 4. Don't put secrets in the Dockerfile

❌ Avoid:

```dockerfile
ENV API_KEY="my-secret-key"
```

Secrets should not be permanently stored inside the image.

We'll learn safer ways to handle secrets later.

---

## 5. Keep images small

Don't install unnecessary packages.

Smaller images generally mean:

```text
Less storage
    ↓
Faster download
    ↓
Faster deployment
```

---

## 6. Use `.dockerignore`

A `.dockerignore` file tells Docker which files should not be included in the build context.

Example:

```text
.git
.env
__pycache__
*.pyc
venv
```

This is useful because we don't want unnecessary files, secrets, virtual environments, or large datasets sent as part of the build context.

---

# 10. Our Day 4 Dockerfile

```dockerfile
FROM ubuntu

RUN apt update

WORKDIR /app

COPY hello.txt /app/

ENV APP_NAME="MLOps-Docker"

EXPOSE 8000

CMD ["cat", "/app/hello.txt"]
```

### Read it in simple English

```text
FROM
↓
Start with Ubuntu

RUN
↓
Update Ubuntu packages

WORKDIR
↓
Work inside /app

COPY
↓
Put hello.txt inside /app

ENV
↓
Create APP_NAME

EXPOSE
↓
Application is expected to use port 8000

CMD
↓
Print hello.txt when the container starts
```

---

# 11. Practical Commands

## Build Image

```powershell
docker build -t my-first-docker-image:v3 .\01-Docker
```

## Check Images

```powershell
docker images
```

## Run Container

```powershell
docker run --name day4-container my-first-docker-image:v3
```

Expected:

```text
Hello from my Docker project!
```

## Check Containers

```powershell
docker ps -a
```

## Test Environment Variable

```powershell
docker run --rm my-first-docker-image:v3 printenv APP_NAME
```

Expected:

```text
MLOps-Docker
```

---

# 12. Important Interview Questions

### Q1. What is ENV?

`ENV` creates an environment variable inside the image/container.

### Q2. What is EXPOSE?

`EXPOSE` documents the port that the application inside the container is expected to use.

### Q3. Does EXPOSE publish the port?

No.

Port publishing is done using:

```bash
docker run -p 8000:8000 image-name
```

### Q4. What is CMD?

`CMD` provides the default command or arguments when a container starts.

### Q5. What is ENTRYPOINT?

`ENTRYPOINT` defines the main executable of the container.

### Q6. Difference between CMD and ENTRYPOINT?

```text
CMD → Default command/arguments

ENTRYPOINT → Main executable
```

### Q7. Difference between RUN and CMD?

```text
RUN → During image build

CMD → When container starts
```

### Q8. Difference between COPY and ADD?

```text
COPY → Simple file copying

ADD → Copying + additional features
```

For normal copying, `COPY` is generally preferred.

### Q9. What is Docker build cache?

Docker build cache allows Docker to reuse results from previous build steps instead of executing them again when possible.

---

# 13. Quick Revision

```text
FROM
→ Base image

RUN
→ Execute command during build

WORKDIR
→ Set working directory

COPY
→ Copy files into image

ENV
→ Store configuration

EXPOSE
→ Container port information

ENTRYPOINT
→ Main program

CMD
→ Default command/arguments
```

### Most Important

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

And:

```text
RUN        → BUILD TIME

CMD        → DEFAULT RUNTIME BEHAVIOR

ENTRYPOINT → MAIN RUNTIME PROGRAM

EXPOSE     → PORT INFORMATION
```

---

# Day 4 Status

```text
✅ ENV
✅ EXPOSE
✅ ENTRYPOINT
✅ CMD
✅ CMD vs ENTRYPOINT
✅ COPY vs ADD
✅ RUN vs CMD vs ENTRYPOINT
✅ Docker Build Cache
✅ Dockerfile Best Practices
```

**Day 4 Dockerfile Advanced — Complete**
