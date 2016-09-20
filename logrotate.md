# Logrotate

## Installation
It's already installed on Ubuntu

## Usage
**Put your logrotate config files inside /etc/logrotate.d/ with your application's name**
```bash
# vim /etc/logrotate.d/escritoriovirtual
```
```conf
/home/escritoriovirtual/log/*.log {
  daily
  missingok
  rotate 4
  compress
  delaycompress
  notifempty
  copytruncate
  create 0600 escritoriovirtual escritoriovirtual
  sharedscripts
  postrotate
    /sbin/restart escritoriovirtual
  endscript
}
```
