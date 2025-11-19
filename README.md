# Vagrant-Ansible Single-Node Big Data Lab  
### Hadoop + Hive + Pig on Ubuntu 22.04

This repository provides a reproducible single-node Big Data environment using **Vagrant** and **Ansible**.  
The setup installs and configures Hadoop, Hive, and Pig inside an Ubuntu virtual machine for learning and experimentation without any cloud dependency.

---

## Included Components

| Component | Version | Purpose |
|----------|----------|---------|
| Hadoop   | 3.3.3    | HDFS and YARN |
| Hive     | 3.1.2    | SQL-based data warehouse on Hadoop |
| Pig      | 0.16.0   | Data processing scripting for ETL |

---

## Repository Structure
.
├── Vagrantfile # Creates VM and executes Ansible playbook
├── bigdata.yml # Ansible playbook for Hadoop + Hive + Pig
└── README.md


---

## Prerequisites

Install the following on the host machine (Windows, macOS, or Linux):

| Requirement | Link |
|-------------|------|
| Vagrant | https://www.vagrantup.com/downloads |
| VirtualBox | https://www.virtualbox.org/wiki/Downloads |

---

## Getting Started

Clone the repository and provision the VM:

```bash
git clone https://github.com/<username>/<repository>.git
cd <repository>
vagrant up --provision
```
The process automatically performs:

- VM creation (12GB RAM / 4 CPUs)
- Java installation
- Hadoop installation and configuration
- Hive installation and Derby metastore initialization
- Pig installation and configuration

### Accessing the VM

```
vagrant ssh
#Switch to Hadoop user
sudo su - hadoop
source ~/.profile
```

### Running Hadoop

```
start-dfs.sh
start-yarn.sh
jps
```
Expected running services:

```
NameNode
DataNode
SecondaryNameNode
ResourceManager
NodeManager
```
Web interfaces:

---
| Service | URL |
|---------|-----|
| YARN Resource Manager | http://192.168.33.10:8088/cluster |
| NameNode UI | http://192.168.33.10:9870 (if local logs directory is writable) |
---
## Running Hive
Start Hive Metastore and HiveServer2:

```
nohup hive --service metastore > /tmp/hive-metastore.log 2>&1 &
nohup hive --service hiveserver2 > /tmp/hiveserver2.log 2>&1 &
```
---

### Connect using Beeline:

```
beeline -u jdbc:hive2://
show databases;
```
---

### PIG

```
#Check Version
pig -version
#Start Pig
pig
```
Example command:

```
A = LOAD '/user/hive/warehouse' USING PigStorage(',');
DUMP A;
```
---

## Rebuild or Remove the Lab
```
vagrant destroy -f
vagrant up --provision
```