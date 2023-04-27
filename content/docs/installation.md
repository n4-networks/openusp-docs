---
title: Installation
type: docs
---

# Installation 

There are many ways **OpenUSP** can be installed on local server or on the cloud (e.g. GCP, AWS etc.). To start with you can use Docker Compose utility to install and run all the components in one shot.

## USP Controller and other modules as docker containers

### Dependencies and tools
OpenUSP consists of Controller, API Server and CLI written in golang by OpenUSP contributers. Rest of the open source components, Mongo, ApacheMQ, OBUSPA are used from their respective projects/organisations.

To build Controller, API Server and CLI you would need to install [golang](https://go.dev) and [Make](https://www.gnu.org/software/make/) utility.

Please follow the following links to install these:

1. Golang: https://go.dev/doc/install
2. Make utility:
{{< tabs "makeinstall" >}}

{{< tab "Ubuntu" >}}
```
sudo apt install make
or
sudo apt-get install make
```
{{< /tab >}}

{{< tab "Fedora" >}}
```
yum install make
```
{{< /tab >}}
{{</tabs>}}


3. Docker: https://docs.docker.com/engine/install/


### Clone the github repo to your local machine
```
git clone git@github.com:n4-networks/openusp
cd openusp
```
### Build and Run
From the top level directory (openusp) you can invoke make to build all the components (Controller, API Server, CLI packages and binaries)
or you can use the below command to build and run using Docker compose

```
docker compose -f deployments/docker-compose.yaml up -d

```
With the above commands the following actions would take place:

1. All the 3rd party docker images (e.g. ActiveMQ, Mongo, Redis Cache etc.) would be fetched from docker hub.
2. Controller, API Server and CLI docker images would be built locally from your clone directory.
3. A docker network bridge named "openusp" would be created with gateway IP as 10.5.0.1 and IP range of 10.5.0.0/16
4. All the containers would be created and run locally, they would be connected to "openusp" network

## OBUSP Agent

Open Broadband USP Agent (OBUSPA) is provided by Broadband forum and can be found at https://github.com/BroadbandForum/obuspa/.

```
git clone git@github.com:BroadbandForum/obuspa.git
```
Follow instruction from [here](https://github.com/BroadbandForum/obuspa/blob/master/QUICK_START_GUIDE.md) to build OBUSP Agent.
Edit factory_reset_example.txt and change the following agent parameters:

```
Device.LocalAgent.Controller.1.EndpointID "self::usp-controller.com" to Device.LocalAgent.Controller.1.EndpointID "self::usp-controller"
Device.STOMP.Connection.1.Host "10.5.0.3"
Device.STOMP.Connection.1.Username "admin"
Device.STOMP.Connection.1.Password "admin"
```

Here ``Device.STOMP.Connection.1.Host`` parameter represents IP address / DNS name of Message Broker (ActiveMQ in this case). ActiveMQ server container has already been created through the above docker compose file.

## CLI

```
docker run --env-file configs/openusp.env --network=openusp -it n4networks/openusp-cli

```
The above docker command would create CLI container from the image built earlier through above docker compose command. It would also run the container in interactive mode.
The env file has few ENVIRONMENT variables to connect CLI to the API Server and the Controller.


For any help, please feel free to contact [support@openusp.io](mailto:support@openusp.io).
