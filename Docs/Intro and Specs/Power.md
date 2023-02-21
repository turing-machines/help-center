<p>Of course, you need to power your Compute Modules, and our Turing Pi V2 made this very easy for you. We have integrated the standard <strong>ATX 24 Pin</strong> power connector and all the power regulation is done for you automatically.</p>
<p>You can use any <strong>ATX PC PSU</strong>, just make sure it provides enough power to power Compute Modules and all attached peripherals. You should easily get away with 300W+ PSU (It seems to be harder to find lower Watt PSU in standard ATX format).</p>
<p>Another option, popular with custom cases and 3D printed cases, is the use of <strong>Pico PSU. </strong>We have tested with 150W Pico PSU but keep in mind that in this case the power source for the Pico PSU is important and we do not recommend going under 10A for 12V power supply. <strong>Always check your Pico PSU documentation for the recommended input voltage.</strong></p>
<p> </p>
<p>Tested with:</p>
<p><a href="https://turingpi.com/product/pico-psu/" target="_blank" rel="noopener">Pico PSU 160w</a> + <a href="https://turingpi.com/product/power-supply/" target="_blank" rel="noopener">DC 12V 12A Power Supply</a></p>
<p><a href="https://www.gigabyte.com/Power-Supply/GP-P450B" target="_self">Gigabyte P450B</a></p>
<h2> </h2>
<h2>Power Draw</h2>
<p>This table is just sample information, and real world power draw will be influenced by compute module combination, type of PSU, actual load and attached external peripherals.</p>
<p> </p>
<table style="border-collapse: collapse; width: 119.14%; height: 154px;" border="1">
<tbody>
<tr style="height: 22px;">
<td class="wysiwyg-text-align-center" style="width: 17.6418%; height: 22px;"><strong>PSU Type</strong></td>
<td class="wysiwyg-text-align-center" style="width: 13.7849%; height: 22px;"><strong>Slot 1</strong></td>
<td class="wysiwyg-text-align-center" style="width: 15.8927%; height: 22px;"><strong>Slot 2</strong></td>
<td class="wysiwyg-text-align-center" style="width: 19.9946%; height: 22px;"><strong>Slot 3</strong></td>
<td class="wysiwyg-text-align-center" style="width: 16.9939%; height: 22px;"><strong>Slot 4</strong></td>
<td class="wysiwyg-text-align-center" style="width: 33.5979%; height: 22px;"><strong>Power Draw</strong></td>
</tr>
<tr style="height: 22px;">
<td style="width: 17.6418%; height: 22px;">Pico PCU 150W</td>
<td style="width: 13.7849%; height: 22px;">Empty</td>
<td style="width: 15.8927%; height: 22px;">Empty</td>
<td style="width: 19.9946%; height: 22px;">Empty</td>
<td style="width: 16.9939%; height: 22px;">Empty</td>
<td style="width: 33.5979%; height: 22px;">3.6W - 4W</td>
</tr>
<tr style="height: 22px;">
<td style="width: 17.6418%; height: 22px;">Gigabyte P450B</td>
<td style="width: 13.7849%; height: 22px;">Empty</td>
<td style="width: 15.8927%; height: 22px;">Empty</td>
<td style="width: 19.9946%; height: 22px;">Empty</td>
<td style="width: 16.9939%; height: 22px;">Empty</td>
<td style="width: 33.5979%; height: 22px;">2.4W</td>
</tr>
<tr style="height: 22px;">
<td style="width: 17.6418%; height: 22px;">Pico PCU 150W</td>
<td style="width: 13.7849%; height: 22px;">Raspberry CM</td>
<td style="width: 15.8927%; height: 22px;">Raspberry CM</td>
<td style="width: 19.9946%; height: 22px;">Raspberry CM</td>
<td style="width: 16.9939%; height: 22px;">Raspberry CM</td>
<td style="width: 33.5979%; height: 22px;">15.9W - 20W</td>
</tr>
<tr style="height: 22px;">
<td style="width: 17.6418%; height: 22px;">Gigabyte P450B</td>
<td style="width: 13.7849%; height: 22px;">Raspberry CM</td>
<td style="width: 15.8927%; height: 22px;">Raspberry CM</td>
<td style="width: 19.9946%; height: 22px;">Raspberry CM</td>
<td style="width: 16.9939%; height: 22px;">Raspberry CM</td>
<td style="width: 33.5979%; height: 22px;">24.4W</td>
</tr>
<tr style="height: 22px;">
<td style="width: 17.6418%; height: 22px;">Pico PCU 150W</td>
<td style="width: 13.7849%; height: 22px;">Raspberry CM</td>
<td style="width: 15.8927%; height: 22px;">Raspberry CM</td>
<td style="width: 19.9946%; height: 22px;">Nvidia Jetson Nano</td>
<td style="width: 16.9939%; height: 22px;">Nvidia Jetson TX2</td>
<td style="width: 33.5979%; height: 22px;">18.7W - 20.1W</td>
</tr>
<tr style="height: 22px;">
<td style="width: 17.6418%; height: 22px;">Gigabyte P450B</td>
<td style="width: 13.7849%; height: 22px;">Raspberry CM</td>
<td style="width: 15.8927%; height: 22px;">Raspberry CM</td>
<td style="width: 19.9946%; height: 22px;">Nvidia Jetson Nano</td>
<td style="width: 16.9939%; height: 22px;">Nvidia Jetson TX2</td>
<td style="width: 33.5979%; height: 22px;">25.6W</td>
</tr>
</tbody>
</table>
<p> </p>