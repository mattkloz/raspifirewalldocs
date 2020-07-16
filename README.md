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

