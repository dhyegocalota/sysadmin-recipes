# UFW (Uncomplicated Firewall)
UFW, or Uncomplicated Firewall, is a front-end to iptables. Its main goal is to make managing your firewall drop-dead simple and to provide an easy-to-use interface. It’s well-supported and popular in the Linux community—even installed by default in a lot of distros. As such, it’s a great way to get started securing your sever.

## Installation
```bash
# apt-get install ufw
```

## Usage
The main idea is to deny any incoming/outgoing packet and than allow only services which we'll have to use.

**Deny any incoming/outgoing packet**
```bash
# ufw default deny
```

**Allow only services which we'll have to use**
*p.s. don't forget to allow ssh before enable UFW, otherwise you're gonna have your ssh connection denied*
```bash
# ufw allow ssh
# ufw allow http
# ufw allow https
# ufw allow smtp
```

*You can use any services with UFW*
```bash
# cat /etc/services
```

*or manual TCP/UDP rule*
```bash
# ufw allow 55960/tcp
# ufw allow 55960/udp
```

**Finally, enable UFW**
```bash
# ufw enable
```

**Any other applied rule will have effect after reload UFW**
```bash
# ufw reload
```

## More information
https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server
