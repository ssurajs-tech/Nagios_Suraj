# Nagios Monitoring Tool

Nagios is an open-source software for continuous monitoring of systems, networks and infrastructure. It runs plugins stored on a server which is connected with a host or another server on your network or the internet. In case of any failure, Nagios alerts about the issues so that the technical team can perform recovery processes immediately.

***

## Table of Contents
- [History of Nagios](#history-of-nagios)  
- [Why Nagios](#why-nagios)  
- [Features of Nagios](#features-of-nagios)  
- [Phases of Continuous Monitoring](#phases-of-continuous-monitoring)  
- [Nagios Architecture](#nagios-architecture)  
- [How Does Nagios Work](#how-does-nagios-work)  
- [Pre-requisites](#pre-requisites)  
- [Main Configuration](#main-configuration-file)  
- [Dashboard Overview](#dashboard-overview)  
- [Installation Guide](#installation-of-nagios-on-linux)  

***

## History of Nagios
- In the year 1999, Ethan Galstad developed it as a part of netsaint distribution.  
- 2002, Ethan renames the project to “Nagios” because of trademark issues with the name “netsaint”.  
- 2009, Nagios releases its first commercial version, Nagios XI.  
- In 2012, Nagios again renamed as Nagios core.  
- It uses port number **5666, 5667, and 5668** to monitor its client.  

***

## Why Nagios
- Detect all types of network or server issues.  
- Helps to find the root cause of the problem which allows you to get a permanent solution.  
- Reduce downtime.  
- Active monitoring of entire infrastructure.  
- Allow you to monitor and troubleshoot server performance issues.  
- Automatically fix problems.  

***

## Features of Nagios
- Oldest and latest.  
- Good log and database system.  
- Informative and attractive web interface.  
- Automatically send alerts if conditions change.  
- Helps you to detect network errors or server crashes.  
- You can monitor the entire business process and IT infrastructure with a single pass.  
- Monitor network services like http, smtp, snmp, ftp, ssh, pop, DNS, LDAP, IPMI etc.  

***

## Phases of Continuous Monitoring
1. Define: develop a monitoring strategy  
2. Establish: how frequently you are going to monitor it.  
3. Implement  
4. Analyze data and report findings  
5. Respond  
6. Review and update  

***

## Nagios Architecture
Nagios uses a client-server architecture. Usually, on a network, a Nagios server is running on a host and plugins are running on all the remote hosts that need to be monitored.

***

## How Does Nagios Work
- Mention all details in configuration files.  
- Daemon reads those details on what data to collect.  
- Daemon uses NRPE plugins to collect data from nodes and store them in its own database.  
- Finally, everything is shown in the dashboard.  

***

## Pre-requisites
- Httpd (browser)  
- Php (dashboard)  
- Gcc and gd (compiler)  
- Makefile (to build)  
- Perl (script)  

***

## Main Configuration File
- Main configuration file: `/usr/local/Nagios/etc.Nagios.cfg`  
- All monitoring checks are called **services**.  

Example: 5 servers – 4 checks each → 5 × 4 = 20 services  

***

## Dashboard Overview
In the dashboard, you can see:  
- Host: down, unreachable, up, recovery, none  
- Service: warning, unknown, critical, recovery, pending  

***

## Installation of Nagios on Linux
To start Nagios core installation, you must have your EC2 instance up and running and SSH configured to it.  

### Step-1: Install pre-requisites
```bash
sudo su
yum install httpd php
yum install gcc glibc glibc-common
yum install gd gd-devel
```

### Step-2: Create Account
```bash
adduser -m Nagios
passwd Nagios   # password = 12345
groupadd Nagioscmd
usermod -a -G Nagioscmd Nagios
usermod -a -G Nagioscmd apache
```

### Step-3: Download Nagios Core and Plugins
```bash
mkdir ~/downloads
cd ~/downloads
wget http://prdownloads.sourceforge.net/sourceforge/Nagios/Nagios-4.0.8.tar.gz
wget http://Nagios-plugins.org/download/Nagios-plugins-2.0.3.tar.gz
```

### Step-4: Compile and Install Nagios
```bash
tar zxvf Nagios-4.0.8.tar.gz
cd Nagios-4.0.8
./configure –with-command-grouo=Nagioscmd
make all
make install
make install-init
make install-config
make install-commandmode
```

### Step-5: Configure the Web Interface
```bash
make install-webconf
```

### Step-6: Create Web Access Account
```bash
htpasswd -c /usr/local/Nagios/etc/htpasswd.users Nagiosadmin
service httpd restart
```

### Step-7: Compile & Install Plugins
```bash
cd ~/downloads
tar zxvf Nagios-plugins-2.0.3.tar.gz
cd Nagios-plugins-2.0.3
../configure --with-Nagios-user=Nagios --with-Nagios-group=Nagios
make
make install
```

### Step-8: Start Nagios
```bash
chkconfig --add Nagios
chkconfig Nagios on
/usr/local/Nagios/bin/Nagios -v /usr/local/Nagios/etc/Nagios.cfg
service Nagios start
service httpd restart
```

### Step-9: Accessing the Dashboard
Copy the **public IP** of your EC2 instance and paste it into Google Chrome in the given format.

***
