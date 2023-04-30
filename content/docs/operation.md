---
title: Operation 
type: docs
---

# Operation 

If you have come to Operation page, we assume you are able to install and run all the containers successfully. If so, we appreciate your effort and time and if not, please feel free to contact [support@openusp.io](mailto:support@openusp.io). We will be happy to help for sure.

Let's talk about how to run the CLI to interact with OpenUSP. CLI (Command Line Interface) talks to API Server to get and set all availiable data. As mentioned earlier, API Server talks to Controller through gRPC and Controller interacts to OBUSP Agent through supported MTP (Message Transfer Protocol). 

## Invoke OpenUSP CLI
Since CLI needs to connect to API Server it needs to know the DNS name or IP address of the OpenUSP API Server.

```
API_SERVER_ADDR=http://openusp-apiserver:8081
AGENT_ID=os::012345-525400C8712E
HISTORY_FILENAME=history
LOGGING=all

```
Out of these only the ``API_SERVER_ADDR`` is mandatory and rest can be set through CLI after it is invoked.
** API_SERVER_ADDR: ** OpenUSP API server address
** AGENT_ID: ** CLI can talk to one agent at a time even though API Server and Controller can handle multiple CLI and Agents at the same time. This parameter sets the Agent endpoint ID in its context. If you don't pass it as an environment variable you will not be able to talk to the agent. However you will be able to run certain commands without this parameter set.
** HISTORY_FILENAME: ** This is an optional parameter to store all executed commands. OpenUSP CLI supports history like bash.
** LOGGING: ** This is again an optiontial parameter to switch on/off verbose context of CLI. Default is set to all.

You can pass these environment variables through command line or through file as given below

```
docker run --env-file configs/openusp.env --network=openusp -it n4networks/openusp-cli

```
If everything goes fine as expected you would see ``OpenUSP-Cli>>`` prompt on your terminal.

## CLI Help

Once you get CLI prompt, the first thing you could do is to run help:

### help
```
OpenUsp-Cli>> help

Commands:
  add            add     bridging|devinfo|dhcpv4|ip|nat|wifi|instance|...
  addcfg         addcfg  bridging|dhcpv4|ip|nat|wifi|...
  clear          clear the screen
  exit           exit the program
  help           display help
  operate        operate    bridging|command|device|devinfo|ip|wifi|param|instance|...
  reconnect      reconnect  db|mtp|stomp|...
  remove         remove     bridging|db|devinfo|dhcpv4|history|ip|nat|stomp|wifi|param|instance|...
  removecfg      removecfg  bridging|dhcpv4|ip|nat|wifi|...
  set            set        agent|devinfo|bridging|history|ip|logging|nat|wifi|param|...
  setcfg         setcfg     bridging|devinfo|ip|nat|wifi|...
  show           show       agent|bridging|devinfo|eth|dhcpv4|history|ip|logging|nat|nw|wifi|datamodel|param|instance|version|...
  showcfg        showcfg    bridging|devinfo|eth|dhcpv4|ip|nat|wifi|...
  unset          unset      agent|...
  update         update     bridging|dhcpv4|ip|nat|wifi|datamodel|param|instance|...


OpenUsp-Cli>>  
```

** Many of these commands need the agent to support datamodel. The [OB-USP-AGENT](https://github.com/BroadbandForum/obuspa) used here is a reference implemenation of USP specification. It supports very basic datamodel. **


### show help

```
penUsp-Cli>> show help

show    agent|bridging|devinfo|eth|dhcpv4|history|ip|logging|nat|nw|wifi|datamodel|param|instance|version|...

Commands:
  agent          show agent <cli-id|all-ids|mtp|threshold|controller|subsription|request|cert|controller-trust> [id] <coap|stomp|websocket|mqtt|mtp|boot-params|e2e|roles|creds|challenges> [id]
  bridging       show bridging <bridge|filter|provbridge|> [id] <port|vlan|vlanport> [id] <stats|pricodepoint>
  datamodel      show datamodel <path>
  device         show device
  devinfo        show devinfo <vendorcfg|memory|proc|temp|net|cpu|logfile|loc|imagefile|firmware> <id>
  dhcpv4         show dhcpv4 server <pool> [id|name] <option|client|staticaddr> [id|name]
  eth            show eth <intf|link|vlanterm|rmonstats|wol|lag> [id] <stats>
  history        show history settings
  instance       show instance <path|objname>
  ip             show ip <intf|port|twmp> [id] <stats|ipv4addr|ipv6addr|ipv6prefix> [id]
  logging        show logging
  mtp            show mtp
  nat            show nat <intf-setting|port-mapping> [id|name]
  nw             show nw wired|wireless
  param          show param <path>
  stomp          show stomp
  time           show time
  version        show version
  wifi           show wifi <ssid|radio|accesspoint|endpoint> [id] <stats>


OpenUsp-Cli>> 

```

### show agent

Show agent prints agent related information. 
```
OpenUsp-Cli>> show agent help

show agent <cli-id|all-ids|mtp|threshold|controller|subsription|request|cert|controller-trust> [id] <coap|stomp|websocket|mqtt|mtp|boot-params|e2e|roles|creds|challenges> [id]
```

#### show agent cli-id

CLI operates on one agent at a time. It fetches all data based on the agent endpoint ID set either through CLI comand "set" or through env variable while invoking the CLI. This can as well be changed to different id from CLI prompt: `` set agent id `` 

`` show agent cli-id `` prints the agent endpoint it would be using for all the commands while fetching data from OpenUSP controller.

```
OpenUsp-Cli>> show agent cli-id
Cli Agent EndpointId      : os::012345-525400C8712E
OpenUsp-Cli>>
```
#### show agent all-ids

It prints all the agents connected to OpenUSP controller. 

```
OpenUsp-Cli>> show agent all-ids
Agent Number              : 1           
EndpointId                : os::012345-525400C8712E
-------------------------------------------------
OpenUsp-Cli>>  
```

#### show agent *

`` show agent `` displays agent data model related parmeters, functions o

```
OpenUsp-Cli>> show agent
Object Path               : Device.LocalAgent.
 UpTime                   : 69170       
 SupportedProtocols       : STOMP, CoAP, MQTT, WebSocket
 SoftwareVersion          : 7.0.2       
 EndpointID               : os::012345-525400C8712E
 CertificateNumberOfEntries : 0           
 SupportedFingerprintAlgorithms : SHA-1, SHA-224, SHA-256, SHA-384, SHA-512
 ControllerNumberOfEntries : 1           
 MTPNumberOfEntries       : 1           
 SubscriptionNumberOfEntries : 0           
 RequestNumberOfEntries   : 0           
-------------------------------------------------
```

Here are two more examples on how to show more information using show command

```
OpenUsp-Cli>> show agent mtp
Object Path               : Device.LocalAgent.MTP.1.
 Alias                    : cpe-1       
 Protocol                 : STOMP       
 Enable                   : true        
 Status                   : Up          
-------------------------------------------------
OpenUsp-Cli>> show agent mtp 1 stomp
Object Path               : Device.LocalAgent.MTP.1.STOMP.
 Reference                : Device.STOMP.Connection.1
 Destination              : /queue/agent-1
 DestinationFromServer    :             
-------------------------------------------------
```

