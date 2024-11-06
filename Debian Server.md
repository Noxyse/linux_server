- Use of VirtualBox (Oracle)
- Make sure to add a NAT Network before starting the server setup and enable the NAT Network for your newly created VM:
	![[NAT Network.png]]
	![Adapterimages](/images/Adapter.png)
- Image of Debian Server 12.7.0 available [here](https://www.debian.org/distrib/)
	- User: nox
	- Password: 1234
- Updating and upgrading the server using `sudo apt update` and `sudo apt upgrade`
- Preparing the setup by installing packets: 
	- `sudo apt install openssh-server`
	- `sudo apt install openssh-client`
	- `sudo apt install bind9 bind9-utils bind9-dnsutils -y`
