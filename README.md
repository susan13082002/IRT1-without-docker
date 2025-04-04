Security Automation Incident Response Without Using Docker


# Virtual Machines to be Created on VirtualBox:
> Ubuntu 22 VM for Cortex
>
> Ubuntu 22 VM for MISP
>
> CentOS 7.9 for Hive, Elasticsearch, Cassandra
>
> Ubuntu 24 Desktop for Web UI Interfaces

# Network Configuration:
> Bridged Network for Communication between VMs

# Step by Step Installation:
>
>
## Step 1- Installing Ubuntu 22 VM for Cortex 



## Step2- Installing Ubuntu 22 VM for Cortex




## Step 3- Installing Cent OS 7.9 for Hive, Elasticsearch and Cassandra

>Allocate 14Gb RAM and 6CPU and enter a name for the VM
>
>Select the ISO image and skip unattended installation
>
>Test this media & Install CentOs 7

> Choose Language ENglish
>
> Select automated partitioning and Done
>
> Start Installation
>
> Set root password and create user
>
> Reboot the VM
>
> ip a
>
> sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3
-----------------------------

>sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s8

----------------------------------

>sudo mv /etc/yum.repos.d/CentOS-Base.repo/etc/yum.repos.d/CentOS-Base.repo.bak

>sudo vi /etc/yum.repos.d/CentOS-Base.repo:

[base] 

name=CentOS-7 - Base 

baseurl=http://vault.centos.org/centos/7/os/x86_64/ 

gpgcheck=1 

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 
  

[updates] 

name=CentOS-7 - Updates 

baseurl=http://vault.centos.org/centos/7/updates/x86_64/ 

gpgcheck=1 

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 
  

[extras] 

name=CentOS-7 - Extras 

baseurl=http://vault.centos.org/centos/7/extras/x86_64/ 

gpgcheck=1 

gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 

>sudo yum clean all 

>sudo yum makecache 

>sudo yum update -y 






## Step 5- Installing MISP
>sudo apt update
>
>cd /opt
>
>wget --no-cache -O /tmp/INSTALL.sh https://raw.githubusercontent.com/MISP/MISP/2.4/INSTALL/INSTALL.sh
>
>bash /tmp/INSTALL.sh -A
>
>Initial Login Credential
>Username:  admin@admin.test 
>Password: admin



    
   








## Step 6- Installing Cortex

>sudo apt update

>sudo groupadd docker

>sudo usermod -aG docker $USER

>newgrp docker

>sudo chmod 777 /usr/bin/

>wget -q -O /tmp/install.sh https://archives.strangebee.com/scripts/install.sh ; sudo -v ; bash /tmp/install.sh

>yes
>
>Install cortex run neurons locally
>



## Step 7 - Install Cassandra on Cent OS
>sudo yum install -y java-1.8.0-openjdk-headless.x86_64
>
>echo JAVA_HOME="/var/lib/jvm/jre-1.80">> /etc/environment JAVA_HOME="/usr/lib/jvm/jre-1.8.0"
>
>rpm --import https://downloads.apache.org/cassandra/KEYS
>
>/etc/yum.repos.d/cassandra.repo

[cassandra]
name=Apache Cassandra

baseurl=https://redhat.cassandra.apache.org/311x

gpgcheck=1

repo_gpgcheck=0

gpgkey=https://downloads.apache.org/cassandra/KEYS

>yum clean all

>yum makecache

>yum install cassandra -y -nogpgcheck

>sudo nano /etc/cassandra/default.conf/cassandra.yaml

cluster_name: 'thp'

seeds: "vm ip address"

listening_address: vm address

rpc_address: vm address

rpc_port:9160

> sudo systemctl enable cassandra

>  sudo systemctl restart cassandra

## Step 8 - Install Elasticsearch on Cent OS
> rpm --import https://artifacts.elastic.co/CPG-KEY-elasticsearch

> sudo nano /etc/yum.repos.d/elasticsearch.repo

[elasticsearch]

name=Elasticsearch repository for 7.x packages

baseurl=https://artifacts.elastic.co/packages/7.x/yum

gpgcheck=1

gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch

enabled=0

autorefresh=1

type=rpm-md

> yum makecache

> sudo yum install --enablerepo=elasticsearch elasticsearch

> sudo yum install --enablerepo=elasticsearch elasticsearch

 node.name: hive-mode
 
 path.data: /var/lib/elasticsearch
 
 path.logs: /var/log/elasticsearch
 
 thread_pool.search.queue_size: 100000
 
 network.host:vm-ip
 
 http.port:9200
 
 discovery.type: single-node

 >sudo chown -R elasticsearch:elasticsearch /etc/elasticsearch

>sudo chmod -R 755 /etc/elasticsearch

>sudo systemctl enable elasticsearch

>sudo systemctl restart elasticsearch

## Step 9 - Install Hive on Cent OS
>/etc/yum.repos.d/strangebee.repo

[thehive-project]
enabled=1

priority=1

name=Thehive-Project RPM repository

baseurl=https://rpm.thehive-project.org/release/noarch

gpgcheck=1

>yum install thehive4 --nogpgcheck

>yum install thehive4 --nogpgcheck

>chown -R /opt/thp/thehive/files

>sudo nano /etc/thehive/application.conf

-----------------------------
---------------------------------

>sudo systemctl enable thehive

>sudo systemctl restart thehive

## Step 10 - MISP Setup for Integration

Create an Organization on MISP

Click "administration"

Click "add organization"

Enter Organization details as desired

Generate UID

Click "Submit"

Add User to Organization

Click on "administration"

Click "Add user"

Enter User details

Chose the created organization

Set user role

click "create"

Create an Auth Key

Click on "Auth Keys"

Click "Add Authentication Key"

Enter details and click submit.

Copy the displayed authkey

Click "I have noted down my key, take me back now"


## Step 11 - Cortex Setup for Integration

Click "Add organization" in Cortex

Enter Organization details

Click on "users"

Click "Add users"

Enter the user details as desired

Click "save user"

Click "new password"

Enter password

press Enter

Click "Create API Key"

Click "reveal" to see the API Key

## Step 12 - Adding the API Key

Add Cortex Integration to thehive configuration file

sudo nano /etc/thehive/application.conf

![image](https://github.com/user-attachments/assets/c0a7e2d3-5aa2-4483-8397-ded035960774)

![image](https://github.com/user-attachments/assets/0d967199-69d4-4add-9769-8d55c26e47c1)

![image](https://github.com/user-attachments/assets/bc620847-112f-4f7e-8f13-4089206fc8f2)


## Step 13 - Setting up Cortex Analysers

Add the files in analysers in cortex
Structure:

analyzers/
└── MyAnalyzer/
    ├── analyzer.json

analyzer.json
{
  "name": "MyAnalyzer",
  "version": "1.0",
  "author": "Rohit KC",
  "url": "https://github.com/your-repo",
  "description": "Example analyzer",
  "license": "MIT",
  "command": "python3 my_analyzer.py",
  "baseConfig": "myanalyzer.conf",
  "dataTypeList": ["domain", "ip", "hash", "url"]
}


analyzer logic: myanalyzer.py
from cortexutils.analyzer import Analyzer

class MyAnalyzer(Analyzer):
    def _init_(self):
        Analyzer._init_(self)

    def run(self):
        self.report({'message': f"Analyzed {self.get_data()}!"})

if _name_ == '_main_':
    MyAnalyzer().run()
    

Analyzer Configuration 
/etc/cortex/application.conf
MyAnalyzer {
  apiKey = "your-api-key-if-needed"
  ...
}


to deploy the analyzer
sudo /opt/cortex/bin/cortex-cli analyzer sync


    
