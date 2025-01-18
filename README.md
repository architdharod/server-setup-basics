# server-setup-basics

## SSH
Goal: Protect root account. 
1. On the VPS or remote machine, make a new regular user and add them to the sudo group:
   
   ```sudo adduser newuser```
   
   ```sudo usermod -aG sudo newuser```

2. Create / Use the Key pair for the VPS or remote machine. To create the key pair, use:

   ```ssh-keygen -t ed25519 -C "your_email@example.com"```

   Copy the public key over to your sever run on your local machine:

   ```ssh-copy-id -i ~/.ssh/id_ed25519.pub newuser@your_server_ip```

   Now you can log in via SSH. To turn off the username and password login of the VPC/remote machine:

   ```sudo nano /etc/ssh/sshd_config```

   Find these values and set them in the following manner:

   ```
    Port 2222     # Change default port (use a number between 1024 and 65535)
    PermitRootLogin no                 # Disable root login
    PasswordAuthentication no          # Disable password authentication
    PubkeyAuthentication yes           # Enable public key authentication
    AuthorizedKeysFile .ssh/authorized_keys # Specify authorized_keys file location
    AllowUsers newuser                 # Only allow specific users to login
   ```

## Basic Network Safety
Goal: lock down ports that dont need to be exposed to the internet using UFW; Optional: banning ips that try to access wrong port or in an unauthorised way using Fail2Ban
#### UFW
1. install using the following commands: 
```
sudo apt install ufw
sudo apt install fail2ban
```

2. Set least privileges:
```
sudo ufw default deny incoming
sudo ufw allow outgoing
```

3. Allowing connection to the server in a few useful ways:
```
sudo ufw allow ssh
sudo ufw allow 80  //If a web server is being used
sudo ufw allow 443  //If a web server is being used
```

other useful commands: 
```
#List rules with numbers:
sudo ufw status numbered

Delete by rule specification:
sudo ufw delete allow 80

#You can allow connections from specific IP addresses:
sudo ufw allow from 192.168.1.100
#You can also only allow an IP to connect to a specfic port with: 
sudo ufw allow from 192.168.1.100 to any port 22

#If you neeed to allow a range of ports: 
sudo ufw allow 6000:6007/tcp
#To further protect from brut force attacks you can rate limit specific ports with: 
sudo ufw limit ssh
#This would limit port 22 to 6 connections in 30 seconds from a single IP. To see the status of the firewall you can use: 

#Reset incase you need to start over: 
sudo ufw reset

#and to enable and disable: 
sudo ufw enable 
sudo ufw disable 

#finaly to enable logging and adjusting the log level: 
sudo ufw logging on
sudo ufw logging medium # levels are low, medium, high, full 
```


---
credit: [become sovran](https://becomesovran.com/blog/server-setup-basics.html)
