---
title: Implementation 
type: docs
---

# Implementation

# **OpenUSP** Components

## USP Agent

OpenUSP uses OBUSP Agent from Broadband Forum. The Open Source Broadband Forum's USP Agent (OBUSPA) is a lightweight implementation of USP Agent. It is written in 'C'. The latest version of OPUSPA supports MQTT, STOMP and WebSocket as Message Transfer Protocol (MTP) to interact with USP Controller. 

Agent needs to be ported on Devices and should be embedded with device software/firmeware. OpenUSP.IO group members have ported it onto OpenWRT which is the most popular device middleware fraemwork in the Router, Gateway segment.

## Controller

The heart of the USP is a controller which uses MQTT, STOMP and WebSocket protocols on the south bound interface to interact with Device Agent component remotely through a message broker in case of MQTT and STOMP and directly with the Agent in P2P mode. 

Controller is written in golang.

## Message Broker

For MQTT and STOMP OpenUSP uses Apache ActiveMQ as a message broker. 

## Database

OpenUSP stores all the device data in a NoSQL database (mongo as of today) and uses APIs (DB package) store and retrieve device information and data.

## API Server

On the North Bound Interface (NBI), OpenUSP has an HTTP/S server written in golang. API server provides RESTful APIs to manage all device data model objects (Parameters, Instances, Notifications etc.).

Any 3rd party device manager or network management softtware (NMS) can operate on device data objects through supported REST APIs. 

## Command Line Interface (CLI)

CLI module is again written in golang and connects to API server to invoke CRUD operations to operate on Datamodels and network elements. 

