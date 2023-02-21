<h2>Install</h2>
<p>Install <strong>on all nodes</strong>:</p>
<pre>apt-get -y install glusterfs-server</pre>
<pre><code>sudo systemctl start glusterd
sudo systemctl enable glusterd</code></pre>
<h2>Prepare disks</h2>
<p>We will be using Node1 to Node3 that are equipped with SATA SSD storage (through the use of microPCIe to SATA converters). However, you can also opt for using only Node3 that has built-in SATA connectors and a single disk, but it will be the sole point of failure in the system.</p>
<h3>Bricks</h3>
<p>Bricks in GlusterFS are basic building blocks that represent individual disk storage. A GlusterFS storage volume is created by grouping multiple bricks together. These bricks can be located on different servers and combined to create a large and scalable distributed file system. Bricks are used to store actual data and make up the physical storage of the GlusterFS volume.</p>
<p><strong>On nodes with disks:</strong></p>
<p>Find your disk/s</p>
<pre>root@cube03:~# fdisk -l<br><br>Disk /dev/ram0: 4 MiB, 4194304 bytes, 8192 sectors<br>Units: sectors of 1 * 512 = 512 bytes<br>Sector size (logical/physical): 512 bytes / 4096 bytes<br>I/O size (minimum/optimal): 4096 bytes / 4096 bytes<br>.<br>.<br>.<br>Disk /dev/sda: 465.76 GiB, 500107862016 bytes, 976773168 sectors<br>Disk model: Samsung SSD 870<br>Units: sectors of 1 * 512 = 512 bytes<br>Sector size (logical/physical): 512 bytes / 512 bytes<br>I/O size (minimum/optimal): 512 bytes / 512 bytes</pre>
<p>So for us its <strong>/dev/sda.</strong></p>
<pre># Format your disk (If you decided to use your internal storage, just ignore the mount and format steps)<br>mkfs.xfs -f /dev/sda<br><br># Create brick folder location<br>mkdir -p /data/glusterfs/myvol1/brick1<br><span id="e419" class="mj ko ih mg b ge mr ml l mm mn" data-selectable-paragraph=""><br># Add line to /etc/fstab to auto mount this disk to /data on boot<br>echo "/dev/sda /data/glusterfs/myvol1/brick1 xfs defaults 0 0" | tee -a /etc/fstab<br><br># Create brick folder <br>mkdir -p /data/glusterfs/myvol1/brick1/brick<br><br># Mount<br>mount -a<br><br># Check <br>df -h /data/glusterfs/myvol1/brick1<br></span></pre>
<h2>Cluster</h2>
<p>On the designated master node, we can now inform GlusterFS of the presence of all nodes in the network.</p>
<pre>root@cube01:~# gluster peer probe cube02<br>peer probe: success<br>root@cube01:~# gluster peer probe cube03<br>peer probe: success<br>root@cube01:~# gluster peer probe cube04<br>peer probe: success<br># Check<br>root@cube01:~# gluster pool list<br>UUID Hostname State<br>a67aa75b-a359-4064-844e-e32ebedda4a0 cube02 Connected <br>475c599d-57dc-4c9d-a8d9-90da965cdddb cube03 Connected <br>df807f9a-1e56-4dd7-a8be-4dc98de57192 cube04 Connected <br>4cd1dc90-3f42-4bd7-9510-54ad9a3fcdb2 localhost Connected </pre>
<h2>Volume</h2>
<p>In GlusterFS, a volume is a logical unit of storage created from one or more bricks (physical storage resources). The volume is the main entity through which data is stored, retrieved, and managed. It provides a single namespace for data and enables high availability, scalability, and data protection features. Simply put, a GlusterFS volume is a pool of storage space that can be accessed from multiple servers in a cluster.</p>
<h3>Option 1:</h3>
<p>Creating volume on <strong>single node</strong> with disk (node3)</p>
<pre>root@cube01:~# gluster volume create dockervol1 cube03:/data/glusterfs/myvol1/brick1/brick<br>volume create: dockervol1: success: please start the volume to access data</pre>
<p>Check</p>
<pre>root@cube01:~# gluster volume info<br><br>Volume Name: dockervol1<br>Type: Distribute<br>Volume ID: a8b4fa7f-df8c-4788-be70-2acc0544afd6<br>Status: Created<br>Snapshot Count: 0<br>Number of Bricks: 1<br>Transport-type: tcp<br>Bricks:<br>Brick1: cube03:/data/glusterfs/myvol1/brick1/brick<br>Options Reconfigured:<br>storage.fips-mode-rchecksum: on<br>transport.address-family: inet<br>nfs.disable: on<br>root@cube01:~# gluster volume start dockervol1<br>volume start: dockervol1: success<br>root@cube01:~# gluster volume info</pre>
<p>Start the volume before we can mount it.</p>
<pre>root@cube01:~# gluster volume start dockervol1<br>volume start: dockervol1: success</pre>
<h3>Option 2:</h3>
<p>To create a GlusterFS volume using 3 nodes, make sure the disks have been mounted under <strong>"/data/glusterfs/myvol1</strong>" and that the folder "<strong>/data/glusterfs/myvol1/brick2</strong>" has been created on all 3 nodes. Note, if you are using the root file system, you will need to include the "force" option at the end of the command.</p>
<p><strong>On master node:</strong></p>
<pre>gluster volume create dockervol2 disperse 3 redundancy 1 cube01:/data/glusterfs/myvol1/brick2 cube02:/data/glusterfs/myvol1/brick2 cube03:/data/glusterfs/myvol1/brick2</pre>
<p>Interesting parts are:</p>
<ul>
<li>
<strong>disperse</strong> - Read more <a href="https://docs.gluster.org/en/main/Administrator-Guide/Setting-Up-Volumes/#volume-types" target="_blank" rel="noopener noreferrer"><strong>HERE</strong></a>
</li>
<li>
<strong>redundancy</strong> - Read more <a href="https://docs.gluster.org/en/main/Administrator-Guide/Setting-Up-Volumes/#volume-types" target="_blank" rel="noopener noreferrer"><strong>HERE</strong></a>
</li>
</ul>
<pre>root@cube01:~# gluster volume info dockervol2<br><br>Volume Name: dockervol2<br>Type: Disperse<br>Volume ID: 27c79497-848e-4fe4-80c5-18b7d5471001<br>Status: Started<br>Snapshot Count: 0<br>Number of Bricks: 1 x (2 + 1) = 3<br>Transport-type: tcp<br>Bricks:<br>Brick1: cube01:/data/glusterfs/myvol1/brick2<br>Brick2: cube02:/data/glusterfs/myvol1/brick2<br>Brick3: cube03:/data/glusterfs/myvol1/brick2<br>Options Reconfigured:<br>storage.fips-mode-rchecksum: on<br>transport.address-family: inet<br>nfs.disable: on</pre>
<p><strong>Start the volume.</strong></p>
<pre>root@cube01:~# gluster volume start dockervol2<br>volume start: dockervol2: success</pre>
<h2>Mounting GlusterFS</h2>
<p>We have our volumes, now we can mount one of them to nodes.</p>
<p><strong>On all nodes</strong>:</p>
<pre>mkdir /mnt/docker-storage<br>echo "localhost:/dockervol1 /mnt/docker-storage glusterfs defaults,_netdev 0 0" | tee -a /etc/fstab<br>mount -a<br><br></pre>
<p>Our persistent shared storage will be "/mnt/docker-storage"</p>
<h3>Check:</h3>
<pre>root@cube02:~# df -h /mnt/docker-storage<br>Filesystem Size Used Avail Use% Mounted on<br>localhost:/dockervol1 466G 8.0G 458G 2% /mnt/docker-storage</pre>
<h3>Test:</h3>
<pre># Create file on node4<br>touch /mnt/docker-storage/test_file<br># Check on node1 in same location and the file should be there.</pre>
<p>Now that the GlusterFS volume has been created and mounted to "/mnt/docker-storage", you can utilize this directory in your Docker deployments to ensure that any persistent files will be accessible regardless of which node in the swarm the container is running on.</p>
<h2>Security</h2>
<p>By default, GlusterFS server allows mounting volume to everybody, you can limit it to specific IPs. In our case, IPs of our nodes. On your master node (cube01):</p>
<pre>gluster volume set dockervol1 auth.allow 127.0.0.1,10.0.0.60,10.0.0.61,10.0.0.62,10.0.0.63<br>gluster volume set dockervol2 auth.allow 127.0.0.1,10.0.0.60,10.0.0.61,10.0.0.62,10.0.0.63</pre>