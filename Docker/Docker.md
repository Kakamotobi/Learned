# Docker

## Table of Contents
- [What is Docker?](#what-is-docker)
  - [Why use Docker?](#why-use-docker)
  - [Docker vs Virtual Machine](#docker-vs-virtual-machine)
    - [Virtualization vs Containerization](#virtualization-vs-containerization)
  - [Docker Image](#docker-image)
  - [Docker Container](#docker-container)
  - [Docker daemon](#docker-daemon)
- [How does Docker Work?](#how-does-docker-work)
  - [Namespace](#namespace)
  - [Control Groups(Cgroups)](#control-groupscgroups)
  - [`chroot`](#chroot)
- [Getting Started with Docker](#getting-started-with-docker)
- [Setting up MySQL 8 on Docker](#setting-up-mysql-8-on-docker)

## What is Docker?
- **Docker is a software platform that packages software into standardized units called containers.**
  - _A container is essentially a package of code and dependencies required to run that code (Ex: Node.js code + Node.js runtime)_.
  - Support for containers is built into modern operating systems.
- Docker is not concerned with the specific infrastructure and tech stack that an application is built on. Therefore, it provides the same development and deployment environment.
- Docker is based on OS system calls.
  - All the API that container-based virtual platforms like Docker is available in the OS.
- Docker enables us to quickly deploy and scale applications into any environment confidently.
### Why use Docker?
- To make the development and production environments the same.
- To make the development environments within a team/company the same.
- To prevent clashing of versions of tools between different projects.
### Docker vs Virtual Machine
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Docker/refImg/docker-vs-vm.png" alt="Docker vs Virtual Machine" width="80%" /><br/>
  <em>https://www.docker.com/resources/what-container/</em>
</p>

- Docker can be described as a lightweight virtual machine.
  - A container is not a full OS. It only includes the OS processes and dependencies that are necessary to execute the code.
  - Containers are measured in MBs while VMs are measured in GBs.
- A containerized application can be written just once and run anywhere, unlike on VMs.
- VMs emulate hardware as software on top of an existing OS/hypervisor, run an OS on it, and run a process on that OS.
  - cf. containers do not emulate hardware and instead, run a process as though it is in an isolated environment.
    - Therefore, a container is merely another process in the host OS. The container does not run on top of Docker. It runs at the same level as other processes in the host OS.
- Docker provides all the functionality and benefits of VMs, including application isolation, cost-effective scalability, and disposability.
#### Virtualization vs Containerization
<p align="center">
  <img src="https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F14409324-6525-49f9-85b5-ea416d4efffb_2556x1383.jpeg" alt="Virtualization vs Containerization" width="100%" />
</p>

### Docker Image
> A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings. | Docker

- **A Docker image is a read-only file that contains everything that the application code needs to run as a container.**
- _Running a Docker image creates an instance of a container._
- A Docker image is made up of layers where each layer is a version of the image.
  - Making changes to the image results in the creation of another layer on top.
  - Creating a container from an image creates a top layer called the "container layer", which only exists while the container is running.
  - This allows multiple live container instances to run from a single base image.
- [Docker Hub - public repo of Docker images](https://hub.docker.com/).
#### Dockerfile
- A text-based script containing instructions that is used to create a Docker image.
### Docker Container
> A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. | Docker

- A Docker container is a live instance of a Docker image that can be interacted with.
  - Kind of like the application and process relationship.
  - Since containers are created using images, they have everything that the software needs to run (Ex: libraries, system tools, code, runtime).
- Containers do not rely on the computer environment and packages software to be environment-agnostic.
### Docker daemon
- The Docker daemon service creates and manages Docker images based on the client's commands.
  - Basically the control center of your Docker implementation.
- The Docker daemon runs on the server called the Docker host.

## How does Docker Work?
- Docker is based on a few key concepts.
### Namespace
- When processes are made, they are each given their allocated system resources.
- Some namespaces are shared by different processes (since processes are created through the `fork()` system call), and some are not accessible by other processes.
- Therefore, processes are isolated and are restricted from accessing other resources/areas of the system.
  - _Similar to what a hypervisor does for Virtual Machines._
### Control Groups(Cgroups)
- Virtualization capabilities.
- Cgroups decides how much resources to allocate among processes.
### `chroot`
- The `chroot` command is used to change the root directy for the currently running process and its children.
- `chroot` is used to isolate a process (and its children).
- Syntax
  ```zsh
  $ chroot [<options>] <new root> [command [arg]...]
  ```

## Getting Started with Docker
- Download (or create) a Docker image file.
- Build an image using a Dockerfile.
  - `docker build [options] <path or url to the files to be used for the build's context>`
  - Ex: `docker build -t <tagname> .` &rarr; build a docker image from the current directory and give the image a tagname.
- Create and run a new container from an image.
  - `docker run [options] <image> [command] [args]`
  - `docker run -d -p 80:80 --name <container name> <image tagname>` &rarr; build a container using the specified image.
    - `-p 80:80` - host's port 80 is mapped to container's port 80 (_port-forwarding_).
      - Port forwarding is needed to allow access from outside the container.
- Save and share the image
  - `docker tag <source image> <target image tag>` &rarr; create a tag that erfers to the source image.
  - `docker push <target image>` &rarr; push to Docker Hub

## Setting up MySQL 8 on Docker
- **Pull MySQL image from Docker Hub**
  ```zsh
  $ docker pull mysql:8
  ```
  - Check docker images
    ```zsh
    $ docker images
    ```
- **Create and run a new MySQL container, and set password.**
  ```zsh
  $ docker run --name <container name> -d -e MYSQL_ROOT_PASSWORD=<some password> -p 3306:3306 mysql:8
  ```
  - Check all containers.
    ```zsh
    $ docker ps -a
    ```
  - Check logs of a container.
    ```zsh
    $ docker logs <container name>
    ```
  - Setup volumes to persist data.
- **Create a new Docker volume to be mounted into the container.**
  - This is where MySQL will store its data files.
  ```zsh
  $ docker run --name <container name> -d -e MYSQL_ROOT_PASSWORD=<some password> -p 3306:3306 -v mysql:/var/lib/<volume name> mysql:8
  ```
  - Destroy the volume
    ```zsh
    $ docker volume rm <volume name>
    ```
- **Start, stop, restart container.**
  ```zsh
  $ docker start <container name>
  ```
  ```zsh
  $ docker stop <container name>
  ```
  ```zsh
  $ docker restart <container name>
  ```
- **Access the container through bash and log in to mysql.**
  ```zsh
  $ docker exec -it <container name> bash
  ```
  ```zsh
  bash-4.4# mysql -u root -p
  ```
  - Alternative
    ```zsh
    $ docker exec -it <container name> mysql -p
    ```

## Reference
[What is a Container? | Docker](https://www.docker.com/resources/what-container/)  
[What is Docker? | IBM](https://www.ibm.com/topics/docker)  
[What are the differences between Virtualization (VMware) and Containerization (Docker)?](https://blog.bytebytego.com/p/what-are-the-differences-between)  
[컨테이너 기초 - chroot를 사용한 프로세스의 루트 디렉터리 격리 | 44BITS](https://www.44bits.io/ko/post/change-root-directory-by-using-chroot)  
[How to Use Docker for Your MySQL Database - Earthly Blog](https://earthly.dev/blog/docker-mysql/#:~:text=With%20Docker%2C%20you%20can%20run,your%20database%20from%20your%20code.)  
[Docker를 사용하여 MySQL 설치하고 접속하기 | PoiemaWeb](https://poiemaweb.com/docker-mysql)  
