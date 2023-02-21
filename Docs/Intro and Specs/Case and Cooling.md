<h1>Case</h1>
<p>Turing Pi V2 board will fit standard Mini ITX format.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8882485899037" alt="TuringPi_V2_size.png"></p>
<p> </p>
<p>This will allow you to mount this board to many if not all PC cases that support Mini ITX format motherboard.</p>
<h3><span class="wysiwyg-color-green120">Note:</span></h3>
<p>Since there are M.2 slots on the back of the board, some M.2 SSDs come with attached heat sinks, for example:</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8776967367709" alt="viper_ssd.png"></p>
<p>This might not fit with the standard size standoffs on the motherboard. You may need additional hardware in the form of <strong>ISO M3 standard</strong> standoffs. These look the same as the one on the motherboard, but are not. PC cases use<strong> either the ISO M3 standard or UTS standard #6-32UNC</strong>, and the main difference between the two is the size and thread pitch of the screws that they use.</p>
<p>You might need a few of these, depending on the size of the heat sink, and screw them in the current standoffs to extend the height.</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8777014367645" alt="m3_stand_off.png" width="337" height="203"></p>
<p> </p>
<h1>Cooling</h1>
<p>Although it's possible to achieve fan-less operation, we highly recommend at least one FAN in your case. Both Raspberry Pi and Nvidia Jetson will work without an attached heat sink, but they're not recommended, and you will most certainly hit thermal throttling and performance issues.</p>
<p> </p>
<p>If you are going to use a standard PC case or 3D printed case with fan over the compute modules, passive heat sinks should be enough.</p>
<p> </p>
<p>Next to each Node1-4 slot is a Micro JST 1.25 mm 4 Pin Male Connector that can be used for individual fans for each compute module. Also on board is a 12V case fan header located near the UART ports. These ports can be controlled by BMC with $tpcli.</p>
<p> </p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8833374519069" alt="TuringPi_V2_FANs.png"></p>
<p> </p>
<h2>Tested</h2>
<p>During the writing of this guide, we have tested the following types of heat sinks, and they fit without an issue. You should be able to fit heat sinks/coolers <strong>up to 20 mm in height.</strong></p>
<h3>CM4</h3>
<p>Low profile heat sink (no brand)<img src="https://help.turingpi.com/hc/article_attachments/8780273287837" alt="cm4_heatsink.png"></p>
<p> </p>
<h3>Nvidia Jetson</h3>
<p>Jetson Nano, fits fine, but you will not be able to fit an active fan on the top</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8780453240861" alt="heatsink_nvidia_jetson_nano.png"></p>
<p>XHG301 for Nvidia Jetson TX2<img src="https://help.turingpi.com/hc/article_attachments/8780453169437" alt="XHG301_heatsink_TX2.png"></p>
<p>Please share any additional heat sinks and cooling methods you verified working on in the comments below.</p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>