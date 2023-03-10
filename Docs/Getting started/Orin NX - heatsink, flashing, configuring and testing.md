<p>As of the time of writing this article, the official Nvidia Jetson Orin NX heatsink was not available yet as well as there's no support for Orin NX in the SDK Manager, which means both heatsink and flashing need a bit of creativity, but in the end, the whole process is not too complicated, just a bit inconvenient.</p>
<p> </p>
<h1>Cooling</h1>
<p>To successfully flash and run the Orin NX module, you must ensure it's properly cooled. Once the dedicated cooling solutions are available, they will be the best option. For now, Xavier NX + a thermopad is a good and working well option.</p>
<p>I bought a <a href="https://www.amazon.com/Waveshare-Official-Compatible-Speed-Adjustable-Height-Limited/dp/B09TF5T12B" target="_self" rel="undefined">Waveshare NX-FAN-PWM heatsink</a>:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9767110895261" alt="nx-fan-pwm-wentylator-do-jetson-xavier-nx.png" width="345" height="247"></p>
<p class="wysiwyg-text-align-left">While the mounting hole layout and the bracket are the same, the height of the elements on the boards is slightly different between Xavier NX and Orin NX, causing the Orin NX CPU/GPU core not to touch the heatsink - you can see how the core is being reflected on the heatsink surface and the gap is clearly visible:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9767159617949" alt="2023-01-30_19.54.22_1_.jpg" width="345" height="194"> <img src="https://help.turingpi.com/hc/article_attachments/9767160104861" alt="2023-01-30_19.53.58_1_.jpg" width="345" height="194"></p>
<p class="wysiwyg-text-align-left">I used the solution to remove the thermal paste and use a 0.5mm thick thermopad. To make the heat conductivity decent I used <span class="wysiwyg-color-orange">Thermopad Thermal Grizzly Minus Pad 8 (30 × 30 × 0,5 mm)</span>:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9767250356893" alt="2023-02-01_22.34.22_1_.jpg" width="345" height="194"> <img src="https://help.turingpi.com/hc/article_attachments/9767284621469" alt="Thermal-Grizzly-Minus-Pad-8-podk-adka-termiczna-8-0W-MK-multi-size-CPU-karta-graficzna_1_.png" width="194" height="194"></p>
<p class="wysiwyg-text-align-left">The thing to remember is the screws are not going to screw all way in. Put the pad between the CPU/GPU core and the heatsink and screw in all of the screws so the 4 square coils touch the heatsink.</p>
<p class="wysiwyg-text-align-left">I tested this solution under heavy CPU and GPU loads and the results were very good.</p>
<p class="wysiwyg-text-align-left"> </p>
<h1 class="wysiwyg-text-align-left">USB Keyboard and Mouse</h1>
<p class="wysiwyg-text-align-left">During the flashing process, I used a USB keyboard and mouse. I'll be using them (and the attached monitor) later too. The Jetson devices cannot put their USB controller used for flashing purposes into the host mode, this function only works on the Raspberry Pi Compute Module 4s. This means an additional USB controller is necessary.</p>
<p class="wysiwyg-text-align-left">For the USB ports, I used an <a href="https://www.amazon.com/Inline-66905-Mini-PCIe-Card-USB/dp/B00N2ACIG4" target="_self">Inline 66905 Mini-PCIe USB3 controller</a> based on the <span class="wysiwyg-color-orange">Renesas D720201</span> chip - it works out of the box:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9768205218589" alt="2023-02-22_13.02.17_1_.jpg" width="345" height="194"> <img src="https://help.turingpi.com/hc/article_attachments/9768235066909" alt="2023-02-21_08.54.22_1_.jpg" width="345" height="194"></p>
<p class="wysiwyg-text-align-left"> </p>
<h1 class="wysiwyg-text-align-left">Flashing</h1>
<p class="wysiwyg-text-align-left">I tested flashing with bare-metal Ubuntu 20.04 LTS. As far as I know, using WSL on Windows or WMWare Player (free) with Ubuntu 20.04 LTS should also work. If you tested such a solution, let us know!</p>
<p class="wysiwyg-text-align-left">The installation process also assumes flashing in Node 2 since some users experienced difficulties in flashing in Node 1.</p>
<p class="wysiwyg-text-align-left"> </p>
<h2 class="wysiwyg-text-align-left">PC Preparation</h2>
<p class="wysiwyg-text-align-left">Install Ubuntu 20.04 LTS (this exact version, not 22.04 LTS or any other) on a PC.</p>
<p class="wysiwyg-text-align-left">Currently, SDK Manager does not support Orin devices, so we have to flash them "by hand". In the future, the whole process should be simpler, but for now, this is what we have to do.</p>
<p class="wysiwyg-text-align-left">Install required libraries:</p>
<pre class="wysiwyg-text-align-left">sudo apt install -y wget qemu-user-static nano</pre>
<p class="wysiwyg-text-align-left">Navigate to the <a href="https://developer.nvidia.com/linux-tegra" target="_self">Jetson Linux page</a>, then click on the green button of the latest Jetson Linux version:</p>
<p class="wysiwyg-text-align-left"><img src="https://help.turingpi.com/hc/article_attachments/9767498293661" alt="Jetson_Linux.png"></p>
<p class="wysiwyg-text-align-left">Scroll down to the download table. From the Drivers section copy links for both <span class="wysiwyg-color-orange">Driver Package (BSP)</span> and <span class="wysiwyg-color-orange">Sample Root Filesystem</span>:</p>
<p class="wysiwyg-text-align-left"><img src="https://help.turingpi.com/hc/article_attachments/9767540986653" alt="Files.png"></p>
<p class="wysiwyg-text-align-left">For example, for <span class="wysiwyg-color-orange">Jetson Linux 35.2.1</span> the links are:<br>- <span class="wysiwyg-color-orange">Driver Package (BSP)</span>: <a href="https://developer.nvidia.com/downloads/jetson-linux-r3521-aarch64tbz2" target="_self">https://developer.nvidia.com/downloads/jetson-linux-r3521-aarch64tbz2</a><br>- <span class="wysiwyg-color-orange">Sample Root Filesystem</span>: <a href="https://developer.nvidia.com/downloads/linux-sample-root-filesystem-r3521aarch64tbz2" target="_self">https://developer.nvidia.com/downloads/linux-sample-root-filesystem-r3521aarch64tbz2</a></p>
<p>Download both files to, for example, the home directory - with the above URL example:</p>
<pre>wget <a href="https://developer.nvidia.com/downloads/jetson-linux-r3521-aarch64tbz2">https://developer.nvidia.com/downloads/jetson-linux-r3521-aarch64tbz2</a><br>wget <a href="https://developer.nvidia.com/downloads/linux-sample-root-filesystem-r3521aarch64tbz2">https://developer.nvidia.com/downloads/linux-sample-root-filesystem-r3521aarch64tbz2</a></pre>
<p>Unpack the Driver Package (BSP) (again, using names from the example URLs above):</p>
<pre>tar xpf jetson-linux-r3521-aarch64tbz2</pre>
<p>Unpack the Sample Root Filesystem into the Driver Package (BSP) (<span class="wysiwyg-color-orange">sudo</span> is important here):</p>
<pre>sudo tar xpf linux-sample-root-filesystem-r3521aarch64tbz2 -C Linux_for_Tegra/rootfs/</pre>
<p>Turing Pi 2 (similar to some other custom carrier boards) does not have the onboard EEPROM that the module or the flasher can access. The flasher, however, expects the EEPROM to exist as it does on the official Xavier NX carrier boards. We need to modify one file to set the EEPROM size to <span class="wysiwyg-color-orange">0</span>.</p>
<pre>sudo nano Linux_for_Tegra/bootloader/t186ref/BCT/tegra234-mb2-bct-misc-p3767-0000.dts</pre>
<p>The last EEPROM configuration line says:</p>
<pre>cvb_eeprom_read_size = &lt;0x100&gt;;</pre>
<p>Replace the value of <span class="wysiwyg-color-orange">0x100</span> with <span class="wysiwyg-color-orange">0x0</span> (make sure to not modify <span class="wysiwyg-color-orange">cvm_eeprom_read_size</span> instead - the name is similar, but starts with <span class="wysiwyg-color-orange">cvm</span>, modify the one whose name starts with <span class="wysiwyg-color-orange90">cvb</span> - <span class="wysiwyg-color-orange">b</span> like a board):</p>
<pre>cvb_eeprom_read_size = &lt;0x0&gt;;</pre>
<p>Press F3 and F2 to save and exit:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9767790966557" alt="Eeprom.png"></p>
<p>Prepare the firmware:</p>
<pre>cd Linux_for_Tegra/<br>sudo ./apply_binaries.sh<br>sudo ./tools/l4t_flash_prerequisites.sh</pre>
<p> </p>
<h2>Turing Pi 2 Preparation</h2>
<p>Insert Orin NX into Node 2 and install the NVMe drive for Node 2 (I don't yet know if that'll work with the USB drive on Turing Pi 2 - you could, in theory, use Mini PCIe to SATA controller but the bootloader would have to support it and I did not test this possibility, yet - if you happened to test this configuration, please let us know!).</p>
<p>Now, let's put the Orin NX device into the <span class="wysiwyg-color-orange">Forced Recovery Mode</span> using the web panel:</p>
<ul>
<li>turn the Node 2 power off</li>
<li>set Node 2 into the device mode</li>
<li>turn the Node 2 power on</li>
</ul>
<p>You can also use the command line version:</p>
<ul>
<li>
<span class="wysiwyg-color-orange">tpi -p on</span> (turns on all nodes)</li>
<li><span class="wysiwyg-color-orange">tpi -u device -n 2</span></li>
<li>
<span class="wysiwyg-color-orange">tpi -p off</span> (turns off all nodes)</li>
</ul>
<p>Connect the USB A-A cable to the PC and verify that the Orin NX device has been detected by invoking <span class="wysiwyg-color-orange">lsusb</span>. It should pop up as the <span class="wysiwyg-color-orange">Nvidia Corp. APX</span> device on the list:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9768031241245" alt="APX.png"></p>
<p> </p>
<h2>Flashing</h2>
<p>Assuming you're still in the <span class="wysiwyg-color-orange">Linux_for_Tegra</span> directory, flash Orin NX with NVMe drive using:</p>
<pre>sudo ./tools/kernel_flash/l4t_initrd_flash.sh --external-device nvme0n1p1 \<br>-c tools/kernel_flash/flash_l4t_external.xml -p "-c bootloader/t186ref/cfg/flash_t234_qspi.xml" \<br>--showlogs --network usb0 p3509-a02+p3767-0000 internal</pre>
<p>If you want to use a USB drive (untested by me):</p>
<pre>sudo ./tools/kernel_flash/l4t_initrd_flash.sh --external-device sda1 \<br>-c tools/kernel_flash/flash_l4t_external.xml -p "-c bootloader/t186ref/cfg/flash_t234_qspi.xml" \<br>--showlogs --network usb0 p3509-a02+p3767-0000 internal</pre>
<p>Flashing will take a longer time and the flasher will exit once it's done. The Orin NX FAN would not be spinning for the first part of the flashing process, but this is not a problem - the Orin NX will work just fine cooled passively by the heatsink for this part of the flashing process.</p>
<p>After flashing is done, it's time to move the module into Node 1. Sadly we have no way to turn it off gracefully since we have no input device to shut the operating system down, but this should not be a problem. Turn off the power using either the web panel or the command (<span class="wysiwyg-color-orange">tpi -p off</span>) as before when putting the module into device mode.</p>
<p> </p>
<h2>Finishing the OS Installation</h2>
<p><span class="wysiwyg-color-green110">Note:</span> There might be a way to configure Orin NX without a keyboard/mouse and monitor using one of the scripts in the <span class="wysiwyg-color-orange">tools</span> folder (<span class="wysiwyg-color-orange">l4t_creat_default_user.sh</span>) but I haven't attempted or tested that.</p>
<p>To finish up the configuration:</p>
<ul>
<li>disconnect the USB A-A cable used for flashing</li>
<li>move the module into Node 1</li>
<li>move the NVMe drive</li>
<li>insert the USB controller into the Mini PCIe slot</li>
<li>connect the keyboard and mouse to the USB controller</li>
<li>connect a monitor to the HDMI port (remember to <a href="https://help.turingpi.com/hc/en-us/articles/8685766680477-Specifications-and-I-O-Ports" target="_self">set the SW1 switch for Jetson devices</a> otherwise you will not see the output on the monitor)</li>
<li>turn on the module power using either the web panel or the command (<span class="wysiwyg-color-orange">tpi -p on</span>)</li>
</ul>
<p>Using a keyboard and mouse, go through the configuration steps visible on the monitor and wait for the setup to finish - until you get a desktop environment. These are the standard Ubuntu initial configuration steps.</p>
<p>At this stage we have a bare operating system that does not even contain the Jetpack - there are a few more required and suggested steps that we need to perform</p>
<p>These steps can be done using SSH - one of the steps asked for a hostname - if your PC/Mac has the mDNS running, you can use this name directly to connect to it via SSH, otherwise you need to find and use the IP address.</p>
<p> </p>
<h1>Configuration</h1>
<h2>The FAN Profile</h2>
<p>First, we need <span class="wysiwyg-color-orange">nano</span> or any other editor of choice:</p>
<pre>sudo apt install -y nano</pre>
<p>Then, to change the FAN profile, invoke:</p>
<pre>sudo systemctl stop nvfancontrol<br>sudo nano /etc/nvfancontrol.conf</pre>
<p>Find the line containing `FAN_DEFAULT_PROFILE` - near the bottom of the file content:</p>
<pre>FAN_DEFAULT_PROFILE quiet</pre>
<p>And replace quiet with cool:</p>
<pre>FAN_DEFAULT_PROFILE cool</pre>
<p>Press F3 and F2 to save and exit:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9768624989341" alt="Profile.png"></p>
<p>Run:</p>
<pre>sudo rm /var/lib/nvfancontrol/status<br>sudo systemctl start nvfancontrol</pre>
<p> </p>
<h2>The Power Mode</h2>
<p>By default, the device is in the <span class="wysiwyg-color-orange">15W</span> power mode - change it to <span class="wysiwyg-color-orange">MAXN</span> using the setting in the top-right part of the screen.:</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9768912004509" alt="MAXN.png"></p>
<p> </p>
<h2>The Jetpack</h2>
<p>As mentioned before, this (currently the only) way of installation does not provide the Jetpack. It's time to install it.</p>
<p>To install Jetpack, let's first update the operating system:</p>
<pre>sudo apt update<br>sudo apt -y upgrade<br>sudo apt -y dist-upgrade<br>sudo reboot</pre>
<p>And install the Jetpack:</p>
<pre>sudo apt -y install nvidia-jetpack</pre>
<p> </p>
<h1>Testing</h1>
<p>This section is optional, it shows how to test the module under full CPU and GPU load.</p>
<pre>We'll be using TensorFlow to put the load on the GPU:<br>sudo apt -y install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran<br>sudo apt -y install python3-pip<br>sudo pip3 install -U pip testresources setuptools<br>sudo pip3 install -U numpy==1.21.1 future==0.18.2 mock==3.0.5 keras_preprocessing==1.1.2 keras_applications==1.0.8 gast==0.4.0 protobuf pybind11 cython pkgconfig packaging h5py==3.6.0<br>sudo pip3 install --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v51 tensorflow==2.11.0+nv23.01</pre>
<p>To put some load on the CPU we'll use <span class="wysiwyg-color-orange">stress</span>:</p>
<pre>sudo apt -y install stress</pre>
<p>Reboot:</p>
<pre>sudo reboot</pre>
<p>Now we can open Jetson Power GUI - it can be found in the upper-right corner when we click on the power profile, below the power mode setting</p>
<p>Additionally, open 2 terminal windows.</p>
<p>Save this Python code into the <span class="wysiwyg-color-orange">test.py</span> file - this is a neural network that does really nothing useful (computes some noise), but puts a load on the GPU:</p>
<pre>import os<br>import time<br>import subprocess<br>from threading import Thread<br>import tensorflow as tf<br>from tensorflow.keras import optimizers, layers, models<br>import numpy as np<br><br><br>BATCH_SIZE = 4<br>HIDDEN_LAYERS = 2<br>HIDDEN_LAYER_KERNELS = 4<br>DATASET_SIZE = 2048<br>DATA_SHAPE = (256, 256, 3)<br><br>model = models.Sequential()<br>model.add(layers.Conv2D(HIDDEN_LAYER_KERNELS, (3, 3), activation='relu', input_shape=DATA_SHAPE, strides=(1, 1), padding="same"))<br>model.add(layers.MaxPooling2D((2, 2), strides=(1, 1), padding="same"))<br>for _ in range(HIDDEN_LAYERS):<br>model.add(layers.Conv2D(HIDDEN_LAYER_KERNELS, (5, 5), activation='relu', strides=(1, 1), padding="same"))<br>model.add(layers.MaxPooling2D((5, 5), strides=(1, 1), padding="same"))<br><br>model.add(layers.Conv2D(2, (DATA_SHAPE[0] // 8, DATA_SHAPE[1] // 8), activation='relu'))<br>model.add(layers.Flatten())<br>model.add(layers.Dense(64, activation='relu'))<br>model.add(layers.Dense(10))<br><br>model.summary()<br><br>X = np.ones((DATASET_SIZE, *DATA_SHAPE))<br>y = np.ones((DATASET_SIZE, 10))<br>data = tf.data.Dataset.from_tensor_slices((X, y))<br>data = data.batch(BATCH_SIZE)<br><br>model.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=1e-3),<br>loss=tf.keras.losses.BinaryCrossentropy())<br><br>model.fit(data, epochs=1000)</pre>
<p>In one terminal window run:</p>
<pre>stress -c 8</pre>
<p>to stress the CPU, and in another tun:</p>
<pre>python3.8 test.py</pre>
<p>to stress the GPU at the same time. At this stage, the Orin NX might start showing over-current messages which means we are an amount of load beyond what it can handle.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9769222860573" alt="Screenshot_from_2023-03-10_12-34-52.png"></p>
<p> </p>