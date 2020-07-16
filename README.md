# raspifirewalldocs

## Setup for RaspberryPi-based Firewall running CentOS

### Load CentOS on RPi

### Create Rules

1. mkdir /etc/raspifirewall
2. Add Setup, Takedown, Reload files (add Raspifirewall folder through git or manually)
3. Bash: chmod +x raspifirewall* (makes the files executable)
4. Bash: ./raspifirewall (runs the executable files)
5. Bash: iptables -nL (list all rules)

---

#### Reload
This command will stop drop all of the rules and then initialize them again

---

#### Takedown
This command will drop all of the rules and open up all traffic. Useful if you run into an issue with something being blocked that shouldn't be and you need a quick way to allow that traffic through while you append the rules.
