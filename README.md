# raspifirewalldocs

## Setup for RaspberryPi-based Firewall running CentOS

### Requirements

1. [RaspberryPi 4](https://vilros.com/collections/raspberry-pi-4/products/raspberry-pi-4-4gb-ram)
2. 32GB or greater micro SD card
3. [USB 3.0 Ethernet Adapter](https://www.amazon.com/gp/product/B00FFJ0RKE/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)


### Load CentOS on RPi
[CentOS 7.8.2003](http://mirrors.ocf.berkeley.edu/centos-altarch/7.8.2003/isos/armhfp/CentOS-Userland-7-armv7hl-RaspberryPI-Minimal-4-2003-sda.raw.xz)

#### Balena Etcher
Used to flash the SD card
[Balena Etcher](https://www.balena.io/etcher/)

### Add .Net Core
```shell
wget https://download.visualstudio.microsoft.com/download/pr/349f13f0-400e-476c-ba10-fe284b35b932/44a5863469051c5cf103129f1423ddb8/dotnet-sdk-3.1.102-linux-arm.tar.gz
```


### Create Rules

1. mkdir /etc/raspifirewall
2. Add Setup, Takedown, Reload files (add Raspifirewall folder through git or manually)
3. Bash: chmod +x raspifirewall* (makes the files executable)
4. Bash: ./raspifirewall (runs the executable files)
5. Bash: iptables -nL (list all rules)

---

#### Setup
This command creates the initial rule chain and it's rules. Included in the Setup are most variations of rules available. As a best practice, all traffic should be blocked that doesn't have an "Allow" rule, rather than blocking specific traffic but there are options to go either route. 

---

#### Reload
This command will stop drop all of the rules and then initialize them again

---

#### Takedown
This command will drop all of the rules and open up all traffic. Useful if you run into an issue with something being blocked that shouldn't be and you need a quick way to allow that traffic through while you append the rules.

---

### Web Interface

The web interface (still in development) is a progressive web app controlling a node.js server that edits the Setup file (and then reloads the rules). These commands include:

- Takedown Firewall
- Reload Firewall
- Add Rule(s)
- Edit Rules
- View logs
- Alerts for dropped packets
- View successful traffic (in packets by IP) over 24 hours
- Post logs to Azure TableService

