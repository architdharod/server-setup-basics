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

## Users


---
credit: [become sovran](https://becomesovran.com/blog/server-setup-basics.html)
