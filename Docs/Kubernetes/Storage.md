<h1>Option 1: The NFS provider</h1>
<p>This is the easiest and most straightforward option, which requires the least amount of physical disks. In this setup, we will convert Node3 to double as a Samba server. Node3 is connected to two SATA ports on the Turing Pi V2. Simply connect a disk of your choice to one of these ports. It is important to note that you should not hot-plug the disk. To do this, shut down the node using the $tpi command or through the BMC web interface, plug in the disk, and then restart the node.</p>
<p>When the Node3 is up and ready, SSH connect to its OS.</p>
<p>You should be able to identify the disk using<strong> fdisk -l</strong> command, in our case it's Samsung SSD 870</p>
<pre>root@cube03:~# fdisk -l<br><br>Disk /dev/ram0: 4 MiB, 4194304 bytes, 8192 sectors<br>Units: sectors of 1 * 512 = 512 bytes<br>Sector size (logical/physical): 512 bytes / 4096 bytes<br>I/O size (minimum/optimal): 4096 bytes / 4096 bytes<br>.<br>.<br>.<br>Disk /dev/sda: 465.76 GiB, 500107862016 bytes, 976773168 sectors<br>Disk model: Samsung SSD 870 <br>Units: sectors of 1 * 512 = 512 bytes<br>Sector size (logical/physical): 512 bytes / 512 bytes<br>I/O size (minimum/optimal): 512 bytes / 512 bytes<br><br></pre>
<p>So for us its <strong>/dev/sda.</strong></p>
<h2>Install NFS server</h2>
<p>On Node3, we will be configuring the NFS server. This involves formatting the disk, mounting it, and exporting it to make it accessible to other nodes in the network.</p>
<pre class="mb mc md me fz mf mg mh mi aw go bi"><span id="e419" class="mj ko ih mg b ge mr ml l mm mn" data-selectable-paragraph=""># Formating the new disk<br>mkfs.ext4 /dev/sda<br><br># Create /data folder<br>mkdir /data<br><br># Add line to /etc/fstab to auto mount this disk to /data on boot<br>echo "/dev/sda /data ext4 defaults 0 0" | tee -a /etc/fstab<br><br># Mount the disk to /data folder<br>mount -a<br><br># Confirm the /data is mounted with<br>df -h /data<br><br># Above should return something similar to this:<br>Filesystem Size Used Avail Use% Mounted on<br>/dev/sda 458G 28K 435G 1% /data<br><br># Install NFS server<br>apt install -y nfs-server<br><br># Tell NFS what to export<br>echo "/data *(rw,no_subtree_check,no_root_squash)" | tee -a /etc/exports<br><br># Enable and start NFS server<br>systemctl enable --now nfs-server<br><br># Tell NFS server to reload and re-export what's in /etc/exports (just in case)<br>exportfs -ar<br><br># Check if the /data is exported<br>root@cube03:~# showmount -e localhost<br>Export list for localhost:<br>/data *<br></span><span id="e419" class="mj ko ih mg b ge mr ml l mm mn" data-selectable-paragraph=""></span></pre>
<h2>Install NFS client</h2>
<p>On every single node, install NFS client</p>
<pre>apt install -y nfs-common</pre>
<h2>Install NFS StorageClass</h2>
<p>In Kubernetes, a StorageClass is a way to define a set of parameters for a specific type of storage that is used in a cluster. This type of storage is used by Persistent Volumes (PVs) and Persistent Volume Claims (PVCs).</p>
<p>A Persistent Volume (PV) is a piece of storage in the cluster that has been dynamically provisioned, and it represents a specific amount of storage capacity.</p>
<p>A Persistent Volume Claim (PVC) is a request for storage by a user. It asks for a specific amount of storage to be dynamically provisioned based on the specified StorageClass. The PVC is then matched to a PV that meets the requirements defined in the PVC, such as storage capacity, access mode, and other attributes.</p>
<p>The relationship between PVs, PVCs and StorageClasses is that the PVC specifies the desired storage attributes and the StorageClass provides a way to dynamically provision the matching PV, while the PV represents the actual piece of storage that is used by the PVC.</p>
<p>We are going to use <a href="https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner" target="_blank" rel="noopener noreferrer"><strong>nfs-subdir-external-provisioner</strong></a> and install it via Helm. <strong>Please do the following on your master node.</strong></p>
<pre># Add repo to helm<br>helm repo add nfs-subdir-external-provisioner <a href="https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/">https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/</a><br># Install<br>helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \<br>--set nfs.server=10.0.0.62 \<br>--set nfs.path=/data \<br>--create-namespace \<br>--namespace nfs-system</pre>
<p>Note, we have specified IP of our NFS server (Node3), and what is the exported path.</p>
<p>Check if you have StorageClass with "<strong>kubectl get storageclas</strong>":</p>
<pre>root@cube01:~# kubectl get storageclass<br>NAME PROVISIONER RECLAIMPOLICY VOLUMEBINDINGMODE ALLOWVOLUMEEXPANSION AGE<br>nfs-client cluster.local/nfs-subdir-external-provisioner Delete Immediate true 10m</pre>
<h2>Test</h2>
<p>We are going to create a Persistent Volume Claim (PVC) using the recently created StorageClass and test it with a test application.</p>
<p>The process will involve using test files from a git repository, which you can preview in your browser. The first file will define the PVC with a size of 1 MB and the second file will create a container, mount the PVC, and create a file named "SUCCESS". This will help us verify if the entire process happens without any problems.</p>
<p><strong>On your master node:</strong></p>
<pre>kubectl create -f https://raw.githubusercontent.com/kubernetes-sigs/nfs-subdir-external-provisioner/master/deploy/test-claim.yaml</pre>
<p><strong>Check:</strong></p>
<pre>root@cube01:~# kubectl get pv<br>NAME CAPACITY ACCESS MODES RECLAIM POLICY STATUS CLAIM STORAGECLASS REASON AGE<br>pvc-f512863d-6d94-4887-a7ff-ab05bf384f39 1Mi RWX Delete Bound default/test-claim nfs-client 11s<br><br>root@cube01:~# kubectl get pvc<br>NAME STATUS VOLUME CAPACITY ACCESS MODES STORAGECLASS AGE<br>test-claim Bound pvc-f512863d-6d94-4887-a7ff-ab05bf384f39 1Mi RWX nfs-client 15s</pre>
<p>We have requested PVC of 1MB and StorageClass using the NFS server created PV of size 1MB</p>
<p><strong>Create POD:</strong></p>
<pre>kubectl create -f https://raw.githubusercontent.com/kubernetes-sigs/nfs-subdir-external-provisioner/master/deploy/test-pod.yaml</pre>
<p><strong>Check:</strong></p>
<pre>root@cube01:~# kubectl get pods<br>NAME READY STATUS RESTARTS AGE<br>test-pod 0/1 Completed 0 45s</pre>
<p>Test pod started, and finished ok. Now we should have something in /data on our NFS server (Node3)</p>
<pre>root@cube03:~# cd /data<br>root@cube03:/data# ls<br>default-test-claim-pvc-f512863d-6d94-4887-a7ff-ab05bf384f39 lost+found<br>root@cube03:/data# cd default-test-claim-pvc-f512863d-6d94-4887-a7ff-ab05bf384f39/<br>root@cube03:/data/default-test-claim-pvc-f512863d-6d94-4887-a7ff-ab05bf384f39# ls<br>SUCCESS<br><br></pre>
<p>As you can see there is a folder "<strong>default-test-claim-pvc-...." </strong>and in it file called SUCCESS.</p>
<h3><span class="wysiwyg-color-black">Cleanup</span></h3>
<p><span class="wysiwyg-color-black">To remove the tests, we can call the same files in reverse order with "delete":</span></p>
<pre>kubectl delete -f https://raw.githubusercontent.com/kubernetes-sigs/nfs-subdir-external-provisioner/master/deploy/test-pod.yaml<br>kubectl delete -f https://raw.githubusercontent.com/kubernetes-sigs/nfs-subdir-external-provisioner/master/deploy/test-claim.yaml</pre>
<p><span class="wysiwyg-color-green120"><strong>W</strong><strong>e can now assume the test was successful and this StorageClass can be used across our cluster.</strong></span></p>
<h1>Option 2: The Longhorn</h1>
<p><a href="https://longhorn.io/" target="_blank" rel="noopener noreferrer">Longhorn</a> is probably the most lightweight option for distributed file system, and therefore the clear choice for our cluster. Its setup is not difficult, but it does require more hardware than option 1.</p>
<p>Longhorn provides several benefits in a Kubernetes environment:</p>
<ul>
<li>
<strong>High availability:</strong> Longhorn ensures that data is available even in the event of a node failure.</li>
<li>
<strong>Easy data management</strong>: Longhorn provides a simple interface for managing volumes and snapshots, which can simplify data management.</li>
<li>
<strong>Scalable storage</strong>: Longhorn is designed to scale dynamically, allowing storage capacity to grow as needed.</li>
<li>
<strong>Performance</strong>: Longhorn leverages local storage to provide high-performance storage for containers.</li>
<li>
<strong>Data protection</strong>: Longhorn provides data protection through snapshots and backups, ensuring data is not lost in the event of a disaster.</li>
</ul>
<p>Longhorn is designed to use the underlying operating system's file system, rather than RAW devices. In a Kubernetes environment, Longhorn uses a designated /&lt;directory&gt; mount point on each node. The exact nature of the storage used at that mount point can vary. It can be the internal storage or an SD card, or it can be a separate SATA disk that has been mounted at that location, which is the recommended setup. Longhorn is agnostic to the specific storage technology.</p>
<h2>Prepare</h2>
<p><strong>On each node, install the following:</strong></p>
<pre>apt -y install nfs-common open-iscsi util-linux</pre>
<p><strong>Prepare mount points</strong>, this is same as in NFS part where you identify your disk, format to ext4, add to /etc/fstab and mount it. For the purpose of this guide, we are going to assume the folder is <strong>/storage. <span class="wysiwyg-color-red">Create this folder on all nodes, even when there is no disk mounted to it, this is to avoid issues during installation.</span></strong></p>
<h2><span class="wysiwyg-color-black">Install Longhorn</span></h2>
<p><span class="wysiwyg-color-black">We are again going to use Helm. Please do the following on your main control node.</span></p>
<pre># Add longhorn to the helm<br>helm repo add longhorn https://charts.longhorn.io<br><br># Install Option1<br>helm install longhorn longhorn/longhorn --namespace longhorn-system --create-namespace --set defaultSettings.defaultDataPath="/storage"<br># Option 2<br>helm install longhorn longhorn/longhorn --namespace longhorn-system --create-namespace --set defaultSettings.defaultDataPath="/storage" --set service.ui.loadBalancerIP="10.0.0.71" --set service.ui.type="LoadBalancer"</pre>
<p><span class="wysiwyg-color-black">I have included two options how to install the Longhorn. <strong>Option</strong> <strong>1</strong> will install Longhorn as usual, but <strong>Option 2 </strong>create service for Longhorn UI and make is accessible on IP 10.0.0.71. The Option 2 is using the MetalLB we set up earlier.</span></p>
<p><span class="wysiwyg-color-black">Deployment can take a while (10 minutes sometimes), please wait until every pod is in status "Running"</span></p>
<pre>root@cube01:~# kubectl -n longhorn-system get pod<br>NAME READY STATUS RESTARTS AGE<br>longhorn-conversion-webhook-57986cf858-5frxd 1/1 Running 0 15m<br>longhorn-conversion-webhook-57986cf858-5qnnl 1/1 Running 0 15m<br>longhorn-recovery-backend-864d6fb7c-szflh 1/1 Running 0 15m<br>longhorn-ui-6bb85455bf-gdqwx 1/1 Running 0 15m<br>longhorn-recovery-backend-864d6fb7c-2gxxz 1/1 Running 0 15m<br>longhorn-admission-webhook-df85b9887-lvzm6 1/1 Running 0 15m<br>.<br>.<br>.<span class="wysiwyg-color-black"><br></span></pre>
<p>Check if StorageClass is present</p>
<pre>root@cube01:~# kubectl get storageclass<br>NAME     PROVISIONER        RECLAIMPOLICY VOLUMEBINDINGMODE ALLOWVOLUMEEXPANSION AGE<br>longhorn driver.longhorn.io Delete        Immediate         true                 16m</pre>
<h2>Configure</h2>
<p>Currently Longhorn is using /storage on every node, unless you are using SD card or internal storage, you might want to limit this to only Node1 to 3 which can have SSDs attached. Easyest way to do it is via web GUI. If you setup MetalLB and used <strong>Option 2</strong> install you can go<strong> <a href="http://10.0.0.71">http://10.0.0.71</a> </strong>and get the Longhorn GUI. If you have choosent <strong>Option 1</strong>, you can export the service to localhost with command:</p>
<pre>kubectl --namespace longhorn-system port-forward service/longhorn-frontend 9090:80</pre>
<p>And you should be able to get the sam GUI on http://127.0.0.1/#/dashboard</p>
<p><strong>Switch to Node tab, click on Operations and choose Edit node and add disks.</strong></p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9072170397853" alt="longhorn_ui_1.png"></p>
<p>Switch the disk "Scheduling" option to Disabled and click the Bin icon. Then click Save. This will remove the disk from the pool. Depending on your setup you might want to do that on Node4</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9072200936861" alt="longhorn_ui_2.png"></p>
<p> </p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9072200847773" alt="longhorn_ui_3.png"></p>
<p>In the same dialogue you add "disk" even though its a mount points.</p>
<h2>Test</h2>
<p>We will create sample PVC (Persistent Volume Claim) if size 10MB, create file <strong>pvc_test.yaml</strong> on your master control node.</p>
<pre>---<br>apiVersion: v1<br>kind: PersistentVolumeClaim<br>metadata:<br>  name: test-pvc<br>  namespace: default<br>spec:<br>  accessModes:<br>    - ReadWriteOnce<br>  storageClassName: longhorn<br>  resources:<br>    requests:<br>      storage: 10Mi</pre>
<p>And then apply it with:</p>
<pre>kubectl apply -f pvc_test.yaml</pre>
<p>You should be able to see the claim in GUI or with comand:</p>
<pre>root@cube01:~# kubectl get pvc<br>NAME     STATUS  VOLUME                                   CAPACITY ACCESS MODES STORAGECLASS AGE<br>test-pvc Bound   pvc-0da280c6-f26d-48d6-9fbc-5e17e4b9a750 10Mi     RWO          longhorn     39s</pre>
<p>In GUI</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9072471290781" alt="longhorn_ui_4.png"></p>
<h3><span class="wysiwyg-color-black">Cleanup</span></h3>
<p>Delete the PVC with:</p>
<pre>kubectl delete -f pvc_test.yam</pre>
<p><span class="wysiwyg-color-green120"><strong>W</strong><strong>e can now assume the test was successful and this StorageClass can be used across our cluster.</strong></span></p>