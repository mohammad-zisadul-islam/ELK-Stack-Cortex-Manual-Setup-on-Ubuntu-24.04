# ELK-Stack-Cortex-Manual-Setup-on-Ubuntu-24.04
## Last Update Date: April 23, 2026  
## Contex : Mohammad Zisadul islam

**This document explains how to perform a manual installation of the ELK Stack and Cortex on an Ubuntu 24.04.4 operating system. The purpose of this project is to manually set up a log management and security analysis system by utilizing the ELK stack and Cortex.  
In this project, it is shown that the ELK stack can be installed and configured in order to have a log management tool as well as a Cortex to be used for incident response and observable analysis tasks.**


<p align="center">
  <img width="931" height="614" alt="thehive-logo" src="https://github.com/user-attachments/assets/75c4cba4-e280-4f4a-9b80-28e478111cad" />
</p>



## ⚙️ Project Workflow (Step-by-Step)
<p align="center">
<img width="680" height="339" alt="image" src="https://github.com/user-attachments/assets/f77da543-886d-4a89-acf8-42b3af5e7fff" />
</p>


1. Environment Preparation
- Install Ubuntu 24.04.4 (VM or physical machine)
- Update system packages

```bash
sudo apt update && sudo apt upgrade -y
```  
### Install basic dependencies:
```bash
sudo apt install curl wget unzip apt-transport-https -y
```


## Install Linux Packages 
```bash
sudo apt install wget curl gnupg coreutils apt-transport-https git ca-certificates ca-certificates-java software-properties-common python3-pip lsb-release unzip
```
## Install in java machin 
### Adding the GPG key

```bash
wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto.gpg echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" | sudo tee -a /etc/apt/sources.list.d/corretto.sources.list
```

## Updating package index and Install Java-11-JDK
```bash
 sudo apt update sudo apt install java-common java-11-amazon-corretto-jdk
```
```bash
echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" 
```

# Chack java 
```bash
java -version
```

## Elasticsearch Installation and Configuration

```bash
 sudo apt-get install apt-transport-https wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```
```bash
 echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic8.x.list 
```

## Update package index and install Elasticsearch : 
```bash
 sudo apt update
```
```bash
 sudo apt install elasticsearch 
```
## Configuration file parth and create a file 
```bash
sudo nano /etc/elasticsearch/jvm.options.d/jvm.options
```


# Create a file path and this value past the file parh 
- 4GB Ram consume in cortex 

```bash
-Dlog4j2.formatMsgNoLookups=true
-Xms512m
-Xmx512m
```

## Configuration file path 
### Open file path and modify 

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

### Edite the Path (Remove=#) 

```bash
path.logs: "/var/log/elasticsearch"
path.data: "/var/lib/elasticsearch"
http.port: 9200
cluster.name: hive
xpack.security.enabled: false
http.host: 192.168.33.144 
transport.host: 192.168.33.144
Write it below addet
script.allowed_types: "inline,stored"
thread_pool.search.queue_size: 100000
```

- Than Save and exit. Ctrl+x, enter y, enter

## Status Chack 

```bash
sudo systemctl enable elasticsearch.service  
sudo systemctl start elasticsearch.service  
sudo systemctl status elasticsearch.service  
sudo systemctl daemon-reexec  
sudo systemctl restart elasticsearch  
```

- Cortex enable,start and status all is ok 

## Chack Status in Tarminal
```bash
curl http://192.168.33.144:9200
```
### Output
```bash
{
  "name" : "test",
  "cluster_name" : "Hive",
  "cluster_uuid" : "uCKRUkcCRP6QskjnwZnApQ",
  "version" : {
    "number" : "8.19.14",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "f9adf4c29021dbda28cae7d9c11924471798723d",
    "build_date" : "2026-04-02T15:09:12.561504739Z",
    "build_snapshot" : false,
    "lucene_version" : "9.12.2",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

### Chack Statis in Browser 
<p align="center">
<img width="1919" height="487" alt="Screenshot 2026-04-23 165920" src="https://github.com/user-attachments/assets/48877519-4f9e-4e68-9831-16a22aec55d4" />
</p>











