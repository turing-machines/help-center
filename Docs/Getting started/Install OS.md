<h1>Raspberry Pi + CM4 Adapter</h1>
<p>Raspberry Pi Compute Modules come in various flavors, and some do not have eMMC storage at all, relying on microSD cards. Some have up to 32GB eMMC directly on board.</p>
<p>For both types of storage, we are going to use a fantastic tool called: <a href="https://www.raspberrypi.com/software/" target="_blank" rel="noopener noreferrer"><strong>Raspberry Pi Imager.</strong></a> This software works on Windows/macOS/Linux. Download it and install it on your PC.</p>
<p>Raspberry Pi Imager allows you to download, customize and copy most OS versions you might like to your RPI Compute Module.</p>
<p><strong>Note: Do not use either microSD card and eMMC during installation, remove the microSD card temporarily when installing to eMMC.</strong></p>
<p>With the microSD card, you have two options, either use the microSD card reader and flash the OS on your PC. Or you can use the same method to access the microSD card as we are going to describe for installing it on eMMC.</p>
<p>If you have chosen the second method, you will need a USB cable. You need to connect USB 2.0 on Turing Pi V2 (it's the vertical one, next to HDMI) to your PC.</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8886281919389" alt="cm4_usb.png" width="496" height="370"></p>
<p><strong>Ideally</strong>, you will need USB A-A (or also called USB M/M) that looks like this on both sides:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8886143073949" alt="usb_m_m.png" width="466" height="389"></p>
<p class="wysiwyg-text-align-left">However, you can also use USB-A to USB-C, it depends on if your computer have USB-C input.</p>
<p class="wysiwyg-text-align-left"><strong>Note: We have seen some reports from users where USB-A to USB-C was not working correctly. If you face issues, please use preferred method USB-A to USB-A. </strong></p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8886274372637" alt="usb_a_c.png" width="509" height="369"></p>
<p class="wysiwyg-text-align-left"> </p>
<h2 class="wysiwyg-text-align-left">Driver</h2>
<p>For our PC to recognize the Raspberry Pi storage when switched to "storage" mode, you need to install the <strong>rpiboot</strong> tool.</p>
<ul>
<li>For Windows users, install the <a href="https://github.com/raspberrypi/usbboot/raw/master/win32/rpiboot_setup.exe" target="_blank" rel="noopener noreferrer">rpiboot installer</a>.</li>
<li>For Mac &amp; Linux users, <a href="https://github.com/raspberrypi/usbboot#building" target="_self">follow the build instructions</a>.</li>
</ul>
<p>Then run the tool in OS of your choice, usually its:</p>
<pre>sudo ./rpiboot</pre>
<p>But for windows you need to run <strong>CMD. </strong>The easiest way to launch the Command Prompt (CMD) in Windows is by using the start menu. Press the Windows key on your keyboard or click on the Windows icon in the bottom left corner of your screen, then type "cmd" into the search bar. Click on the "Command Prompt" result that appears. Another way to launch CMD is by using the Run dialog box. Press the Windows key + R on your keyboard, type "cmd" into the box, and press Enter.</p>
<pre>cd "C:\Program Files (x86)\Raspberry Pi"<br>rpiboot -d .\msd</pre>
<p>I should look like this:</p>
<pre>C:\Program Files (x86)\Raspberry Pi&gt;rpiboot -d .\msd<br>RPIBOOT: build-date Dec 16 2022 version 20221215~105525 1afa26c5<br>Loading: .\msd/bootcode.bin<br>Loading: .\msd/bootcode4.bin<br>Waiting for BCM2835/6/7/2711...</pre>
<h2>Switching to USB mode</h2>
<p>Assuming you have <strong>connected the USB CM4 from Turing Pi 2 to your PC,</strong> where <strong>rpiboot is now waiting</strong> for connection, and you turned on power to your Compute Modules. You need to execute the following command inside BMC:</p>
<pre>tpi -u device -n 1</pre>
<p><strong>-n 1 to 4</strong> represents your Node1-4 slots.</p>
<p>On your PC side, you should see rpiboot detecting the Compute Module storage and presenting it to you as a new disk.</p>
<p>Here is an example with a brand new microSD card:</p>
<pre>C:\Program Files (x86)\Raspberry Pi&gt;rpiboot -d .\msd<br>RPIBOOT: build-date Dec 16 2022 version 20221215~105525 1afa26c5<br>Loading: .\msd/bootcode.bin<br>Loading: .\msd/bootcode4.bin<br>Waiting for BCM2835/6/7/2711...<br>Loading: .\msd/bootcode4.bin<br>Loading: .\msd/bootcode4.bin<br>Sending bootcode.bin<br>Successful read 4 bytes<br>Waiting for BCM2835/6/7/2711...<br>Loading: .\msd/bootcode4.bin<br>Second stage boot server<br>Cannot open file config.txt<br>Cannot open file pieeprom.sig<br>Loading: .\msd/start4.elf<br>File read: start4.elf<br>Cannot open file fixup4.dat<br>Second stage boot server done</pre>
<p>And in disk manager I can see the new disk (this one is already partitioned as the new microSD card tents to be)</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8886937773085" alt="sd_disk_in_win.png"></p>
<h2>Raspberry Pi Imager</h2>
<p>You can now freely use <a href="https://www.raspberrypi.com/software/" target="_blank" rel="noopener noreferrer"><strong>the Raspberry Pi Imager</strong></a> to flash the OS you want to your Compute Module.</p>
<p>We will quickly go over flashing Raspbian 64bit, but you can choose Ubuntu or others.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8887585736605" alt="rpi_choose_os.png"></p>
<p>I have chosen: <strong>Raspberry Pi OS (other) &gt; Raspberry Pi OS Lite (64-bit). </strong></p>
<p><strong>Next, </strong>you can choose <strong>Storage. </strong> Raspberry Pi Imager already offers the correct disk, but make sure you are really choosing the correct one.</p>
<p><strong><img src="https://help.turingpi.com/hc/article_attachments/8887585693341" alt="rasbian_disk.png"></strong></p>
<p><strong>Before Write</strong>, click on cog icon and configure your OS, enable SSH, give it a hostname, set passwords etc..</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8887585653533" alt="rasbian_configure.png"></p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8887615526429" alt="rasbian_configure_2.png"></p>
<p><strong>Write</strong>, click write and confirm "yes" in next pop up window.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8887615653277" alt="rasbian_write.png"></p>
<p>Please wait until the OS is flashed and verified, do not cancel the verification.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8887615601821" alt="rasbian_done.png"></p>
<p>Now that the flashing is done, you can reboot the node by issuing the following command in BMC:</p>
<p><strong>Stop Power to Node1:</strong></p>
<pre>curl -X POST "http://127.0.0.1/api/bmc?opt=set&amp;type=power&amp;node1=0"</pre>
<p><strong>Start Power to Node1:<br></strong></p>
<pre>curl -X POST "http://127.0.0.1/api/bmc?opt=set&amp;type=power&amp;node1=1"</pre>
<p><span class="wysiwyg-color-green120"><strong>Note:</strong> These commands are temporary, proper and easy way will be available soon with newer version off $tp</span>i</p>
<p>We just did a reboot, but if you want to get IP quickly we can modify our script for BMC a little, and after we turn our node off, we can run the following on your PC (change the range to your network) :</p>
<pre>range="10.0.0.*"; echo "Please wait, starting scan..."; \<br>nmap -PN -p 22 --open -oG - "$range" | grep -E '22/open' | \<br>awk '{print $2}' | sort -V &gt; initial_scan.txt; \<br>echo "Start RPi, waiting 60sec..."; sleep 60; \<br>nmap -PN -p 22 --open -oG - "$range" | \<br>grep -E '22/open' | awk '{print $2}' | \<br>sort -V | diff initial_scan.txt -</pre>
<p>And turn it back on when asked. You should get something like this:</p>
<pre><br>vladoportos@DESKTOP-7BCS3LE:~$ range="10.0.0.*"; echo "Please wait, starting scan..."; \<br>nmap -PN -p 22 --open -oG - "$range" | grep -E '22/open' | \<br>awk '{print $2}' | sort -V &gt; initial_scan.txt; \<br>echo "Start RPi, waiting 60sec..."; sleep 60; \<br>nmap -PN -p 22 --open -oG - "$range" | \<br>grep -E '22/open' | awk '{print $2}' | \<br>sort -V | diff initial_scan.txt -<br>Please wait, starting scan...<br>Start RPi, waiting 60sec...<br>10a11<br>&gt; 10.0.0.167</pre>
<p>Since we enabled SSH server during the installation modification, we should be able to ssh now to that IP.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8888167384861" alt="ssh_one.png"></p>
<p><strong>Done!</strong></p>
<p> </p>
<h1>Nvidia Jetson</h1>
<p>This is a bit more complicated, since Nvidia did not release its flash utility called <strong>sdkmanager</strong> for anything other than Linux (Ubuntu 18) and I could not get it to flash 100% on virtualbox Ubuntu which is also not supported by Nvidia as a flashing method. Therefore, we would recommend using a live USB / CD with Ubuntu 18.04.6 LTS, this should work.</p>
<p> </p>
<p>You can change the USB 2.0 to the module with Jestson Nano with the following command from BMC:</p>
<pre>tpi -u device -n 2</pre>
<p>This assumes that your Jetson is in slot Node2 you can then install and run sdkmanager according to <a href="https://docs.nvidia.com/sdk-manager/download-run-sdkm/index.html" target="_blank" rel="noopener noreferrer">Nvidia Documentation</a>.</p>
<p>Once logged in to SDK Manager, it should detect the module:</p>
<p> </p>
<p>You should be able to continue with flashing the OS according to the documentation.</p>
<p> </p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8909160275357" alt="jetson.png"></p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8909137586717" alt="jetson2.png"></p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8909137589277" alt="jetson3.png"></p>
<p> </p>
<p> </p>