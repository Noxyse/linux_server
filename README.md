# Project's requirements
#### Set up the following Linux infrastructure:

1. One server (no GUI) running the following services:
    
    - DHCP (one scope serving the local internal network) #isc-dhcp-server
    - DNS (resolve internal resources, a redirector is used for external resources) #bind
    - WEB server (install and configure an Nginx web server to host a local webpage) #nginx
    - **Required**
        1. Weekly backup the configuration files for each service into one single compressed archive #cron
        2. The server is remotely manageable (SSH) #ssh
    - **Optional**
        1. Backups are placed on a partition located on separate disk, this partition must be mounted for the backup, then unmounted #rsync
2. One workstation running a desktop environment and the following apps:
    
    - LibreOffice
    - Gimp
    - Mullvad browser
    - **Required**
        1. This workstation uses automatic addressing
        2. The /home folder is located on a separate partition, same disk
    - **Optional**
        1. Propose and implement a solution to remotely help a user

# Setup steps
### 1. Installing [Debian Server](Debian_Server.md)
### 2. Setting up [Static IP](Static_ip.md)
### 3. Adding a [Firewall](Firewall.md)
### 4. Setting up a [DNS](DNS.md)
### 5. Setting up [DHCP](DHCP.md)
