---
title: Installation
type: docs
---

# Installation 

There are many ways **OpenUSP** can be installed on local server or on the cloud (e.g. GCP, AWS etc.). To install on a local server, you can use Docker Compose utility to install and run all the components in one shot.

## USP Controller and other modules as docker containers

### Install Docker
Please refer to the following link to install docker on your local server.

https://docs.docker.com/engine/install/

### Clone the github repo to your local machine
```
git clone --recursive git@github.com:n4-networks/openusp
cd openusp

```
### Install and Start Services

```
docker compose -f deployments/docker-compose.yaml up -d

```
With the above commands the following actions would take place:

1. All the 3rd party docker images (e.g. ActiveMQ, Mongo, Redis Cache etc.) are fetched (only for the first time) from docker hub.
2. Pre-built Controller, API Server and  Agent docker images are fetched (only for the first time).
3. A docker network bridge named "openusp" is created with gateway IP as 10.5.0.1 and IP range of 10.5.0.0/16
4. Containers are created for all the Services and are connected to "openusp" bridge network
5. Services have been started

If everything is alright you would see the bellow on your terminal:

```
[+] Running 6/6
 ✔ Container openusp-cache       Running  0.0s 
 ✔ Container openusp-broker      Running  0.0s 
 ✔ Container openusp-agent       Running  0.0s 
 ✔ Container openusp-db          Healthy  1.0s 
 ✔ Container openusp-controller  Running  0.0s 
 ✔ Container openusp-apiserver   Running  0.0s
```

## CLI

To run CLI use the following command:

```
docker compose -f deployment/docker-compose.yaml run --rm openusp-cli

```
The above docker compose command would fetch pre-built OpenUSP CLI from docker hub. Create CLI container and run it in iterative mode.

If everything goes fine you would be prompted `` OpenUSP-Cli>> `` on the screen. Please follow [Operation](https://docs.openusp.io/docs/operation) link to know more about how to operate on OpenUSP platform.


## Validation

To see if all the services are working fine or to know the status you can use the following commands:

```
source ./scripts/bash/aliases.sh
dc ps

dc top

```
To see container logs

```
dc logs openusp-controller
dc logs openusp-agent
dc logs openusp-apiserver

```

