---
title: ""
permalink: /docs/installation-deployment-options/
toc: true
---

# Deployment options

Choose the other deployment options to best fit your needs.

You can install the platform on a single node, but the Sibylla platform can scale up to thousands of nodes without problems.

A good installation scenario that also meets high availability requirements (with failover and
balancing) requires a minimum of three nodes and an ideal configuration that uses 16 nodes, where
each layer is on an isolated and dedicated instance.

![priority plus masthead animation]({{ "/assets/images/architecture.png" | relative_url }})

Each element of this architecture can be deployed on a separated group of machines.

- Gateways:
  - Depending on the communication protocol it uses to interact with the real object, it can be deployed on multiple
    machines (as many as needed) and scaled on a single machine as many times as the resources permit
- Kafka:
  - kafka at Linked in has been deployed on (source 2014):
    - 300> Karfka brokers
    - 18000> topics
    - 140000> partitions
    - 220 billion messages per day
    - 40 Tera bytes IN, 160 Terabytes OUT
    - Peak: 3.25 million messages per sec, 5.5 Gb/sec IN, 18 Gb/sec OUT
- Datapump:
  - Same scalability as for the Gatways: multiple machines and multiple instances per machine as needed
- Core:
  - The core is stateless and can be scaled on multiple machines and multiple instances per machine as needed
- Console:
  - It is a Angular/Node application and also this layer can be scaled on multiple machines and multiple instances per machine as needed