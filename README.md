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

## Step 10 - Creating an logs

Logs include
>event logs (create,modified,deleted)
>user activity logs (logins,actions performed)
>synchronization logs (data shared with other MISP instances)

To generate logs in MISP
enable MISP logging

> / var/www/MISP/app/tmp/logs/

view logs via web ui
>administration
>server settings & maintenance
>logs
>filter logs by type,user,actions or events

To create a log

>click on the "Add Event"

>fill in Event Details:
>distribution:
"Your organization only."
"This community only."
"All communities."
"Connected communities."

>threat Level:

>analysis:

>info:

>date:

>tags:

>galaxy:
galaxies contain structured information related to malware, actors, and other threat intelligence.

>related Events:
link to other related events in MISP if applicable.

>add attributes:
attributes are the core components of an event, providing specific details about the incident.
iP address, domain, URL, file hash

>add objects (Optional):
file, email, network activity

>publish the Event:

>monitor and Update:

## Step 11 - Extracting logs in json and sending logs to Hive for integration 

if Hive is already connted with MISP we can fetch it automatically.

For json export we can use MISP API to pull logs
> curl -k -x GET "https://misp-instance.com/logs/index" \
> -H "Authorization: Your_API_KEY" \
> -H "Accept: application/json"
>-o misp.event.2.json

or 

download it through the web ui

For sending json to thehive
>curl -X POST "https://your-hive-instance.com/api/alert" \
>-H "Authorization: Bearer YOUR_THEHIVE_API_KEY" \
>-H "Content-Type: application/json" \
>-d @misp.event.2.json

New alert will appear in thehive dashboard.






