---
layout: post
title:  Azure IOT Hubs vs Event Hubs - What are the differences?
date:   2020-01-13 00:01:12 +0530
description: Quick summary on the differences between IoT Hubs vs Event Hubs
header-style: text
header-img: "/img/headers/iot-vs-event-hubs.png"
tags: 
- Azure IoT Hubs
- Azure Event Hubs
---

Both Event Hub and IOT Hub are used for stream ingestion.

So when do we use which?  
Below is a quick summary:

---

### Difference #1 : Communication
Event Hub: 1 way communication from devices / sources  
IoT Hub: Bi-directional communication between devices / sources

### Difference #2 : Protocol
Event Hub: Limited protocol support — HTTPS, AMQP, AMQP  
IoT Hub: Has additional — MQTT

### Difference #3: Number of connections
Event Hub: Supports only up to 5000 connections simultaneously  
IoT Hub: Supports > 5000 connections (Up to millions)

### Difference #4: Security
Event Hub: Event Hub — wide identity using Shared Access Token (SAS)  
Iot Hub: Each device has its own security credentials

---

Refer to the following links for more info:  
https://spr.com/azure-integration-part-ii-differences-event-hubs-iot-hubs-notification-hubs/
