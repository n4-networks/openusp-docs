# What is OpenUSP
**OpenUSP** is an open source implementation of Broadband Forum's Universal Services Platform (USP). It consists of a Controller, Database, Message Broker, Command Line Interface (Cli) and REST ApiServer.

# Licenses
The project is licensed under "Apache License 2.0" which means you are free to use for your private or commercial purpose. Please read "Apache License 2.0" terms and conditions for more details




# Installation and Run

## USP Controller and other modules as docker containers
```
git clone git@github.com:n4-networks/openusp
docker compose -f deployments/docker-compose.yaml up -d

```
## OBUSP Agent
```
git clone git@github.com:BroadbandForum/obuspa.git
```
Follow instruction from [here](https://github.com/BroadbandForum/obuspa/blob/master/QUICK_START_GUIDE.md) to build OBUSPA agent
Edit factory_reset_example.txt:
1. Change 
```
Device.LocalAgent.Controller.1.EndpointID "self::usp-controller.com" to Device.LocalAgent.Controller.1.EndpointID "self::usp-controller"
Device.STOMP.Connection.1.Host "10.5.0.3"
Device.STOMP.Connection.1.Username "admin"
Device.STOMP.Connection.1.Password "admin"
```
## CLI

```
docker run --env-file configs/openusp.env --network=openusp -it n4networks/openusp-cli

```

# Components / Modules
OpenUSP has following components/modules to operate:
1. Controller
2. Database
3. Message Broker
4. API Server
5. CLI
6. UI (In Progress)



