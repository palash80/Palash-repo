# Template for Employee API Microservice



| Author         | Created on | Version | Last updated by  | Last edited on |
|----------------|------------|---------|------------------|----------------|
| Palash Kamble  | 24 /03/ 2024 | 1.0     | Palash Kamble    | 24 /03/2024  |


## Table of Contents

1. [Purpose](#purpose)
    1. [Supported Features for the Application](#supported-features-for-the-application)
    2. [Key Components](#key-components)
    3. [System Requirements](#system-requirements)
    4. [Important Ports](#important-ports)
    5. [Pre-requisites](#pre-requisites)
2. [Step-by-step installation](#step-by-step-installation)
    1. [Step1: Install Dependencies required for software](#step1-install-dependencies-required-for-software)
        1. [Install ScyllaDB](#install-scylladb)
        2. [Configure ScyllaDB](#configure-scylladb)
        3. [Scylla Setup](#scylla-setup)
        4. [Install Migrate](#install-migrate)
        5. [Install JQ (JSON processor)](#install-jq-json-processor)
        6. [Install Golang](#install-golang)
3. [Endpoints Information](#endpoints-information)
4. [Contact Information](#contact-information)


## Purpose

The Employee REST API, developed in Golang, manages all employee-related transactions within the [OT-Microservices](https://github.com/OT-MICROSERVICES) repository. It boasts platform independence, enabling deployment across diverse environments effortlessly.

### Supported Features for the Application:

* ScyllaDB as database for storing information.
* Redis as a cache management 
* REST API for web transactions

### Key Components 


| Tool/Software | Usage  |
| ------------- | --------------------------------------------------------- | 
| `Spring boot` | A web framework based on Java, utilizing Tomcat as its web server.  | 
| `ScyllaDB`    | Serves as the primary database for storing all employee data. |
| `Redis`       | Functions as a cache manager to store cache responses.|
| `migrate`     | Facilitates database migration. |



### System Requirements

| System Requirement                |           Minimum                          |
|-----------------------------------|--------------------------------------------|
| Processor/Instance Type           |             Dual Core/t2.medium            | 
| RAM                               |               4GB                          |
| Disk Space                        |               20GB                         |            
| OS Required                       |             Ubuntu 20.04 LTS               |



### Important Ports

| Port | Description             |
|------|-------------------------|
| 22   | ssh                     |
| 8080 | Employee API port       |
| 9042 | ScyllaDB                |
| 6379 | Redis                   |



### Architecture

![image](https://github.com/palash80/Palash-repo/assets/153359214/05379b8b-f11e-4d23-bb9e-8cd73971df38)


### Pre-requisites

The application's setup requirements are minimal, primarily necessitating only database connectivity. Although optional, the integration of Redis as a cache system can enhance performance, yet it is not.

For running the application, we need following things configured:

1. ScyllaDB
2. Redis
3. Java 17
4. Migrate



# Step-by-step installation 
## Step1: Install Dependencies required for software
### Install ScyllaDB

1. Install a repo file and add the ScyllaDB APT repository to your system.

   <tab><tab><pre><code>`sudo mkdir -p /etc/apt/keyrings`</code></pre>

![image](https://github.com/palash80/Sprint-1/assets/153359214/4a8adcba-bf07-405f-9edc-ed145a195a5c)

<tab><tab><pre><code>`sudo gpg --homedir /tmp --no-default-keyring --keyring /etc/apt/keyrings/scylladb.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys d0a112e067426ab2
`</code></pre>

![image](https://github.com/palash80/Sprint-1/assets/153359214/4a8adcba-bf07-405f-9edc-ed145a195a5c)

<tab><tab><pre><code>`sudo wget -O /etc/apt/sources.list.d/scylla.list http://downloads.scylladb.com/deb/debian/scylla-5.4.list
`</code></pre>

2. Install ScyllaDB packages

<tab><tab><pre><code>`sudo apt-get update`</code></pre>

<tab><tab><pre><code>`sudo apt-get install -y scylla
`</code></pre>


3. (Ubuntu only) Set Java 17

```shell
sudo apt-get update
sudo apt-get install -y openjdk-17-jre-headless
sudo update-java-alternatives --jre-headless -s java-1.17.0-openjdk-amd64
``` 


### Configure ScyllaDB

* Configure the following parameters in the /etc/scylla/scylla.yaml configuration file.

![image](https://github.com/palash80/Sprint-1/assets/153359214/205761c2-1203-4081-bec1-ffcd62e3782b)

![image](https://github.com/palash80/Sprint-1/assets/153359214/03b80294-f754-4e1c-897b-351e057dda9c)

![image](https://github.com/palash80/Sprint-1/assets/153359214/6e2b5690-1ce5-4b83-ac6a-2567bf3628a6)



**NOTE**
In above screenshot We have to make the changes (Private IP) in three terms
1. Listen_address
2. rpc_adress
3. seeds

**seeds** : This is the PRIVATE IP address that Scylla will use to connect to other Scylla nodes in the cluster. 
  Seed nodes are used during startup to bootstrap the gossip process and join the cluster

**listen_address**: This is the PRIVATE IP address that Scylla will use to connect to other Scylla nodes in the cluster. In this case, it's set to 'localhost,' indicating that it will listen for connections on the local machine.

**rpc_address**: This is the PRIVATE IP address of the interface for client connections, such as Thrift and CQL. Similar to listen_address, it is set to 'localhost' in this configuration.

### Scylla Setup

* This script tunes system settings and determines optimal configurations.

  <tab><tab><pre><code>sudo scylla_setup</code></pre>

**NOTE**
* Read the requirenment carefully as per your need and select **YES**/**NO**


![image](https://github.com/palash80/Sprint-1/assets/153359214/be8202d7-a66e-4ed9-bdcc-d72157515968)

![image](https://github.com/palash80/Sprint-1/assets/153359214/30dade57-dca7-4b80-8a0d-871f0a2efe3b)

![image](https://github.com/palash80/Sprint-1/assets/153359214/05a23d15-cc38-4f5b-a0da-56f2280e2a95)

![image](https://github.com/palash80/Sprint-1/assets/153359214/571af304-5d12-41f7-9144-c3687469b88d)

 * Start Service of ScyllaDB

<tab><tab><pre><code>sudo systemctl start scylla-server</code></pre>


![image](https://github.com/palash80/Sprint-1/assets/153359214/42e6fd00-d6f6-4eed-bbf5-d833f4dab805)

* Check the service is running or not

![image](https://github.com/palash80/Sprint-1/assets/153359214/f393869e-e544-49b6-b93b-63df63ef04f3)


* login to scylla shell and play around the features


* Run cqlsh command

<tab><tab><pre><code>cqlsh IP</code></pre>

![image](https://github.com/palash80/Sprint-1/assets/153359214/ad8dc974-175b-4b84-b74f-d331aa503d66)


### Install Migrate


```shell
curl -s https://packagecloud.io/install/repositories/golang-migrate/migrate/script.deb.sh | sudo bash
sudo apt-get update
sudo apt-get install migrate
``` 

### Install JQ (JSON processor)

```shell
sudo apt install jq
``` 

### Install Golang

Now the link is ready, Confirm  you are in home directory:
```
cd ~
``` 
use curl to retrieve the tarball file
```
curl -OL https://golang.org/dl/go1.21.6.linux-amd64.tar.gz
``` 
To verify the integrity of the file you downloaded, run the sha256sum command and pass it to the filename as an argument:
```
sha256sum go1.21.6.linux-amd64.tar.gz
``` 
use tar to extract the tarball file
```
sudo tar -C /usr/local -xvf go1.21.6.linux-amd64.tar.gz

set paths in your environment.

```
sudo vim ~/.profile
``` 

Need to add the following information to the end of the file:

```
export PATH=$PATH:/usr/local/go/bin
``` 

Refresh your profile by running the following command:

```
source ~/.profile
``` 

Check the version of Go

```
go version


### Trouble shoot


**Error**

![error](https://github.com/palash80/Palash-repo/assets/153359214/99b2497b-56a5-4616-bc63-b8f5cdf2108f)


**NOTE**:- When you execute the script, before make sure you have allowed the specific ports & once you found error 
           in the interface replace your public ip address with repect to locahost / with specific port. 

### Endpoints Information

| **Endpoint**                     | **API Method** | **Description**                                                                                      |
|----------------------------------|------------|------------------------------------------------------------------------------------------------------|
| /metrics                         | GET        | Application healthcheck and performance metrics are available on this endpoint     |
| /api/v1/employee/health          | GET        | Endpoint for providing shallow healthcheck information about application health and readiness   |
| /api/v1/employee/health/detail   | GET        | Endpoint for providing detailed health check about dependencies as well like - ScyllaDB and Redis  |
| /api/v1/employee/create          | POST       | Data creation endpoint which accepts certain JSON body to add employee information in database     |
| /api/v1/employee/search          | GET        | Endpoint for searching data information using the params in the URL |                                
| /api/v1/employee/search/all      | GET        | Endpoint for searching all information across the system    |
| /api/v1/employee/search/location | GET        | Application endpoint for getting the count and information of location |
| /swagger/index.html              | GET        | Swagger endpoint for the application API documentation and their data models |



### Contact Information

| Palash Kamble                   | palash.kamble@opstree.com                                                                                  
|---------------------------------|------------------------------------------------------------|




### Refrence


|     Description                  | References  
| ---------------------------------| ------------------------------------------------------------------- |
|     Documentation Template       | https://github.com/opstree/OT-Microservices/tree/master/employee









