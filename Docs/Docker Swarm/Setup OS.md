<p>The process for setting up the operating system for Docker Swarm is similar to the process of setting up the operating system for a Kubernetes cluster, as outlined in our <strong><a href="https://help.turingpi.com/hc/en-us/articles/8942913487901" target="_self">Kubernetes OS setup guide</a></strong>. While following this guide, simply make<strong> one exception</strong> when editing the <strong>dietpi.txt</strong> file.</p>
<p>When configuring software to install <strong>in the dietpi.txt</strong> file, it is important to <strong>include</strong> the following line among the lines that begin with "AUTO_SETUP_INSTALL_SOFTWARE_ID".</p>
<p>Note, you can find the list of IDs available for automated installation <a href="https://github.com/MichaIng/DietPi/wiki/DietPi-Software-list" target="_blank" rel="noopener"><strong>HERE</strong></a>.</p>
<ul>
<li>ID 134 - Docker Compose</li>
<li>ID 162 - Docker</li>
<li>ID 17 - Git</li>
</ul>
<pre>AUTO_SETUP_INSTALL_SOFTWARE_ID=134<br>AUTO_SETUP_INSTALL_SOFTWARE_ID=162<br>AUTO_SETUP_INSTALL_SOFTWARE_ID=17</pre>
<p><strong><span class="wysiwyg-color-green120">When you can SSH to each node, please return here to this guide and continue with Swarm Deploy.</span></strong></p>
<p> </p>