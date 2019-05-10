---
title: "Installation - On Premise"
permalink: /docs/installation-on-premise/
toc: true
---

## Ubuntu 18.04 64 bit - Single machine (on premise)

### Hardware requirements

To install sibylla on your system you need at least:
- 4 core CPU
- 16 GB RAM
- 256 GB of disk space

### Prerequisites

To install sibylla on your system you need to install:
- JRE 11 (commercial or open)
- node 11.2.0
- nginx (versione: _TBD_. As the entry point for the Sibylla platform: console, cores, gateways, ...)
- A user sibylla (group sibylla)
- Utilities: vi, wget, ...

### Download the sibylla installation package (community edition)

Login into the machine as user: sibylla (group sibylla).

Get the product from here and place in the parent directory you want sibylla to be installed:

`-$ cd <parent directory of the sibylla product> (Suggested: the home directory of the sibylla user)`

`-$ wget https://github.com/green-vulcano/releases/download/sibylla/sibylla-1.0.tar`

> **TBD**: il contenuto del file conterrà: sibylla + sottodirectories: scripts, core, console, n-1-...
> n-1: deve contenere, script start/stop, application, properties

### Prepare the directory structure

Login into the machine as user: sibylla (group sibylla).

From the parent directory where you want to install sibylla, untar the sibylla package:

`-$ tar xzvf sibylla-1.0.tar`


The command above creates a directory with the following structure:

```
Project root:
- sibylla

Products download area (it can be removed when installation is finished):
- sibylla/download

3rd party software:
- sibylla/3rd-party

Scripts:
- sibylla/scripts

Application:
- sibylla/sibylla-console
- sibylla/sibylla-core

Networks:
- sibylla/sibylla-network-1/sibylla-gateway-http (Generic JSON/HTTP)
- sibylla/sibylla-network-1/sibylla-gateway-mqtt (Generic JSON/MQTT)
- ... other networks will be installed separately
```

### Set the environment

Modify the sibylla/scripts/set-environment.source-me to meet your environment:

`-$ vi sibylla/scripts/set-environment.source-me`

Change this ENV variable

`-$ export SIBYL_HOME=<The directory where sibylla has been installed>`

Load the sibylla environment at login:

`-$ vi .bashrc (or equivalent)`

add this line at the end of the script

`-$ . <path to the sibylla directory>/scripts/set-environment.source-me`

Try the new setting: logout and login
> NOTE: At this stage some aliases defined by the set-environment.source-me will not work because the installation
>          procedure is not complete yet


### Download these third parties software (portable option)

You can skip any of these steps if you already have these components installed somewhere else, as for example in the
case MySql is installed externally on a different machine or as a service on the same machine. In this case make sure
you have full control of these components and save the connection parameters somewhere, because you will need the
connection information.

The procedure that follows consider the "portable application" approach:
- [Portable_application](https://en.wikipedia.org/wiki/Portable_application)

Create the 3rd parties directory structure:
- mkdir $SIBYLLA_HOME/3rd-party
- mkdir $SIBYLLA_HOME/3rd-party/db-domain
- mkdir $SIBYLLA_HOME/3rd-party/mqtt-domain
- mkdir $SIBYLLA_HOME/3rd-party/jms-domain
- mkdir $SIBYLLA_HOME/3rd-party/mongo-domain
- mkdir $SIBYLLA_HOME/3rd-party/kafka-domain
- mkdir $SIBYLLA_HOME/3rd-party/analytics-domain

Download the packages into this directory: $SIBYLLA_HOME/downloads

Package | Version | Download at
--------|---------|------------
[MySQL](https://dev.mysql.com/downloads/mysql) | 8.0 - Linux generic - 64 bits | [mysql-8.0.15-linux-glibc2.12-x86_64.tar.xz](https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.15-linux-glibc2.12-x86_64.tar.xz)
[Mongo](https://www.mongodb.com/download-center/community)| 4.0.6 - Ubuntu 18.04 64 bits - tgz| [mongodb-linux-x86_64-ubuntu1804-4.0.6.tgz](https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu1804-4.0.6.tgz)
[Kafka](https://kafka.apache.org/downloads)| 2.1.1 - Scala 2.12 - kafka_2.12-2.1.1.tgz (asc, sha512)| [kafka_2.12-2.1.1.tgz](http://it.apache.contactlab.it/kafka/2.1.1/kafka_2.12-2.1.1.tgz)
[ActiveMQ](http://activemq.apache.org/download.html)| Unix/Linux/Cygwin Distribution| [apache-activemq-5.15.9-bin.tar.gz](http://it.apache.contactlab.it//activemq/5.15.9/apache-activemq-5.15.9-bin.tar.gz)
[EMQTT](https://www.emqx.io/downloads/emq/broker)| Linux - 3.1* - Ubuntu 18.04 - Zip| [emqx-ubuntu18.04-v3.1-beta.2.zip](https://www.emqx.io/downloads/broker/v3.1-beta.2/emqx-ubuntu18.04-v3.1-beta.2.zip)
[Metabase](https://www.metabase.com)| Custom install - v0.31.2| [metabase.jar](http://downloads.metabase.com/v0.31.2/metabase.jar)

### Install third party software (portable option)

This part of the procedure will prepare and test all 3rd party software.

**MySQL**

Prepare the directory stucture for MySQL:

  `-$ mkdir $SIBYLLA_HOME/3rd-party/db-domain/bin`

  `-$ mkdir $SIBYLLA_HOME/3rd-party/db-domain/data`

  `-$ mkdir $SIBYLLA_HOME/3rd-party/db-domain/config`

  `-$ mkdir $SIBYLLA_HOME/3rd-party/db-domain/scripts`

  `-$ mkdir $SIBYLLA_HOME/3rd-party/db-domain/logs`

  `-$ mkdir $SIBYLLA_HOME/3rd-party/db-domain/tmp`

Untar mysql into the sibylla/3rd-party/db-domain/bin directory:

`-$ cd $SIBYLLA_HOME/3rd-party/db-domain/bin`

`-$ tar -xJvf $SIBYLLA_HOME/downloads/mysql-8.0.15-linux-glibc2.12-x86_64.tar.xz`

`-$ ln -s mysql-8.0.15-linux-glibc2.12-x86_64 mysql`


Initialize the DB:
In case of doubts follow the official procedure defined at [MySQL docs](https://dev.mysql.com/doc/refman/8.0/en/binary-installation.html).

`-$ cd $SIBYLLA_HOME/3rd-party/db-domain`

`-$ vi config/sibylla.cnf` (or set any additional database options you prefer)

```
  [mysqld]
  sql_mode = NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
  basedir = "<changeit: fullpath to Sibylla>/3rd-party/db-domain/bin/mysql"
  datadir = "<changeit: fullpath to Sibylla>/3rd-party/db-domain/data"
  log-error = "<changeit: fullpath to Sibylla>/3rd-party/db-domain/logs/mysql.log"

  port = "3306"

  # Other parameters can be added here
```

`-$ bin/mysql/bin/mysqld --defaults-file=./config/sibylla.cnf --initialize`

`-$ bin/mysql/bin/mysql_ssl_rsa_setup --defaults-file=./config/sibylla.cnf`

`-$ bin/mysql/bin/mysqld --defaults-file=./config/sibylla.cnf &`
> TBV: `-$ bin/mysql/bin/mysqld_safe --defaults-file=./config/sibylla.cnf &`

Change password:

A root autogenerated password has been create by the "initialize" command

`-$ grep password ./logs/error_log.err`
> Example:
> ```2019-03-08T22:36:20.410102Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: #=heSyL3dfTF```

`-$ bin/mysql/bin/mysql -u root -p`
```
> ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
> CREATE USER 'sibylla' IDENTIFIED BY 'sibylla';
> CREATE SCHEMA sibylla;
> GRANT ALL PRIVILEGES ON sibylla.* TO 'sibylla'@'%';
```

For commands:
```
> use sibylla;
> show tables;
```

Start/stop MySQL (without scripts):

`-$ cd $SIBYLLA_HOME/3rd-party/db-domain`

`-$ bin/mysql/bin/mysqld --defaults-file=./config/sibylla.cnf &`

`-$ bin/mysql/bin/mysqld_safe --defaults-file=./config/sibylla.cnf --user=sibylla &`

Stop:

`-$ bin/mysql/bin/mysqladmin -u root -p shutdown (STOP needs a password)`

Copy the start/stop scripts for MySQL:
`$- cd $SIBYLLA_HOME/3rd-party/db-domain`

`$- cp $SIBYLLA_HOME/scripts/3rd-party-scripts/db-domain/scripts/start.sh scripts`

`$- cp $SIBYLLA_HOME/scripts/3rd-party-scripts/db-domain/scripts/stop.sh scripts`

`$- chmod ug+x scripts/st*.sh`

Start/stop MySQL:

`-$ cd $SIBYLLA_HOME/3rd-party/db-domain`

`-$ scripts/start.sh`

`-$ scripts/stop.sh (STOP needs a password. It is requested when you issue the command)`


**Mongo**

Prepare the directory stucture:

`-$ mkdir $SIBYLLA_HOME/3rd-party/mongodb-domain`

`-$ mkdir $SIBYLLA_HOME/3rd-party/mongodb-domain/bin`

`-$ mkdir $SIBYLLA_HOME/3rd-party/mongodb-domain/config`

`-$ mkdir $SIBYLLA_HOME/3rd-party/mongodb-domain/runtime`

`-$ mkdir $SIBYLLA_HOME/3rd-party/mongodb-domain/runtime/data`

`-$ mkdir $SIBYLLA_HOME/3rd-party/mongodb-domain/runtime/data/sibylla-db`

`-$ mkdir $SIBYLLA_HOME/3rd-party/mongodb-domain/runtime/logs`

`-$ mkdir $SIBYLLA_HOME/3rd-party/mongodb-domain/runtime/pids`

`-$ mkdir $SIBYLLA_HOME/3rd-party/mongodb-domain/scripts`

Untar the product:

`-$ cd $SIBYLLA_HOME/3rd-party/mongodb-domain/bin`
`-$ tar xzvf $SIBYLLA_HOME/downloads/mongodb-linux-x86_64-ubuntu1804-4.0.6.tgz`
`-$ ln -s mongodb-linux-x86_64-ubuntu1804-4.0.6 mongodb`

Prepare the config file:

`-$ cd $SIBYLLA_HOME/3rd-party/mongodb-domain`

`-$ vi config/sibylla-mongodb.conf (use this)`

```
  # mongod.conf
  # for documentation of all options, see:
  #   http://docs.mongodb.org/manual/reference/configuration-options/

  # Where and how to store data.
  storage:
    dbPath: <changeit: fullpath to Sibylla>/3rd-party/mongodb-domain/runtime/data/sibylla-db
    journal:
      enabled: true
  #  engine:
  #  mmapv1:
  #  wiredTiger:

  # where to write logging data.
  systemLog:
    destination: file
    logAppend: true
    path: <changeit: fullpath to Sibylla>/3rd-party/mongodb-domain/runtime/logs/sibylla-mongodb.log

  # network interfaces
  net:
    port: 27017
    bindIp: <changeit: IP (use 0.0.0.0 to allow remote connections)>

  # replication:
  #   replSetName: "rs0"

  # how the process runs
  processManagement:
    timeZoneInfo: /usr/share/zoneinfo
    pidFilePath: <changeit: fullpath to Sibylla>/3rd-party/mongodb-domain/runtime/pids/sibylla-mongodb.pid

  # sharding:
  #   clusterRole: shardsvr
  # security:
  #   operationProfiling:
  # replication:
  #   Enterprise-Only Options:
  # auditLog:
  # snmp:
```

Copy the start/stop scripts for MongoDB:

`-$ cd $SIBYLLA_HOME/3rd-party/mongodb-domain`

`-$ cp $SIBYLLA_HOME/scripts/3rd-party-scripts/mongodb-domain/scripts/start.sh scripts`

`-$ cp $SIBYLLA_HOME/scripts/3rd-party-scripts/mongodb-domain/scripts/stop.sh scripts`

`-$ chmod ug+x scripts/st*.sh`

Start/stop MongoDB:

`-$ cd $SIBYLLA_HOME/3rd-party/mongodb-domain`

`-$ scripts/start.sh`

`-$ scripts/stop.sh`


**Kafka**

Prepare the directory stucture for the new broker:

`-$ mkdir $SIBYLLA_HOME/3rd-party/kafka-domain/`

`-$ mkdir $SIBYLLA_HOME/3rd-party/kafka-domain/runtime`

`-$ mkdir $SIBYLLA_HOME/3rd-party/kafka-domain/runtime/kafka-logs`

`-$ mkdir $SIBYLLA_HOME/3rd-party/kafka-domain/runtime/zookeeper`


Untar the product:

`-$ cd $SIBYLLA_HOME/3rd-party/kafka-domain/`

`-$ tar xzvf $SIBYLLA_HOME/downloads/kafka_2.12-2.1.1.tgz`

`-$ ln -s kafka_2.12-2.1.1 kafka`


Configure Kafka:

`-$ cd kafka/config`

`-$ vi server.properties`

```
  Uncomment and set the following rows:
  listeners=PLAINTEXT://0.0.0.0:9092
  […]
  advertised.listeners=PLAINTEXT://<host>:9092
  […]
  # The maximum size for a message handled by the broker
  replica.fetch.max.bytes=5242880
  message.max.bytes=5242880
  […]
  log.dirs=<changeit: fullpath to Sibylla>/3rd-party/kafka-domain/runtime/kafka-logs
  […]
  num.partitions=12
  […]
  zookeeper.connect=changeit: hostname (use localhost whenever possible)>:2181
  Close and save.
  ```
- vi zookeeper.properties
  ```
  Uncomment and set the following rows:
  dataDir=<changeit: fullpath to Sibylla>/3rd-party/kafka-domain/runtime/zookeeper
  […]
  advertised.listeners=PLAINTEXT://<changeit: hostname (use localhost whenever possible)>:9092
  Close and save.
```

> NOTE: You can use a smart naming convention for directories to support multiple broker on single machine,
> ```
> $SIBYLLA_HOME/3rd-party/kafka-domain/sibylla-kafka-a
> $SIBYLLA_HOME/3rd-party/kafka-domain/sibylla-kafka-b
> $SIBYLLA_HOME/3rd-party/kafka-domain/sibylla-kafka-c
>
> Broker.id and directory names when running on multiple nodes:
> Machine 1
>   Vertical scalability
>     sibylla-kafka-a - broker.id = 10
>     sibylla-kafka-b - broker.id = 11
>     sibylla-kafka-c - broker.id = 12
> Machine 2
>   Vertical scalability
>     sibylla-kafka-a - broker.id = 20
>     sibylla-kafka-b - broker.id = 21
>     sibylla-kafka-c - broker.id = 22
> Machine ...
>   Vertical scalability
>     sibylla-kafka-a - broker.id = 30
>     sibylla-kafka-b - broker.id = 31
>     sibylla-kafka-c - broker.id = 32
> Machine n
>   Vertical scalability
>     sibylla-kafka-a - broker.id = <n>0 (example for machine 120 - broker.id = 1200)
>     sibylla-kafka-b - broker.id = <n>1 (example for machine 120 - broker.id = 1201)
>     sibylla-kafka-c - broker.id = <n>2 (example for machine 120 - broker.id = 1202)
> ```

**ActiveMQ**

Prepare the directory stucture:
- mkdir $SIBYLLA_HOME/3rd-party/jms-domain
- cd $SIBYLLA_HOME/3rd-party/jms-domain
- tar xvzf $SIBYLLA_HOME/downloads/apache-activemq-5.15.9-bin.tar.gz
- ln -s apache-activemq-5.15.9 activemq

- Remove the transportConnector with name="amqp"
  - cd activemq/conf
  - vi activemq.xml
    - Remove the line with the amqp and mqtt transportConnector
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

**EMQX**

Untar the product:
- cd $SIBYLLA_HOME/3rd-party/mqtt-domain
- unzip $SIBYLLA_HOME/downloads/emqx-ubuntu18.04-v3.1-beta.2.zip

Version the installation to simplify future upgrade and tranfer of data:
- mv emqx emqx-v3.1-beta.2
- ln -s emqx-v3.1-beta.2 emqx

Configure EMQX:
- The configuration file is under ./emqx/etc
  - The basic installation does not require any specific configuration

**Metabase**

Prepare the directory stucture:
- mkdir $SIBYLLA_HOME/3rd-party/analytics-domain/metabase
- mkdir $SIBYLLA_HOME/3rd-party/analytics-domain/metabase/bin
- mkdir $SIBYLLA_HOME/3rd-party/analytics-domain/metabase/config
- mkdir $SIBYLLA_HOME/3rd-party/analytics-domain/metabase/data
- mkdir $SIBYLLA_HOME/3rd-party/analytics-domain/metabase/logs
- mkdir $SIBYLLA_HOME/3rd-party/analytics-domain/metabase/scripts

Untar the product:
- cd $SIBYLLA_HOME/3rd-party/analytics-domain/metabase/bin
- cp $SIBYLLA_HOME/downloads/metabase.jar .

Configure Metabase:
- cd $SIBYLLA_HOME/3rd-party/analytics-domain/metabase
- vi scripts/start.sh (only if necessary to change exposed port)

Start/stop Metabase:
- cd $SIBYLLA_HOME/3rd-party/analytics-domain/metabase
- scripts/start.sh
- scripts/stop.sh

### Console - BackEnd installation & setup

In order to install the WebApp's backend service, move into `sibylla/sibylla-console`, [download the console](TODO) and unzip the file.
Than you have to run the following commands:

- `cd backend`
- `npm install --save`
- `cd lib`
- `mkdir config`
- `cd config`
- `touch environment.ts`
- `nano/vi/vim environment.ts`

- Copy/paste the following configuration:

```
    export const environment = {
        mongoUrl: "",
        METABASE_SITE_URL: "",
        METABASE_SECRET_KEY: "",
        webGisFolderPath: "",
        webGisWritePath: ""
    };

```

`mongoUrl` syntaxis is divided in two methods:
- One MongoDB instance: `mongodb://*MONGO_URL*/gv_iot_webapp`
- Multiple MongoDB instance: `mongodb://*1_MONGO_URL*,*2_MONGO_URL*,.../gv_iot_webapp`

(`webGisFolderPath` is the absolute path to a static folder on the machine, where you will save the KML files.
`webGisWritePath` is the relative path inside the machine to the same folder.)

Node's default port is `3000`, you can change it by modifying the server.js inside backend/dist/ folder.

Return to `/backend`
- `sudo npm run build`

This will create a `dist` folder with the production-ready code of the backend application.

- `sudo scp package.json dist`
- `cd dist`
- `sudo npm install`
- `sudo npm run start server.js`

(`CTRL + C` when you will see "Express server listening on port ####")

Now you can either run the app with nohup (npm run prod), or by following the commands down here we will install forever.js, which helps us to keep track of our node services:

- `cd`
- `[sudo] npm install forever -g`
- `[sudo] forever start backend/dist/server.js`


You can check if everything is working correctly by typing:

- `[sudo] forever list`

- Copy the logfile path of your forever.js process retrieved by the above command

- `cat ########.log`

The installation is finished, your application will be hosted on `localhost:3000` or either the port you've specified in the `server.js` file.


### Console - FrontEnd installation & setup

There are two solution for installation & setup of FrontEnd Console:
- Dist folder with static file and Nginx routing
- Full source code with `npm start`

### Dist folder with static file and Nginx routing
To configurate the frotned console run the following commands:
- `cd frontend/dist/webapp/assets`
- `nano/vi/vim appConfig.json`
- Set your endpoints:
```
    {
    "production": true,
    "apiUrl": "",
    "nodeApiUrl": "",
    "metabaseApiUrl": "",
    "myRxStompConfig": {
        "brokerURL": "",
        "login": "",
        "passcode": "",
        "heartbeatIncoming": 0,
        "heartbeatOutgoing": 20000,
        "reconnectDelay": 200
    }
}

```
- `cp appConfig.json data/`

After all add these following instuction in your Nginx's configuration and restart it:
```
    server {
         listen 8080;

   index index.html;

   location /sibyllaconsole {
       alias *path_of_webapp*/gviot-webapp/frontend/dist/webapp;
   }

   location /static {
       alias *path_of_webapp*/static;
   }
}

```

#### Full source code with `npm start`
To configurate the frotned console run the following commands:
- `cd frontend/src/assets`
- `touch appConfig.json`
- `nano/vi/vim appConfig.json`
- Copy/paste the following configuration:
```
    {
    "production": false,
    "apiUrl": "",
    "nodeApiUrl": "",
    "metabaseApiUrl": "",
    "myRxStompConfig": {
        "brokerURL": "",
        "login": "",
        "passcode": "",
        "heartbeatIncoming": 0,
        "heartbeatOutgoing": 20000,
        "reconnectDelay": 200
    }
}

```
- `cp appConfig.json data/`
- `cd ../../`
- `npm install --save`
- `npm start`

The WebApp start at `4200` port.


### Configure the application

**Configure sibylla.conf**

Modify the content of the file to reflect your environment:

    ------------------------------------------
    Instance name            #   port    hosts
    ------------------------------------------
    db                                   localhost
    mongodb                              localhost
    zk                                   localhost
    kafka                                localhost
    jms-main                             localhost
    metabase                             localhost
    core                     1   18180   localhost
    console                              localhost
    n-1-gateway              1   21100   localhost
    n-1-datapump             1           localhost
    n-1-datapump-websocket   1           localhost

**Configure core**

**Configure console**

**Configure sibylla-network-1/sibylla-gateway**

**Configure sibylla-network-1/sibylla-datapump**

**Configure sibylla-network-1/sibylla-datapump-websocket**

**Configure sibylla-network-2/sibylla-gateway**

**Configure sibylla-network-2/sibylla-datapump**

**Configure sibylla-network-2/sibylla-datapump-websocket**

### Test the start and stop of the application

iot start (or sibylla start)

### Next reading: Getting started
