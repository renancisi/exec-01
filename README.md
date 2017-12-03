About this document
===================

As the documentation evolves with the orchestration of the containers:

* check if a subnet was created for the lab (explained below)
* check that the IPs of the containers hit the documentation in "prints".

# Table of Contents
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)

# Prerequisites

The fastest way to get running:

 * [install docker](https://docs.docker.com/installation/#installation)
 * [install docker-compose](https://docs.docker.com/installation/#installation)
 * create docker subnet(subnet-lab): `docker network create subnet-lab`
 
 # Quick start

The fastest way to get running:

 * run docker-compose: `docker-compose up`
 * go to jenkins: `https://localhost:8080 - admin/admin`
 * building the job: `build ftp-docker-project`

[The images used for the environment were prepared exclusively for this exercise.](https://hub.docker.com/u/rcisi/).


## Example compose

```yaml
version: "3"
services:
  lannexxerajenkins:
    build: ./jenkins
    ports:
      - 8080:8080
    networks:
      - subnet-lab
  lannexxerajava:
    build: ./java
    networks:
      - subnet-lab
  lannexxeraftp:
    build: ./ftp
    networks:
      - subnet-lab
networks:
  subnet-lab:
      external: true
```

# Documentation and reasons for the technologies used

By dealing only with the continuous delivery of deploys from the software factory, I chose the jenkins for the flexibility and high level of automation of their pipelines. Ex: For our model we used the following plugins:

- Publish Over FTP (Responsible for downloading / uploading our package, ensuring integrity at both ends, and verification of file extensions using FTP protocol).
- Publish Over SSH (Responsible for downloading / uploading our package, ensuring integrity at both ends, as well as file extension verification using SSH protocol).
- Docker Plugin (For the second step where instead of delivering the packages, we can feed a registry with containers already loading the software factory requirements / packages for the QA / Dev areas and, finally, production).

In addition to the jenkins exercising the CD function, I orchestrated with the compose plus two containers for simulation of the Java environment and external FTP of the supplier.

The build is with manual call but already with the daily call, as requested in the exercise.

I set up a framework that we can evolve as desired but to heal this demand, I have some constants such as the IP of the containers.
