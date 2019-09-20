---

title: SSH - The useful parts
date: "2019-05-16T16:24:00Z"
---

Definition of SSH from wikipedia:
> Secure Shell (SSH) is a cryptographic network protocol for operating network services securely over an unsecured network.

### Overview
This article is just a list of useful commands/features that are good to know about the SSH.

### Generate new keys:
```
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C "john@example.com"
```

[More details about difference between Ed25519 and RSA](https://medium.com/risan/upgrade-your-ssh-key-to-ed25519-c6e8d60d3c54).

### Keys permissions:
```
chmod 700 ~/.ssh
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/authorized_keys
chmod 644 ~/.ssh/known_hosts
chmod 644 ~/.ssh/config
```

### Copy key to the server 
```
ssh-copy-id -i ~/.ssh/id_ed25519.pub root@192.168.0.1
```

### Adding key to keychain
```
ssh-add -K ~/.ssh/id_ed25519
```

Under the hood this command basically adding content of pub key file into server *authorized_keys* file.

### Configure SSH to always use keychain (OSX)
After setup you will not be prompted any more to add key and enter password after each system restart.

Edit config file:

```
vim ~/.ssh/config
```


```
Host *
    AddKeysToAgent yes
    UseKeychain yes
    IdentifyFile /Users/john/.ssh/id_ed25519
```

### Host specific configuration:
If you have multiple hosts instead of each time adding a key and typing a password you could create a configuration alias.

Edit config file:

```
Host myHostNameOrIpAdress
    HostName 192.168.0.1
    User root
    IdentityFile /Users/test/.ssh/id_aws_ed25519
    Port 22
```
Now you could simply:

```
ssh myHostNameOrIpAdress
```

### Local port forwarding  (tunnel)

For example if the **google.com** is blocked in your network then you could create tunnel through the server that is outside this network and access google.

```
ssh -L 8001:google.com:80 root@192.168.0.1
```
This means we are forwarding our local port **8001** to **google.com:80**.

This is also could be helpful to connect database behind the firewall. 

```
ssh -L 5000:localhost:5432 root@192.168.0.1
```

What is important  here is that **localhost:5432** is the server localhost not the yours!

And to connect from the local machine do:

```
psql -h localhost -p 5000
```

### Remote port forwarding

This work let's say in opposite direction when you need to give an access to someone but it's impossible to establish direct connection.
For example if you have a resource that works only inside your corporate network.
You could establish a "reverse tunnel" through the SSH server and access restricted node through it. 

By default this options is off so you need to turn it on:

```
vim /etc/ssh/sshd_config
```
```
GatewayPorts yes
```
```
service ssh restart
```
```
ssh -R 8001:localhost:3000 root@public-sshserver.com
```

Now from any remove host you could access **public-sshserver.com:8001** and this request will be redirected to your **localhost:3000**.
Any one on remote host will able to connect port 8001.

#### Tunnel without terminal
If you don't need terminal but need only a tunnel use **-nNt** flags:

```
ssh -nNT -L 5000:localhost:5432 root@192.168.0.1
```

## Security measures
### Remove all finger prints from ***known_hosts***

```
ssh-keygen -R
```

[More about the difference between **known_hosts** and **authorized_keys**](https://security.stackexchange.com/questions/20706/what-is-the-difference-between-authorized-keys-and-known-hosts-file-for-ssh)

### Disable root login and disable login with password
Edit **sshd_config**:

```
PermitRootLogin no
PasswordAuthentification no
```

Do not forget to restart ssh service. 

### Monitor authentications attempts:

```
less /var/log/auth.log
```

or with

```
lastlog (-u flag for concere user)
```

### Get the command input history for specific user
Go to interested user home directory and read **bash_history** file:

```
cat /Users/test/.bash_history
```

### Comparison between VPN

Here is the nice article of explaining how the [SSH different from VPN](https://www.howtogeek.com/118145/vpn-vs.-ssh-tunnel-which-is-more-secure/).