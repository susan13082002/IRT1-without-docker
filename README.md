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


## Step 10 - 

  
  
    
  
    
   
 




# Step by Step Troubleshooting:

## 1 - 
## 2 - 


## 3 - 


## 4 - 

## 5- 












