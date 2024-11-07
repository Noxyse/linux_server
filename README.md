# Project's requirements
#### Set up the following Linux infrastructure:

1. One server (no GUI) running the following services:
    
    - DHCP (one scope serving the local internal network)
    - DNS (resolve internal resources, a redirector is used for external resources)
    - WEB server (install and configure an Nginx web server to host a local webpage)
    - Weekly backup the configuration files for each service into one single compressed archive
    - The server is remotely manageable (SSH)
  
2. One workstation running a desktop environment and the following apps:
    
    - LibreOffice
    - Gimp
    - Mullvad browser
    - This workstation uses automatic addressing
    - The /home folder is located on a separate partition, same disk

# Configuration
## Server
### 1. Installing a [Debian Server](Debian_Server.md)
### 2. Installing a [Firewall](Firewall.md)
### 3. Setting up a [Static IP](Static_IP.md)
### 4. Setting up a [DNS](DNS.md)
### 5. Setting up [DHCP](DHCP.md)
### 6. Setting up a Web server with [NGINX](NGINX.md)
### 7. Automating a [backup](Automated_backup.md)

## Workstation
### Setting up the [VM](VM_setup.md)

---
By [AKHMerban](https://github.com/AKHMerban), Mohamed & [Nox](https://github.com/Noxyse).
