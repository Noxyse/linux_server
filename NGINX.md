Nginx is a web server and reverse proxy server that is also used as an email proxy and load balancer. It is known for its ability to handle a large number of concurrent connections efficiently, making it popular for serving static content, managing traffic, and improving the performance of web applications. 

#### Installing the packet
We already took care of this part in [Debian_Server](Debian_Server.md).

#### Make it work
We will:
1. Start the service: `sudo systemctl start nginx`
2. Make it boot alongside the server: `sudo systemctl enable nginx`
3. Enable it in the firewall: `sudo ufw allow 'Nginx Full'`
4. And finally check the service's status: `sudo systemctl status nginx`

If everything went well, this is what you should get:

![nginx](/images/nginx.png)

#### Configuring the web server
You might want to check your `/var/www/html` directory as there may be 2 different html files. Should this be the case, rename the nginx one into `index.html` and discard (or rename) the previous one.

For other and more in-depth settings and configuration, you will need to wander into the `/etc/nginx/` directory.

Do not forget to restart the service after each and every modification: `sudo systemctl restart nginx`.
#### How to test it?
Using your workstation, open up your browser and access the server's IP address. In our case: `http://10.0.2.10/`.

Go to [7. Automating a backup](Automated_backup.md)
