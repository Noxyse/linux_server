We first need to save the default configuration. We will just copy the 'interfaces' files and create another one called 'interfaces-default':
`sudo cp /etc/network/interfaces /etc/network/interfaces-default`

Some lines were edited to correspond the interface we are going to use.

Here are the lines that have been added:
```
allow-hotplug enp0s3
iface enp0s8 inet static
address 10.0.2.10
netmask 255.255.255.0
gateway 10.0.2.1
```

>interfaces-default file
>
>![interfaces-default](/images/interfaces-default.png)

>interfaces file
>
>![interfaces](/images/interfaces.png)

- Restart the network service: `sudo systemctl restart networking.service`
- Make sure you have the correct IP address: `ip a`
- Also, make sure that you have internet: `wget -q --spider https://facebook.com/ && echo 'Internet OK' || echo 'Internet KO'`

Go to [4. Setting up a DNS](/DNS.md)
