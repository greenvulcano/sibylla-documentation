---
title: ""
permalink: /docs/deep-intro-overview/
toc: true
---

# Deep intro - Overview

## Intro

To simplify the exposition of the GV IoT platform, in terms of what it is and how it addresses some of the top IoT issues (amount of data to elaborate, security, scalability, storage and analytics), we will describe the trip of a single measurement from Things to Humans and the back trip of a command from Humans to Things.
To make the narration of the trip more intuitive and concrete we will use a single use case described by the reference scenario below.
Since the narration will be done on a specific scenario, we don’t pretend to cover every single aspect of the IoT, or to cover all possibilities we have to solve all obstacles, but we strongly believe that the general understanding of the platform will be a lot better. Anyway on some situation we will refer to the other two scenarios.
During the trip we will often zoom in and out from details to high level architecture, to better under the the capabilities of the GV IoT platform.

Moreover, along the narration we will focus on these aspects of the GV IoT platform:
- Security aspects
- Traffic and scalability of the platform
- The 3M = Modularity, maintainability, monitorability 

We will also describe what choices we do have at each step. For example:
- Are the sensors directly connected to the internet or do we need a dedicated 4G communication?
- On the EdgeGateway if we have to transfer to the Gateway 100 GB per day, what protocol it is better to use
  - MQTT
  - json/http
  - Kafka 2 Kafka communication
  - Raw socket communication
- Do we have to send all the data or only an aggregate value?
- What about filtering and buffering?

## High level architecture

The architecture of the platform will be described in a specific chapter, but for now it is useful to keep it at hand
while we explain some aspects of the platform.

xxx Disegno architettura

## From Things to Humans

xxx Disegno

We will describe all the steps that a single measure will do from Things to Humans:
- Starting point - the Things
  - We will describe how a sensor detects a measure (ex. the temperature of a room at a “T” time), how the measurement is read (raw value) on the sensor and pre-formatted into a digital value (ex. by the firmware and HW on the sensor)
- EdgeGateway: sends the data through the internet to the Gateway
  - We don’t want to spoil :-) Read the chapter that tells the whole story
- Internet: The data crosses the internet
- Enter the Gateway
- Buffering / decoupling (store the raw data into an intermediate storage)
- DataPump from the buffer to the data lake
- DataLake & Streaming
- Analytics
- End point: the Humans

## From Humans to Things

xxx Disegno

We will describe all the steps a single measure will do from Humans to Things:
- Starting point - the Humans
  - We will describe what a person can do to act on a Thing: open a door, turn on the heat, turn off lights, etc.
- The console
  - We don’t want to spoil :-) Read the chapter that tells the whole story
- The core
- Dispatch commands
- The Gateway receive the command
- Internet: The command crosses the internet
- The EdgeGateway receive the command
- End point: the Things

## Tunnels monitoring scenario

Foto tunnel

The scenario consists in monitoring the health of a tunnel, in term of structural deformations that may damage the tunnel itself and put Humans in danger.
Natural causes that affect the structure of a tunnel:
- Landslides
- Earthquakes
- Wind
- Infiltrations
- Temperature
- Etc.

Human causes that affect the structure of a tunnel:
- Traffic
- Heavy vehicles
- Accidents
- Etc.

But how do you actually prepare a tunnel to be monitored for deformations.

We can use a FS22 Industrial BraggMETER (picture 1) and wire the entire tunnel with the fibre cable (picture 2) and
strain sensors (picture 3).

Source: NTSG Val di Sambro: “3 lines of sensors have been installed along the whole tunnel, while the thermal sensors
have been installed at distances previously set. This to compensate the effects, on the readings, of thermal variations
and to obtain a pure mechanical deformation. It is possible to control the longitudinal movements of the tunnel, and
verify if the tunnel keeps the initial shape as designed.”

- Number of sensors: 780
- Sampling rate: 10 Hz
- Wiring: 30 km of optical fiber
- Packet dimension: 6 bytes (single sensor) - 30 bytes header for all
- PLE: 4 (working platform, lifting)
- Working time: 24/24h, 365d/year

We have:
- 780 sensors * 10 Hz * 10 bytes * 60 seconds * 60 minutes * 24 hours
- ~46 Kb per second
- 161,7 MB per hour
- 3,78 GB per day
- 10 messages (~4,6 kb each message) per second to send over the internet

Many information about the IoT technology can be found here: [HBM](https://www.hbm.com/en).

xxx Tabella foto HBM, sensori, etc.

The picture 4 of the BraggMONITOR application (window application that connects via LAN to the Industrial BraggMETER)
shows the signals from the strain sensors connected to the Industrial BraggMETER, that in this case (see picture 1) has
four fibre cables doors.

xxx Tabella foto elementi in galleria