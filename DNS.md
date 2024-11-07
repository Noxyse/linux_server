DNS, or Domain Name System, is like the phonebook of the internet. When you type a website's name (like www.example.com) into your browser, DNS translates that name into an IP address (a series of numbers) that computers use to identify each other on the network. This way, you can access websites using easy-to-remember names instead of complicated numbers.

Documentation used [here](https://adamtheautomator.com/bind-dns-server/).
### Installing BIND Packages
The BIND packages should already be installed from the [Debian Server](Debian_Server.md) setup.

Check if the service *named* is enabled by running the following command: `sudo systemctl is-enabled named`

And check its status: `sudo systemctl status named`

### Configuring BIND DSN Server
Navigate to the */etc/default/* directory and edit the */etc/default/named* file:
- `OPTIONS="-4 -u bind`

Now navigate to the */etc/bind/* directory and edit the */etc/bind/[named.conf.options](/images/named.conf.options.png)* file:
```
// listen port and address 
listen-on port 53 { localhost; 10.0.2.10; }; 

// for public DNS server - allow from any 
allow-query { any; }; 

// define the forwarder for DNS queries 
forwarders { 1.1.1.1; }; 

// enable recursion that provides recursive query 
recursion yes;
```

Do not forget to comment-out the `listen-on-v6` line.

Check the BIND configuration: `sudo named-checkconf`
### Setting up DNS Zones
Navigate to the */etc/bind/* directory and edit the */etc/bind/[named.conf.local](/images/named.conf.local.png)* file: 
```
zone "kamkar3lib.io" {
    type master;
    file "/etc/bind/zones/forward.kamkar3lib.io";
};

zone "2.0.10.in-addr.arpa" {
    type master;
    file "/etc/bind/zones/reverse.kamkar3lib.io";
};
```

Create a new directory */etc/bind/zones*: `sudo mkdir -p /etc/bind/zones/`

Copy the default **forward** and **reverse** zones:
- `sudo cp /etc/bind/db.local /etc/bind/zones/forward.kamkar3lib.io`
- `sudo cp /etc/bind/db.127 /etc/bind/zones/reverse.kamkar3lib.io`

Edit the **forward** zone file */etc/bind/zones/[forward.kamkar3lib.io](/images/forward.kamkar3lib.io.png)*:
```
;
; BIND data file for the local loopback interface
;
$TTL    604800
@       IN      SOA     kamkar3lib.io. root.kamkar3lib.io. (
	                        2     ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL

; Define the default name server to ns1.kamkar3lib.io
@       IN      NS      ns1.kamkar3lib.io.
ns1     IN      A       10.0.2.10

kamkar3lib.io. IN   MX   10   mail.kamkar3lib.io.

www     IN      A       10.0.2.10
mail    IN      A       10.0.2.20
vault   IN      A       10.0.2.30
```

Edit the **reverse** file */etc/bind/zones/[reverse.kamkar3lib.io](/images/reverse.kamkar3lib.io.png)*:
```
;
; BIND reverse data file for the local loopback interface
;
$TTL    604800
@       IN      SOA     kamkar3lib.io. root.kamkar3lib.io. (
                            1         ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL

@       IN      NS      ns1.kamkar3lib.io.
ns1     IN      A       10.0.2.10
10      IN      PTR     ns1.kamkar3lib.io.
20      IN      PTR     mail.kamkar3lib.io.
```

Check the BIND configurations by running the following commands:
- `sudo named-checkconf`
- `sudo named-checkzone kamkar3lib.io /etc/bind/zones/forward.kamkar3lib.io`
- `sudo named-checkzone kamkar3lib.io /etc/bind/zones/reverse.kamkar3lib.io`

Restart and verify the *named* service with the `systemctl` command:
- `sudo systemctl restart named`
- `sudo systemctl status named`

### Opening DNS port with UFW Firewall
- `sudo ufw allow Bind9`
- `sudo ufw status`

### Verify BIND DNS Server Installation
- `dig @10.0.2.10 www.kamkar3lib.io`
- `dig @10.0.2.10 mail.kamkar3lib.io`
- `dig @10.0.2.10 vault.kamkar3lib.io`

- `dig @10.0.2.10 kamkar3lib.io MX`

- `dig @10.0.2.10 -x 10.0.2.10`
- `dig @10.0.2.10 -x 10.0.2.20`

Go to [5. Setting up DHCP](/DHCP.md)
