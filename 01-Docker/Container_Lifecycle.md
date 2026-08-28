# Docker — Container Lifecycle

## 1. What is Container Lifecycle?

A Docker container goes through different states during its lifetime.

Basic lifecycle:

```text
Docker Image
     ↓
docker run
     ↓
New Container + Running
     ↓
docker stop
     ↓
Stopped Container
     ↓
docker start
     ↓
Running Container
     ↓
docker rm
     ↓
Container Deleted
```

The important idea:

> An image is the blueprint/template, while a container is an instance created from that image.

---

## 2. `docker pull`

### What is it?

`docker pull` downloads a Docker image from a container registry such as Docker Hub to your local computer.

### Syntax

```bash
docker pull <image-name>
```

### Example

```bash
docker pull ubuntu
```

Docker downloads:

```text
ubuntu:latest
```

### Important

`docker pull` downloads an **image**.

It does **not** create a container.

### Think

```text
Docker Hub
    ↓
docker pull
    ↓
Local Docker Images
```

---

## 3. `docker images`

### What is it?

Lists the Docker images available on your local machine.

### Usage

```bash
docker images
```

Modern equivalent:

```bash
docker image ls
```

### Example output

```text
REPOSITORY   TAG       IMAGE ID
ubuntu       latest    2260313b31c8
```

### Think

```text
docker images
       ↓
"What images do I have?"
```

---

## 4. `docker run`

### What is it?

`docker run` creates a **new container from an image and starts it**.

### Syntax

```bash
docker run <image-name>
```

### Example

```bash
docker run ubuntu
```

### Interactive container

```bash
docker run -it ubuntu
```

`-i` → interactive input

`-t` → terminal/TTY

Together:

```text
-it → interactive terminal
```

### Named container

```bash
docker run -it --name mlops-ubuntu ubuntu
```

Here:

```text
Image     → ubuntu
Container → mlops-ubuntu
```

### Important

```text
docker run
→ creates NEW container
→ starts it
```

---

## 5. `docker ps`

### What is it?

Shows currently **running containers**.

### Usage

```bash
docker ps
```

Think:

```text
docker ps
   ↓
"Which containers are running right now?"
```

A stopped container will not appear here.

---

## 6. `docker ps -a`

### What is it?

Shows **all containers**, including:

* Running containers
* Stopped containers
* Exited containers

### Usage

```bash
docker ps -a
```

### Important Difference

```text
docker ps
    ↓
Running containers only

docker ps -a
    ↓
All containers
```

This is a very common Docker interview question.

---

## 7. `docker start`

### What is it?

Starts an **existing stopped container**.

### Syntax

```bash
docker start <container-name>
```

### Example

```bash
docker start mlops-ubuntu
```

### Important Difference

```text
docker run
→ creates a NEW container

docker start
→ starts an EXISTING container
```

Example:

```text
ubuntu image
     ↓
docker run
     ↓
mlops-ubuntu container
     ↓
docker stop
     ↓
Stopped
     ↓
docker start
     ↓
Running again
```

---

## 8. `docker stop`

### What is it?

Stops a currently running container.

### Syntax

```bash
docker stop <container-name>
```

### Example

```bash
docker stop mlops-ubuntu
```

### What happens?

```text
Running Container
       ↓
docker stop
       ↓
Stopped Container
```

### Important

`docker stop` does **not** delete the container.

The container still exists and can be started again.

Check it using:

```bash
docker ps -a
```

---

## 9. `docker restart`

### What is it?

Restarts an existing container.

### Usage

```bash
docker restart <container-name>
```

### Example

```bash
docker restart mlops-ubuntu
```

Conceptually:

```text
Running
   ↓
Stop
   ↓
Start
   ↓
Running
```

Instead of manually doing:

```bash
docker stop mlops-ubuntu
docker start mlops-ubuntu
```

you can use:

```bash
docker restart mlops-ubuntu
```

---

## 10. `docker rm`

### What is it?

Removes/deletes a Docker container.

### Syntax

```bash
docker rm <container-name>
```

### Example

```bash
docker rm mlops-ubuntu
```

### What happens?

```text
Container
    ↓
docker rm
    ↓
Container Deleted
```

### Important

`docker rm` removes the **container**, not the image.

For example:

```bash
docker rm mlops-ubuntu
```

removes:

```text
mlops-ubuntu
```

but does not remove:

```text
ubuntu:latest
```

---

# 11. Complete Command Table

| Command               | Meaning                             | Works With        |
| --------------------- | ----------------------------------- | ----------------- |
| `docker pull ubuntu`  | Download an image                   | Image             |
| `docker images`       | List local images                   | Images            |
| `docker run ubuntu`   | Create + start a new container      | Image → Container |
| `docker ps`           | List running containers             | Containers        |
| `docker ps -a`        | List all containers                 | Containers        |
| `docker start NAME`   | Start an existing stopped container | Container         |
| `docker stop NAME`    | Stop a running container            | Container         |
| `docker restart NAME` | Restart a container                 | Container         |
| `docker rm NAME`      | Delete a container                  | Container         |

---

# 12. Image vs Container

This distinction is extremely important.

### Image

An image is a template/blueprint used to create containers.

Example:

```text
ubuntu:latest
```

### Container

A container is an instance created from an image.

Example:

```text
mlops-ubuntu
```

### Relationship

```text
ubuntu IMAGE
      ↓
  docker run
      ↓
mlops-ubuntu CONTAINER
```

One image can be used to create multiple containers.

---

# 13. Container Lifecycle

The complete lifecycle:

```text
             Docker Image
                  │
                  │ docker run
                  ▼
          New Container
                  │
                  ▼
               Running
              /        \
             /          \
    docker stop      docker restart
          ↓               │
       Stopped            │
          │               │
          │          Stop + Start
          │               │
          └─ docker start ┘
                  │
                  ▼
               Running
                  │
                  │ docker rm
                  ▼
              Deleted
```

---

# 14. Commands Practiced

### Download Ubuntu

```bash
docker pull ubuntu
```

### Create and enter an Ubuntu container

```bash
docker run -it --name mlops-ubuntu ubuntu
```

### Execute commands inside container

```bash
echo "Hello from docker container"
```

```bash
pwd
```

```bash
ls
```

### Exit container

```bash
exit
```

### Check running containers

```bash
docker ps
```

### Check all containers

```bash
docker ps -a
```

### Start existing container

```bash
docker start mlops-ubuntu
```

### Stop container

```bash
docker stop mlops-ubuntu
```

### Restart container

```bash
docker restart mlops-ubuntu
```

### Remove container

```bash
docker rm mlops-ubuntu
```

### Check images

```bash
docker images
```

---

# 15. Common Mistakes

### Mistake 1

Using:

```bash
docker ps
```

and thinking your stopped container was deleted.

### Why?

`docker ps` only shows running containers.

### Fix

```bash
docker ps -a
```

---

### Mistake 2

Thinking `docker start` creates a new container.

It doesn't.

```text
docker run
→ NEW container

docker start
→ EXISTING container
```

---

### Mistake 3

Thinking `docker stop` deletes a container.

It doesn't.

```text
docker stop
→ stopped

docker rm
→ deleted
```

---

### Mistake 4

Thinking `docker rm` removes the image.

It doesn't.

```text
docker rm
→ container

docker rmi
→ image
```

We will learn image removal later.

---

# 16. Placement Interview Questions

### Q1. What is a Docker container?

A Docker container is an isolated runtime instance created from a Docker image.

### Q2. What is a Docker image?

A Docker image is a template containing the application and its required environment used to create containers.

### Q3. Difference between `docker run` and `docker start`?

`docker run` creates and starts a new container.

`docker start` starts an existing stopped container.

### Q4. Difference between `docker stop` and `docker rm`?

`docker stop` stops the container but keeps it.

`docker rm` deletes the container.

### Q5. Difference between `docker ps` and `docker ps -a`?

`docker ps` shows running containers.

`docker ps -a` shows all containers, including stopped containers.

### Q6. Does removing a container remove its image?

No.

A container and its image are separate objects.

### Q7. Does `docker pull` create a container?

No.

It only downloads an image.

---

# 17. Quick Revision

```text
docker pull
→ Download image

docker images
→ List images

docker run
→ Create + start NEW container

docker ps
→ Show RUNNING containers

docker ps -a
→ Show ALL containers

docker start
→ Start EXISTING container

docker stop
→ Stop container

docker restart
→ Restart container

docker rm
→ Delete container
```

### Golden Rule

```text
IMAGE
  ↓
docker run
  ↓
CONTAINER
  ↓
docker stop
  ↓
STOPPED CONTAINER
  ↓
docker start
  ↓
RUNNING CONTAINER
  ↓
docker rm
  ↓
DELETED CONTAINER
```

**Understanding > Memorization.**

The most important thing to understand is:

```text
IMAGE ≠ CONTAINER

docker run ≠ docker start

docker stop ≠ docker rm

docker ps ≠ docker ps -a
```
