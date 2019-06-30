# Linux Server Configuration


## Project Overview
You will take a baseline installation of a Linux server and prepare it to host your web applications. You will secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it.


## Configuration Steps

### Secure your server

#### Update all currently installed packages.
```bash
sudo apt-get update
sudo apt-get upgrade
```

#### Change the SSH port from 22 to 2200. Make sure to configure the Lightsail firewall to allow it.
- Edit the file `/etc/ssh/sshd_config`, changing the line with `# Port 22` to `Port 2200`.
- Restart the `sshd` service with `sudo service sshd restart`.
- On Lightsail Networking configurations, add port `2200` and remove port `22` from Firewall settings.

#### Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2200/tcp
sudo ufw allow www
sudo ufw allow ntp
sudo ufw enable
```


## References

- [Changing the SSH Port for Your Linux Server](https://www.godaddy.com/help/changing-the-ssh-port-for-your-linux-server-7306)