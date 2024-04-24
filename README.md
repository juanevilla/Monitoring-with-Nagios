Monitoring with Nagios
=================

## Authors

| Author                | Origin                               |
| --------------------- | ------------------------------------ |
| Juan David Evilla     | UniBarranquilla - IUB                |
| Samir Lora            | UniBarranquilla - IUB                |

## Abstract

Document that guides how to use NAGIOS, a very powerful tool used to monitor your IT services, as well as deploy, configure and troubleshoot this tool in Ubuntu Linux, specifically with services hosted in Docker.

### Screenshots
![image](https://github.com/juanevilla/Monitoring-with-Nagios/assets/135185001/f803ce8b-91d5-480e-9d32-330da8a7fa5a)

![image](https://github.com/juanevilla/Monitoring-with-Nagios/assets/135185001/d8a7a203-454b-43f8-b858-dbf04ac92276)


In the images above we can see the host is properly being monitored and under the services tab, we can see several services being monitored

## TOOLS TIC'S (Herramientas TIC'S)

- `Virtual Box`
- `Ubuntu Server`
- `Docker`
- `NGINX`
- `PuTTY`
- `Nagios`

## Status (Estado del trabajo)

| Status            | Description                          |
| ----------------- | ------------------------------------ |
| <img src="https://img.shields.io/badge/Study%20Status-Design%20Finalized-brightgreen.svg" alt="Study Status: Design Finalized"> | The study was finished | 

## Keyworks

- `Server`
- `Nagios`
- `Ubuntu`
- `Monitoring`
- `Linux`
- `Docker`
- `Virtualization`
- `NGINX`
- `Service`
- `Uptime`
- `NPRE`

## Roadmap

    Pre-requisites

    - Linux Basic knowledge including command usage
    - VMWare Basic knowledge.
    - Networking Basics (TCP/IP, NAT, ETC)

    Installation

     - Windows must be properly provisioned with Virtual Box and Hipervisor configuration on BIOS
     - Ubuntu Server (or any other distribution) must be installed and running.
     - Docker must be properly installed
     - Any Server, such as Apache, NGINX, MariaDB instance should be running on Docker 

### Server

* Make sure everything is up to date

      > sudo apt update
      > sudo apt upgrade

* Install Packets

      > sudo apt install wget unzip vim curl gcc openssl build-essential libgd-dev libssl-dev libapache2-mod-php php-gd php apache2

* Download Nagios

      > export VER="4.4.6"
      > curl -SL https://github.com/NagiosEnterprises/nagioscore/releases/download/nagios-$VER/nagios-$VER.tar.gz | tar -xzf -

* Install Nagios

      > cd nagios-4.4.6
      > ./configure
      > sudo make all
      > sudo make install-groups-users
      > sudo usermod -a -G nagios www-data
      > sudo make install
      > sudo make install-init
      > sudo make install-commandmode
      > sudo make install-config
      > sudo make install-webconf
      > sudo a2enmod rewrite cgi
      > sudo systemctl restart apache2
      > sudo make install-exfoliation
      > sudo make install-classicui

* Create a Nagios User

      > sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin

* Install Nagios Plugins
  
      > VER="2.3.3"
      > curl -SL https://github.com/nagios-plugins/nagios-plugins/releases/download/release-$VER/nagios-plugins-$VER.tar.gz | tar -xzf -
      > cd nagios-plugins-2.3.3
      > ./configure --with-nagios-user=nagios --with-nagios-group=nagios
      > sudo make install
      > sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

* Enable Nagios Daemon

      > sudo systemctl enable --now nagios
      > sudo systemctl status nagios

* Access Nagios

      http://ip_servidor/nagios

* Create clients.cfg file in /usr/local/nagios/etc/servers and add host and service

      define host{
      use linux-server
      host_name client
      
      define service {
      use local-service
      host_name NOMBRE_SERVIDOR_MONITORIZAR
      service_description RAM
      check_command check_nrpe!check_mem


 
  

Clientes (Client)
* NRPE and Nagios Installation

      > sudo apt-get install nagios-nrpe-server nagios-plugins
* NRPE Configuration

      > sudo nano /etc/nagios/nrpe.cfg
* Add IP allowed_hosts=127,0,0,1,ip_servidor_nagios
* Restart NRPE

      > sudo /etc/init.d/nagios-nrpe-server restart
* Check nrpe.cfg file

      command[check_users]=/usr/lib/nagios/plugins/check_users -w 2 -c 3



### Usage

It is important to understand that availability is very important when managing services, this is because companies may lose money if services go down. Monitoring is a very important aspect that requires attention as this will allow IT personel to know if something does down or is not properly running as expected, and take further action if required.

### FAQ
- Is this a pay service? No, Nagios has a free version.
- Basic Networking, Server and Linux knowledge is required? Absolutely.
- What do we get with a paid license? More tools and easier configuration.
- Will this automatically fix issues? Not at the moment, however, it will definitely let IT know something is not working so they may take the corrective actions.
- Can this monitor several services at the same time? Yes, We can monitor various statistics from different services, if properly configured.
- What can I do if i run into problems? Make sure to restart Nagios and NRPE service. Make sure client and server have connectivity and there are no typos.

### Contacts
- jdevilla@unibarranquilla.edu.co
- selora@unibarranquilla.edu.co

### Acknowledgements
- https://tecnolitas.com/blog/como-instalar-nagios-en-ubuntu-20-04/
- https://rincondelsistema.home.blog/2019/03/17/configuracion-clientes-nagios/
- https://www.xmodulo.com/monitor-common-services-nagios.html
- https://www.xmodulo.com/nagios-remote-plugin-executor-nrpe-linux.html
