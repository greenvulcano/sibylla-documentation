---
title: ""
permalink: /docs/installation-on-premise/
toc: true
---

# Installation On Premise

## Ubuntu 18.04 64 bit - Single machine (on premise)

### Hardware requirements

To install sibylla on your system you need at least:
- 4 core CPU
- 16 GB RAM
- 256 GB of disk space

### Software requirements

To install sibylla on your system you need to install:
- JRE 11 (commercial or open)
- Node

### Download the sibylla installation package (community edition):

Login into the machine as user: sibylla (group sibylla).

Get the product from here and place in the parent directory you want sibylla to be installed:
- cd <parent directory of the sibylla product> (Suggested: the home directory of the sibylla user)
- wget https://github.com/green-vulcano/releases/download/sibylla/sibylla-1.0.tar

### Prepare the directory structure

Login into the machine as user: sibylla (group sibylla).

From the parent directory where you want to install sibylla, untar the sibylla package:
- tar xzvf sibylla-1.0.tar

This command above creates a directories with this structure:

```
Project root:
- sibylla

Product and scripts:
- sibylla/downloads
- sibylla/scripts

Application:
- sibylla/sibylla-console
- sibylla/sibylla-core

Networks:
- mkdir sibylla/sibylla-network-1 (hello world)
- ... other networks will be installed separately
```

### Set the environment

Modify the sibylla/scripts/set-environment.source-me to meet your environment:
- vi sibylla/scripts/set-environment.source-me
  - Change this ENV variable
    - export SIBYL_HOME=<The directory where sibylla has been installed>

Load the sibylla environment at login:
- vi .bashrc (or equivalent)
  - add this line at the end of the script
    - . <path to the sibylla directory>/scripts/set-environment.source-me
- Try the new setting: logout and login
  - NOTE: At this stage some aliases defined by the set-environment.source-me will not work because the installation
          procedure is not complete yet

### Download these third parties software (portable option)

You can skip any of these steps if you already have these components installed somewhere else, as for example in the
case MySql is installed externally on a different machine or as a service on the same machine. In this case make sure
you have full control of these components and save the connection parameters somewhere, because you will need the
connection information.

The procedure that follows consider the "portable application" approach:
- [https://en.wikipedia.org/wiki/Portable_application]

Create the 3rd parties directory structure:
- mkdir $SIBYL_HOME/3rd-party
- mkdir $SIBYL_HOME/3rd-party/db-domain
- mkdir $SIBYL_HOME/3rd-party/mqtt-domain
- mkdir $SIBYL_HOME/3rd-party/jms-domain
- mkdir $SIBYL_HOME/3rd-party/mongo-domain
- mkdir $SIBYL_HOME/3rd-party/kafka-domain
- mkdir $SIBYL_HOME/3rd-party/metabase

Download the packages into this directory: $SIBYL_HOME/downloads

- Mysql
  - [https://dev.mysql.com/downloads/mysql]
    - Linux generic - 64 bits
      - https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.15-linux-glibc2.12-x86_64.tar.xz
- Mongo
  - https://www.mongodb.com/download-center/community
    - 4.0.6 - Ubuntu 18.04 64 bits - tgz
      - https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu1804-4.0.6.tgz
- Kafka
  - https://kafka.apache.org/downloads
    - 2.1.1 - Scala 2.12 - kafka_2.12-2.1.1.tgz (asc, sha512)
      - https://www.apache.org/dyn/closer.cgi?path=/kafka/2.1.1/kafka_2.12-2.1.1.tgz
- ActiveMQ
  - http://activemq.apache.org/download.html
    - Unix/Linux/Cygwin Distribution
      - http://www.apache.org/dyn/closer.cgi?filename=/activemq/5.15.8/apache-activemq-5.15.8-bin.tar.gz&action=download
- EMQTT
  - https://www.emqx.io/downloads/emq/broker
    - Linux - 3.1* - Ubuntu 18.04 - Zip
      - https://www.emqx.io/downloads/broker/v3.1-beta.2/emqx-ubuntu18.04-v3.1-beta.2.zip
- Metabase
  - https://www.metabase.com
    - Custom install
      - http://downloads.metabase.com/v0.31.2/metabase.jar

### Install third party software (portable option)

This part of the procedure will prepare and test all 3rd party software.

#### MySQL

Prepare the directory stucture for MySQL:
- mkdir $SIBYL_HOME/3rd-party/db-domain/bin
- mkdir $SIBYL_HOME/3rd-party/db-domain/data
- mkdir $SIBYL_HOME/3rd-party/db-domain/config
- mkdir $SIBYL_HOME/3rd-party/db-domain/scripts
- mkdir $SIBYL_HOME/3rd-party/db-domain/logs
- mkdir $SIBYL_HOME/3rd-party/db-domain/tmp

Untar mysql into the sibylla/3rd-party/db-domain/bin directory:
- cd $SIBYL_HOME/3rd-party/db-domain/bin
- tar xzvf $SIBYL_HOME/downloads/mysql-8.0.15-linux-glibc2.12-x86_64.tar.xz
- ln -s mysql-8.0.15-linux-glibc2.12-x86_64 mysql

Initialize the DB:
- In case of doubts follow the official procudure defined at:
  - https://dev.mysql.com/doc/refman/8.0/en/binary-installation.html
- cd $SIBYL_HOME/3rd-party/db-domain
- vi config/sibylla.cnf (or set any database option you prefer)
  ```
  [mysqld]
  sql_mode = NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
  basedir = "<changeit: path to Sibyl>/3rd-party/db-domain/bin/mysql"
  datadir = "<changeit: path to Sibyl>/3rd-party/db-domain/data"
  log-error = "<changeit: path to Sibyl>/3rd-party/db-domain/logs/error_log.err"

  port = "3306"

  socket = <changeit: path to Sibyl>/3rd-party/db-domain/tmp/mysql_sibylla.socket
  pid-file = <changeit: path to Sibyl>/3rd-party/db-domain/tmp/mysql_sibylla.pid
  
  # Other parameters can be added here
  ```
- `bin/mysql/bin/mysqld --defaults-file=./config/sibylla.cnf --initialize`
- `bin/mysql/bin/mysql_ssl_rsa_setup --defaults-file=./config/sibylla.cnf`
- `bin/mysql/bin/mysqld_safe --defaults-file=./config/sibylla.cnf &`
- `bin/mysql/bin/mysqld --defaults-file=./config/sibylla.cnf &`

Change password:
- A root autogenerated password has been create by the "initialize" command
  - grep password ./logs/error_log.err
    > Example: 2019-03-08T22:36:20.410102Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: #=heSyL3dfTF
  - bin/mysql/bin/mysql -u root -p
    > ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
    > CREATE USER 'sibylla' IDENTIFIED BY 'sibylla';
    > CREATE SCHEMA sibylla;
    > GRANT ALL PRIVILEGES ON sibylla.* TO 'sibylla'@'%';

Start/stop MySQL:
- cd $SIBYL_HOME/3rd-party/db-domain
- bin/mysql/bin/mysqld_safe --defaults-file=./config/sibylla.cnf --user=sibylla &
- bin/mysql/bin/mysqladmin -u root -p shutdown (STOP needs a password)

Copy the start/stop script for MySQL:
- cd $SIBYL_HOME/3rd-party/db-domain
- cp $SIBYL_HOME/scripts/mysql/scripts/start.sh scripts
- cp $SIBYL_HOME/scripts/mysql/scripts/stop.sh scripts

#### Mongo

Prepare the directory stucture:
- mkdir $SIBYL_HOME/3rd-party/xxx

#### Kafka

Prepare the directory stucture for the new broker:
- mkdir $SIBYL_HOME/3rd-party/sibylla-kafka-a

NOTE: Naming convention for broker.id and directory names:
- Machine 1
  - Vertical scalability
    - sibylla-kafka-a - broker.id = 10
    - sibylla-kafka-b - broker.id = 11
    - sibylla-kafka-c - broker.id = 12
- Machine 2
  - Vertical scalability
    - sibylla-kafka-a - broker.id = 20
    - sibylla-kafka-b - broker.id = 21
    - sibylla-kafka-c - broker.id = 22
- Machine ...
  - Vertical scalability
    - sibylla-kafka-a - broker.id = 30
    - sibylla-kafka-b - broker.id = 31
    - sibylla-kafka-c - broker.id = 32
- Machine n
  - Vertical scalability
    - sibylla-kafka-a - broker.id = <n>0 (example for machine 120 - broker.id = 1200)
    - sibylla-kafka-b - broker.id = <n>1 (example for machine 120 - broker.id = 1201)
    - sibylla-kafka-c - broker.id = <n>2 (example for machine 120 - broker.id = 1202)

And now answer this question: who has ever thought to a name convention that can scale up to infinite?

- cd $SIBYL_HOME/3rd-party/sibylla-kafka-a
- tar xzvf $SIBYL_HOME/downloads/kafka_2.12-2.1.1.tgz
- ln -s kafka_2.12-2.1.1 kafka

Configure Kafka configuration:

- cd kafka
- vi TBF

#### ActiveMQ

Prepare the directory stucture:
- mkdir $SIBYL_HOME/3rd-party/jms-domain/jms-main
- cd $SIBYL_HOME/3rd-party/jms-domain/jms-main
- tar xvzf $SIBYL_HOME/downloads/apache-activemq-5.15.8-bin.tar.gz
- ln -s apache-activemq-5.15.8 activemq

- Remove the transportConnector with name="amqp"
  - cd activemq/conf
  - vi activemq.xml
    - Remove the line with the amqp transportConnector
    ```
    <transportConnector name="amqp" uri="amqp://0.0.0.0:5672?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>
    ```
    - Set autopurge of the destination with no consumers after long time (NOTE: Only if used for non persistence scenario)
      - Replace the row
        ```
        <broker xmlns="http://activemq.apache.org/schema/core" brokerName="localhost" dataDirectory="${activemq.data}">
        ```
      - With the following
        ```
        <broker xmlns="http://activemq.apache.org/schema/core" brokerName="localhost" persistent="false" schedulePeriodForDestinationPurge="3600000">
        ```
    - Queue autopurge (NOTE: Only if used for non persistence scenario)
      - Replace the row
        ```
        <policyEntry topic=">" >
        ```
      - With the following
        ```
        <policyEntry topic=">" gcInactiveDestinations="true" inactiveTimoutBeforeGC="600000" >
        ```

#### EMQTT

Prepare the directory stucture:
- mkdir $SIBYL_HOME/3rd-party/xxx

#### Metabase

Prepare the directory stucture:
- mkdir $SIBYL_HOME/3rd-party/xxx

### Configure the application

### Test the start and stop of the application

### Next reading: Getting started
