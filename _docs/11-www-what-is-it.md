---
title: "What is it"
permalink: /docs/www-what-is-it/
toc: true
---

The **Sibylla IoT Platform** integrates IoT devices to enterprise or cloud systems.

One of the most important missions of the **Sibylla IoT Platform** is to make objects speak, with a strong orientation towards infrastructures such as bridges, tunnels, construction sites, buildings, wind turbines and in general all infrastructures that need maintenance and monitoring.

In general, understand from the huge amount of data that the objects communicate, the health of the object itself. To prevent, in this way, failures or problems.

![priority plus masthead animation]({{ "/assets/images/business-value.png" | relative_url }})

It allows the user to create and manage their own IoT ecosystem by facilitating
data flow, communication between objects, device management and, in general,
enabling advanced application functionalities.

For this reason we collaborate with companies that have solid background in monitoring infrastructures, with companies that produce sensors and with expert in the field of what has to be monitored.

Collaborating with physicists, geologists, construction engineers is what makes the difference in succeeding in these projects. We strongly believe that the sum of two competences produces more than their pure algebraic result. Only facing a problem from different perspectives and different backgrounds, in a interdisciplinary approach, permits to solve problems better and faster. 

Next picture describes what the platform does and how raw data from the objects are transormed by the platform into normalized and queryable data.
At this stage data are generally stored into BigData infrastructure and ready for analytics.

# Business value

It is in the last step within the platform that the business value is added:

- Alarms (static and dynamic)
  - If a temperature raises over a specified limit (ex. 0.5 degrees)
  - If an inclination goes over a specified limit (ex. 0.5 degrees)
  - If a vibration persists for a long period of time (peak on the 500 Heartz)
  - and many others
- Reports
  - The huge amount of data that may come from a network of sensors, can be represented and summarized into a report
    that can be send to the end user
- Prediction
  - "tomorrow it will not rain" :-) What about if we can do the same on the data coming from the sensors
    - The FAN#21 will propably stop working in 150 hours. Sibylla suggest a human inspection
    - The inclication of the Pylon#03 is incrising at a rate of 0.02 degree every week
- Automatic actions
- BlockChain certifications of the data transmitted: Source, destination and reports

# Features of each module of the platform

The **Sibylla IoT Platform** has been designed to solve many IoT needs. These is a list of its features:

* EdgeGateway
    * It is the component that "talks" to the real object that needs to be monitored and controlled
    * Extensible and flexible architecture
    * Simple architecture that permits to be easily integrable with the Gateway to access the IoT platform
* Gateway
    * Its architecture extends and facilitates edge computing
    * Data normalization
    * Filtering
    * Security gate
    * Support for multi protocol communication between the device and the platform
    * Data ingestion modules to elaborate huge amount of data
    * Auto provisioning for new Things and Devices. It can be enabled ON or OFF from the console
* Decoupling
    * Based on the Open Source Kafka infrastructure, that proved to be disruptive in many areas of application
    * Scalability and isolation for the entire platform
    * Back pressure regulation
    * Backbone for all internal communication (it is its circulatory system)
* DataPump
    * Security gate
    * Data enrichment
    * Filtering
    * Routing
    * Multi synch architecture, to support any type of store or streaming systems, like: Mongo, Cassandra, relational
      DBs, Hadoop, Spark, Confluent KSQL and many others
* Core
    * "API first" design for use in headless environments
    * Auditing at core API level: any operation on devices, things or networks (D/T/N) is storicized by the platform
    * Query module to get meta info from N/T/D and to get telemetry data from your devices
* Store
    * Any combination of Datalakes, BigData or streaming modules
* General characteristics
    * Built on Open Source technologies: Spring, Kafka, Mongo, MySql, MQTT, Angular, Node
    * Secured by design
    * Analytics, real time alerts and CEP
    * Machine learning
    * E2E traceability functionalities
    * Self monitoring modules
    * Events generated from D/T/N can trigger event notifications
* Console
    * Data navigation
    * Time series graphs on data coming from the devices
    * Frequency analysis
    * Georeferenced graphs and tables
        * Google maps, OpenStreet map, WebGIS
    * Integrated lightweight ticketing system
    * Integrated logbook for production groups, to log their activities during shifts
    * Integrated lightweight CMS (docs related to D/T/N: Devices/Things/Network)
* Business Intelligence modules
    * Alarm definition and management through CEP processing
    * Scheduled and recurring data analysis and report generation
    * Certifications of the sensed data using private of public Blockchain
    * Data analysis using machine learning OpenSource frameworks: SparkML, tensorflow
* Other
    * Integrate other systems through the Open Source GreenVulcano® ESB platform
    * Automatise your work through GreenVulcano® Claudio

# Top 3 features

<table>
  <tr>
    <td>Top 3 features</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>eXtreme scalability</td>
    <td>We have developed a platform that can be setup to manage any type of load. It can do so, through extreme scalability of any component of the platform.<br/><br/>From a single industrial computer on the field to a cluster of thousands of connected computer in the cloud, we can tailor the solution to best respond to your needs.</td>
  </tr>
  <tr>
    <td>Open API</td>
    <td>Any operation you can do from the console can be also done via API.<br/><br/>The entire platform follows the "API first approach".<br/><br/>Note: If the console, with its dashboards customizable by users, wouldn't be enough, we at GreenVulcano can rapidly implement vertical solution with a personalized console to operate your own installation of the **GV IoT platform**.</td>
  </tr>
  <tr>
    <td>Security by design</td>
    <td>Your data is what it is important. They are secured by design, using advanced access policies and protections at the gates (gateways) and on the data.</td>
  </tr>
</table>
