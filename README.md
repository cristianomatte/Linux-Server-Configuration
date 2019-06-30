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

### Give `grader` access.
#### Create a new user account named grader.
```bash
sudo adduser grader --disabled-password
```
#### Give grader the permission to sudo.
- Create the file `/etc/sudoers.d/grader` with the following content
```bash
# User rules for grader
grader ALL=(ALL) NOPASSWD:ALL
```
#### Create an SSH key pair for grader using the ssh-keygen tool.
- Create an RSA key pair on the local machine by using `ssh-keygen`.
- Create the `.ssh` directory and `authorized_keys` file for the `grader` user.
```bash
sudo su grader
mkdir /home/grader/.ssh
touch /home/grader/authorized_keys
```
- Add the content of the `.pub` file generated by `ssh-keygen` to the `authorized_keys` file.
- Change the `.ssh` directory and `authorized_keys` file permissions.
```bash
chmod 700 /home/grader/.ssh
chmod 644 /home/grader/authorized_keys
```

## References

- [Changing the SSH Port for Your Linux Server](https://www.godaddy.com/help/changing-the-ssh-port-for-your-linux-server-7306)
- [ubuntu - Creating a user without a password](https://unix.stackexchange.com/questions/56765/creating-an-user-without-a-password)