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

We are interested in files called: **dietpi.txt and cmdline.txt**

dietpi.txt
==========

This file enables the streamlined, automated bootstrapping process of the DietPi OS. It facilitates the setup of the network, SSH, and the installation of additional software. Although it has the capability to install Kubernetes automatically, we will proceed with the manual installation later on.

For a thorough explanation of this file, please refer to the link provided **[HERE.](https://github.com/MichaIng/DietPi/blob/master/dietpi.txt)**

**Edit the file and replace it content with (read below for variables to change):**

```
AUTO_SETUP_ACCEPT_LICENSE=1AUTO_SETUP_LOCALE=C.UTF-8AUTO_SETUP_KEYBOARD_LAYOUT=usAUTO_SETUP_TIMEZONE=Europe/BratislavaAUTO_SETUP_NET_ETHERNET_ENABLED=1AUTO_SETUP_NET_WIFI_ENABLED=0AUTO_SETUP_NET_ETH_FORCE_SPEED=0AUTO_SETUP_NET_WIFI_COUNTRY_CODE=SKAUTO_SETUP_NET_USESTATIC=1AUTO_SETUP_NET_STATIC_IP=10.0.0.60AUTO_SETUP_NET_STATIC_MASK=255.255.255.0AUTO_SETUP_NET_STATIC_GATEWAY=10.0.0.254AUTO_SETUP_NET_STATIC_DNS=1.1.1.1 8.8.8.8AUTO_SETUP_DHCP_TO_STATIC=0AUTO_SETUP_NET_HOSTNAME=cube01AUTO_SETUP_BOOT_WAIT_FOR_NETWORK=1AUTO_SETUP_SWAPFILE_SIZE=1AUTO_SETUP_SWAPFILE_LOCATION=/var/swapAUTO_SETUP_HEADLESS=1AUTO_UNMASK_LOGIND=0AUTO_SETUP_CUSTOM_SCRIPT_EXEC=0AUTO_SETUP_BACKUP_RESTORE=0AUTO_SETUP_SSH_SERVER_INDEX=-2AUTO_SETUP_LOGGING_INDEX=-1AUTO_SETUP_RAMLOG_MAXSIZE=50AUTO_SETUP_WEB_SERVER_INDEX=0AUTO_SETUP_DESKTOP_INDEX=0AUTO_SETUP_BROWSER_INDEX=0AUTO_SETUP_AUTOSTART_TARGET_INDEX=7AUTO_SETUP_AUTOSTART_LOGIN_USER=rootAUTO_SETUP_GLOBAL_PASSWORD=dietpiAUTO_SETUP_AUTOMATED=1SURVEY_OPTED_IN=0#OpenSSH ClientAUTO_SETUP_INSTALL_SOFTWARE_ID=0#Samba ClientAUTO_SETUP_INSTALL_SOFTWARE_ID=1#vimAUTO_SETUP_INSTALL_SOFTWARE_ID=20#RPi.GPIOAUTO_SETUP_INSTALL_SOFTWARE_ID=69#OpenSSH ServerAUTO_SETUP_INSTALL_SOFTWARE_ID=105#Python 3 pipAUTO_SETUP_INSTALL_SOFTWARE_ID=130CONFIG_CPU_GOVERNOR=schedutilCONFIG_CPU_ONDEMAND_SAMPLE_RATE=25000CONFIG_CPU_ONDEMAND_SAMPLE_DOWNFACTOR=40CONFIG_CPU_USAGE_THROTTLE_UP=50CONFIG_CPU_MAX_FREQ=DisabledCONFIG_CPU_MIN_FREQ=DisabledCONFIG_CPU_DISABLE_TURBO=0CONFIG_PROXY_ADDRESS=MyProxyServer.comCONFIG_PROXY_PORT=8080CONFIG_PROXY_USERNAME=CONFIG_PROXY_PASSWORD=CONFIG_G_CHECK_URL_TIMEOUT=10CONFIG_G_CHECK_URL_ATTEMPTS=5CONFIG_CHECK_CONNECTION_IP=8.8.8.8CONFIG_CHECK_CONNECTION_IPV6=2620:fe::feCONFIG_CHECK_DNS_DOMAIN=google.comCONFIG_CHECK_DIETPI_UPDATES=1CONFIG_CHECK_APT_UPDATES=1CONFIG_NTP_MODE=2CONFIG_SERIAL_CONSOLE_ENABLE=1CONFIG_SOUNDCARD=noneCONFIG_LCDPANEL=noneCONFIG_ENABLE_IPV6=0CONFIG_APT_RASPBIAN_MIRROR=http://raspbian.raspberrypi.org/raspbian/CONFIG_APT_DEBIAN_MIRROR=https://deb.debian.org/debian/CONFIG_NTP_MIRROR=debian.pool.ntp.org#------------------------------------------------------------------------------------------------------##### DietPi-Software settings ######------------------------------------------------------------------------------------------------------SOFTWARE_DISABLE_SSH_PASSWORD_LOGINS=0#------------------------------------------------------------------------------------------------------##### Dev settings ######------------------------------------------------------------------------------------------------------DEV_GITBRANCH=masterDEV_GITOWNER=MichaIng#------------------------------------------------------------------------------------------------------##### Settings, automatically added by dietpi-update ######------------------------------------------------------------------------------------------------------
```

Of course, we need to **change some variables**, most of them stay the same for all four nodes but these you should change:

- **AUTO\_SETUP\_TIMEZONE**=Europe/Bratislava 
    - Setup to your Time Zone, list of TimeZones is [HERE.](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
- **AUTO\_SETUP\_NET\_STATIC\_IP**=10.0.0.60 
    - Set static IP for your Node
- **AUTO\_SETUP\_NET\_STATIC\_MASK**=255.255.255.0 
    - Set Mask (your network specific)
- **AUTO\_SETUP\_NET\_STATIC\_GATEWAY**=10.0.0.254 
    - Set Gateway (your network specific)
- **AUTO\_SETUP\_NET\_HOSTNAME**=cube01 
    - Set hostname, we will go simple here. cube01-04
- **AUTO\_SETUP\_GLOBAL\_PASSWORD**=dietpi 
    - Set your initial root password

cmdline.txt
===========

This is another file that requires editing. It consists of a single line of text, and we will append the following information to the end of that line.

```
group_enable=cpuset cgroup_enable=memory cgroup_memory=1
```

So the final line looks something like:

```
root=PARTUUID=a97c7839-02 rootfstype=ext4 rootwait fsck.repair=yes net.ifnames=0 logo.nologo console=serial0,115200 console=tty1 group_enable=cpuset cgroup_enable=memory cgroup_memory=1
```

**Do not copy over the whole line from here,** just append the:

```
group_enable=cpuset cgroup_enable=memory cgroup_memory=1
```

These variables ensure container software will work properly.

Boot Up
=======

To get started, insert a SD card in the Raspberry Pi 4 (or switch to normal mode if using internal storage) and power it up. Wait a few minutes and keep an eye on your router for a new IP address to appear. It may take some time as the system is performing several tasks. If you have a monitor and keyboard available, you can observe the installation progress.

We hope that you will be able to successfully SSH into the new IPs using the **login credentials: username: root and password: dietpi.**

In our case, the nodes were set up to be on:

- 10.0.0.60
- 10.0.0.61
- 10.0.0.62
- 10.0.0.63

**We will still need to do some minor tweaks before installing K3s.**

You can actually ssh to the nodes before they are ready, and you will get this welcoming message:

![diet_pi_install.png](https://help.turingpi.com/hc/article_attachments/9046185831837)

Please wait until the setup is fully completed, and you see a screen like this:

![diet_pi_done.png](https://help.turingpi.com/hc/article_attachments/9046258502301)

Finishing the setup
-------------------

With your favorite ssh client, log in to all 4 nodes.

You can check if the OS is up-to-date with (It should be, as the update is done automatically during install.):

```
apt update apt upgrade
```

**Next**, on every node, edit file **/etc/hosts** to look like this (matching your IP setting, of course). You can use ***nano /etc/hosts*** or ***vi /etc/hosts***

```
127.0.0.1 localhost10.0.0.60 cube01 cube01.local10.0.0.61 cube02 cube02.local10.0.0.62 cube03 cube03.local10.0.0.63 cube04 cube04.local
```

This will allow us to use node names instead IPs. For example:

```
root@cube01:~# ping cube02PING cube02 (10.0.0.61) 56(84) bytes of data.64 bytes from cube02 (10.0.0.61): icmp_seq=1 ttl=64 time=0.335 ms64 bytes from cube02 (10.0.0.61): icmp_seq=2 ttl=64 time=0.135 ms64 bytes from cube02 (10.0.0.61): icmp_seq=3 ttl=64 time=0.110 ms--- cube02 ping statistics ---3 packets transmitted, 3 received, 0% packet loss, time 2036msrtt min/avg/max/mdev = 0.110/0.193/0.335/0.100 msroot@cube01:~# 
```

And the last thing we need it to **install iptables** on **all nodes:**

```
apt -y install iptables
```

We are ready to install Kubernetes.