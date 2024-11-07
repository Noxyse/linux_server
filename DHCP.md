DHCP, or Dynamic Host Configuration Protocol, is a system that automatically assigns IP addresses to devices on a network. When you connect a device (like a computer or smartphone) to a Wi-Fi network, DHCP gives it a unique IP address so it can communicate with other devices and access the internet. This process happens automatically, so you don’t have to set up the IP address manually.

Documentation used [here](https://wiki.debian.org/fr/DHCP_Server)

Install the `isc-dhcp-server` packet.

Navigate to the */etc/dhcp/* directory and make a copy of the `dhcpd.conf` file.

Then, edit the */etc/dhcp/[dhcpd.conf](/images/dhcpd.conf.png)* file:
```
subnet 10.0.2.0 netmask 255.255.255.0 {
	range 10.0.2.100 10.0.2.200;
	option domain-name-servers 10.0.2.10;
	option routers 10.0.2.1;
}
```

Check the */etc/default/isc-dhcp-server* file and edit it. Make sure to comment-out the INTERFACESv6 and add the interface you are using in INTERFACESv4.

Restart the service: `sudo service isc-dhcp-server restart`

Go to [6. Setting up a Web server with NGINX](NGINX.md)
