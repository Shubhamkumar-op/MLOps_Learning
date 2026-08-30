# Docker

## Day 1 — Docker Basics

### What is Docker?

Docker is a containerization platform that packages an application
and its dependencies into a container so that it can run
consistently across different environments.

### Docker Image

A Docker image is a template/blueprint used to create containers.

Example:

Image
↓
Container

### Docker Container

A container is a running instance of a Docker image.

### Simple Analogy

Image = Recipe

Container = Dish made from the recipe

## Important Commands

### Check Docker version

docker --version

### Check Docker Engine

docker info

### Run a container

docker run hello-world

### List images

docker images

### List running containers

docker ps

### List all containers

docker ps -a

## Important Difference

docker images
→ Shows images

docker ps
→ Shows running containers

docker ps -a
→ Shows all containers

## Docker Workflow

Docker Image
↓
docker run
↓
Container
↓
Application runs
