# How to Achieve Metrics for Infrastructure Monitoring

| **Author**         | **Created on** | **Version** | **Last Updated by** | **Last Updated on** |
|:------------------:|:--------------:|:-----------:|:-------------------:|:-------------------:|
| Palash Kamble      | 22-06-2024     | 1.0         | Palash Kamble       | 22-06-2024          |

---

# Table of Contents

| Sr No. | Topic                                    |
|--------|------------------------------------------|
| 1.     | [Introduction](#introduction)            |
| 2.     | [Components of the POC](#components-of-the-poc) |
| 3.     | [Pre-requisites](#pre-requisites)        |
| 4.     | [Proof of Concept (POC)](#proof-of-concept-poc) |
| 5.     | [Conclusion](#conclusion)                |
| 6.     | [Contact](#contact)                      |
| 7.     | [Reference](#reference)                  |

---

## Introduction

This document outlines a proof of concept (POC) for designing infrastructure monitoring using Prometheus and Grafana. The goal is to establish a robust system for capturing metrics, gaining insights into infrastructure behavior, efficiently resolving errors, and optimizing performance.

---

## Components of the POC

| **Component**      | **Description**                                                                                           |
|--------------------|-----------------------------------------------------------------------------------------------------------|
| **Prometheus Setup**  | Prometheus is used to collect and store metrics from various sources, including the PostgreSQL database and system. |
| **Grafana Setup**     | Grafana is employed to visualize the collected metrics and create informative dashboards.                 |
| **Prometheus Exporters** | Exporters are used to expose metrics from the database and server for Prometheus to scrape.            |
| **Alerting**          | Prometheus is configured to alert for critical service errors and resource issues.                        |
| **Visualization**     | Grafana is used to create comprehensive dashboards to identify and diagnose issues quickly.                |
| **Performance Optimization** | Metrics are analyzed to identify performance bottlenecks and optimize the system.                     |

---

## Pre-requisites

Ensure the following hardware, software, and security requirements are met.

| **Component**       | **Hardware Specifications**             | **Minimum Requirements**         |
|---------------------|-----------------------------------------|----------------------------------|
| **Application Server** | Processor: Dual-core                  | RAM: 4GB                         |
| **Prometheus**      | Disk: 20GB                              | OS: Ubuntu 22.04 LTS             |
| **Grafana**         | Instance Type: T2.medium                |                                  |

| **Dependencies**    | **Name**       | **Version** | **Description**             |
|---------------------|----------------|-------------|-----------------------------|
| **Prometheus**      | Prometheus     | 2.53.0      | Monitoring application      |
| **Exporter**        | Node Exporter  | 1.8.1       | Expose metrics for Prometheus|
| **Grafana**         | Grafana        | 10.1.3      | Visualization tool          |
| **Alert Manager**   | Alert Manager  | 0.27.0      | Alerts for warnings and errors |

## Work flow 




![image](https://github.com/palash80/Palash-repo/assets/153359214/07da1374-b60a-4eb6-9b21-2a3d1334f094)



##  POC 

1. Install Prometheus on server.


![image](https://github.com/palash80/Palash-repo/assets/153359214/6664f9ab-1151-4f05-a69b-d505e1fedcf3)


- Download the source using curl, untar it, and rename the extracted folder to prometheus-files.
<details>
  <summary>Prometheus Installation Steps</summary>

<br> <tab><tab><pre><code>sudo apt update
wget https://github.com/prometheus/prometheus/releases/download/v2.53.0/prometheus-2.53.0.linux-amd64.tar.gz
tar -xzvf prometheus-2.53.0.linux-amd64.tar.gz
mv prometheus-2.53.0.linux-amd64 prometheus-files

</pre></code><br>**OUTPUT :** <br>![Screenshot from 2024-06-20 05-59-48](https://github.com/MyGurukulam-P8/Sanatak_batchP8_Doc/assets/164150254/7f9e719d-4b65-4fc3-86c3-51d5dec2a7e3)<br>
![Screenshot from 2024-06-20 06-04-11](https://github.com/MyGurukulam-P8/Sanatak_batchP8_Doc/assets/164150254/e7b13389-bdc4-4936-bb12-a9c56d47b581)<br>


- Create a Prometheus user, required directories, and make Prometheus the user as the owner of those directories.



<br> <tab><tab><pre><code>sudo useradd --no-create-home --shell /bin/false prometheus
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus</pre></code><br>**OUTPUT :** <br>![Screenshot from 2024-06-20 06-07-41](https://github.com/MyGurukulam-P8/Sanatak_batchP8_Doc/assets/164150254/c043cb89-bf6b-4c94-861e-f8e2aeab4de1)
<br>

- Copy prometheus and promtool binary from prometheus-files folder to /usr/local/bin and change the ownership to prometheus user.

<br> <tab><tab><pre><code>sudo cp prometheus-files/prometheus /usr/local/bin/
sudo cp prometheus-files/promtool /usr/local/bin/
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool</pre></code><br>**OUTPUT :** <br>![Screenshot from 2024-06-20 06-08-05](https://github.com/MyGurukulam-P8/Sanatak_batchP8_Doc/assets/164150254/be4f5d3d-2733-4fd7-8bf3-55c4e4629fca)
<br>

- Move the consoles and console_libraries directories from prometheus-files to /etc/prometheus folder and change the ownership to prometheus user.


<br> <tab><tab><pre><code>sudo cp -r prometheus-files/consoles /etc/prometheus
sudo cp -r prometheus-files/console_libraries /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries</pre></code><br>**OUTPUT :** <br>![Screenshot from 2024-06-20 06-07-59](https://github.com/MyGurukulam-P8/Sanatak_batchP8_Doc/assets/164150254/bd091059-39d4-4cd4-b5d8-5ba5e27dd3ad)
<br>



- Setup Prometheus Configuration and copy the following contents to the prometheus.yml file


<br> <tab><tab><pre><code>sudo vi /etc/prometheus/prometheus.yml</pre></code><br>


<br> <tab><tab><pre><code>

```
global:
  scrape_interval: 10s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']
      


```
      
</pre></code><br>




- Change the ownership of the file to prometheus user.

<br> <tab><tab><pre><code>sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml</pre></code><br>


- Create a prometheus service file.Copy the following content to the file.


<br> <tab><tab><pre><code>sudo vi /etc/systemd/system/prometheus.service</pre></code><br>



<br> <tab><tab><pre><code>[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target</pre></code><br>


- Reload the systemd service to register the prometheus service and start the prometheus service & check the status.

<br> <tab><tab><pre><code>sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl status prometheus</pre></code><br><br>**OUTPUT :** <br>![Screenshot from 2024-06-20 06-14-28](https://github.com/MyGurukulam-P8/Sanatak_batchP8_Doc/assets/164150254/e9434af0-773b-4b0b-8e58-859c760c4025)
<br>

- Now you will be able to access the prometheus UI on 9090 port of the prometheus server. 


<br> <tab><tab><pre><code>http://prometheus-ip:9090/graph</pre></code><br><br>**OUTPUT :** <br>![Screenshot from 2024-06-20 06-17-10](https://github.com/MyGurukulam-P8/Sanatak_batchP8_Doc/assets/164150254/3902089c-b17f-487f-a40e-5c70f42adcb7)
<br>

</details>

2. Install exporter on Db server to expose metrics.



![image](https://github.com/palash80/Palash-repo/assets/153359214/5162d058-c769-4d2b-8590-a70357a0be14)




<details>
  <summary>Node Exporter  Installation Steps</summary>

- Installing Node exporter.Download the Node Exporter binary to each Couchbase Server that you want to monitor. The Node Exporter will export system related stats.

<br> <tab><tab><pre><code>wget https://github.com/prometheus/node_exporter/releases/download/v1.8.1/node_exporter-1.8.1.linux-amd64.tar.gz</pre></code><br>**OUTPUT :** 

- Create a Node Exporter user, required directories, and make prometheus user as the owner of those directories.

<br> <tab><tab><pre><code>sudo groupadd -f node_exporter
sudo useradd -g node_exporter --no-create-home --shell /bin/false node_exporter
sudo mkdir /etc/node_exporter
sudo chown node_exporter:node_exporter /etc/node_exporter</pre></code><br>**OUTPUT :** <br>![Screenshot from 2024-06-20 06-46-17](https://github.com/MyGurukulam-P8/Sanatak_batchP8_Doc/assets/164150254/990d4138-d56c-417d-aef5-3fe4ad95f22b)
<br>


- Untar and move the downloaded Node Exporter binary
<br> <tab><tab><pre><code>tar -xzvf node_exporter-1.8.1.linux-amd64.tar.gz 
mv node_exporter-1.8.1.linux-amd64 node_exporter-files

</pre></code><br>**OUTPUT :** <br>![Screenshot from 2024-06-20 06-49-42](https://github.com/MyGurukulam-P8/Sanatak_batchP8_Doc/assets/164150254/d0d6dac1-b96b-434f-b318-56024997fa0a)
<br>


- Copy node_exporter binary from node_exporter-files folder to /usr/bin and change the ownership to prometheus user.

<br> <tab><tab><pre><code>sudo cp node_exporter-files/node_exporter /usr/bin/
sudo chown node_exporter:node_exporter /usr/bin/node_exporter<</pre></code><br>


- Create a node_exporter service file.

<br> <tab><tab><pre><code>sudo vi /usr/lib/systemd/system/node_exporter.service</pre></code><br>

- Add the following configuration




<br> <tab><tab><pre><code>[Unit]
Description=Node Exporter
Documentation=https://prometheus.io/docs/guides/node-exporter/
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
ExecStart=/usr/bin/node_exporter \
  --web.listen-address=:9200

[Install]
WantedBy=multi-user.target</pre></code>

<br> <tab><tab><pre><code>sudo chmod 664 /usr/lib/systemd/system/node_exporter.service

</pre></code><br>


- Reload the systemd service to register the prometheus service and start the prometheus service.
<br> <tab><tab><pre><code>sudo systemctl daemon-reload
sudo systemctl start node_exporter

</pre></code><br>**OUTPUT :** <br><br>![Screenshot from 2024-06-20 06-58-32](https://github.com/MyGurukulam-P8/Sanatak_batchP8_Doc/assets/164150254/d4335cce-1eb0-4a34-9615-db725c5ea655)<br>



</details>

---------



3 Installation grafana tool



![image](https://github.com/palash80/Palash-repo/assets/153359214/b6c8f2c5-3304-4e04-93cc-d26fb5b0a0be)


(https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/)

* Note- Grafana by default port is 3000, when you successfully installed grafana hit ip:3000

![image](https://github.com/palash80/Palash-repo/assets/153359214/8d4bc9f8-5acb-4fd3-8f8d-616f2ccdcc7a)

* Then create dashboard & attch query of service metrics for real time monitoring

![image](https://github.com/palash80/Palash-repo/assets/153359214/fd12e57e-e56f-4a26-bff5-e62e2cf61893)


**OUTPUT**

![image](https://github.com/palash80/Palash-repo/assets/153359214/98f3bfad-ab87-4c53-b05b-0683727962b7)

## Conclusion

This document's proof of concept (POC) demonstrates using Prometheus and Grafana to monitor and optimize databases effectively. Prometheus collects metrics, Grafana visualizes them, providing insights into database performance, detecting errors, and ensuring system health. This setup supports real-time monitoring, proactive issue resolution, and performance optimization, enhancing database management efficiency.


## Contact 

|     Name         | Email  |
| -----------------| ------------------------------------ |
| Palash Kamble    | palash.kamble@opstree.com |


## Reference

| **Reference**       | **Link**                                                                          |
|----------------|-------------------------------------------------------------------------------------------------|
| Grafana        | [Install Grafana on Debian/Ubuntu](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/) |
| Prometheus     | [Prometheus Installation on Linux (Ubuntu)](https://medium.com/@abdullah.eid.2604/prometheus-installation-on-linux-ubuntu-c4497e5154f6) |




