# Create Kali box from Debian Azure Image

First create a VM instance with image parameters:

```
--image Debian11 \
--size Standard_DS1_v2 \
```

wget and screen are already installed with Azure Debian11 image. 

Download Kali keys.

```
wget https://archive.kali.org/archive-key.asc
sudo cp archive-key.asc /etc/apt/trusted.gpg.d/
```

Then as root update package list

```
echo "deb http://http.kali.org/kali kali-rolling main contrib non-free" >> /etc/apt/sources.list
apt-get  update -y 
```


Chrony and ssh launch dialogs prompt to keep current configuration. 
![Chrony](./img/chrony.png)

![ssh](./img/openssh.png)

A libc6 dialog prompts for restart of services. 

Adjust Debconf settings and use configuration parameters to avoid interactive restart and confirmation dialogs.  
```
echo 'libc6 libraries/restart-without-asking boolean true' | sudo debconf-set-selections
```

I like to use python virtual-env (Force old SSH config to stay):
```
DEBIAN_FRONTEND="noninteractive" UCF_FORCE_CONFOLD=1  apt-get install -o Dpkg::Options::="--force-confold" python3-venv -y 

```

Now install Kali
```
sudo apt-get install kali-linux-everything -y 
```
Error
```firmware-misc-nonfree but it is not installable```

```apt-get upgrade```