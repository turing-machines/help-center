<h1 class="wysiwyg-text-align-left">Get your Turing Pi V2 ready to turn on.</h1>
<h2>Prepare Compute Modules</h2>
<p dir="auto">Before inserting your Compute Modules, make sure to mount any cooler or heat sinks. Raspberry Pi Compute Modules, will need our CM4 Adapter board to allow it to be slotted into the 260-pin DDR4 slot.</p>
<p dir="auto">Nvidia Jetson modules do not need any additional adaptors as they fit directly into the 260 pin DDR4 slot. We would recommend attaching any heatsinks prior to installation.</p>
<p dir="auto">Raspberry Pi Compute Modues fit securely into the 100-pin, high density connectors on our CM4 adapter board, and generally will not require any additional fixing. If the unit is going to be used in edge computing or an application which may subject the unit to high levels of vibration, please ensure the modules are securely secured to the adaptor board. <strong>There is a 1.5 mm space between the RPI module and Adapter board.</strong></p>
<p class="wysiwyg-text-align-center"><strong><img src="https://help.turingpi.com/hc/article_attachments/8865688012189" alt="rpi_cm_adapter_space.png"></strong></p>
<p class="wysiwyg-text-align-center"><strong><img src="https://help.turingpi.com/hc/article_attachments/8895101893405" alt="CM4_Adapterboard_back.png"><img src="https://help.turingpi.com/hc/article_attachments/8895101842077" alt="CM4_Adapterboard_front.png"></strong></p>
<p> </p>
<h4><strong>Note!</strong></h4>
<p class="wysiwyg-indent2"><strong>It's possible to mount the Raspberry Pi Compute Module at the wrong orientation. On the Adapter board there is an outline and the RPI module should fit into it correctly.</strong></p>
<p class="wysiwyg-indent2"><strong><img src="https://help.turingpi.com/hc/article_attachments/8895200388765" alt="cm4_wrong_mount.png"><img src="https://help.turingpi.com/hc/article_attachments/8895211212189" alt="cm4_correct_mount.png"></strong></p>
<h2> </h2>
<h2>SD Card (CM4 Adapter)</h2>
<p>Before you insert Compute Modules, you can also install SD card, although the card is easily accessible from the side even when the modules are inserted. You can install OS on the SD card at this point, however we are going to use BMC to flash the SD cards later on.</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8807278435613" alt="cms_sd_card.png"></p>
<p> </p>
<h2>Installing Modules</h2>
<p>Inserting modules into the Turing Pi V2 DDR4 260Pin slot is quite straightforward. Simply align the sides and notch of the module with the slot, then press it vertically until you hear a click and the side arms fall into place.</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8807277345053" alt="slide_in_260pin.png"><img src="https://help.turingpi.com/hc/article_attachments/8807277347997" alt="slide_in_260pin_done.png"></p>
<h3 class="wysiwyg-text-align-left">How to remove the compute module</h3>
<p>Removing the Compute Module is easy, you just need to release the white arms on both sides of the module at the same time. This will lift the module out of the socket, and you can easily remove it.</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8807727745693" alt="slide_out_260pin.png"></p>
<h2 class="wysiwyg-text-align-left">Power</h2>
<p>Turing Pi V2 and Compute Modules are powered from the ATX 24 Pin socket. Pico PSU or ATX PSU connector should fit only one way, please make sure it is slotted all the way in and the latch on the side is engaged. </p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8807957201565" alt="psu_latch.png"><img src="https://help.turingpi.com/hc/article_attachments/8807970487965" alt="psu_front.png"><img src="https://help.turingpi.com/hc/article_attachments/8807970490013" alt="psu_top.png"></p>
<h2>Network</h2>
<p>Turing Pi V2 board features 2x RJ45 1Gbps network connectors, they are by default in bridge configuration. If you want to connect it to your already existing network, <strong>you only need to connect one port</strong> to your router/switch network device. If you have DHCP on your network, BMC and Compute Modules should get their IPs from it. To get the BMC IP, head over to the BMC specific page where we will go over all the options.</p>
<h1>Powering ON</h1>
<p>Once you apply power to the 24Pin socket, either by plugging in a power source for Pico PSU or switching the ATX PSU switch to ON, the BMC will boot up automatically. This will take a few seconds and you can observe the whole boot process with the serial console if you want (see the BMC dedicated section on how to do it). It should not take more than 10 to 20 seconds.</p>
<p>Once the BMC is booted up, you can either use the Front I/O pins or the physical buttons (Key1) to turn on the Modules 1 to 4 (Refer to the I/O section about pin outs). Simply press <strong>"Key1"</strong> for about a second, and you will see <strong><span class="wysiwyg-color-red">RED</span></strong> LEDs turning on next to the Module slots, meaning they are supplied with power. By default, this is a staggered power on sequence, meaning they turn on one after another.</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/8839389413021" alt="power_led.png"></p>
<p class="wysiwyg-text-align-left">On the other side of the Node1-4 slot, you can also find the network activity indicator LED.</p>
<p class="wysiwyg-text-align-left"><img src="https://help.turingpi.com/hc/article_attachments/8839436269469" alt="nw_led.png"></p>
<p class="wysiwyg-text-align-left"> </p>
<p class="wysiwyg-text-align-left">At this point, you should be ready to interact with the Compute Module or BMC. <strong>You can access BMC without any Compute Module inserted</strong>, check more on the BMC dedicated page.</p>
<h1>RTC Clock Battery</h1>
<p><strong>Note: users reported that the RTC battery may restart or interfere with starting the BMC. We are investigating it at the moment. </strong></p>
<p>Insert Real Time Clock battery, <strong>CR 2032</strong>, into its holder. There is a notch on one side and a small metal lever on the other, slide the battery under the plastic notch and press down on the opposite side. It could be a tight fit, and you might need a small screwdriver to get the battery out later on.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8805530949789" alt="rtc_bat.png"></p>
<p> </p>