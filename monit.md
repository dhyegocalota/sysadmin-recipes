# Monit

## Installation
```bash
# apt-get install -y monit
```

## Usage
**Change monit interval if you like**
```bash
# vim /etc/monit/monitrc.conf
```
```conf
# ...
	set daemon 10
```

**Create your monit task conf to restart your upstart task for example**
```bash
# vim /etc/monit/conf.d/escritoriovirtual.conf
```
```conf
check file restart.txt with path /home/escritoriovirtual/lk8-bonus/shared/tmp/restart.txt
	if changed timestamp then
		exec "/sbin/restart escritoriovirtual"
```
