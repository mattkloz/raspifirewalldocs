# raspifirewalldocs

## Setup for RaspberryPi-based Firewall running CentOS

### Requirements

1. [RaspberryPi 4](https://vilros.com/collections/raspberry-pi-4/products/raspberry-pi-4-4gb-ram)
2. 32GB or greater micro SD card
3. [USB 3.0 Ethernet Adapter](https://www.amazon.com/gp/product/B00FFJ0RKE/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
4. [Noctua 5V fan](https://www.amazon.com/gp/product/B072Q3CMRW/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)
5. Optional: [RPi4 + Noctua Fan Case](https://www.thingiverse.com/thing:4154600)

---

### Load CentOS on RPi

#### Get CentOS Image
[CentOS 7.8.2003](http://mirrors.ocf.berkeley.edu/centos-altarch/7.8.2003/isos/armhfp/CentOS-Userland-7-armv7hl-RaspberryPI-Minimal-4-2003-sda.raw.xz)

#### Use Balena Etcher to flash SD card
[Balena Etcher](https://www.balena.io/etcher/)

1. Download CentOS image from link
2. Download Balena Etcher from link
3. Insert microSD card into Mac
4. Open Balena Etcher, select CentOS image, select SD card (should be selected by default), click Flash to flash the card
5. Insert SD Card into RPi

### Boot Instructions
1. After Inserting etched SD card into RPi, connect RPi to a monitor, keyboard and mouse
2. CentOS desktop will be displayed
3. Right-click on Desktop > Open in Terminal

### Config SSH
```shell
sudo yum –y install openssh-server openssh-clients
sudo systemctl start sshd
sudo systemctl status sshd (displays ssh status)
systemctl stop sshd (ensure that ssh service has stopped)
sudo systemctl enable sshd (enables ssh to start automatically)
sudo nano /etc/ssh/sshd_config
```
Set the following:
1. Uncomment and set PermitRootLogin to no (to prevent root user attacks)
2. Change port # to something non-standard (like 2003)
3. Save and Exit the file
```shell
service sshd restart
```
Add the following to the Setup file to restrict access to SSH (by IP) if you set the port above to 2003
```shell
-A INPUT -s <Your IP here> -m state --state NEW -p tcp --dport 2003 -j ACCEPT
run ./Reload
```



---

### Add .Net Core
Run the following shell commands on the RPi

#### Get .Net Core
```shell
wget https://download.visualstudio.microsoft.com/download/pr/349f13f0-400e-476c-ba10-fe284b35b932/44a5863469051c5cf103129f1423ddb8/dotnet-sdk-3.1.102-linux-arm.tar.gz
wget https://download.visualstudio.microsoft.com/download/pr/8ccacf09-e5eb-481b-a407-2398b08ac6ac/1cef921566cb9d1ca8c742c9c26a521c/aspnetcore-runtime-3.1.2-linux-arm.tar.gz
```

#### Create a directory and extract the .Net files to that directory
```shell
mkdir dotnet-arm32
tar zxf dotnet-sdk-3.1.102-linux-arm.tar.gz -C $HOME/dotnet-arm32
tar zxf aspnetcore-runtime-3.1.2-linux-arm.tar.gz -C $HOME/dotnet-arm32
```
#### Make .Net available globally
```shell
export DOTNET_ROOT=$HOME/dotnet-arm32
export PATH=$PATH:$HOME/dotnet-arm32
```
#### Check that .Net is available outside of the dotnet-arm32 directory
```shell
dotnet --info
```
---

### Add Nodejs
```shell
yum install -y gcc-c++ make
curl -sL https://rpm.nodesource.com/setup_12.x | sudo -E bash -
sudo yum install nodejs
node -v (just ensuring it is installed)
npm -v (just ensuring it is installed)
yum install nano (optional for editing)
```
---

### Create Rules

```shell
mkdir /etc/raspifirewall
* Add Setup, Takedown, Reload files to directory (add Raspifirewall folder through git or manually)
```
```shell
chmod +x raspifirewall* (makes the files executable)
./raspifirewall (runs the executable files)
iptables -nL (list all rules)
```


#### Setup
This command creates the initial rule chain and it's rules. Included in the Setup are most variations of rules available. As a best practice, all traffic should be blocked that doesn't have an "Allow" rule, rather than blocking specific traffic but there are options to go either route. 


#### Reload
This command will stop drop all of the rules and then initialize them again


#### Takedown
This command will drop all of the rules and open up all traffic. Useful if you run into an issue with something being blocked that shouldn't be and you need a quick way to allow that traffic through while you append the rules.


### Web Interface

[Firewall Interface](https://github.com/mattkloz/raspifirewall_app)

The web interface (still in development) is a progressive web app controlling a node.js server that edits the Setup file (and then reloads the rules). These commands include:

- Takedown Firewall
- Reload Firewall
- Add Rule(s)
- Edit Rules
- View logs
- Alerts for dropped packets
- View successful traffic (in packets by IP) over 24 hours
- Post logs to Azure TableService

