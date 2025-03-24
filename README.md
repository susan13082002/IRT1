Security Automation Incident Response Using Docker


# Virtual Machines to be Created on VirtualBox:
>Security Operations Center (SOC) VM – Runs TheHive, MISP, Cortex

>SIEM & Log Analysis VM – Manages log collection and analysis Elasticsearch

>Test Workstation (Attacker/Defender Simulation) – Used for security testing

# Network Configuration:
>Create an Internal NAT Network in VirtualBox for communication between VMs

>Assign static IPs for easy integration between TheHive, MISP, Cortex, and SIEM



# Step by Step Installation:

## Step 1-Install Ubuntu VM as a SIEM machine

Open your VM software (e.g., VirtualBox or VMware).

Click New to create a new VM.

Name it Ubuntu and set the type as Linux and version as Ubuntu (64-bit).

Allocate RAM (at least 4GB, recommended 8GB or more).

Create a Virtual Hard Disk (at least 25GB, recommended 50GB).

Once the installation is done, restart the VM.

Log in and start using Ubuntu

## Step2- Install Ubuntu VM as a SOC (Securty Operation Center)
Open VirtualBox and click "New".

Name your VM (e.g., Ubuntu-SOC).

Set Type: Linux

Set Version: Ubuntu (64-bit)

Click Next.

Memory (RAM): Set to 8GB (8192MB) (recommended for SOC operations).

CPU Cores: Allocate at least 4 cores.

Storage: Create a 50GB Virtual Hard Disk.

Follow the on-screen installation steps:

Choose Language and Keyboard Layout.

Select Install Ubuntu Server.

Configure Network Settings (set static IP if required).

Set Hostname (e.g., soc-server).

Create a user with a strong password.

Install OpenSSH server (for remote access).

Select "Guided - Use entire disk" for partitioning.

Start installation and reboot after completion.

## Step 3- Hive5 and Cortex Installation

![hive-license](https://github.com/user-attachments/assets/bf85fb2f-2305-4ba3-984c-65eb28238c43)

![new cortex and hive](https://github.com/user-attachments/assets/a72d9e52-28ea-49c0-bde1-16c74996329b)

![cortex loogin](https://github.com/user-attachments/assets/d033655c-b6ac-4357-a1a6-6e0dc7bcb7b6)

![cortexuser](https://github.com/user-attachments/assets/eac3b48b-ce94-4815-8ccf-140ab6a60e8e)

![elastic search gui](https://github.com/user-attachments/assets/37bf04db-7215-4095-9c28-2ff79559b33e)

![elastic search working 2](https://github.com/user-attachments/assets/c332a6c3-8aeb-40b9-bc4e-e021633cfbcb)



## Step 4- Transitioning from Hive5 to Hive4

> The reason to transition from Hive5 to Hive 4 was the drawback regarding the free version license of 14 days. Post that we again the license expires, we would not be able to work with it.
> Hence transitioned to Hive4

## Step 5- Hive4 and Cortex Installation


>sudo apt install -y wget git python3 python3-pip openjdk-11-jre

>sudo apt update && sudo apt upgrade -y

>sudo apt install -y docker.io docker-compose

>sudo systemctl enable --now docker


>mkdir -p ~/thehive && cd ~/thehive

>sudo nano docker-compose.yml

version: '3.7'

services:

  elasticsearch:
  
    image: docker.elastic.co/elasticsearch/elasticsearch:7.10.2
    
    container_name: elasticsearch

    environment:
    
      - discovery.type=single-node
      
      - "ES_JAVA_OPTS=-Xms1g -Xmx1g"
      
    ports:
    
      - "9200:9200"
      
    restart: unless-stopped

 thehive:
  
    image: strangebee/thehive:5
    
    container_name: thehive
    
    environment:
    
      - JAVA_OPTS=-Xms512m -Xmx1g
      
    depends_on:
    
      - elasticsearch
      
    ports:
    
      - "9000:9000"
      
    restart: unless-stopped
    
  cortex:
  
    image: thehiveproject/cortex:latest
    
    container_name: cortex
    
    depends_on:
    
      - elasticsearch
      
    ports:
    
      - "9001:9001"
      
    restart: unless-stopped

>sudo apt install python3-pip

>sudo apt install python3-setuptools

>sudo docker-compose up -d

TheHive: Open http://192.168.1.10:9000

Cortex: Open http://192.168.1.10:9001

docker ps

hive:
id admin@thehive.local
pass admin

hive user
id susan@gmail.com
pass susan


Cortex
id cortex
pass cortex

admin
susan
susan

user
rohit
rohit

![cortex loogin](https://github.com/user-attachments/assets/3435140d-26cf-4db6-91e0-a7c9ff943b65)




## Step 6- Elastic Search Installation

sudo apt update

sudo apt install openjdk-11-jdk -y

sudo apt install elasticsearch -y

sudo systemctl start elasticsearch

sudo systemctl enable elasticsearch  

curl -X GET "localhost:9200/"

 
![elastic search gui](https://github.com/user-attachments/assets/5845920e-9d3b-4720-8dd6-3bc8c6ae1cd9)

![elastic search working 2](https://github.com/user-attachments/assets/813321a3-4dbb-42e4-810f-688d07b09ab7)

![elastic search yml file working](https://github.com/user-attachments/assets/227f38cc-91e7-4843-b6a2-5ddf40794ea4)

![elastic search working](https://github.com/user-attachments/assets/1bdd97d5-6104-4797-bd85-6b28393eb7d8)

## Step 7 - Installation of MISP

git clone https://github.com/MISP/misp-docker.git

cd misp-docker/

cp template.env .env

sudo nano docker-compose.yml

(Change start_interval to start_period) 

sudo docker-compose pull

sudo docker-compose up -d

MISP Dashboard: https://localhost

username-admin@admin.test
pass admin

rohit@gmail.com
Pucchu456@10
changed: Pucchu456@27

## Step 8 - Integrating MISP and thehive

## Step 9 - Integrating Cortex and thehive

## Step 10 - Setting up Cortex Analysers


# Step by Step Troubleshooting:

## 1 - Cortex Voumes/Database not persistent

## 2 - SSL certificate issues MISP and hive

## 3 - Transitioning from Hive 5 to Hive 4 due to licnese issues








