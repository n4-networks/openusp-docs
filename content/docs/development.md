---
title: Development 
type: docs
---

# Development

If you want to contribute to the project, you are very much welcome. The follow the below instruction would help you to set up a development environment.

## Dependencies and tools

OpenUSP platform consists of Controller, API Server and CLI written in golang by OpenUSP contributers. Rest of the open source components, Mongo, ApacheMQ, OBUSPA are used from their respective projects/organisations.

### Golang

Controller, API Server and CLI have been written in golang. Hence you need to install latest from https://go.dev/doc/install

### Make

GNU Make is used to build all golang modules. There are other ways to build without make as well which is described later in this document.

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

To know more about make you can refer to [GNU Make](https://www.gnu.org/software/make/).

### Docker

Please refer to https://docs.docker.com/engine/install/ to install Docker on your local machine.

## Build
You can build the binaries using make or "go build" tool. You can also use docker compose to build images from the source.

### Building Docker images

Docker images are built through docker compose by passing controller, apiserver, cli and obuspa Dockerfile from build directory.

```
docker compose -f deployment/docker-compose.yaml build

```

### Building binaries

If you want to build applications individually please follow these

```
cd openusp
make clean
make

```
The top level make would build all golang packages and service binaries at "./cmd/controller, ./cmd/apiserver, ./cmd/cli" folders.
You can also use `` go build `` or `` make `` from these directories to build indivivisual binaries.


## Run

```
docker compose -f deployment/docker-compose.yaml up -d

```

Once all the services are up and running, you can run CLI either as a container or as an application

### CLI as a container

```
docker compose -f deployment/docker-compose.yaml run --rm openusp-cli

```

It should give you `` OpenUSP-Cli>> `` prompt on your terminal. Issue "help" from the prompt to see a list of supported commands.

## Aliases

```
source scripts/bash/aliases.sh
alias

```
Now you can use "dc" instead of "docker compose -f deployment/docker-compose.yaml" everywhere.
