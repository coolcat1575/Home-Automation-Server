# Pierre's Home Automation Server
This guide is aimed at everyone setting up their own home automation server for the first time.
>The guide will be update when time is. 
 
## Installation:  
 
### Prerequisite: 
- 64 bit capable computer 
- Debian amd64 iso (Get the netinstallation ISO from here https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/ )
- USB memory 4 GB
- Balena Etcher (Get it here https://www.balena.io/etcher/ )
- Basic linux knowledge

### Install Debian:
- Burn ISO to USB memory with Balena Etcher
- Boot on USB memory and use graphical interface to install Debian according to notes below
  - Select keymap to match your keyboard
  - configure hostname
  - configure domain name
  - configure root password
  - Configure new user
  - Select "Guided - use entire disk" and coninue
  - Uncheck "Debian desktop environment"
  - Uncheck "Print server"
  - check "SSH Server"
  - Install Grub
- Remove the USB memory and reboot computer

### Install Docker:
- Log into computer with the new user
- run the following command to switch to root user:
  - ```su -```
- run the following command to install wget and unzip: 
  - ```apt install wget unzip ```
- run the following command to download the repository: 
  - ```wget https://github.com/coolcat1575/Home-Automation-Server/archive/refs/heads/main.zip```
- extract the file with: 
  - ```unzip main.zip```
- run the following command to move the Docker-main folder:
  - ```mv Docker-main/ /usr/local/docker```
- run the following command to make the files executable:
  - ```chmod 755 /usr/local/docker/install_docker.sh```
  - ```chmod 755 /usr/local/docker/cleandocker.sh```
  - ```chmod 755 /usr/local/docker/wait-for-it.sh```
- run the following script to install docker:
  - ```/usr/local/docker/install_docker.sh```
  
### Test the installation. 
- run the following command to move the Docker-main folder:
  - ```docker-compose --version```

### Enable normal user to run docker:
- run the following command to create the group "docker":
  - ```groupadd docker```
- run the following command to add the user "user" the group "docker":
  - ```usermod -aG docker user``` 
 
### Reboot computer. 
- run the following command to reboot the :
  - ```reboot```

## Configuring & running Docker:

### Configure enviromental file. 
- run the following command to configure the env. file:
  - ```mv /usr/local/docker/env /usr/local/docker/.env```
  - ```nano /usr/local/docker/.env```
  - Update the file with your passowords
  - ```ctrl + x``` to save the file 

### Configure docker-compose.conf file. 
- run the following command to configure the docker-compose.conf file:
  - ```nano /usr/local/docker/docker-compose.conf```
  - Update "mac_vlan:" section to reflect your local network 

### start docker 
- run the following command to **start** all docker containers:
  - ```/usr/local/docker/docker-compose up -d``` 

- run the following command to **stop** all docker containers:
  - ```/usr/local/docker/docker-compose down``` 

- run the following command to **update** all docker containers:
  - ```/usr/local/docker/docker-compose pull``` 

## Docker Applications:

|Application|Description|Settings|
|-----------|-----------|--------|
|adguardhome|Pi-Hole alternative|mac_vlan|
|codeserver|Visual Basic Codeserver|Exposes port 8444|                        
|glances|Process explorer|Exposes port 61208|           
|hadb|Home Assistand DB|Exposes port 3306|
|heimdall|Startpage|Exposes port 7080|
|homeassistant|Home Assistant|Network=Host|
|librespeed|Own Speedtest server|Exposes port 8888|
|mqtt|MQTT server used by Hom-Assistant|Network=Host|
|portainer|Manage docker containers|Exposes port 9000|
|postgresserver|Zabbix DB|Exposes port 5432|
|speedtest|Schedules speedtests|Exposes port 8765|
|zabbixagent2|Zabbix Agent 2|Network=Host|
|zabbixserver|Zabbix network monitor server|Network=Host|
|zabbixsnmptraps|Zabbix trap server|Exposes port 162|
|zabbixwebnginx|Zabbix Web Server|Network=Host|

## Roadmap / To Do:
- Move more seetings from docker-compose.conf to .env file
- Automate syncronisation local-github-local
- add error management in installation script
- Autoclean unused docker images, containers, networks, a.s.o
- Add configuration for different docker containers
- add dockerfiles
