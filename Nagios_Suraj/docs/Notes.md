## History of Nagios:
- In 1999, Ethan Galstad developed it as part of the NetSaint distribution.
- In 2002, Ethan renamed the project to “Nagios” due to trademark issues with the name “NetSaint”.
- In 2009, Nagios released its first commercial version, Nagios XI.
- In 2012, Nagios was again renamed as Nagios Core.
- It uses port numbers 5666, 5667, and 5668 to monitor its clients.

## Why Nagios?
- Detect all types of network or server issues.
- Helps to find the root cause of the problem, allowing for a permanent solution.
- Reduces downtime.
- Provides active monitoring of the entire infrastructure.
- Allows monitoring and troubleshooting of server performance issues.
- Can automatically fix problems.

## Features of Nagios:
- Oldest and latest monitoring solution.
- Robust log and database system.
- Informative and attractive web interface.
- Automatically sends alerts if conditions change.
- Helps detect network errors or server crashes.
- Monitors the entire business process and IT infrastructure with a single pass.
- Monitors network services like HTTP, SMTP, SNMP, FTP, SSH, POP, DNS, LDAP, IPMI, etc.

## Phases of Continuous Monitoring:
1. **Define**: Develop a monitoring strategy.
2. **Establish**: Determine how frequently monitoring will occur.
3. **Implement**.
4. **Analyze**: Data analysis and reporting findings.
5. **Respond**.
6. **Review and Update**.

## Nagios Architecture:
Nagios operates on a client-server architecture. Typically, a Nagios server runs on a host, while plugins run on all the remote hosts that need monitoring.

## How Nagios Works:
- Configuration details are specified in configuration files.
- The daemon reads these details to determine what data to collect.
- The daemon uses NRPE plugins to collect data from nodes and stores it in its own database.
- Finally, it displays everything on the dashboard.

## Prerequisites:
- HTTPD (browser)
- PHP (dashboard)
- GCC and GD (compiler)
- Makefile (to build)
- Perl (script)

## Main Configuration File:
`/usr/local/Nagios/etc/Nagios.cfg`

All monitoring entities are referred to as services. For example, if you have 5 servers with 4 checks each, you will monitor 5 * 4 = 20 services.

## Dashboard Overview:
In the dashboard, you can see:
- **Host**: down, unreachable, up, recovery, none
- **Service**: warning, unknown, critical, recovery, pending

## Installation of Nagios on Linux:
To start the Nagios Core installation, ensure your EC2 instance is running and that you have configured SSH access.

### Step 1: Install Prerequisite Software
```bash
sudo su
yum install httpd php
yum install gcc glibc glibc-common
yum install gd gd-devel
```

### Step 2: Create Nagios User
```bash
adduser -m Nagios
passwd Nagios  # Set password to '12345'
groupadd Nagioscmd
usermod -a -G Nagioscmd Nagios
usermod -a -G Nagioscmd apache
```

### Step 3: Download Nagios Core and Plugins
```bash
mkdir ~/downloads
cd ~/downloads
wget http://prdownloads.sourceforge.net/sourceforge/Nagios/Nagios-4.0.8.tar.gz
wget http://Nagios-plugins.org/download/Nagios-plugins-2.0.3.tar.gz
```

### Step 4: Install Nagios
```bash
tar zxvf Nagios-4.0.8.tar.gz
cd Nagios-4.0.8
./configure --with-command-group=Nagioscmd
make all
make install
make install-init
make install-config
make install-commandmode
```

### Step 5: Configure the Web Interface
```bash
make install-webconf
```

### Step 6: Create Nagiosadmin Account
```bash
htpasswd -c /usr/local/Nagios/etc/htpasswd.users Nagiosadmin
service httpd restart
```

### Step 7: Compile and Install Nagios Plugins
```bash
cd ~/downloads
tar zxvf Nagios-plugins-2.0.3.tar.gz
cd Nagios-plugins-2.0.3
../configure --with-Nagios-user=Nagios --with-Nagios-group=Nagios
make
make install
```

### Step 8: Start Nagios
```bash
chkconfig --add Nagios
chkconfig Nagios on
/usr/local/Nagios/bin/Nagios -v /usr/local/Nagios/etc/Nagios.cfg
service Nagios start
service httpd restart
```

### Step 9: Access the Nagios Web Interface
Copy the public IP of your EC2 instance and paste it into your browser in the following format:
`http://<your-ec2-ip>/Nagios/Nagios`
You will be prompted for a username (Nagiosadmin) and the password you set earlier.