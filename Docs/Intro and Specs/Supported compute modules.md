<p>Turing Pi V2 Board contains four vertical 260-pin SO-DIMM sockets. This allows us to support not only the Raspberry Pi compute Module (with CM4 Adapter Board), directly attachable Nvidia Jetson compute Modules, but also our upcoming Turing Pi compute module!</p>
<p><strong>You are <span class="wysiwyg-color-green120">not</span> limited to using only one model for every slot</strong>, or even one vendor. Feel free to mix the Compute Modules however you like.</p>
<p>You may want an all-purpose Kubernetes cluster made out of 4 Raspberry Pi modules, but you can replace one or more RPI modules with Nvidia Jetson and suddenly your cluster has Machine Learning capable nodes.</p>
<p> </p>
<h2>Supported Modules</h2>
<table style="width: 751px;">
<tbody>
<tr>
<td style="width: 120px;"><strong>Vendor</strong></td>
<td style="width: 171px;"><strong>Model</strong></td>
<td style="width: 312px;"><strong>CPU</strong></td>
<td style="width: 68px;"><strong>Cores</strong></td>
<td style="width: 80px;"><strong>RAM, GB</strong></td>
</tr>
<tr>
<td style="width: 120px;">Turing Pi</td>
<td style="width: 171px;">RK1</td>
<td style="width: 312px;">Rockchip RK3588</td>
<td class="wysiwyg-text-align-center" style="width: 68px;">8</td>
<td class="wysiwyg-text-align-center" style="width: 80px;">8, 16, 32</td>
</tr>
<tr>
<td style="width: 120px;">Raspberry Pi</td>
<td style="width: 171px;">CM4</td>
<td style="width: 312px;">Broadcom BCM2711</td>
<td class="wysiwyg-text-align-center" style="width: 68px;">4</td>
<td class="wysiwyg-text-align-center" style="width: 80px;">2, 4, 8</td>
</tr>
<tr>
<td style="width: 120px;">Nvidia</td>
<td style="width: 171px;">Jetson Nano</td>
<td style="width: 312px;">Quad-core ARM Cortex-A57 MPCore</td>
<td class="wysiwyg-text-align-center" style="width: 68px;">4</td>
<td class="wysiwyg-text-align-center" style="width: 80px;">2, 4</td>
</tr>
<tr>
<td style="width: 120px;">Nvidia</td>
<td style="width: 171px;">Jetson TX2 NX</td>
<td style="width: 312px;">Dual-core NVIDIA Denver 2<br>Quad-core ARM A57 Complex</td>
<td class="wysiwyg-text-align-center" style="width: 68px;">6</td>
<td class="wysiwyg-text-align-center" style="width: 80px;">4</td>
</tr>
<tr>
<td style="width: 120px;">Nvidia</td>
<td style="width: 171px;">Jetson Xavier NX</td>
<td style="width: 312px;">NVIDIA Carmel ARM®v8.2</td>
<td class="wysiwyg-text-align-center" style="width: 68px;">6</td>
<td class="wysiwyg-text-align-center" style="width: 80px;">8</td>
</tr>
</tbody>
</table>
<p> </p>
<p>However, keep in mind that not all Compute Modules offer the same features.</p>
<p> </p>
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
<h2><span class="wysiwyg-color-black"><strong>Raspberry Pi Compute Module Adapter Board</strong></span></h2>
<p class="wysiwyg-text-align-center"><span class="wysiwyg-color-orange"><strong><img src="https://help.turingpi.com/hc/article_attachments/8895108042909" alt="CM4_Adapterboard_back.png"><img src="https://help.turingpi.com/hc/article_attachments/8895097120413" alt="CM4_Adapterboard_front.png"></strong></span></p>
<h2><span class="wysiwyg-color-black"><strong>Nvidia Jetson Compute Module<br></strong></span></h2>
<p class="wysiwyg-text-align-center"><span class="wysiwyg-color-orange"><strong><img src="https://help.turingpi.com/hc/article_attachments/8764529891485" alt="nvidia_jetson.png"></strong></span></p>
<p class="wysiwyg-text-align-center"> </p>
<h1 class="wysiwyg-text-align-left">Additional modules tested</h1>
<p>In addition to the Raspberry Compute Modules, we have evaluated various alternative options that could serve as drop-in replacements. However, it's important to note that most of these alternatives utilize different types of chips that may not be compatible with the peripheral devices on the Turing Pi V2. This means that if you choose to use a different module, it may not work seamlessly with the existing hardware on the Turing Pi V2, potentially affecting the performance or functionality of the system as a whole.</p>
<h2>SOQUARTZ COMPUTE MODULE</h2>
<p>We had the opportunity to test the Soquartz CM 2GB and found that it does successfully boot up. However, we encountered an issue where <strong>only the network was visible</strong> in the operating system. We used <strong><a href="https://dietpi.com/#downloadinfo" target="_blank" rel="noopener noreferrer">DietPi OS</a></strong>, specifically designed for this compute module, during our testing. Despite this limitation, it is still possible to use the Soquartz CM 2GB as a compute module in a Kubernetes cluster.</p>
<p>If you choose to purchase the module without the built-in eMMC, the SD card on the CM4 adapter board can be utilized, and the operating system can be booted from it. However, if you opt for the version with the eMMC extension, you will need to use the USB eMMC adapter tool provided by Pine64 to flash the operating system onto the module.</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9170063145629" alt="pine64_3.png"></p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9169990889885" alt="pine64.png"></p>
<p> </p>