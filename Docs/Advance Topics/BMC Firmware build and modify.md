<p><span class="wysiwyg-color-red wysiwyg-font-size-x-large">Note, this guide is not fully working, build process is failing with missing library, waiting for developer to respond.</span></p>
<p> </p>
<p>In this guide, we are going to look how can you build and modify the BMC firmware. Mind you, since the firmware is still in active development, this is subject to change. We will try to keep this guide updated as much as possible.</p>
<h2>Where do I get the BMC firmware ?</h2>
<p>You can find the BMC and $tpi on our <a href="https://github.com/wenyi0421/turing-pi" target="_blank" rel="noopener"><strong>GitHub repo</strong></a>.</p>
<p>Some important folders:</p>
<ul>
<li>
<strong>app &gt; bmc</strong>  - Here you can find source code to the BMC and the web interface</li>
<li>
<strong>app &gt; tpi</strong> - $tpi command line utility</li>
<li>
<strong>app &gt; board &gt; rootfs_overlay &gt; mnt &gt; var &gt; www </strong>- HTML files for web interface</li>
</ul>
<p>These utilities are written in C and you can modify them as you like, however we would advise against it at the moment, since the software is in active development. Both apps are build automatically by Buildroot during image creation.</p>
<p>In this section we are going to focus more on modifying the underlying Linux OS, maybe you want to add some utility, enable functionality or just want to rebuild the BMC from scratch and ref lash the Turing Pi V2 board.</p>
<h2>Reconfiguring BMC Linux OS</h2>
<p><strong>For this process you will need a Linux OS with internet access</strong>, if you are on a Windows machine you can use <a href="https://learn.microsoft.com/en-us/windows/wsl/install" target="_blank" rel="noopener"><strong>WSL</strong> </a>just fine. We will use Ubuntu WSL in this example, but the process should be nearly identical on other version of Linux except maybe the required package installation command.</p>
<p> </p>
<h2>A bit of theory</h2>
<p>We are using <a href="https://buildroot.org/" target="_blank" rel="noopener"><strong>Buildroot</strong> </a>project to build an embedded version of Linux OS for our Turing Pi V2, these have its advantages like:</p>
<ul>
<li>It is lightweight and easy to use, making it suitable for small embedded systems.</li>
<li>It can be easily customized to fit the specific needs of the project.</li>
<li>It allows for easy management of package dependencies.</li>
</ul>
<p>However, one disadvantage is that every time you change something you need to rebuild the whole Linux OS, and that can take time (depending on your HW specs).</p>
<h3>The Buildroot project folder structure is organized as follows:</h3>
<ul>
<li>buildroot/: The top-level directory of the Buildroot project.</li>
<li>buildroot/board/: This directory contains the board-specific configuration files for different architectures.</li>
<li>buildroot/configs/: This directory contains the default configuration files for different boards and architectures.</li>
<li>buildroot/dl/: This directory is used to store downloaded source files for packages.</li>
<li>buildroot/package/: This directory contains the package configuration files and patches.</li>
<li>buildroot/output/: This directory contains the output of the Buildroot build process.</li>
<li>buildroot/output/build/: This directory contains the built packages and target filesystem.</li>
<li>buildroot/output/images/: This directory contains the final images that can be written to the target device.</li>
<li>buildroot/output/host/: This directory contains the host utilities and libraries.</li>
<li>buildroot/Makefile: This is the top-level Makefile that controls the build process.</li>
<li>buildroot/.config: This file contains the current configuration of the Buildroot project.</li>
<li>buildroot/README: This file contains the project documentation</li>
</ul>
<p>This is the general structure, and we have some more folder that modify the buildroot like br2t113pro:</p>
<ul>
<li>
<strong>br2t113pro &gt; configs &gt; 100ask_t113-pro_spinand_core_defconfig</strong> - contains configuration for Turing Pi V2 board</li>
</ul>
<h2>Building the image</h2>
<p><strong>Install</strong> the following required packages for building the image.</p>
<pre>sudo apt-get install build-essential subversion git-core libncurses5-dev \<br>zlib1g-dev gawk flex quilt libssl-dev xsltproc libxml-parser-perl mercurial \<br>bzr ecj cvs unzip lib32z1 lib32z1-dev lib32stdc++6 libstdc++6 libncurses-dev \<br>u-boot-tools -y</pre>
<p><strong>Clone </strong>the GitHub repository.</p>
<pre>git clone <a href="https://github.com/wenyi0421/turing-pi">https://github.com/wenyi0421/turing-pi</a><br>cd turing-pi/buildroot</pre>
<p><strong>Set up</strong> the make with custom configuration.</p>
<pre>make   BR2_EXTERNAL="../br2t113pro"  100ask_t113-pro_spinand_core_defconfig</pre>
<p><strong>Customize</strong>. Here you have space to add more custom software to the OS</p>
<pre>make menuconfig</pre>
<p>The above command will show you a text based GUI where you can modify further the Linux OS we are going to create.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/8942382244893" alt="makemenuconfig.png"></p>
<p>You can go to <strong>"Target packages"</strong> and enable additional software. Keep in mind that the onboard CPU and RAM is limited, so we would not recommend including stuff like docker or any CPU/RAM intensive processes.</p>
<p>For example, if you want to add screen go <strong>"Target packages&gt;Shell and utilities&gt;screen"</strong></p>
<p><strong><img src="https://help.turingpi.com/hc/article_attachments/8942331405597" alt="makemenuconfig2.png"></strong></p>
<p>You can then <strong>Save and Exit.</strong></p>
<h2>Build</h2>
<p>Run build command.</p>
<pre>make V=1</pre>
<p>This can take some time to finish, depending on your PC. Make will compile all the source code to useful binary applications, and in the end produce .img file that you can use to flash to Turing Pi V2.</p>
<h2>Images</h2>
<p>You can find the .img in <strong>buildroot/output/images/ </strong> and if you want to build <strong>.swu</strong> for OTA update do following:</p>
<pre>cd output/images/<br>./genSWU.sh 1.0.0</pre>