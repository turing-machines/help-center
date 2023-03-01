While the choice of operating system for a Kubernetes setup is not crucial from a Kubernetes perspective, the correct choice of OS can greatly simplify post-install tasks.

For this Kubernetes setup, we have chosen DietPi as our operating system. For its ease of setup and availability of automatic bootstrap options, enabling quick and effortless preparation of the nodes.

You can download the DietPi ISO image **[HERE](https://dietpi.com/#downloadinfo)**. **Choose the Raspberry Pi 2/3/4** version.

To flash the ISO onto the Raspberry Pi CM4 module, you need to use your preferred operating system and follow the guide provided ([see guide link](https://help.turingpi.com/hc/en-us/articles/8687165986205)). If you are using CM4 modules with SD card, you may use an SD card reader.

Instead of choosing one of the images option in **Raspberry Pi Imager**, scroll down and choose "**Use custom**".

![use_custom.png](https://help.turingpi.com/hc/article_attachments/9042999711261)

And choose the downloaded DietPi image.

The remaining steps to complete the flashing process will be the same as outlined in our "[Install OS](https://help.turingpi.com/hc/en-us/articles/8687165986205)" guide, **with one exception**. Instead of utilizing the customization options, the image will be directly written to the CM4 storage without any modifications.

After completing the flashing process, you will have a single partition available for writing (In windows, since it's FAT23, other OS can access the main partition as well) with a size of 126 MB. This partition serves as the boot disk for DietPi. **It may be necessary to unplug and reinsert the storage device for it to be recognized.**

It should contain files like this:

![dietpi_files.png](https://help.turingpi.com/hc/article_attachments/9044106102173)

We are interested in files called: **dietpi.txt and cmdline.txt  
**

dietpi.txt
==========

This file enables the streamlined, automated bootstrapping process of the DietPi OS. It facilitates the setup of the network, SSH, and the installation of additional software. Although it has the capability to install Kubernetes automatically, we will proceed with the manual installation later on.

For a thorough explanation of this file, please refer to the link provided **[HERE.](https://github.com/MichaIng/DietPi/blob/master/dietpi.txt)**

**Edit the file and replace it content with (read below for variables to change):**

AUTO\_SETUP\_ACCEPT\_LICENSE=1  
AUTO\_SETUP\_LOCALE=C.UTF-8  
AUTO\_SETUP\_KEYBOARD\_LAYOUT=us  
AUTO\_SETUP\_TIMEZONE=Europe/Bratislava  
AUTO\_SETUP\_NET\_ETHERNET\_ENABLED=1  
AUTO\_SETUP\_NET\_WIFI\_ENABLED=0  
AUTO\_SETUP\_NET\_ETH\_FORCE\_SPEED=0  
AUTO\_SETUP\_NET\_WIFI\_COUNTRY\_CODE=SK  
  
AUTO\_SETUP\_NET\_USESTATIC=1  
AUTO\_SETUP\_NET\_STATIC\_IP=10.0.0.60  
AUTO\_SETUP\_NET\_STATIC\_MASK=255.255.255.0  
AUTO\_SETUP\_NET\_STATIC\_GATEWAY=10.0.0.254  
AUTO\_SETUP\_NET\_STATIC\_DNS=1.1.1.1 8.8.8.8  
  
AUTO\_SETUP\_DHCP\_TO\_STATIC=0  
  
AUTO\_SETUP\_NET\_HOSTNAME=cube01  
  
AUTO\_SETUP\_BOOT\_WAIT\_FOR\_NETWORK=1  
AUTO\_SETUP\_SWAPFILE\_SIZE=1  
AUTO\_SETUP\_SWAPFILE\_LOCATION=/var/swap  
AUTO\_SETUP\_HEADLESS=1  
AUTO\_UNMASK\_LOGIND=0  
AUTO\_SETUP\_CUSTOM\_SCRIPT\_EXEC=0  
AUTO\_SETUP\_BACKUP\_RESTORE=0  
AUTO\_SETUP\_SSH\_SERVER\_INDEX=-2  
AUTO\_SETUP\_LOGGING\_INDEX=-1  
AUTO\_SETUP\_RAMLOG\_MAXSIZE=50  
  
AUTO\_SETUP\_WEB\_SERVER\_INDEX=0  
AUTO\_SETUP\_DESKTOP\_INDEX=0  
AUTO\_SETUP\_BROWSER\_INDEX=0  
AUTO\_SETUP\_AUTOSTART\_TARGET\_INDEX=7  
AUTO\_SETUP\_AUTOSTART\_LOGIN\_USER=root  
AUTO\_SETUP\_GLOBAL\_PASSWORD=dietpi  
AUTO\_SETUP\_AUTOMATED=1  
SURVEY\_OPTED\_IN=0  
  
#OpenSSH Client  
AUTO\_SETUP\_INSTALL\_SOFTWARE\_ID=0  
#Samba Client  
AUTO\_SETUP\_INSTALL\_SOFTWARE\_ID=1  
#vim  
AUTO\_SETUP\_INSTALL\_SOFTWARE\_ID=20  
#RPi.GPIO  
AUTO\_SETUP\_INSTALL\_SOFTWARE\_ID=69  
#OpenSSH Server  
AUTO\_SETUP\_INSTALL\_SOFTWARE\_ID=105  
#Python 3 pip  
AUTO\_SETUP\_INSTALL\_SOFTWARE\_ID=130  
  
CONFIG\_CPU\_GOVERNOR=schedutil  
CONFIG\_CPU\_ONDEMAND\_SAMPLE\_RATE=25000  
CONFIG\_CPU\_ONDEMAND\_SAMPLE\_DOWNFACTOR=40  
CONFIG\_CPU\_USAGE\_THROTTLE\_UP=50  
  
CONFIG\_CPU\_MAX\_FREQ=Disabled  
CONFIG\_CPU\_MIN\_FREQ=Disabled  
  
CONFIG\_CPU\_DISABLE\_TURBO=0  
  
CONFIG\_PROXY\_ADDRESS=MyProxyServer.com  
CONFIG\_PROXY\_PORT=8080  
CONFIG\_PROXY\_USERNAME=  
CONFIG\_PROXY\_PASSWORD=  
  
CONFIG\_G\_CHECK\_URL\_TIMEOUT=10  
CONFIG\_G\_CHECK\_URL\_ATTEMPTS=5  
CONFIG\_CHECK\_CONNECTION\_IP=8.8.8.8  
CONFIG\_CHECK\_CONNECTION\_IPV6=2620:fe::fe  
CONFIG\_CHECK\_DNS\_DOMAIN=google.com  
  
CONFIG\_CHECK\_DIETPI\_UPDATES=1  
CONFIG\_CHECK\_APT\_UPDATES=1  
CONFIG\_NTP\_MODE=2  
CONFIG\_SERIAL\_CONSOLE\_ENABLE=1  
CONFIG\_SOUNDCARD=none  
CONFIG\_LCDPANEL=none  
CONFIG\_ENABLE\_IPV6=0  
  
CONFIG\_APT\_RASPBIAN\_MIRROR=http://raspbian.raspberrypi.org/raspbian/  
CONFIG\_APT\_DEBIAN\_MIRROR=https://deb.debian.org/debian/  
CONFIG\_NTP\_MIRROR=debian.pool.ntp.org  
  
#------------------------------------------------------------------------------------------------------  
\##### DietPi-Software settings #####  
#------------------------------------------------------------------------------------------------------  
SOFTWARE\_DISABLE\_SSH\_PASSWORD\_LOGINS=0  
  
#------------------------------------------------------------------------------------------------------  
\##### Dev settings #####  
#------------------------------------------------------------------------------------------------------  
DEV\_GITBRANCH=master  
DEV\_GITOWNER=MichaIng  
  
#------------------------------------------------------------------------------------------------------  
\##### Settings, automatically added by dietpi-update #####  
#------------------------------------------------------------------------------------------------------  
  
  

Of course, we need to **change some variables**, most of them stay the same for all four nodes but these you should change:

*   **AUTO\_SETUP\_TIMEZONE**\=Europe/Bratislava
    *   Setup to your Time Zone, list of TimeZones is [HERE.](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
*   **AUTO\_SETUP\_NET\_STATIC\_IP**\=10.0.0.60
    *   Set static IP for your Node
*   **AUTO\_SETUP\_NET\_STATIC\_MASK**\=255.255.255.0
    *   Set Mask (your network specific)
*   **AUTO\_SETUP\_NET\_STATIC\_GATEWAY**\=10.0.0.254
    *   Set Gateway (your network specific)
*   **AUTO\_SETUP\_NET\_HOSTNAME**\=cube01
    *   Set hostname, we will go simple here. cube01-04
*   **AUTO\_SETUP\_GLOBAL\_PASSWORD**\=dietpi
    *   Set your initial root password

cmdline.txt
===========

This is another file that requires editing. It consists of a single line of text, and we will append the following information to the end of that line.

group\_enable=cpuset cgroup\_enable=memory cgroup\_memory=1

So the final line looks something like:

root=PARTUUID=a97c7839-02 rootfstype=ext4 rootwait fsck.repair=yes net.ifnames=0 logo.nologo console=serial0,115200 console=tty1 group\_enable=cpuset cgroup\_enable=memory cgroup\_memory=1

**Do not copy over the whole line from here,** just append the:

group\_enable=cpuset cgroup\_enable=memory cgroup\_memory=1

These variables ensure container software will work properly.

Boot Up
=======

To get started, insert a SD card in the Raspberry Pi 4 (or switch to normal mode if using internal storage) and power it up. Wait a few minutes and keep an eye on your router for a new IP address to appear. It may take some time as the system is performing several tasks. If you have a monitor and keyboard available, you can observe the installation progress.

We hope that you will be able to successfully SSH into the new IPs using the **login credentials: username: root and password: dietpi.**

In our case, the nodes were set up to be on:

*   10.0.0.60
*   10.0.0.61
*   10.0.0.62
*   10.0.0.63

**We will still need to do some minor tweaks before installing K3s.**

You can actually ssh to the nodes before they are ready, and you will get this welcoming message:

![diet_pi_install.png](https://help.turingpi.com/hc/article_attachments/9046185831837)

Please wait until the setup is fully completed, and you see a screen like this:

![diet_pi_done.png](https://help.turingpi.com/hc/article_attachments/9046258502301)

Finishing the setup
-------------------

With your favorite ssh client, log in to all 4 nodes.

You can check if the OS is up-to-date with (It should be, as the update is done automatically during install.):

apt update   
apt upgrade

**Next**, on every node, edit file **/etc/hosts** to look like this (matching your IP setting, of course). You can use **_nano /etc/hosts_** or **_vi /etc/hosts_**

127.0.0.1 localhost  
10.0.0.60 cube01 cube01.local  
10.0.0.61 cube02 cube02.local  
10.0.0.62 cube03 cube03.local  
10.0.0.63 cube04 cube04.local

This will allow us to use node names instead IPs. For example:

root@cube01:~# **ping cube02**  
PING cube02 (10.0.0.61) 56(84) bytes of data.  
64 bytes from cube02 (10.0.0.61): icmp\_seq=1 ttl=64 time=0.335 ms  
64 bytes from cube02 (10.0.0.61): icmp\_seq=2 ttl=64 time=0.135 ms  
64 bytes from cube02 (10.0.0.61): icmp\_seq=3 ttl=64 time=0.110 ms  
\--- cube02 ping statistics ---  
3 packets transmitted, 3 received, 0% packet loss, time 2036ms  
rtt min/avg/max/mdev = 0.110/0.193/0.335/0.100 ms  
root@cube01:~# 

And the last thing we need it to **install iptables** on **all nodes:**

apt -y install iptables

We are ready to install Kubernetes.