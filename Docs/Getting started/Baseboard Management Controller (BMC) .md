<h1>What is it?</h1>
<p><strong>Note:</strong> some users reported that the RTC battery may restart or interfere with starting the BMC. We are investigating it at the moment. </p>
<p>BMC, or Baseboard Management Controller, is a specialized microcontroller that is built into the motherboard of a computer or server. It is responsible for monitoring the system's hardware and providing remote management capabilities, such as power control or temperature monitoring.</p>
<p>Our Turing Pi V2 board is equipped with an Allwinner T113-S3, which provides BMC functionality. This is essentially a mini-computer built into the motherboard. It is equipped with features you would expect from a PC such as a dual-core processor, RAM, and a hard drive, albeit in limited capacity. The best part is that it runs on <span class="wysiwyg-color-green110"><strong>Linux OS</strong></span>, yes, really!</p>
<ul>
<li>
<strong>Baseboard Management Controller:</strong> Allwinner T113-S3</li>
<li>ARM Cortex-A7 Dual-Core</li>
<li>128MB DDR3</li>
<li>1GB NAND Flash Memory (MX35LF1GE4AB)</li>
<li>SD Card slot</li>
</ul>
<p>And since BMC is integrated with most of the features of the board, it gives <strong>us </strong>and <strong>you</strong> an excellent platform for automatization, monitoring and management without need of diving in to very low level programming.</p>
<h1>Open Source</h1>
<p><strong>BMC Firmware is an open source project.</strong> We are continuously working and improving our BMC project to give you a solid base for the best possible experience and flexibility when it comes to managing and controlling your system's hardware. With our open source approach, anyone can access the source code and make modifications to suit their specific needs. We believe that this level of transparency and collaboration will lead to a stronger and more robust BMC firmware for everyone to use.</p>
<h3 class="LC20lb MBeuO DKV0Md">👉 <strong>GitHub Repo: </strong><a href="https://github.com/wenyi0421/turing-pi" target="_blank" rel="noopener noreferrer">HERE</a>
</h3>
<p> </p>
<p>How to build and modify the BMC Firmware will be discussed on its own dedicated page in "Advance documentation"</p>
<h1>How do I access it ?</h1>
<p>When you apply power to the Turing Pi V2 board, BMC will start automatically and go through its boot process.</p>
<h2>Serial Console</h2>
<p>If you want to observe the process from the start, you will need a USB to TTL converter cable, which will allow you to connect to the BMC's serial console and view the system's boot process, kernel messages, and other debug information. This can be extremely useful for troubleshooting issues, and also for understanding how the BMC firmware works and how it interacts with the rest of the system. Additionally, the USB to TTL converter cable will allow you to access the BMC's command line interface (CLI) and make configuration changes, if necessary. The username and password are the same as for SSH access.</p>
<p> </p>
<p>Observing the boot up process via Serial Console also helps to quickly identify assigned IP to BMC</p>
<ul>
<li>
<strong>User</strong>: root</li>
<li>
<strong>Password:</strong> turing</li>
</ul>
<h3>Windows USB to TTL:</h3>
<div class="flex flex-grow flex-col gap-3">
<div class="min-h-[20px] flex flex-col items-start gap-4 whitespace-pre-wrap">
<div class="markdown prose w-full break-words dark:prose-invert result-streaming light">
<p>Here is a short tutorial on how to use a USB to TTL converter cable with Putty on Windows:</p>
<ol>
<li>Connect the USB to TTL converter cable to your computer's USB port and to the serial port on the BMC. The exact number of pins on your USB to TTL cable can be different, but the general idea is:</li>
</ol>
<ul>
<li>GND -&gt; GND</li>
<li>TXD -&gt; RXD</li>
<li>RXD -&gt; TXD</li>
</ul>
<p><br><img src="https://help.turingpi.com/hc/article_attachments/8840851524125" alt="usb_ttl.png"></p>
<p>1. When plugged in, it should show as a COM port in Device Manager</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8840980020765" alt="usb_com.png"></p>
<p>2. Download and install Putty, a free and open-source terminal emulator, from the official website (putty.org).</p>
<p>3. Open Putty and select the Serial option under Connection Type.</p>
<p>4. In the Serial line field, enter the name of the serial port that the USB to TTL converter cable is connected to (e.g. "COM9")</p>
<p>5. In the Speed field, enter the baud rate of the serial connection: 115200</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8840530892445" alt="putty.png"></p>
<p>6. Click the Open button to open the connection to BMC's serial console.</p>
<p>7. You should now be able to view the system's boot process, kernel messages, and other debug information in the Putty terminal window. You might need to press the "BMC Reset" button to have it reboot and print all the messages again.</p>
<p>8. To access BMC's command line interface (CLI), you will need to enter the appropriate login credentials when prompted.</p>
<p>9. Once you are logged in, you can run Linux commands and make configuration changes as necessary.</p>
<p><strong>Note:</strong> The exact setup and configuration steps may vary depending on the device and the specific USB to TTL converter cable you are using. We recommend checking the device's documentation for more information.</p>
<p> </p>
</div>
</div>
</div>
<h3>Linux USB to TTL:</h3>
<ol>
<li>Connect the USB to TTL converter cable to your computer's USB port and to the serial port on the BMC.</li>
<li>Open a terminal window and use the "dmesg" command to determine the name of the serial port that the USB to TTL converter cable is connected to. It should be something like "/dev/ttyUSB0" or "/dev/ttyACM0".</li>
<li>Use the "screen" command to open a session on the serial port. For example: "screen /dev/ttyUSB0 115200" or "screen /dev/ttyACM0 115200"</li>
<li>If prompted, press enter to agree to the device's baud rate, which is 115200.</li>
<li>You should now be able to view the system's boot process, kernel messages, and other debug information in the terminal window.</li>
<li>To access BMC's command line interface (CLI), you will need to enter the appropriate login credentials when prompted.</li>
<li>Once you are logged in, you can run commands and make configuration changes as necessary.</li>
</ol>
<h3>Mac USB to TTL:</h3>
<ol>
<li>
<p>Connect the USB to TTL converter cable to your computer's USB port and to the serial port on the BMC.</p>
</li>
<li>
<p>Open the terminal window and use the "ls /dev/cu.*" command to determine the name of the serial port that the USB to TTL converter cable is connected to. It should be something like "/dev/cu.usbserial" or "/dev/cu.usbmodem"</p>
</li>
<li>
<p>Use the "screen" command to open a session on the serial port. For example: "screen /dev/cu.usbserial 115200" or "screen /dev/cu.usbmodem 115200"</p>
</li>
<li>
<p>If prompted, press enter to agree to the device's baud rate, which is 115200.</p>
</li>
<li>
<p>You should now be able to view the system's boot process, kernel messages, and other debug information in the terminal window.</p>
</li>
<li>
<p>To access BMC's command line interface (CLI), you will need to enter the appropriate login credentials when prompted.</p>
</li>
<li>
<p>Once you are logged in, you can run commands and make configuration changes as necessary.</p>
</li>
</ol>
<p><strong>Note:</strong> On MacOS, you may also use serial terminal software such as CoolTerm, Serial, or ZTerm.</p>
<h2>SSH</h2>
<p>BMC after booting up will start a standard SSH server on port 22. You can use any of your favorite SSH client to log in.</p>
<ul>
<li>
<strong>User</strong>: root</li>
<li>
<strong>Password:</strong> turing</li>
</ul>
<p><strong>Note: </strong>If you have an older version of BMC Firmware, ssh access as root is not allowed and needs to be changed via Serial Console. You can either flash the new version of BMC Firmware or use the Serial Console to log in and change the settings with the following one-liner:</p>
<p> </p>
<pre>sed -i.bkp -E 's/^#?PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config &amp;&amp; \<br>/etc/init.d/S50sshd restart</pre>
<p> </p>
<h1>Determine BMC IP</h1>
<p>Before you can SSH, interact with WWW interface or API provided by BMC, you need to determine its IP on your network. <strong>Currently, at the time of writing the BMC is getting random MAC address at every boot, which will result in a new IP assigned to BMC every reboot.<br></strong></p>
<p> </p>
<p>The easiest way to get the IP of BMC on Linux, Windows and Mac is to install nmap. For windows, you can get nmap binary, but we would recommend using the Windows Subsystem for Linux (WSL) (<a href="https://learn.microsoft.com/en-us/windows/wsl/install" target="_blank" rel="noopener noreferrer">Tutorial on how to turn it on</a>)</p>
<p> </p>
<h3>Linux/Mac/WSL Windows one line script</h3>
<p>Unplug the Turing Pi V2 from the network first, run the following one line script and plug it back in when asked. You will need nmap and replace the 10.0.0.* with your network range.</p>
<p>One-liner:</p>
<p> </p>
<pre>range="10.0.0.*"; echo "Please wait, starting scan..."; \<br>nmap -PN -p 22,80 --open -oG - "$range" | grep -E '22/open.*80/open' | \<br>awk '{print $2}' | sort -V &gt; initial_scan.txt; \<br>echo "Plug in BMC, waiting 30sec..."; sleep 30; \<br>nmap -PN -p 22,80 --open -oG - "$range" | \<br>grep -E '22/open.*80/open' | awk '{print $2}' | \<br>sort -V | diff initial_scan.txt -</pre>
<p>Example:</p>
<pre>vladoportos@DESKTOP-7BCS3LE:~$ range="10.0.0.*"; echo "Please wait, starting scan..." ; nmap -PN -p 22,80 --open -oG - "$range" | grep -E '22/open.*80/open' | awk '{print $2}' | sort -V &gt; initial_scan.txt; echo "Plug in BMC, waiting 30sec..."; sleep 30; nmap -PN -p 22,80 --open -oG - "$range" | grep -E '22/open.*80/open' | awk '{print $2}' | sort -V | diff initial_scan.txt -<br>Please wait, starting scan...<br>Plug in BMC, waiting 30sec...<br>2a3<br>&gt; 10.0.0.53</pre>
<p>As you can see above, the detected IP was 10.0.0.53. The script scans in the IP range specified, and stores all IPs on the network that have ports 80 and 22 open. Then it will ask you to plug in the Turing Pi V2, wait 30 seconds and do the scan again. Finally, compare the results and print out the new IPs</p>
<p>You should be able to ssh or even visit port 80 with your web browser of choice.</p>
<h1>Set Fixed IP</h1>
<p>Once you are logged in, you might want to set up a fixed IP for your BMC.</p>
<p>In Buildroot Linux OS, you can set a fixed IP, netmask, and gateway by configuring the network settings in the root filesystem. Here are the general steps to set a fixed IP, netmask, and gateway:</p>
<p><strong>Configure the network settings:</strong><br>You can configure the network settings in the file <code>/etc/network/interfaces</code> in the root filesystem. For example, to set a fixed IP of 192.168.1.100, netmask of 255.255.255.0, and gateway of 192.168.1.1 on the eth0 interface, you can add the following lines to the file using <code>vi</code> as an editor.</p>
<p>Currently the original file looks like this:</p>
<pre class="bg-black mb-4 rounded-md"># interface file auto-generated by buildroot<br><br>auto lo<br>iface lo inet loopback<br><br>auto eth0<br>iface eth0 inet dhcp<br>  pre-up /etc/network/nfs_check<br>  wait-delay 15<br>  hostname $(hostname)</pre>
<p>Edit it to look like this:</p>
<pre># interface file auto-generated by buildroot<br><br>auto lo<br>iface lo inet loopback<br><br>auto eth0<br>iface eth0 inet static<br>  address 192.168.1.100<br>  netmask 255.255.255.0<br>  gateway 192.168.1.1<br>  pre-up /etc/network/nfs_check<br>  wait-delay 15<br>  hostname $(hostname)</pre>
<p> </p>
<p><strong>Configure DNS settings</strong>:</p>
<p>You can configure the DNS settings in the file <code>/etc/resolv.conf</code> in the root filesystem.</p>
<p>For example, setting the DNS server to 8.8.8.8</p>
<p>Currently the original file looks like this:</p>
<pre>search localdomain # eth0<br>nameserver 1.1.1.1 # eth0<br>nameserver 1.0.0.1 # eth0<br>nameserver 4.4.4.4 # eth0</pre>
<p>Change it to:</p>
<div class="flex items-center relative text-gray-200 bg-gray-800 px-4 py-2 text-xs font-sans">search localdomain # eth0</div>
<pre class="bg-black mb-4 rounded-md">nameserver 8.8.8.8 # eth0</pre>
<div class="bg-black mb-4 rounded-md">
<div class="p-4 overflow-y-auto"> </div>
</div>
<p><strong>Restart the network: </strong></p>
<p>After configuring the network settings, you will need to restart the network service. You can do this by running this command: <code>/etc/init.d/S40network restart</code></p>
<p><strong>Check if the IP is set correctly:</strong></p>
<p>You can use commands like <code>ip -a</code>, <code>ifconfig</code>or <code>route -n</code>to check the current IP configuration of the device, you should see that the IP, netmask, and gateway are set to the values you configured.</p>
<p>These settings will persist during the restart of BMC.</p>
<p> </p>
<h1>Set Fixed MAC</h1>
<p>If you prefer to manage IP assignments from your router instead of setting a fixed IP address, you can still set a fixed MAC address for the network interface on BMC. To do this, you need to edit the <code>/etc/network/interfaces</code> file in a similar way as you would for setting a fixed IP address.</p>
<p>Edit it to look like this:</p>
<pre># interface file auto-generated by buildroot<br><br>auto lo<br>iface lo inet loopback<br><br>auto eth0<br>iface eth0 inet dhcp<br>  hwaddress ether 12:34:56:78:9A:BC<br>  pre-up /etc/network/nfs_check<br>  wait-delay 15<br>  hostname $(hostname)<br><br></pre>
<p>This will assign a MAC address: 12:34:56:78:9A:BC to the interface. You can easily generate random MAC address for example with this one line script:</p>
<pre>printf '%02x:%02x:%02x:%02x:%02x:%02x\n' $((0x02)) $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256))</pre>
<p><span class="wysiwyg-color-red">But you need to make sure that the assigned MAC is not already used by any device on the network. </span></p>
<h1>Set HW clock</h1>
<p>At the time of writing, there is no ntp client installed in BMC. However, you can quickly set up the clock and HW clock with the following one-liner. Just replace the "Europe/Bratislava" for your location.</p>
<pre>date -s @"$(curl -s "http://worldtimeapi.org/api/timezone/Europe/Bratislava" | \<br>sed -n 's/.*"unixtime":\([0-9]*\).*/\1/p')"<br>hwclock --systohc</pre>
<p>Note: worldtimeapi.org tends to not respond every single time, so if you get error, try again few times.</p>
<p> </p>
<h1>BMC Linux OS</h1>
<div class="flex flex-grow flex-col gap-3">
<div class="min-h-[20px] flex flex-col items-start gap-4 whitespace-pre-wrap">
<div class="markdown prose w-full break-words dark:prose-invert light">The Turing Pi V2 board uses a Builroot-compiled Linux OS that offers basic tools, similar if not identical to other Linux distributions. However, it does not have a package system and adding new software generally requires recompiling and re-flashing. The internal eMMC memory should not be used for persistent storage for your scripts or ISO images. Instead, please use the SD card interface. The Advanced section will have a separate guide for building and modifying the BMC Linux OS.</div>
<div class="markdown prose w-full break-words dark:prose-invert light">
<br>
<h1>BMC SD Card</h1>
<p>On the back of the Turing Pi V2 board is an SD card slot, this slot is connected to BMC and BMC will attempt to mount it up during its boot process. You can use it as storage for ISO images, custom scripts or programs.</p>
<p>The supported filesystem for SD cards is:</p>
<ul>
<li>Ext2/3 Linux</li>
<li>NTFS (Make sure you eject the card from Windows, or the BMC Linux will complain and mount it read only )</li>
<li>exFAT (recommended for Windows)</li>
<li>FAT32 (not recommended)</li>
</ul>
<p><strong>SD card is automatically mounted to /mnt/sdcard</strong></p>
</div>
<h1 class="markdown prose w-full break-words dark:prose-invert light">How to update BMC</h1>
</div>
</div>
<p>You have two options.</p>
<h2>The first one is OTA (<span class="wysiwyg-color-green120">Preffered</span>).</h2>
<p>OTA (Over-the-Air) firmware update is a method of updating the firmware or software of a device wirelessly, without the need for a physical connection to a computer. In our case, you can update the BMC over the HTTP management website.</p>
<p> </p>
<p>You can enter the IP of the BMC into the browser of your choice, and you should be welcomed by the web interface. You can then switch to update, select update file and submit.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8848581719453" alt="ota-update.png"></p>
<p><strong>You will be able to find updates and releases on our</strong><a href="https://github.com/wenyi0421/turing-pi/releases" target="_blank" rel="noopener noreferrer"><strong> GitHub repository</strong></a>, <span class="wysiwyg-color-red"><strong>OTA files ends with </strong></span><span class="wysiwyg-color-green110"><strong>.</strong></span><span class="wysiwyg-color-green120"><strong>swu</strong></span><span class="wysiwyg-color-red"><strong> extension.</strong></span></p>
<h2>The second option is Flashing IMG file.</h2>
<p>It could happen that you forgot your root password or you were experimenting with custom builds and it came to a situation where you can't use OTA update method.</p>
<p><span class="wysiwyg-color-red"><strong>Note that this part is temporary and we are working on our custom flash method.</strong></span></p>
<p><span class="wysiwyg-color-black">With the help of Phoenix Suit you can connect micro usb port to your PC and flash .img to the BMC. This also requires installing unsigned UsbDrivers (located in UsbDriver folder). <br></span></p>
<p><span class="wysiwyg-color-black"><img src="https://help.turingpi.com/hc/article_attachments/8848969900701" alt="otg_micro.png"></span></p>
<p> </p>
<ul>
<li><span class="wysiwyg-color-black">.IMG will be available on the release page on our <a href="https://github.com/wenyi0421/turing-pi/releases" target="_blank" rel="noopener noreferrer">GitHub.</a></span></li>
</ul>
<p><span class="wysiwyg-color-black"><img src="https://help.turingpi.com/hc/article_attachments/8848793005213" alt="BMC_PhoenixSuit_Connected.png"></span></p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8848793002781" alt="BMC_PhoenixSuit_Flash.png"></p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8848806050973" alt="BMC_PhoenixSuit_Flash_progress.png"></p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8848793143453" alt="BMC_PhoenixSuit_Flash_Done.png"></p>
<h1>ADB Shell</h1>
<p>In addition to the usual method, there is another way to access your BMC (Baseboard Management Controller) in case you have forgotten your password and don't want to go through the hassle of reflashing the entire image. This alternative solution can save you time and effort.</p>
<h2>What is ABD ?</h2>
<p>ADB stands for Android Debug Bridge, which is a command-line tool used to communicate with Android devices, including smartphones and tablets, for development purposes. It enables developers to perform various tasks, such as installing and debugging apps, modifying device settings, and accessing device logs.</p>
<p><strong>The components of ADB include:</strong></p>
<ul>
<li>
<strong>ADB clien</strong>t: a client that runs on the host machine, which can send commands to an ADB server.</li>
<li>
<strong>ADB server</strong>: a server that runs on the host machine, which can receive commands from an ADB client.</li>
<li>
<strong>ADB daemon</strong>: a background process running on an Android device, which can receive commands from an ADB server.</li>
</ul>
<p>The ADB client and server communicate with each other through a socket connection. The ADB client sends commands to the ADB server, which then forwards the commands to the ADB daemon on the Android device. The ADB daemon processes the commands and returns the results back to the ADB server, which then returns the results to the ADB client.</p>
<h2>
<img src="https://help.turingpi.com/hc/article_attachments/9164055363741" alt="mermaid-diagram.png">Install ADB on your PC</h2>
<p>You can follow excellent tutorial by Skanda Hazarika -&gt; <strong><a href="https://www.xda-developers.com/install-adb-windows-macos-linux/" target="_blank" rel="noopener noreferrer">HERE</a> </strong>for installing ADB on Windows, Linux or Mac.</p>
<h2>Connect</h2>
<p>When you switch to the directory with command line, and execute `<strong>adb devices</strong>`, ADB will start the server on your PC.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9164835248669" alt="adb_1.png"></p>
<p>Then use the micro USB port dedicated to BMC and connect it to your PC.</p>
<p>Check for devices again and you should see a new device connected.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9164835426205" alt="adb_2.png"></p>
<p>The serial number of the device in this example is 0402101560. If you want to run ADB commands on a specific device, you can specify the device serial number using the -s option, like this:</p>
<pre>adb -s 0402101560 shell</pre>
<p><img src="https://help.turingpi.com/hc/article_attachments/9165073715101" alt="adb_3.png"></p>
<p>You are now a root on the BMC you can enable ssh root login by editing <strong>/etc/ssh/sshd_config </strong>and restarting with <strong>/etc/init.d/S40network restart</strong></p>
<p>To change a root password, you need to generate the hash first:</p>
<pre>/ # mkpasswd --method=md5 --stdin<br>Password:<br>$1$AhimSt2i$B.SlOuTdRN5UnPvT09aVL/</pre>
<p>Then you can replace the hash in /etc/shadow for root entry.</p>
<pre>#root:$1$L.V2NdFU$0SGB00rL.3tQ1IEyt8BDJ/:1::::::<br>root:$1$AhimSt2i$B.SlOuTdRN5UnPvT09aVL/:1::::::<br>daemon:*:::::::<br>bin:*:::::::<br>sys:*:::::::<br>sync:*:::::::<br>mail:*:::::::<br>www-data:*:::::::<br>operator:*:::::::<br>nobody:*:::::::<br>sshd:*:::::::</pre>
<h2>Exit</h2>
<p>To leave the shell, simply type:</p>
<pre>exit</pre>
<p>And to stop the ADB server:</p>
<pre>adb kill-server</pre>
