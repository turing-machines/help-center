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
<h1 class="wysiwyg-text-align-left">Flashing</h1>
<p class="wysiwyg-text-align-left">I tested flashing with bare-metal Ubuntu 20.04 LTS. As far as I know, using WSL on Windows or WMWare Player (free) with Ubuntu 20.04 LTS should also work. If you tested such a solution, let us know!</p>
<p class="wysiwyg-text-align-left">The installation process also assumes flashing in Node 2 since some users experienced difficulties in flashing in Node 1.</p>
<p class="wysiwyg-text-align-left"> </p>
<h2 class="wysiwyg-text-align-left">PC preparation</h2>
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
<p> </p>