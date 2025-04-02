# Autonomous Build System

## Description
This repository contains docker compose config to deploy gitlab, gitlab-runner and nexus services.
The ``sample/.gitlab-ci.yml`` file contains pipeline config to build and save docker images in nexus
docker hosted repository on new tag push. Pipeline and Dockerfile are configured to use docker and
go proxy repositories for caching project dependencies.

## Example
#### When new tag is pushed, pipeline is triggered:
![alt text](./assets/pipeline.png)

#### Built images are saved in docker hosted repo:
![alt text](./assets/hosted.png)

#### Required images are cached in docker proxy repo:
![alt text](./assets/docker-proxy.png)

#### Required packages are cached in go proxy repo:
![alt text](./assets/go-proxy.png)

## Features
- **Docker image caching**: images caching allow offline builds, or when requested image origin is no longer accessible.
- **Go packages caching**: packages caching allow offline builds, or when requested package origin is no longer accessible.

## Getting started

### Run ansible playbook
```
cd ansible && ansible-playbook -i inventory/hosts.ini playbook.yml
```
The playbook will
 - Start docker containers
 - Wait until Gitlab is ready
 - Obtain Gitlab Runner authentication token
 - Register Gitlab Runner
 - Remove default Nexus repositories
 - Create docker hosted, docker proxy and go proxy Nexus repositories

### Create a project
 - Add new project to gitlab
 - Add Dockerfile
 - Add CI\CD pipeline ``sample/.gitlab-ci.yml``
 - Create CI\CD variables:
   - ``DOCKER_REGISTRY_URL``
   - ``DOCKER_REGISTRY_USER``
   - ``DOCKER_REGISTRY_PASSWORD``