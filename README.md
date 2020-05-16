**Google Cloud VM: How to install Cuckoo Sandbox on Ubuntu 18.04 (desktop environment) with Teamviewer remote access**
 


#### Teamviewer setup

> https://github.com/teddybugs/teamviewerongooglecloud
 

#### Commands required for Cuckoo installation

```
sudo apt update
sudo apt upgrade 
```
Adding the user to set up the Sandbox:
```
sudo adduser cuckoo
sudo groupadd pcap
sudo usermod -a -G pcap cuckoo
sudo gpasswd -a cuckoo sudo
```

```
sudo apt-get install python python-pip python-dev libffi-dev libssl-dev
sudo apt-get install python-virtualenv python-setuptools
sudo apt-get install libjpeg-dev zlib1g-dev swig
sudo apt-get install mongodb
sudo apt-get install postgresql libpq-dev
sudo apt install virtualbox
sudo apt-get install tcpdump apparmor-utils

sudo chgrp pcap /usr/sbin/tcpdump
sudo setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump

getcap /usr/sbin/tcpdump
sudo aa-disable /usr/sbin/tcpdump

sudo pip install m2crypto

sudo usermod -a -G vboxusers cuckoo
```
The file in the link will be downloaded.

> https://gist.github.com/jstrosch/de20131dda2aac5cd1116dd44b8f2474


```
chmod +x cuckoo-setup-virtualenv.sh
sudo -u cuckoo ./cuckoo-setup-virtualenv.sh
```
```
source ~/.bashrc
mkvirtualenv -p python2.7 cuckoo-test
pip install -U pip setuptools
pip install -U cuckoo
```

```
wget https://cuckoo.sh/win7ultimate.iso
sudo mkdir /mnt/win7
sudo mount -o ro,loop win7ultimate.iso /mnt/win7
```
```
sudo apt-get -y install build-essential libssl-dev libffi-dev python-dev genisoimage
sudo apt-get -y install zlib1g-dev libjpeg-dev
sudo apt-get -y install python-pip python-virtualenv python-setuptools swig
```

Virtual machine setup can be done with vmcloak.
```
pip install -U vmcloak
```
Host-only Networking  Settings:

```
vmcloak-vboxnet0

sudo sysctl -w net.ipv4.conf.vboxnet0.forwarding=1
sudo sysctl -w net.ipv4.conf.ens4.forwarding=1

sudo iptables -t nat -A POSTROUTING -o ens4 -s 192.168.56.0/24 -j MASQUERADE
sudo iptables -P FORWARD DROP
sudo iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -s 192.168.56.0/24 -j ACCEPT
```

Creating a virtual machine:

```
vmcloak init --verbose --win7x64 win7x64base --cpus 2 --ramsize 2048

vmcloak clone win7x64base win7x64cuckoo

vmcloak install win7x64cuckoo adobepdf pillow dotnet java flash vcredist vcredist.version=2015u3 chrome

vmcloak install win7x64cuckoo2 ie11

vmcloak snapshot --count 4 win7x64cuckoo2 192.168.56.101

vmcloak list vms
```


