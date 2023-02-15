<p>Turing Pi V2 board have I/O on both sides and fits Mini-ITX standard dimensions, 17 × 17 cm (6.7 × 6.7 in).</p>
<h2>Turing Pi V2 - Front</h2>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8833272660125" alt="TuringPi_V2_Front.png"></p>
<p> </p>
<ul>
<li><span class="wysiwyg-font-size-large"><strong>ATX 24 Pin</strong> - Standard ATX 24 Pin socket, fits PC ATX PSU or PicoPSU*</span></li>
<li>
<strong><span class="wysiwyg-font-size-large">2x SATA 3 </span></strong><span class="wysiwyg-font-size-large">- Standard SATA 6 connectors, up to 6Gbps per port*<br></span>
</li>
<li><span class="wysiwyg-font-size-large"><strong>2x USB 3.0 Front Panel</strong> - 20 pin USB header</span></li>
<li><span class="wysiwyg-font-size-large"><strong>Front IO</strong> - Pins to hook up power button, reset and power led</span></li>
<li><span class="wysiwyg-font-size-large"><strong>1x DSI</strong> - Display Serial Interface</span></li>
<li><strong><span class="wysiwyg-font-size-large">2x Mini PCIe</span></strong></li>
<li><strong><span class="wysiwyg-font-size-large">1x HDMI</span></strong></li>
<li><span class="wysiwyg-font-size-large"><strong>2x 1Gbps Ethernet</strong> - In bridge mode by default.</span></li>
<li><strong><span class="wysiwyg-font-size-large">2x USB 3.0</span></strong></li>
<li>
<strong><span class="wysiwyg-font-size-large">CM4 USB Mode </span></strong><span class="wysiwyg-font-size-large"> - USB 2.0 that can be assigned to any Node via $tpcli (in BMC). Please note that USB 2.0 and PCIe share the same resource, it is recommended to remove PCIe devices before using USB 2.0 for flashing.<br></span>
</li>
<li><span class="wysiwyg-font-size-large"><strong>4x DDR4 (260-pin)</strong> </span></li>
<li><span class="wysiwyg-font-size-large"><strong>4x FAN Header</strong> - <span class="a-list-item">Micro JST 1.25 mm 4 Pin Male Connector</span></span></li>
<li>
<strong><span class="wysiwyg-font-size-large"><span class="a-list-item">1x Case FAN header </span></span></strong><span class="wysiwyg-font-size-large"><span class="a-list-item">- 2 Pin small </span></span><span class="wysiwyg-font-size-large"><span class="a-list-item">Molex Fan Power Connector</span></span>
</li>
<li>
<strong><span class="wysiwyg-font-size-large"><span class="a-list-item">GPIO 40 Pin</span></span></strong><span class="wysiwyg-font-size-large"><span class="a-list-item"> - Standard GPIO same as on Raspberry Pi</span></span>
</li>
<li><span class="wysiwyg-font-size-large"><span class="a-list-item"><strong>Battery socket</strong> - For onboard HW clock, supports CR 2032 battery</span></span></li>
<li>
<strong><span class="wysiwyg-font-size-large"><span class="a-list-item">BMC Micro USB</span></span></strong><span class="wysiwyg-font-size-large"><span class="a-list-item"> - USB port for flashing Board Management Controller</span></span>
</li>
<li><span class="wysiwyg-font-size-large"><span class="a-list-item"><strong>BMC Serial Console </strong>- serial communication port for Board Management Controller, Use USB to TTL to get direct access to BMC OS.<br></span></span></li>
<li><span class="wysiwyg-font-size-large"><span class="a-list-item"><strong>UART2 DEBU2-4</strong> - serial communication ports are attached to their respective Node2-4 ports. For Node1, use UART pins in the GPIO 40pin header.</span></span></li>
<li>
<strong><span class="wysiwyg-font-size-large"><span class="a-list-item">SW1 </span></span></strong><span class="wysiwyg-font-size-large"><span class="a-list-item">- HDMI circuit switch<br></span></span>
</li>
<li><span class="wysiwyg-font-size-large"><span class="a-list-item"><strong>Key1</strong> - Power button to turn Module1-4 on and off</span></span></li>
<li><span class="wysiwyg-font-size-large"><span class="a-list-item"><strong>BMC Reset</strong> - Reset button for Baseboard Management Controller</span></span></li>
</ul>
<p> </p>
<p><em>* Make sure you are using a power supply with enough power to support all attached devices, at least 120W+ (For example, 12V/10A, 12A would be better)</em></p>
<p><em>** 6Gbps is maximal theoretical speed, it very much depends on compute module and other connected devices.</em></p>
<p><em> </em></p>
<p><strong><span class="wysiwyg-font-size-large"><span class="wysiwyg-color-red90">Note !</span> </span></strong></p>
<p><span class="wysiwyg-color-black"><strong><span class="wysiwyg-font-size-large">Not every I/O is connected to every Node slot. Please read the whole documentation to understand what is connected to where</span></strong><em>.</em></span></p>
<p> </p>
<h2>Turing Pi V2 - Back</h2>
<p><img src="https://help.turingpi.com/hc/article_attachments/8745618497181" alt="TuringPi_V2_Back.png" width="572" height="573"></p>
<ul>
<li>
<strong><span class="wysiwyg-font-size-large"><span class="a-list-item">4x M.2 Slot</span></span></strong><span class="wysiwyg-font-size-large"><span class="a-list-item"> - Full length M.2 slot for disks<span class="wysiwyg-color-red">*</span><br></span></span>
</li>
<li><span class="wysiwyg-font-size-large"><span class="a-list-item"><strong>SD Card slot</strong> - Additional storage for custom scripts/tools. available for BMC Operating System<br></span></span></li>
</ul>
<p><span class="wysiwyg-font-size-medium"><span class="a-list-item"><em><span class="wysiwyg-color-red90">*</span> M.2 slots are available only to SBC modules that have more than one PCIe lane, this currently does not include any Raspberry Pi Compute modules. For now, it is only supported by Nvidia Jetson and the upcoming Turing RK1.<br></em></span></span></p>
<p> </p>
<h1>Interconnections</h1>
<p>Due to the limited capabilities of various SBC modules, not every I/O slot is available for every SBC.</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8833249438493" alt="TuringPi_V2_interconnection.png"></p>
<h3>Port for Node 1:</h3>
<p>This is your primary. It is usually dedicated to being the entry point for your cluster, or Kubernetes master node. Therefore, it has the most I/O exposure.</p>
<ul>
<li><strong>GPIO 40-pin</strong></li>
<li><strong>Mini PCIe with SIM card slot<br></strong></li>
<li><strong>HDMI</strong></li>
<li><strong>DSI</strong></li>
<li><strong>M.2 T1 (on the back)*</strong></li>
<li><strong> USB 2.0 (<span class="wysiwyg-color-green110">this can be switched between Nodes</span>)<br></strong></li>
</ul>
<h3>Port for Node 2:</h3>
<ul>
<li><strong>M.2 T2 (on the back)*</strong></li>
<li><strong>Mini PCIe</strong></li>
</ul>
<h3>Port for Node 3:</h3>
<ul>
<li><strong>M.2 T3 (on the back)*</strong></li>
<li><strong>2x SATA 3<br></strong></li>
</ul>
<h3>Port for Node 4:</h3>
<ul>
<li><strong>M.2 T4 (on the back)*</strong></li>
<li><strong>4x USB 3.0<br></strong></li>
</ul>
<p><em><strong>*</strong> Again, although the connection to M.2 is there, it requires a second lane by attached SBC.</em></p>
<p> </p>
<h1>I/O Details</h1>
<h2>UART</h2>
<p>Turing Pi V2 contains multiple UART pin outs that allow serial connection not only to BMC, but to each individual Node1-4 slot as well.</p>
<h3>Node1</h3>
<p>This is part of the 40 Pin GPIO, refer to the Compute Module pinout. For example, Raspberry Pi:</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8788649500189" alt="rpi4_gpio.png"></p>
<h3>Node2-4</h3>
<p>These pins are located on the top side of the board, near HW buttons for BMC and micro USB. They are marked with "UART2 DEBUG" 2 to 4</p>
<h3>BMC</h3>
<p>BMC UART pins are also located on top of the board near the hardware buttons. Since BMC is running Linux OS, you can use this serial interface to connect to it directly.</p>
<p> </p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8833101067677" alt="uarts-pins.png"></p>
<p> </p>
<p> </p>
<h2>HDMI Circuit Switch</h2>
<p>Since the HDMI output between Raspberry Pi and Nvidia Jetson compute modules is different, we have included a 4 pin switch to give you the option to switch between these two modes.</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8833191732381" alt="hdmi-switches.png"></p>
<ul>
<li>Raspberry Pi: 1:ON 2:OFF 3:OFF 4:ON</li>
<li>Nvidia Jetson: 1:OFF 2:ON 3:ON 4:OFF </li>
</ul>
<h2>Front I/O</h2>
<p>Front I/O pins offer similar features to standard PC motherboards with pins for:</p>
<ul>
<li>Power LED</li>
<li>Status LED</li>
<li>Power Button</li>
<li>Reset Button</li>
</ul>
<p>This allows easy integration to standard PC cases.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9314569774493" alt="front-io.png"></p>
<h2>Buttons</h2>
<p>On Turing Pi V2 board are two physical buttons:</p>
<ul>
<li><strong>KEY1</strong></li>
<li><strong>BMC RESET</strong></li>
</ul>
<p class="wysiwyg-text-align-center"><strong><img src="https://help.turingpi.com/hc/article_attachments/8833244194589" alt="buttons.png"></strong></p>
<p class="wysiwyg-text-align-left">You can use these instead of front panel I/O pins.</p>
<p class="wysiwyg-text-align-left">After you plug in power to Turing Pi V2, BMC will take few seconds to boot up</p>
<ul>
<li class="wysiwyg-text-align-left">Pressing and releasing "Key1" button for more than 1s will initialize sequential start of the Moudels1 to 4</li>
<li class="wysiwyg-text-align-left">Pressing and holding the "Key1" button for 3s will shut down all Modules sequentially.</li>
<li class="wysiwyg-text-align-left">Pressing "BMC Reset" button for more than 1s will currently stop all modules and restart BMC manager (you will need to press Power button again to start Modules1 to 4)</li>
</ul>
<h1>PCIe Mapping</h1>
<table style="border-collapse: collapse; width: 114.429%; height: 154px;" border="1">
<tbody>
<tr style="height: 22px;">
<td style="width: 14.2857%; height: 22px;"> </td>
<td style="width: 23.4285%; height: 22px;"> </td>
<td class="wysiwyg-text-align-center" style="width: 30%; height: 22px;" colspan="3">PCIe Lanes</td>
<td style="width: 19.0165%; height: 22px;"> </td>
<td style="width: 10.9835%; height: 22px;"> </td>
<td style="width: 1.42857%; height: 22px;"> </td>
<td style="width: 17.7789%; height: 22px;"> </td>
</tr>
<tr style="height: 22px;">
<td class="wysiwyg-text-align-center" style="width: 14.2857%; height: 22px;">Vendor</td>
<td class="wysiwyg-text-align-center" style="width: 23.4285%; height: 22px;">Model</td>
<td class="wysiwyg-text-align-center" style="width: 10.7143%; height: 22px;">Gen 2</td>
<td class="wysiwyg-text-align-center" style="width: 9.42856%; height: 22px;">Gen 3</td>
<td class="wysiwyg-text-align-center" style="width: 9.85712%; height: 22px;">Gen 4</td>
<td class="wysiwyg-text-align-center" style="width: 19.0165%; height: 22px;">
<p>NVMe speed</p>
<p>up to*</p>
</td>
<td class="wysiwyg-text-align-center" style="width: 10.9835%; height: 22px;">
<p>Mini PCIe</p>
<p>Gen 2</p>
</td>
<td class="wysiwyg-text-align-center" style="width: 1.42857%; height: 22px;">
<p>SATA 3</p>
</td>
<td class="wysiwyg-text-align-center" style="width: 17.7789%; height: 22px;">
<p>USB 3.0</p>
</td>
</tr>
<tr style="height: 22px;">
<td style="width: 14.2857%; height: 22px;">Raspberry</td>
<td style="width: 23.4285%; height: 22px;">CM4</td>
<td style="width: 10.7143%; height: 22px;"><span class="wysiwyg-color-green120"><strong>x1</strong></span></td>
<td style="width: 9.42856%; height: 22px;"> </td>
<td style="width: 9.85712%; height: 22px;"> </td>
<td style="width: 19.0165%; height: 22px;">N/A</td>
<td style="width: 10.9835%; height: 22px;">Yes</td>
<td style="width: 1.42857%; height: 22px;">Yes</td>
<td style="width: 17.7789%; height: 22px;">Yes</td>
</tr>
<tr style="height: 22px;">
<td style="width: 14.2857%; height: 22px;">Turing</td>
<td style="width: 23.4285%; height: 22px;">RK1</td>
<td style="width: 10.7143%; height: 22px;"><span class="wysiwyg-color-green120"><strong>x1</strong></span></td>
<td style="width: 9.42856%; height: 22px;"><span class="wysiwyg-color-orange"><strong>x4</strong></span></td>
<td style="width: 9.85712%; height: 22px;"> </td>
<td style="width: 19.0165%; height: 22px;">4 GB/s</td>
<td style="width: 10.9835%; height: 22px;">Yes</td>
<td style="width: 1.42857%; height: 22px;">Yes</td>
<td style="width: 17.7789%; height: 22px;">Yes</td>
</tr>
<tr style="height: 22px;">
<td style="width: 14.2857%; height: 22px;">Nvidia</td>
<td style="width: 23.4285%; height: 22px;">Jetson TX2 NX</td>
<td style="width: 10.7143%; height: 22px;"><span class="wysiwyg-color-green120"><strong>x1</strong></span></td>
<td style="width: 9.42856%; height: 22px;"><span class="wysiwyg-color-orange"><strong>x2</strong></span></td>
<td style="width: 9.85712%; height: 22px;"> </td>
<td style="width: 19.0165%; height: 22px;">2 GB/s</td>
<td style="width: 10.9835%; height: 22px;">Yes</td>
<td style="width: 1.42857%; height: 22px;">Yes</td>
<td style="width: 17.7789%; height: 22px;">Yes</td>
</tr>
<tr style="height: 22px;">
<td style="width: 14.2857%; height: 22px;">Nvidia</td>
<td style="width: 23.4285%; height: 22px;">Jetson Xavier NX</td>
<td style="width: 10.7143%; height: 22px;"> </td>
<td style="width: 9.42856%; height: 22px;"><span class="wysiwyg-color-green120"><strong>x1</strong></span></td>
<td style="width: 9.85712%; height: 22px;"><span class="wysiwyg-color-orange"><strong>x4</strong></span></td>
<td style="width: 19.0165%; height: 22px;">8 GB/s</td>
<td style="width: 10.9835%; height: 22px;">Yes</td>
<td style="width: 1.42857%; height: 22px;">Yes</td>
<td style="width: 17.7789%; height: 22px;">Yes</td>
</tr>
<tr style="height: 22px;">
<td style="width: 14.2857%; height: 22px;">Nvidia</td>
<td style="width: 23.4285%; height: 22px;">Jetson Nano</td>
<td style="width: 10.7143%; height: 22px;"><span class="wysiwyg-color-orange"><strong>x1</strong></span></td>
<td style="width: 9.42856%; height: 22px;"> </td>
<td style="width: 9.85712%; height: 22px;"> </td>
<td style="width: 19.0165%; height: 22px;">1 GB/s</td>
<td style="width: 10.9835%; height: 22px;">N/A</td>
<td style="width: 1.42857%; height: 22px;">N/A</td>
<td style="width: 17.7789%; height: 22px;">N/A</td>
</tr>
</tbody>
</table>
<ul>
<li><span class="wysiwyg-color-green120"><strong>Mini PCIe top side of the board</strong></span></li>
<li><span class="wysiwyg-color-orange"><strong>M.2 M-key on the bottom side of the board.</strong></span></li>
</ul>
<h1>Other Integrated Circuits on the board</h1>
<p>Although most of them will have their own documentation, it is good to mention them.</p>
<ul>
<li>
<strong>Baseboard Management Controller:</strong> Allwinner T113-S3<br>
<ul>
<li>ARM Cortex-A7 Dual-Core</li>
<li>128MB DDR3</li>
<li>1GB NAND Flash Memory (MX35LF1GE4AB)</li>
<li>SD Card slot</li>
</ul>
</li>
<li>
<strong>1Gbps Switch: </strong>RTL8370MB-CG +
<ul>
<li>Data Rate 10 Mb/s, 100 Mb/s, 1 Gb/s</li>
<li>Duplex Full Duplex, Half Duplex</li>
<li>VLAN Support</li>
</ul>
</li>
</ul>
<p> </p>