<p dir="auto">To change a hostname, you should first login into the node using SSH and then type</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>sudo nano /etc/cloud/cloud.cfg
</code></pre>
</div>
<p dir="auto">Then find<code>preserve_hostname: false</code>and change it to<code>preserve_hostname: true</code></p>
<p dir="auto">Save and exit.</p>
<p dir="auto"><a href="https://github.com/turing-machines/v1-docs/blob/master/.gitbook/assets/image%20%285%29.png" target="_blank" rel="noopener noreferrer"><img src="https://github.com/turing-machines/v1-docs/raw/master/.gitbook/assets/image%20%285%29.png" alt=""></a></p>
<p dir="auto">Then type the following command in your terminal window</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>sudo nano /etc/hostname
</code></pre>
</div>
<p dir="auto">Change the hostname to<code>worker01</code>, for example.</p>
<p dir="auto">Save, exit, and reboot.</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>sudo reboot</code></pre>
</div>