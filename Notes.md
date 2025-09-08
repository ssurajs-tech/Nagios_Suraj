NAGIOS 
Nagios is an open-source software for continuous monitoring of systems, networks and 
infrastructure. It runs plugins stored on a server which is connected with a host or another servr 
on your network or the internet. In case of any failure Nagios alerts about the issues so that the 
technical team can perform recovery process immediately. 
History of Nagios: 
➢ In the year 1999, Ethan Galstad developed it as a part of netsaint distribution. 
➢ 2002, ethan renames the project to “Nagios” because of trademark issues with the name 
“netsaint”. 
➢ 2009, Nagios releases its first commercial version, Nagios XI. 
➢ In 2012, Nagios again renamed as Nagios core. 
➢ It uses port number 5666, 5667 and 5668 to monitor its client. 
Why Nagios? 
➢ Detect all types of network or server issues. 
➢ Helps to find the root cause of the problem which allow you to get the permanent 
solution to the problem. 
➢ Reduce downtime. 
➢ Active monitoring of entire infrastructure. 
➢ Allow you to monitor and troubleshoot server performance issues. 
➢ Automatically fix problems. 
Features of Nagios: 
➢ Oldest and latest. 
➢ Good log and database system. 
➢ Informative and attractive web interface. 
➢ Automatically send alerts if condition changes. 
➢ Helps you to detect network errors or server crashes. 
➢ You can monitor the entire business process and IT infrastructure with a single pass. 
➢ Monitor network services like http, smtp, snmp, ftp, ssh, pop, DNS, LDAP, IPMI etc. 
Phases of Continuous Monitoring: 
1. Define: develop a monitoring strategy 
2. Establish: how frequently you are going to monitor it. 
3. Implement 
4. Analyze data and report finding 
5. Respond 
6. Review and update 
Nagios Architecture: 
Nagios is a client-server architecture. Usually on a network, a Nagios server is running on 
a host and plugins are running on all the remote host which should you monitor. 
How does Nagios works? 
➢ Mention all details in configuration files. 
➢ Daemon read those details what data to be collected. 
➢ Daemon use NRPE plugins to collect data from nodes and store in its own database. 
➢ Finally shows everything in dashboard. 
Pre-requisites: 
• Httpd (browser) 
• Php (dashboard) 
• Gcc and gd (compiler) 
• Makefile (to build) 
• Perl (script) 
Main configuration file: /usr/local/Nagios/etc.Nagios.cfg 
All monitoring things called as service. 
For e.g: 5 servers – 4 checks each 
Then you have to monitor 5*4= 20 services 
Dashboard overview: 
In dashboard you can see 
Host: down, unreachable, up, recovery, none 
Service: warning, unknown, critical, recovery, pending 
Installation of Nagios on Linux: 
To start Nagios core installation, you must have your EC2 instance up and run and have already 
configured SSH access to the instance. 
Step-1: install pre-requisites software on your EC2 machine prior to Nagios installation like 
apache, php, gcc compiler and gd development libraries. 
# sudo su 
# yum install httpd php 
# yum install gcc glibc glibc-common 
# yum install gd gd-devel 
Step-2: create account information you need to setup a Nagios user, run the following 
commands, 
# adduser -m Nagios 
# passwd Nagios 
Now it will ask to enter new password give ‘12345’ as password. 
# groupadd Nagioscmd 
# usermod -a -G Nagioscmd Nagios 
# usermod -a -G Nagioscmd apache 
Step-3: download Nagios core and the plugins. Create a directory for storing the downloads. 
# mkdir ~/downloads 
# cd ~/downloads 
Download the source code tarballs of both Nagios and the Nagios plugins. 
# wget http://prdownloads.sourceforge.net/sourceforge/Nagios/Nagios-4.0.8.tar.gz 
#wget http://Nagios-plugins.org/download/Nagios-plugins-2.0.3.tar.gz 
Step-4: complete and install Nagios extract the Nagios source code tarball 
# tar zxvf Nagios-4.0.8.tar.gz 
# cd Nagios-4.0.8 
Run the configuration script with the name of the group which you have create in above step. 
# ./configure –with-command-grouo=Nagioscmd   
Compile the Nagios source code 
# make all 
Install binaries, init script, sample config files and set permissions on the external command 
directly. 
# make install 
# make install-init 
# make install-config 
# make install-commandmode 
Setp-5: configure the web interface 
# make install-webconf 
Step-6: create a ‘Nagiosadmin’ account for login into Nagios web interface, set password as 
well. 
# htpasswd -c /usr/local/Nagios/etc/htpasswd.users Nagiosadmin 
Asking for a password, set a new pwd 
# service httpd restart 
Step-7: compile and install the Nagios plugins. Extract the Nagios plugins source code tarball. 
# cd ~/downloads 
# tar zxvf Nagios-plugins-2.0.3.tar.gz 
# cd Nagios-plugins-2.0.3 
Compile and install the plugins 
# ../configure --with-Nagios-user=Nagios --with-Nagios-group=Nagios 
# make 
# make install 
Step-8: start Nagios. Add Nagios to the list of system services and have it automatically start 
when the system boots. 
# chkconfig --add Nagios 
# chkconfig Nagios on 
Verify the sample Nagios configuration files 
# /usr/local/Nagios/bin/Nagios -v 
# usr/local/Nagios/etc/Nagios.cfg 
If there are no errors, start Nagios 
# service Nagios start 
# service httpd restart 
Step-9: copy public ip of EC2 instance and paste in google chrome, in given way 
For e.g 20.1.1.1/Nagios/Nagios 
Ask for username- Nagiosadmin 
 and Password- 