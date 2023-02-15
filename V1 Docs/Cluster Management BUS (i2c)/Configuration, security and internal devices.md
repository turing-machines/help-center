<p dir="auto">Turing Pi cluster management bus configuration, security and internal devices</p>
<h3 dir="auto">
<a id="user-content-configure-i2c-and-devices-" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#configure-i2c-and-devices-" aria-hidden="true"></a>Configure I2C and devices<a id="user-content-configure-i2c-and-devices"></a>
</h3>
<p dir="auto">First аdd<code>dtoverlay</code></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>sudo nano /boot/config.txt
</code></pre>
</div>
<p dir="auto">and add the following lines</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>dtoverlay=i2c1,pins_44_45
dtoverlay=i2c-rtc,mcp7940x
</code></pre>
</div>
<p dir="auto">Enable I2C interface in<code>sudo raspi-config</code>-&gt; Interfacing Options -&gt; I2C -&gt; enable and reboot</p>
<p dir="auto">Check everything is fine with I2C and RTC</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>dmesg | grep i2c
[    4.414436] i2c /dev entries driver  # ok

ls /dev/*i2c*
/dev/i2c-1  # ok

dmesg | grep rtc
[    6.489206] rtc-ds1307 1-006f: registered as rtc0

ls /dev/*rtc*
/dev/rtc  /dev/rtc0
</code></pre>
</div>
<h3 dir="auto">
<a id="user-content-i2c-tools-" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#i2c-tools-" aria-hidden="true"></a>I2C tools<a id="user-content-i2c-tools"></a>
</h3>
<p dir="auto">Install i2c-tools</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>sudo apt-get install i2c-tools
</code></pre>
</div>
<p dir="auto">{% hint style="info" %} If the command doesn't work update your pi</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>sudo apt-get update
</code></pre>
</div>
<p dir="auto">{% endhint %}</p>
<p dir="auto">Now check our I2C devices on the CMB</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>sudo i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- 57 -- -- -- -- 5c -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 6f
70: -- -- -- -- -- -- -- --
</code></pre>
</div>
<p dir="auto">You can see three I2C devices Ethernet Switch (0x5Ch), I2C expander (0x57h) and RTCC (0x6Fh).</p>
<p dir="auto">The<code>i2cget</code>command is used to read a byte from a specified register on the I2C device. The format for this command is as follows</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>i2cget [-f] [-y] 1   [MODE]
</code></pre>
</div>
<p dir="auto">[-f] [-y] Options:</p>
<ul dir="auto">
<li>-f force access to the device even if the device is still busy. However, be careful. This can cause the i2cget command to return an invalid value. We recommend avoiding this option.</li>
<li>-y disable interactive mode. Using this command will skip the prompt for confirmation from the i2cget command. By default, it will wait for the confirmation from the user before interacting with the I2C device.</li>
<li>DEVICE_ADDRESS is an I2C bus address (e.g 0x04h)</li>
<li>ADDRESS is the address on the slave from which to read data (eg. 0x00h)</li>
</ul>
<p dir="auto">The optional MODE can be one of the following:</p>
<ul dir="auto">
<li>b – read/write a byte of data, this is the default if left blank</li>
<li>w – read/write a word of data (two bytes)</li>
<li>c – write byte/read byte transaction</li>
</ul>
<p dir="auto">Example of reading from register 0xF2h where factory default value is 0xFFh</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>sudo i2cget -y 1 0x57 0xf2
0xff
</code></pre>
</div>
<p dir="auto">Use the<code>i2cset</code>command to write data to an I2C device. The format of this command as follows</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>i2cset [-m mask] [-f] [-y] 1    [MODE]
</code></pre>
</div>
<p dir="auto">Options:</p>
<ul dir="auto">
<li>-f, -y, DEVICE_ADDRESS, ADDRESS as for i2cget</li>
<li>-m mask the bit mask parameter, if specified, describes which bits of value will be actually written to data-address. Bits set to 1 in the mask are taken from VALUE, while bits set to 0 will be read from ADDRESS and thus preserved by the operation</li>
</ul>
<p dir="auto">Example of writing value 0x77h to the first byte of user space SRAM (starts from 0x00h) and then reading</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>sudo i2cset -y 1 0x57 0x00 0x77
sudo i2cget -y 1 0x57 0x00
0x77
</code></pre>
</div>
<h3 dir="auto">
<a id="user-content-security-" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#security-" aria-hidden="true"></a>Security<a id="user-content-security"></a>
</h3>
<p dir="auto">As a security precaution, system devices are not exposed by default inside Docker containers. You can expose specific devices to your container using the<code>--device</code>option to docker run, as in</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>docker run --device /dev/i2c-1 app-image
</code></pre>
</div>
<p dir="auto">or remove all restrictions with the<code>--privileged</code>flag</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>docker run --privileged app-image
</code></pre>
</div>
<h3 dir="auto">
<a id="user-content-user-space-eeprom-" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#user-space-eeprom-" aria-hidden="true"></a>User space EEPROM<a id="user-content-user-space-eeprom"></a>
</h3>
<p dir="auto">I2C expander and RTCC each have 64 bytes of general purpose user memory, so combined you have 128 bytes of EEPROM. On device 0x57h user space address from 0x00h to 0x3Fh and on 0x6Fh from 0x20h to 0x5Fh.</p>
<h3 dir="auto">
<a id="user-content-ethernet-switch-" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#ethernet-switch-" aria-hidden="true"></a>Ethernet Switch<a id="user-content-ethernet-switch"></a>
</h3>
<p dir="auto">The onboard RTL8370 integrate all the functions of a high-speed switch system; including SRAM for packet buffering, non-blocking switch fabric, and internal register management into a single CMOS device.</p>
<p dir="auto">Device Address: 0x5Ch</p>
<h3 dir="auto">
<a id="user-content-i2c-expander-" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#i2c-expander-" aria-hidden="true"></a>I2C expander<a id="user-content-i2c-expander"></a>
</h3>
<p dir="auto">I2C expander offers users a digitally programmable alternative to hardware jumpers and mechanical switches that are being used to control power on compute nodes.</p>
<p dir="auto">Device Address: 0x57h</p>
<table>
<thead>
<tr>
<th align="left">Register</th>
<th align="left">Register Description</th>
<th align="left">Default Value</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">0x00h to 3Fh</td>
<td align="left">64 bytes of general-purpose user EEPROM</td>
<td align="left">0x00h</td>
</tr>
<tr>
<td align="left">0xF2h</td>
<td align="left">Control Register</td>
<td align="left">0xFFh</td>
</tr>
<tr>
<td align="left">0xF8h</td>
<td align="left">Status Register</td>
<td align="left">–</td>
</tr>
<tr>
<td align="left">0xFAh to 0xFFh</td>
<td align="left">6 bytes of general-purpose user SRAM</td>
<td align="left">–</td>
</tr>
</tbody>
</table>
<h3 dir="auto">
<a id="user-content-power-management" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#power-management" aria-hidden="true"></a>Power Management</h3>
<p dir="auto">On/off compute module through register 0xF2h</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code># Bits  : 76543210
# Slots : 5674321x

# Power off worker nodes 2,3,4,5,6,7
sudo i2cset -m 0xfc -y 1 0x57 0xf2 0x00

# Power on worker nodes
sudo i2cset -m 0xfc -y 1 0x57 0xf2 0xff
</code></pre>
</div>
<p dir="auto">On/off compute module by node number</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>
# 0x02 : Node #1 (Master)
# 0x04 : Node #2 (Worker 1)
# 0x08 : Node #3 (Worker 2)
# 0x10 : Node #4 (Worker 3)
# 0x80 : Node #5 (Worker 4)
# 0x40 : Node #6 (Worker 5)
# 0x20 : Node #7 (Worker 6)

# Power off node #3
sudo i2cset -m 0x08 -y 1 0x57 0xf2 0x00

# Power on node #7
sudo i2cset -m 0x20 -y 1 0x57 0xf2 0xff
</code></pre>
</div>
<p dir="auto">Read on/off status</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>sudo i2cget -y 1 0x57 0xf2
0xff
</code></pre>
</div>
<h4 dir="auto">
<a id="user-content-compute-module-status-" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#compute-module-status-" aria-hidden="true"></a>Compute Module status<a id="user-content-compute-module-status"></a>
</h4>
<p dir="auto">Read compute module status through register 0xF8h</p>
<ul dir="auto">
<li>1 = Installed in slot AND switched on</li>
<li>0 = Not installed in slot OR switched off</li>
</ul>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code># Bits  : 76543210
# Slots : 5674321x

sudo i2cget -y 1 0x57 0xf8
0x0e  # register 0xf2 = 0xff, all slots are power up but installed only three
</code></pre>
</div>
<h3 dir="auto">
<a id="user-content-real-time-clockcalendar-" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#real-time-clockcalendar-" aria-hidden="true"></a>Real-Time Clock/Calendar<a id="user-content-real-time-clock-calendar"></a>
</h3>
<p dir="auto">Real-Time Clock/Calendar (RTCC) tracks time using internal counters for hours, minutes, seconds, days, months, years, and day of week. Alarms can be configured on all counters up to and including months.</p>
<p dir="auto">SRAM and timekeeping circuitry are powered from the back-up supply when main power is lost, allowing the device to maintain accurate time and the SRAM contents. The times when the device switches over to the back-up supply and when primary power returns are both logged by the power-fail time-stamp.</p>
<p dir="auto">Device Address: 0x6Fh</p>
<table>
<thead>
<tr>
<th align="left">Register</th>
<th align="left">Register Description</th>
<th align="left">Default Value</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">0x00h to 0x06h</td>
<td align="left">Time &amp; Date</td>
<td align="left">–</td>
</tr>
<tr>
<td align="left">0x07h to 0x09h</td>
<td align="left">Configuration and Trimming</td>
<td align="left">–</td>
</tr>
<tr>
<td align="left">0x0Ah to 0x10h</td>
<td align="left">Alarm 0</td>
<td align="left">–</td>
</tr>
<tr>
<td align="left">0x11h to 0x17h</td>
<td align="left">Alarm 1</td>
<td align="left">–</td>
</tr>
<tr>
<td align="left">0x18h to 0x1Fh</td>
<td align="left">Power-Fail/Power-Up Time-Stamps</td>
<td align="left">–</td>
</tr>
<tr>
<td align="left">0x20h to 0x5Fh</td>
<td align="left">64 bytes of general-purpose user SRAM</td>
<td align="left">–</td>
</tr>
</tbody>
</table>
<h4 dir="auto">
<a id="user-content-time--date-" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#time--date-" aria-hidden="true"></a>Time &amp; Date<a id="user-content-time-date"></a>
</h4>
<p dir="auto">Show time from hardware RTC</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>sudo hwclock --show
2019-10-26 20:21:03.005326+01:00
</code></pre>
</div>
<h3 dir="auto">
<a id="user-content-external-i2c-port-" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#external-i2c-port-" aria-hidden="true"></a>External I2C port<a id="user-content-external-i2c-port"></a>
</h3>
<p dir="auto">Pinouts</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>|  1  |  2  |  3  |  4  |
| GND | VCC | SCL | SDA |
</code></pre>
</div>
<h2 dir="auto">
<a id="user-content-reference-links-" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/cluster-management-bus-i2c.md#reference-links-" aria-hidden="true"></a>Reference Links<a id="user-content-reference-links"></a>
</h2>
<ul dir="auto">
<li><a href="https://datasheets.maximintegrated.com/en/ds/DS4520.pdf" rel="nofollow">I2C expander datasheet</a></li>
<li><a href="http://ww1.microchip.com/downloads/en/DeviceDoc/MCP7940N-Battery-Backed-I2C-RTCC-with-SRAM-20005010G.pdf" rel="nofollow">RTC datasheet</a></li>
</ul>