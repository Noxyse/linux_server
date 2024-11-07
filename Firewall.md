Using the `ufw` command:
- `sudo apt install ufw`

Make sure that those are the default parameter settings:
- `sudo ufw default deny incoming`
- `sudo ufw default allow outgoing`

Now, allow the SSH connection: `sudo ufw allow ssh` - port 22
You will need to do the same with other ports:
- `sudo ufw allow http` - port 80
- `sudo ufw allow https` - port 443
- `sudo ufw allow dns` - port 53

You can activate the firewall with `sudo ufw enable` and check its status with `sudo ufw status`

![firewall](/images/firewall.png)

Go to [3. Setting up Static IP](/Static_IP.md)
