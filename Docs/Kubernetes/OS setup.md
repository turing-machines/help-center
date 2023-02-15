<p>While the choice of operating system for a Kubernetes setup is not crucial from a Kubernetes perspective, the correct choice of OS can greatly simplify post-install tasks.</p>
<p>For this Kubernetes setup, we have chosen DietPi as our operating system. For its ease of setup and availability of automatic bootstrap options, enabling quick and effortless preparation of the nodes.</p>
<p>You can download the DietPi ISO image <strong><a href="https://dietpi.com/#downloadinfo" target="_blank" rel="noopener noreferrer">HERE</a></strong>. <strong>Choose the Raspberry Pi 2/3/4</strong> version.</p>
<p>To flash the ISO onto the Raspberry Pi CM4 module, you need to use your preferred operating system and follow the guide provided (<a href="https://help.turingpi.com/hc/en-us/articles/8687165986205" target="_blank" rel="noopener noreferrer">see guide link</a>). If you are using CM4 modules with SD card, you may use an SD card reader.</p>
<p>Instead of choosing one of the images option in <strong>Raspberry Pi Imager</strong>, scroll down and choose "<strong>Use custom</strong>".</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9042999711261" alt="use_custom.png"></p>
<p class="wysiwyg-text-align-left">And choose the downloaded DietPi image.</p>
<p class="wysiwyg-text-align-left">The remaining steps to complete the flashing process will be the same as outlined in our "<a href="https://help.turingpi.com/hc/en-us/articles/8687165986205" target="_blank" rel="noopener noreferrer">Install OS</a>" guide, <strong>with one exception</strong>. Instead of utilizing the customization options, the image will be directly written to the CM4 storage without any modifications.</p>
<p class="wysiwyg-text-align-left">After completing the flashing process, you will have a single partition available for writing (In windows, since it's FAT23, other OS can access the main partition as well) with a size of 126 MB. This partition serves as the boot disk for DietPi. <strong>It may be necessary to unplug and reinsert the storage device for it to be recognized.</strong></p>
<p class="wysiwyg-text-align-left">It should contain files like this:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9044106102173" alt="dietpi_files.png"></p>
<p class="wysiwyg-text-align-left">We are interested in files called: <strong>dietpi.txt and cmdline.txt<br></strong></p>
<h1 class="wysiwyg-text-align-left">dietpi.txt</h1>
<p>This file enables the streamlined, automated bootstrapping process of the DietPi OS. It facilitates the setup of the network, SSH, and the installation of additional software. Although it has the capability to install Kubernetes automatically, we will proceed with the manual installation later on.</p>
<p>For a thorough explanation of this file, please refer to the link provided <strong><a href="https://github.com/MichaIng/DietPi/blob/master/dietpi.txt" target="_blank" rel="noopener noreferrer">HERE.</a></strong></p>
<p><strong>Edit the file and replace it content with (read below for variables to change):</strong></p>
<pre>AUTO_SETUP_ACCEPT_LICENSE=1<br>AUTO_SETUP_LOCALE=C.UTF-8<br>AUTO_SETUP_KEYBOARD_LAYOUT=us<br>AUTO_SETUP_TIMEZONE=Europe/Bratislava<br>AUTO_SETUP_NET_ETHERNET_ENABLED=1<br>AUTO_SETUP_NET_WIFI_ENABLED=0<br>AUTO_SETUP_NET_ETH_FORCE_SPEED=0<br>AUTO_SETUP_NET_WIFI_COUNTRY_CODE=SK<br><br>AUTO_SETUP_NET_USESTATIC=1<br>AUTO_SETUP_NET_STATIC_IP=10.0.0.60<br>AUTO_SETUP_NET_STATIC_MASK=255.255.255.0<br>AUTO_SETUP_NET_STATIC_GATEWAY=10.0.0.254<br>AUTO_SETUP_NET_STATIC_DNS=1.1.1.1 8.8.8.8<br><br>AUTO_SETUP_DHCP_TO_STATIC=0<br><br>AUTO_SETUP_NET_HOSTNAME=cube01<br><br>AUTO_SETUP_BOOT_WAIT_FOR_NETWORK=1<br>AUTO_SETUP_SWAPFILE_SIZE=1<br>AUTO_SETUP_SWAPFILE_LOCATION=/var/swap<br>AUTO_SETUP_HEADLESS=1<br>AUTO_UNMASK_LOGIND=0<br>AUTO_SETUP_CUSTOM_SCRIPT_EXEC=0<br>AUTO_SETUP_BACKUP_RESTORE=0<br>AUTO_SETUP_SSH_SERVER_INDEX=-2<br>AUTO_SETUP_LOGGING_INDEX=-1<br>AUTO_SETUP_RAMLOG_MAXSIZE=50<br><br>AUTO_SETUP_WEB_SERVER_INDEX=0<br>AUTO_SETUP_DESKTOP_INDEX=0<br>AUTO_SETUP_BROWSER_INDEX=0<br>AUTO_SETUP_AUTOSTART_TARGET_INDEX=7<br>AUTO_SETUP_AUTOSTART_LOGIN_USER=root<br>AUTO_SETUP_GLOBAL_PASSWORD=dietpi<br>AUTO_SETUP_AUTOMATED=1<br>SURVEY_OPTED_IN=0<br><br>#OpenSSH Client<br>AUTO_SETUP_INSTALL_SOFTWARE_ID=0<br>#Samba Client<br>AUTO_SETUP_INSTALL_SOFTWARE_ID=1<br>#vim<br>AUTO_SETUP_INSTALL_SOFTWARE_ID=20<br>#RPi.GPIO<br>AUTO_SETUP_INSTALL_SOFTWARE_ID=69<br>#OpenSSH Server<br>AUTO_SETUP_INSTALL_SOFTWARE_ID=105<br>#Python 3 pip<br>AUTO_SETUP_INSTALL_SOFTWARE_ID=130<br><br>CONFIG_CPU_GOVERNOR=schedutil<br>CONFIG_CPU_ONDEMAND_SAMPLE_RATE=25000<br>CONFIG_CPU_ONDEMAND_SAMPLE_DOWNFACTOR=40<br>CONFIG_CPU_USAGE_THROTTLE_UP=50<br><br>CONFIG_CPU_MAX_FREQ=Disabled<br>CONFIG_CPU_MIN_FREQ=Disabled<br><br>CONFIG_CPU_DISABLE_TURBO=0<br><br>CONFIG_PROXY_ADDRESS=MyProxyServer.com<br>CONFIG_PROXY_PORT=8080<br>CONFIG_PROXY_USERNAME=<br>CONFIG_PROXY_PASSWORD=<br><br>CONFIG_G_CHECK_URL_TIMEOUT=10<br>CONFIG_G_CHECK_URL_ATTEMPTS=5<br>CONFIG_CHECK_CONNECTION_IP=8.8.8.8<br>CONFIG_CHECK_CONNECTION_IPV6=2620:fe::fe<br>CONFIG_CHECK_DNS_DOMAIN=google.com<br><br>CONFIG_CHECK_DIETPI_UPDATES=1<br>CONFIG_CHECK_APT_UPDATES=1<br>CONFIG_NTP_MODE=2<br>CONFIG_SERIAL_CONSOLE_ENABLE=1<br>CONFIG_SOUNDCARD=none<br>CONFIG_LCDPANEL=none<br>CONFIG_ENABLE_IPV6=0<br><br>CONFIG_APT_RASPBIAN_MIRROR=http://raspbian.raspberrypi.org/raspbian/<br>CONFIG_APT_DEBIAN_MIRROR=https://deb.debian.org/debian/<br>CONFIG_NTP_MIRROR=debian.pool.ntp.org<br><br>#------------------------------------------------------------------------------------------------------<br>##### DietPi-Software settings #####<br>#------------------------------------------------------------------------------------------------------<br>SOFTWARE_DISABLE_SSH_PASSWORD_LOGINS=0<br><br>#------------------------------------------------------------------------------------------------------<br>##### Dev settings #####<br>#------------------------------------------------------------------------------------------------------<br>DEV_GITBRANCH=master<br>DEV_GITOWNER=MichaIng<br><br>#------------------------------------------------------------------------------------------------------<br>##### Settings, automatically added by dietpi-update #####<br>#------------------------------------------------------------------------------------------------------<br><br><br></pre>
<p>Of course, we need to <span class="wysiwyg-color-red"><strong>change some variables</strong></span>, most of them stay the same for all four nodes but these you should change:</p>
<ul>
<li>
<strong>AUTO_SETUP_TIMEZONE</strong>=Europe/Bratislava
<ul>
<li>Setup to your Time Zone, list of TimeZones is <a href="https://en.wikipedia.org/wiki/List_of_tz_database_time_zones" target="_blank" rel="noopener noreferrer">HERE.</a>
</li>
</ul>
</li>
<li>
<strong>AUTO_SETUP_NET_STATIC_IP</strong>=10.0.0.60
<ul>
<li>Set static IP for your Node</li>
</ul>
</li>
<li>
<strong>AUTO_SETUP_NET_STATIC_MASK</strong>=255.255.255.0
<ul>
<li>Set Mask (your network specific)</li>
</ul>
</li>
<li>
<strong>AUTO_SETUP_NET_STATIC_GATEWAY</strong>=10.0.0.254
<ul>
<li>Set Gateway (your network specific)</li>
</ul>
</li>
<li>
<strong>AUTO_SETUP_NET_HOSTNAME</strong>=cube01
<ul>
<li>Set hostname, we will go simple here. cube01-04</li>
</ul>
</li>
<li>
<strong>AUTO_SETUP_GLOBAL_PASSWORD</strong>=dietpi
<ul>
<li>Set your initial root password</li>
</ul>
</li>
</ul>
<h1>cmdline.txt</h1>
<p>This is another file that requires editing. It consists of a single line of text, and we will append the following information to the end of that line.</p>
<pre>group_enable=cpuset cgroup_enable=memory cgroup_memory=1</pre>
<p>So the final line looks something like:</p>
<pre>root=PARTUUID=a97c7839-02 rootfstype=ext4 rootwait fsck.repair=yes net.ifnames=0 logo.nologo console=serial0,115200 console=tty1 group_enable=cpuset cgroup_enable=memory cgroup_memory=1</pre>
<p><strong>Do not copy over the whole line from here,</strong> just append the:</p>
<pre>group_enable=cpuset cgroup_enable=memory cgroup_memory=1</pre>
<p>These variables ensure container software will work properly.</p>
<h1>Boot Up</h1>
<p>To get started, insert a SD card in the Raspberry Pi 4 (or switch to normal mode if using internal storage) and power it up. Wait a few minutes and keep an eye on your router for a new IP address to appear. It may take some time as the system is performing several tasks. If you have a monitor and keyboard available, you can observe the installation progress.</p>
<p>We hope that you will be able to successfully SSH into the new IPs using the<strong> login credentials: username: root and password: dietpi.</strong></p>
<p>In our case, the nodes were set up to be on:</p>
<ul>
<li>10.0.0.60</li>
<li>10.0.0.61</li>
<li>10.0.0.62</li>
<li>10.0.0.63</li>
</ul>
<p><strong>We will still need to do some minor tweaks before installing K3s.</strong></p>
<p>You can actually ssh to the nodes before they are ready, and you will get this welcoming message:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9046185831837" alt="diet_pi_install.png"></p>
<p class="wysiwyg-text-align-left">Please wait until the setup is fully completed, and you see a screen like this:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9046258502301" alt="diet_pi_done.png"></p>
<p class="wysiwyg-text-align-left"> </p>
<h2 class="wysiwyg-text-align-left">Finishing the setup</h2>
<p>With your favorite ssh client, log in to all 4 nodes.</p>
<p class="wysiwyg-text-align-left">You can check if the OS is up-to-date with (It should be, as the update is done automatically during install.):</p>
<pre class="wysiwyg-text-align-left">apt update <br>apt upgrade</pre>
<p><strong>Next</strong>, on every node, edit file<strong> /etc/hosts</strong> to look like this (matching your IP setting, of course). You can use <strong><em>nano /etc/hosts</em></strong> or <strong><em>vi /etc/hosts</em></strong></p>
<pre class="wysiwyg-text-align-left">127.0.0.1 localhost<br>10.0.0.60 cube01 cube01.local<br>10.0.0.61 cube02 cube02.local<br>10.0.0.62 cube03 cube03.local<br>10.0.0.63 cube04 cube04.local</pre>
<p>This will allow us to use node names instead IPs. For example:</p>
<pre>root@cube01:~# <strong>ping cube02</strong><br>PING cube02 (10.0.0.61) 56(84) bytes of data.<br>64 bytes from cube02 (10.0.0.61): icmp_seq=1 ttl=64 time=0.335 ms<br>64 bytes from cube02 (10.0.0.61): icmp_seq=2 ttl=64 time=0.135 ms<br>64 bytes from cube02 (10.0.0.61): icmp_seq=3 ttl=64 time=0.110 ms<br>--- cube02 ping statistics ---<br>3 packets transmitted, 3 received, 0% packet loss, time 2036ms<br>rtt min/avg/max/mdev = 0.110/0.193/0.335/0.100 ms<br>root@cube01:~# </pre>
<p>And the last thing we need it to<strong> install iptables</strong> on <strong>all nodes:</strong></p>
<pre>apt -y install iptables</pre>
<p>We are ready to install Kubernetes.</p>